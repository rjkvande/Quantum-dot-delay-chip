# H1 Analysis Pipeline

**Repository:** quantum-dot-delay-chip  
**Path:** `/analysis/h1/`  
**Protocol doc:** `../../experimental-design.md`  
**Status:** Pipeline specification v0.1 — no data yet  

---

## Overview

This directory contains all analysis scripts, notebooks, and configuration
files for the H1 experiment (pump-timing and phase-modulation control of
polariton flow in a single perovskite waveguide).

Raw data lives **outside** this repo (see §Data Storage below). Scripts here
transform raw `.tif` stacks into HDF5 archives, extract dependent variables,
fit models, and produce the statistical outputs needed to evaluate H1's
falsification criteria (see `experimental-design.md §6`).

---

## Directory Structure

```
analysis/h1/
├── README.md                  ← this file
├── requirements.txt           ← Python dependencies (pinned)
├── config/
│   └── pipeline_config.yaml   ← paths, thresholds, fit parameters
├── scripts/
│   ├── 00_ingest.py           ← raw .tif stacks → HDF5
│   ├── 01_preprocess.py       ← dark subtraction, flat-field, drift correction
│   ├── 02_centroid.py         ← spatial centroid extraction from RS images
│   ├── 03_kspace.py           ← momentum-space peak fitting
│   ├── 04_phase_map.py        ← interferometric phase reconstruction
│   ├── 05_arrival_time.py     ← streak-camera wavepacket arrival fitting
│   ├── 06_fit_timing.py       ← sinusoidal + decay fit for Sub-Exp A (Δτ sweep)
│   ├── 07_fit_phase.py        ← linear slope fit for Sub-Exp B (φ_spatial sweep)
│   ├── 08_statistics.py       ← permutation test, Cohen's d, CI estimation
│   └── 09_figures.py          ← publication-quality figure generation
├── notebooks/
│   ├── h1_exploratory.ipynb   ← scratch/exploratory analysis
│   └── h1_report.ipynb        ← reproducible report; runs all scripts in order
└── tests/
    ├── test_centroid.py        ← unit tests for centroid extraction
    ├── test_phase_map.py       ← unit tests for phase unwrapping
    └── synthetic_data/        ← synthetic .tif stacks for CI testing
```

---

## Data Flow

```
Raw acquisition
    [CCD .tif stacks]  [Spectrometer .spe/.csv]  [Streak .tif]
           │                      │                     │
           └──────────────────────┴─────────────────────┘
                                  │
                          00_ingest.py
                                  │
                    [run_YYYYMMDD_NNN.hdf5]   ← one file per run
                         /          \
               01_preprocess.py      (metadata preserved verbatim)
                         │
              ┌──────────┼──────────┬────────────┐
              ▼          ▼          ▼            ▼
        02_centroid  03_kspace  04_phase_map  05_arrival_time
              │          │          │            │
              └──────────┴──────────┴────────────┘
                                  │
                         [processed.hdf5]
                         /              \
               06_fit_timing          07_fit_phase
               (Sub-Exp A)            (Sub-Exp B)
                         \              /
                          08_statistics
                                │
                         09_figures + h1_report.ipynb
```

---

## HDF5 File Schema

Every run produces exactly one `run_<run_id>.hdf5` file with the following key hierarchy:

```
run_<run_id>.hdf5
├── metadata/
│   ├── run_id              (str)   e.g. "H1-RUN-007"
│   ├── sample_id           (str)   matches materials-log.md batch_id
│   ├── waveguide_id        (str)   e.g. "WG2"
│   ├── date                (str)   ISO 8601
│   ├── operator            (str)
│   ├── git_commit          (str)   SHA of analysis repo at time of acquisition
│   ├── temperature_K       (float)
│   ├── pump_power_uW       (float) measured before objective
│   ├── pump_power_fraction_pth (float)
│   ├── sub_experiment      (str)   "A_timing" | "B_phase" | "baseline" | "control"
│   ├── independent_var     (str)   e.g. "delta_tau_ps" | "phi_spatial_rad_per_um"
│   └── independent_var_value (float)
├── raw/
│   ├── rs_images           (uint16, shape: [N_frames, H, W])
│   ├── kspace_images       (uint16, shape: [N_frames, H, W])
│   ├── interferogram       (uint16, shape: [N_frames, H, W])
│   ├── spectrometer        (float32, shape: [N_frames, N_pixels])
│   └── streak              (uint16, shape: [N_sweeps, N_time, N_space])
└── processed/              ← written by scripts 01–05; empty until pipeline runs
    ├── centroid_x_um       (float, shape: [N_frames])
    ├── centroid_x_mean_um  (float scalar)
    ├── centroid_x_std_um   (float scalar)
    ├── kspace_peak_kx      (float, shape: [N_frames])
    ├── phase_map           (float32, shape: [H, W])
    ├── phase_map_mean      (float32, shape: [H, W])
    ├── pl_intensity        (float, shape: [N_frames])
    └── arrival_time_ps     (float scalar)
```

---

## Script Specifications

### `00_ingest.py`
**Input:** Directory of raw acquisition files for one run  
**Output:** `run_<run_id>.hdf5` with all raw arrays and metadata populated  
**Key behaviour:**
- Reads acquisition metadata from the instrument sidecar file (`.xml` / `.json`).
- Embeds `git_commit` of the analysis repo at ingest time.
- Verifies frame count matches expected value from `pipeline_config.yaml`.
- Raises `IngestError` if any required metadata field is missing — no silent defaults.

### `01_preprocess.py`
**Input:** `run_<run_id>.hdf5` (raw/)  
**Output:** Writes corrected frames back to `processed/` group  
**Steps:** dark-frame subtraction → flat-field normalization → rigid drift correction
(cross-correlation between frame 0 and each subsequent frame; discard frames with
drift > 0.5 µm from `config.drift_threshold_um`).

### `02_centroid.py`
**Input:** Processed RS image stack  
**Output:** `centroid_x_um` array; `centroid_x_mean_um`; `centroid_x_std_um`  
**Method:** Intensity-weighted centroid:
```
x̄ = Σ(I_i · x_i) / Σ(I_i)
```
restricted to a spatial ROI defined in `pipeline_config.yaml`.
Uncertainty: standard deviation across frames (not standard error — captures physical fluctuations).

### `03_kspace.py`
**Input:** Processed k-space image stack  
**Output:** `kspace_peak_kx` (peak k_x per frame)  
**Method:** Gaussian fit to the k_x marginal distribution of each frame.
Report fit χ² per frame; flag frames where χ² > `config.kspace_chi2_threshold`.

### `04_phase_map.py`
**Input:** Processed interferogram stack  
**Output:** `phase_map` (per-frame); `phase_map_mean`  
**Method:**
1. Fourier filter: isolate the +1 diffraction order.
2. Inverse Fourier transform → complex field.
3. Take argument → wrapped phase.
4. 2D phase unwrapping (Goldstein branch-cut algorithm or quality-guided).
5. Subtract reference phase map (from baseline run at same waveguide position).

### `05_arrival_time.py`
**Input:** Processed streak-camera data  
**Output:** `arrival_time_ps`  
**Method:** Fit a Gaussian or asymmetric Gaussian to the temporal marginal of the
streak image at the output facet spatial coordinate.
Report fit centre (arrival time) and FWHM (wavepacket duration).

### `06_fit_timing.py` — Sub-Experiment A model fit
**Input:** Aggregated `centroid_x_mean_um` vs. `delta_tau_ps` across all positions  
**Model:**
```
x̄_pol(Δτ) = A · cos(ω · Δτ + φ₀) · exp(−Δτ / τ_decay) + x̄₀
```
**Fit method:** Non-linear least squares (scipy.optimize.curve_fit); report 95% CI
on each parameter.  
**Output:** `fit_timing_results.json` with keys: `A`, `A_ci`, `omega`, `omega_ci`,
`tau_decay`, `tau_decay_ci`, `x0`, `phi0`, `r_squared`.

### `07_fit_phase.py` — Sub-Experiment B model fit
**Input:** Aggregated `centroid_x_mean_um` vs. `phi_spatial_rad_per_um`  
**Model:** `x̄_pol = m · φ_spatial + b`  
**Fit method:** Ordinary least squares with bootstrap CI (N=10,000 resamples).  
**Output:** `fit_phase_results.json` with keys: `slope_m`, `slope_m_ci_95`,
`intercept_b`, `r_squared`, `n_points`.

### `08_statistics.py`
**Purpose:** Evaluate H1 falsification criteria from `experimental-design.md §6`  
**Tests run:**
1. **Two-sided permutation test** on centroid shift magnitude (N=10,000 permutations).
   Report p-value.
2. **Cohen's d** for effect size between modulated and baseline centroid distributions.
3. **Sign-flip test** (Sub-Exp B only): verify that negative φ_spatial ramp produces
   negative centroid shift. Report fraction of position pairs where sign is consistent.
4. **Control contrast**: compare modulated-guide shifts vs. control-condition shifts;
   confirm controls are null (p > 0.10 for control, p < 0.05 for H1).

**Output:** `h1_statistics_summary.json`; human-readable `h1_statistics_summary.md`
that maps each result to the corresponding falsification criterion by number.

### `09_figures.py`
**Figures produced:**
| Filename | Content |
|----------|----------|
| `fig_baseline_profiles.pdf` | RS emission and k-space baseline at 3 power levels |
| `fig_timing_centroid.pdf` | x̄_pol vs. Δτ with model fit and 95% CI band |
| `fig_timing_kspace.pdf` | k_x peak shift vs. Δτ |
| `fig_phase_centroid.pdf` | x̄_pol vs. φ_spatial with linear fit; inset: sign-flip panel |
| `fig_phase_maps.pdf` | Phase map panels at φ_spatial = 0, π/2, π rad/µm |
| `fig_controls.pdf` | Control conditions (bare SiO₂, above-resonance) centroid distributions |
| `fig_statistics_summary.pdf` | Forest plot of effect sizes and CIs across all conditions |

All figures: 300 dpi PDF, colour-blind-safe palette (Okabe-Ito).

---

## Reproducing Results

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Edit config/pipeline_config.yaml to point to your raw data directory

# 3. Ingest a run
python scripts/00_ingest.py --run-dir /path/to/raw/H1-RUN-007/

# 4. Run the full pipeline on one file
python scripts/01_preprocess.py run_H1-RUN-007.hdf5
python scripts/02_centroid.py   run_H1-RUN-007.hdf5
python scripts/03_kspace.py     run_H1-RUN-007.hdf5
python scripts/04_phase_map.py  run_H1-RUN-007.hdf5
python scripts/05_arrival_time.py run_H1-RUN-007.hdf5

# 5. Aggregate across runs and fit models (after all runs are processed)
python scripts/06_fit_timing.py  --sub-exp A
python scripts/07_fit_phase.py   --sub-exp B
python scripts/08_statistics.py
python scripts/09_figures.py

# 6. Or run everything reproducibly via the report notebook
jupyter nbconvert --to notebook --execute notebooks/h1_report.ipynb
```

---

## Versioning and Reproducibility Rules

- Every HDF5 file stores the `git_commit` of this repo at the time of ingest. If you
  re-analyse old data with a new script version, add a `reanalysis_git_commit` key to
  the `processed/` group — **never overwrite the original `git_commit`**.
- `pipeline_config.yaml` is version-controlled. Any parameter change that would alter
  numerical results must be committed with a message explaining the reason before
  re-running the pipeline.
- Do not manually edit HDF5 files. If a value needs correction, fix the upstream script
  and re-run the pipeline from the affected step.
- The `tests/` suite must pass before any script version is tagged as used in a
  publication-bound analysis run:
  ```bash
  pytest tests/ -v
  ```

---

## Run ID Convention

`H1-RUN-NNN` where NNN is zero-padded to three digits, incrementing globally
across sub-experiments.  
Examples: `H1-RUN-001` (first baseline), `H1-RUN-014` (Sub-Exp B, run 3).

Track the run registry in `run_registry.csv` at the repo root (one row per run):

```csv
run_id,date,sample_id,waveguide_id,sub_experiment,independent_var,independent_var_value,operator,notes
H1-RUN-001,2026-06-10,MAT-20260601-001,WG2,baseline,none,0.0,RV,First baseline acquisition
```
