# Experimental Design — H1: Pump-Timing and Phase-Modulation Control of Polariton Flow in a Single Perovskite Waveguide

**Repository:** quantum-dot-delay-chip  
**Hypothesis:** H1  
**Status:** Draft v0.1  
**Author(s):** *(fill in)*  
**Date:** 2026-06-05  
**Depends on:** `docs/physics-assumptions.md §H1`, `materials-log.md`, `fab-notes.md`  

---

## Table of Contents

1. [Objective](#1-objective)
2. [Waveguide Geometry and Optical Layout](#2-waveguide-geometry-and-optical-layout)
3. [Variables](#3-variables)
4. [Experimental Protocol](#4-experimental-protocol)
5. [Expected Signatures of Success](#5-expected-signatures-of-success)
6. [Falsification Criteria](#6-falsification-criteria)
7. [Ambiguous-Result Interpretation](#7-ambiguous-result-interpretation)
8. [Risks and Failure Modes](#8-risks-and-failure-modes)
9. [Connection to H2 and H3](#9-connection-to-h2-and-h3)
10. [Appendix: Measurement Checklist](#10-appendix-measurement-checklist)

---

## 1. Objective

### 1.1 Hypothesis Statement (H1)

> *Structured pump-timing or phase modulation applied to the excitation field of a single perovskite exciton-polariton waveguide can deterministically reshape the spatial density profile or accumulated phase of the propagating polariton wavepacket, producing a measurable and reproducible contrast in the output field relative to an unmodulated reference.*

### 1.2 Scientific Motivation

Exciton-polaritons in perovskite waveguides inherit both the nonlinear repulsive interactions of their excitonic fraction and the micron-scale propagation length of their photonic fraction. These properties make them candidates for reconfigurable delay and routing elements in integrated photonic circuits — particularly in architectures where quantum-dot (QD) emitters serve as single-photon sources that must be routed with sub-picosecond timing precision.

Before QD integration (H2) or multi-guide networking (H3) can be attempted, it must be established at the single-waveguide level that:

1. The polariton wavepacket is steerable by *external* temporal or phase control rather than only by static lithographic boundaries.
2. The degree of steering is large enough to exceed measurement noise and fabrication-induced disorder — i.e., the effect is not masked by sample-to-sample variance.
3. The phenomenon is reproducible across multiple pump cycles at the same sample position.

This experiment addresses all three requirements.

### 1.3 Success Criterion (One-Sentence)

H1 is supported if, at ≥ 3 independent sample positions, a statistically significant (p < 0.05, Cohen's d > 0.5) shift in the polariton spatial centroid or interferometric phase contrast is observed as a function of a single controlled pump parameter, with the direction of the shift following the predicted monotonic trend.

---

## 2. Waveguide Geometry and Optical Layout

### 2.1 Sample Geometry

```
      z  (propagation axis)
      ↑
      │   ┌─────────────────────────────────────────┐
      │   │  Perovskite ridge waveguide (WG1)        │
      │   │  Width w = 3–5 µm  ·  Height h = 200 nm  │
      │   └─────────────────────────────────────────┘
      │        ← L = 50–100 µm propagation length →
      │
      └──────────────────────────────────────────────────── x
              [Pump spot, ~2 µm FWHM, positioned at z = 0]
              [Collection window at z = L]
```

- **Material:** Methylammonium lead iodide (MAPbI₃) or CsPbBr₃ thin-film ridge on SiO₂/Si.
- **Polariton branch:** Lower polariton branch (LPB); Rabi splitting target Ω_R ≥ 50 meV.
- **Operating temperature:** 4–77 K (cryostat); record all data at a fixed temperature ± 0.5 K.
- **Waveguide selection:** Choose guides with disorder-induced emission variance < 5% along the propagation axis (verified by PL mapping before H1 runs begin).

### 2.2 Optical Setup

```
  Ti:Sa oscillator (80 MHz)
      │
      ├──[Pulse shaper]──────────────────────────────────────────┐
      │   (4f grating + SLM or EOM)                              │
      │   Controls: Δτ (inter-pulse delay), Δφ (spectral phase)  │
      │                                                           ↓
      │                                               [Microscope objective, NA 0.7]
      │                                                  [Sample in cryostat]
      │                                                  [Collection objective]
      │                                                           │
      └──[Reference arm (unmodulated)]                           │
              │                                                  ↓
              └─────────────────────[Beam splitter]──[CCD / EMCCD]
                                                      [Spectrometer]
                                                      [HBT setup — reserved for H2]
```

- **Pump wavelength:** Tuned 2–5 meV above LPB minimum; verified via reflectance spectroscopy before each run.
- **Pump power:** Set below the polariton lasing threshold P_th (measure P_th first; run H1 at 0.1 P_th, 0.3 P_th, 0.5 P_th to characterise the density regime).
- **Imaging:** Real-space (RS) emission images and momentum-space (k-space / Fourier-plane) images collected on separate CCD channels or sequentially with a flip mirror.
- **Interferometry arm:** Michelson configuration to extract the spatial phase map; path-length difference stabilized to < λ/20 using a piezo-driven retroreflector with feedback.

---

## 3. Variables

### 3.1 Independent Variables

| Symbol | Parameter | Range | Steps | Notes |
|--------|-----------|-------|-------|-------|
| Δτ | Inter-pulse delay (pump-timing arm) | 0 – 20 ps | ≥ 8 | Referenced to polariton lifetime τ_pol |
| Δφ | Global spectral phase shift (flat, GDD, TOD) | 0 – 2π (flat); ±5000 fs² (GDD) | ≥ 6 | Applied via SLM in 4f shaper |
| φ_spatial | Transverse spatial phase ramp across pump spot | 0 – π rad/µm | ≥ 5 | Imparts lateral k-kick to injected polaritons |
| P_pump | Pump fluence | 0.1 – 0.5 P_th | 3 | Fixes density regime; not the primary swept variable |

Run Δτ and Δφ sweeps in separate sub-experiments (§4.3 and §4.4) to avoid a combinatorial explosion in the first pass. A joint sweep (Δτ × φ_spatial) is planned for H1 v2 only if both sub-experiments individually show significant effect.

### 3.2 Dependent Variables

| Symbol | Parameter | Instrument | Precision target |
|--------|-----------|------------|-----------------|
| x̄_pol | Polariton spatial centroid along x at output facet | RS-CCD (16-bit) | ± 0.1 µm |
| I(k_x) | Far-field momentum distribution | k-space CCD | k-resolution < 0.1 µm⁻¹ |
| Φ(x, z) | Spatial phase map (interferometric) | Michelson + CCD | ± 0.05 rad |
| n_pol | Integrated polariton density proxy (PL intensity) | Spectrometer | ± 3% shot-to-shot |
| τ_arrival | Polariton wavepacket arrival time at z = L | Streak camera | ± 0.5 ps |

### 3.3 Controlled Variables (must be logged for every run)

- Sample temperature (± 0.5 K)
- Laser power (measured before objective, logged per acquisition)
- Pump spot position on waveguide (piezo stage; drift < 0.2 µm per run sequence)
- Spectrometer grating position and centre wavelength
- Integration time and number of frames averaged
- Cryostat vibration level (accelerometer log; discard runs with > threshold)

### 3.4 Nuisance Variables and Mitigation

| Nuisance | Source | Mitigation |
|----------|--------|------------|
| Phonon-bath fluctuations | Temperature drift | Closed-loop cryo PID; log temp per acquisition |
| Disorder-induced scattering | Film grain boundaries | Pre-select guides by PL uniformity map |
| Laser pointing drift | Thermal expansion of optical table | Active beam-stabilisation or check-and-correct every 30 min |
| SLM phase drift | SLM temperature | Calibrate SLM phase response at start of each session |
| Photobleaching / degradation | Halide perovskite instability | Track PL peak position and intensity; replace sample if drift > 5% |

---

## 4. Experimental Protocol

### 4.0 Prerequisites (complete before any H1 run)

- [ ] Reflectance / transmission spectrum collected; LPB minimum wavelength identified.
- [ ] Polariton lasing threshold P_th measured by power-dependent PL (log-log slope analysis).
- [ ] PL uniformity map of candidate waveguides collected; select guides meeting < 5% variance.
- [ ] SLM calibration completed (phase-vs-voltage lookup table validated at pump wavelength).
- [ ] Interferometer fringe visibility > 0.8 confirmed on a reference mirror.
- [ ] Streak camera timing calibrated with a known optical delay line.
- [ ] `materials-log.md` entry created for this sample batch.

### 4.1 Baseline Acquisition (no modulation)

**Purpose:** Establish the unperturbed polariton output profile against which all modulation runs will be compared.

1. Set pulse shaper to pass-through (flat spectral phase, single pulse, no delay arm).
2. Set pump power to each of the three density levels (0.1, 0.3, 0.5 P_th).
3. At each power level, collect:
   - 200-frame RS image stack.
   - 200-frame k-space image stack.
   - 50-frame interferometric phase map.
   - Single spectrometer trace (LPB peak position, linewidth).
   - Streak-camera trace (arrival time distribution, 100 sweeps).
4. Repeat at 3 spatially distinct positions on the same waveguide (step ≥ 10 µm apart).
5. Compute mean and standard deviation of each dependent variable. These become the baseline distributions B(x̄_pol), B(I(k_x)), B(Φ), B(n_pol), B(τ_arrival).

> **Stop condition:** If shot-to-shot variance in x̄_pol > 0.5 µm under baseline, the waveguide likely has excessive disorder. Select a different guide and repeat §4.1.

### 4.2 Reference Modulation Check (sanity)

Before sweeping physics parameters, verify the pulse shaper is introducing the intended modulation by routing the pump beam to a photodiode / cross-correlator and confirming:

- Δτ sweep produces the expected autocorrelation fringe shift (cross-correlator).
- Δφ (GDD) sweep produces the expected pulse broadening (FROG or intensity autocorrelator).
- φ_spatial ramp produces the expected far-field tilt in a free-space beam (CCD outside cryostat).

Log all calibration traces. Do **not** proceed to §4.3 if any calibration fails.

### 4.3 Sub-Experiment A — Pump-Timing (Δτ) Sweep

**Physical mechanism under test:** Two successive pump pulses separated by Δτ inject two polariton wavepackets. Constructive or destructive interference between the wavepackets at the propagation wavefront should produce a Δτ-dependent modulation of the output density and momentum distribution. At specific delays matching τ_pol or its harmonics, the trailing pulse may re-amplify the leading wavepacket rather than interfere destructively, producing an extended polariton tail that shifts the output centroid.

**Procedure:**

1. Fix P_pump = 0.3 P_th (intermediate density; nonlinear effects present but below lasing).
2. Split pump into two arms; set arm-2 power = arm-1 power (equal-amplitude pulses).
3. Sweep Δτ from 0 to 20 ps in steps of 2.5 ps (8 steps). Record all 5 dependent variables at each Δτ.
4. Repeat at 3 waveguide positions.
5. Repeat the full sweep once at P_pump = 0.1 P_th (linear regime) to decouple density effects.

**Data reduction:**

- Plot x̄_pol vs. Δτ and fit a sinusoidal + exponential decay model:
  `x̄_pol(Δτ) = A · cos(ω · Δτ + φ₀) · exp(−Δτ / τ_decay) + x̄₀`
- Extract: amplitude A, frequency ω, decay constant τ_decay.
- Compare τ_decay to independently measured polariton lifetime (from time-resolved PL).

### 4.4 Sub-Experiment B — Phase Modulation Sweep

**Physical mechanism under test:** A spatial phase ramp φ_spatial across the pump focal spot imparts a lateral momentum kick Δk_x = φ_spatial / µm to the injected polariton population. Because the polariton dispersion E(k_x) is approximately parabolic, a momentum kick translates into a transverse drift velocity v_x = ħk_x / m*, deflecting the flow and displacing the output centroid.

**Procedure:**

1. Fix P_pump = 0.3 P_th; Δτ = 0 (single pulse).
2. Apply SLM-generated spatial phase ramp φ_spatial = 0, π/4, π/2, 3π/4, π rad/µm across the pump spot.
3. At each φ_spatial, collect all 5 dependent variables at 3 waveguide positions.
4. Repeat at a negative ramp (−π/4, −π/2 rad/µm) to confirm the sign reversal of Δx̄_pol.
5. Optionally: replace the linear ramp with a quadratic (focusing) phase profile to test whether polariton wavepacket compression is achievable.

**Data reduction:**

- Plot x̄_pol vs. φ_spatial and fit a linear model `x̄_pol = m · φ_spatial + b`.
- Extract slope m (µm per rad/µm); compare to prediction from polariton effective mass m* (estimated from LPB curvature measured in k-space images at baseline).
- Confirm sign flip between positive and negative ramp.

### 4.5 Control Experiments

| Control | Purpose | Pass criterion |
|---------|---------|----------------|
| Bare SiO₂ substrate under identical pump conditions | Confirm output-centroid shift is not a pump-scattering artefact | No centroid shift > 0.1 µm |
| Pump above exciton resonance (no polariton formation) | Confirm shift requires the polariton regime | No shift in output centroid |
| Blocked collection arm (dark counts) | Quantify detector noise floor | Counts < 0.5% of signal |
| Frozen Δτ at 0 but arm-2 power varied | Distinguish timing from intensity effects | No centroid shift at fixed Δτ = 0 |

### 4.6 Data Archiving Requirements

- Raw frames saved as `.tif` stacks with acquisition metadata embedded in EXIF/header.
- Processed results saved as `.hdf5` (one file per run; keys: `raw`, `processed`, `metadata`).
- Each run tagged with: `run_id`, `sample_id`, `waveguide_id`, `date`, `operator`, `git_commit` of analysis script.
- All analysis scripts version-controlled in `/analysis/h1/`; no manual data editing without a logged reason.

---

## 5. Expected Signatures of Success

### 5.1 Timing Sub-Experiment (A)

| Observable | Expected (H1 true) | Expected (H1 false / null) |
|------------|-------------------|---------------------------|
| x̄_pol vs. Δτ | Oscillatory with period ≈ 2π/ω_LPB; amplitude > 0.3 µm | Flat; variation within baseline σ |
| I(k_x) vs. Δτ | Peak shifts laterally and narrows at constructive delays | No systematic shift |
| τ_arrival vs. Δτ | Modulated arrival time (constructive delay → earlier arrival) | Flat |
| Phase map Φ | Phase fringes shift by ~π between constructive and destructive delays | No shift beyond noise |

### 5.2 Phase-Modulation Sub-Experiment (B)

| Observable | Expected (H1 true) | Expected (H1 false / null) |
|------------|-------------------|---------------------------|
| x̄_pol vs. φ_spatial | Linear with slope m = ħL / (m* · v_g); typical estimate: 0.15–0.5 µm per π rad/µm | No slope; m consistent with zero |
| k_x peak in far field | Shifts by Δk_x ≈ φ_spatial in the direction of the ramp | Centred on k_x = 0 regardless of ramp |
| Sign flip on negative ramp | x̄_pol reverses sign; essential for confirming directionality | No reversal |
| Phase map Φ | Acquired phase accumulates additional tilt matching φ_spatial | Phase map identical to baseline |

### 5.3 Cross-Signatures (both sub-experiments)

- Effect magnitude should scale with the photonic fraction (|X_k|²) of the LPB mode at the pump k-vector. Verify by comparing results at slightly different pump detunings.
- Effect should **not** be present above P_th (lasing regime), where the condensate locks to a single k-state regardless of pump phase, or the relationship should be clearly different. This provides an internal control on the polariton nature of the effect.
- Sample-to-sample reproducibility: at least 2 independent sample chips should show qualitatively consistent results before H1 is declared supported.

---

## 6. Falsification Criteria

H1 is **falsified** if **any** of the following conditions are met:

1. **Null centroid response (timing):** After pooling data across ≥ 3 waveguide positions and ≥ 2 sample chips, the fitted amplitude A from §4.3 is statistically indistinguishable from zero (95% CI contains zero; permutation test p > 0.10).

2. **Null centroid response (phase):** The slope m from §4.4 is not significantly different from zero by the same statistical criteria, *and* no systematic trend is visible in the raw data by eye across all ramp values.

3. **Artefact reproduction:** The same centroid shifts appear in the bare-SiO₂ control or the above-resonance control (§4.5), indicating the effect is a pump-scattering or thermal artefact rather than a polariton phenomenon.

4. **Irreproducibility within a sample:** The direction of x̄_pol shift reverses non-monotonically across waveguide positions at a fixed parameter value, with position-to-position variance exceeding the modulation amplitude, suggesting disorder is the dominant effect.

5. **Loss of polariton character:** PL spectral linewidth under modulation broadens by > 3× relative to baseline, indicating the pump is pushing the system above the polariton coherence window rather than shaping it.

> **Important:** Failure to observe an effect at a single sample chip or waveguide is **not** sufficient for falsification — disorder and fabrication variance are expected. The bar is consistent null results across chips and positions combined.

---

## 7. Ambiguous-Result Interpretation

This section defines pre-registered decision rules for outcomes that fall between clear support and clear falsification.

### 7.1 Partial Significance (one sub-experiment passes, one fails)

- **Phase modulation works, timing does not:**
  - Most likely cause: polariton lifetime τ_pol is shorter than the minimum accessible Δτ (2.5 ps), so the trailing pulse arrives after the leading wavepacket has decayed.
  - Action: Measure τ_pol independently via time-resolved PL and compare to sweep range. If τ_pol < 2.5 ps, shorten the Δτ step to 0.5 ps (requires reconfiguring the delay arm). Record as H1-A-limited; H1-B confirmed; update hypotheses.md.

- **Timing works, phase modulation does not:**
  - Most likely cause: SLM spatial resolution is insufficient to impart a well-defined phase ramp within the 2 µm pump spot, or the waveguide is too narrow to support transverse displacement.
  - Action: Verify SLM pixel mapping at the pump plane. If the ramp is confirmed by free-space calibration, try a wider waveguide (w = 8–10 µm). Record as H1-B-geometry-limited.

### 7.2 Effect Present but Below Predicted Magnitude

- Centroid shift is statistically significant but smaller than predicted by the effective-mass estimate by > 2×.
- Interpretation: The polariton effective mass m* at the operating point is heavier than estimated from the LPB curvature (possible if the pump k-vector lies outside the parabolic region), or disorder reduces the mean free path.
- Action: Re-measure m* by pump-angle-resolved PL; fit the dispersion more carefully near the pump k-vector. Update the quantitative prediction and re-assess.
- **Do not downgrade H1 to falsified** solely because the magnitude is smaller than expected; downgrade only if the effect is directionally inconsistent or statistically null.

### 7.3 Effect Present Only at One Power Level

- If the shift is observed at 0.5 P_th but not at 0.1 P_th:
  - Likely nonlinear polariton-polariton interactions are required to translate the pump phase into a density redistribution; the linear regime is too weakly coupled.
  - Record as H1-nonlinear-regime; update hypotheses.md with the power-regime caveat.
  - Implication for H2: QD photon injection into the waveguide will need to produce sufficient polariton density, which constrains the coupling efficiency spec.

### 7.4 Inconsistent Sign of Effect Across Positions

- If the direction of x̄_pol shift is positive at some positions and negative at others for the same parameter:
  - Strongly suggests disorder-induced mode hybridization, not a deterministic effect of the pump.
  - Action: Collect PL maps of all tested positions and correlate shift direction with local PL intensity / linewidth. If correlation is found, the disorder is the modulating variable, not the pump.
  - Flag as H1-disorder-dominated; this is a partial falsification of the *deterministic* claim even if the magnitude is non-zero.

### 7.5 Sample Degradation Mid-Run

- If PL peak position drifts > 2 nm or intensity drops > 10% during a run:
  - Abort the run; do not include data in analysis.
  - Switch to a fresh sample region or new chip.
  - If degradation is systematic and unavoidable, investigate encapsulation options before continuing (see §8.2); log in `materials-log.md`.

---

## 8. Risks and Failure Modes

### 8.1 Technical Risks

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Polariton lifetime < 2.5 ps (too short for Δτ sweep) | Medium | High | Pre-measure τ_pol by TRPL; adjust delay step size before committing to §4.3 |
| SLM phase resolution insufficient at pump-spot scale | Low–Medium | Medium | Pre-calibrate SLM at the focussed spot plane; switch to amplitude DMD if insufficient |
| Waveguide disorder dominates over pump control | Medium | High | Rigorous pre-selection (§4.0); multiple chips; if 3+ chips fail, reassess material |
| Perovskite photobleaching under repeated illumination | High | Medium | Limit cumulative dose per position; translate sample between sweeps; use encapsulated films |
| Interferometer phase drift during acquisition | Medium | High | Active PZT stabilization + reference fringe monitoring; discard frames with fringe drift > λ/20 |
| Pump power instability (laser mode-hopping) | Low | Medium | Monitor power before objective with fast photodiode; discard acquisitions flagged by monitor |
| Cryostat vibration coupling into the focus | Medium | Medium | Vibration-isolation legs; measure accelerometer; schedule runs during low-vibration periods |

### 8.2 Scientific / Conceptual Risks

| Risk | Description | Contingency |
|------|-------------|-------------|
| Polariton propagation length < waveguide length | Polaritons decay before reaching the collection window at z = L | Shorten guide to L = 20 µm or shift collection window; test at higher photonic fraction by detuning |
| Strong disorder localisation | Polaritons are Anderson-localised; no propagating mode exists | Switch to lower-disorder material (CsPbBr₃ monocrystalline plates or GaN-based guides); park H1 pending material improvement |
| Polariton condensation at low threshold | Spontaneous lasing below intended operating power removes the pump-phase sensitivity | Reduce temperature to raise threshold; use lower-quality-factor geometry |
| Output signal dominated by scattered laser | Insufficient pump/signal separation | Add spatial filter at pump position; use cross-polarisation rejection; verify via Raman/PL spectral separation |
| Effective-mass nonparabolicity at pump k-vector | Predictions from parabolic model fail; quantitative comparison is unreliable | Measure m*(k) directly and use the local curvature for predictions |

### 8.3 Go / No-Go Decision Points

| Milestone | Go criterion | No-go action |
|-----------|-------------|-------------|
| After §4.0 pre-screening | ≥ 2 guides pass PL uniformity test | Fabricate new batch with improved recipe before H1 runs begin |
| After §4.2 calibration | All 3 modulation calibrations pass | Diagnose and fix pulse shaper before proceeding |
| After §4.1 baseline (first chip) | Shot-to-shot x̄_pol variance < 0.5 µm | Select new guide; if fails on all guides, reassess material |
| After §4.3 + §4.4 (first chip) | At least one sub-experiment shows p < 0.10 trend | Run second chip immediately; if both chips null, escalate to full team review |

---

## 9. Connection to H2 and H3

### 9.1 How H1 Enables H2

**H2 (working title):** *A quantum dot evanescently coupled to the perovskite waveguide can inject single photons that hybridize with polaritons, and the resulting polariton-photon quasiparticle can be routed or delayed by the same pump-timing and phase controls validated in H1.*

H1 provides two essential prerequisites for H2:

1. **Validated control handles.** If H1 confirms that Δτ and φ_spatial deterministically steer polariton flow, these exact parameters become the routing controls in H2. The quantitative relationship (slope m, amplitude A) measured in H1 directly informs the required control precision for QD-polariton routing.

2. **Polariton lifetime and coherence budget.** The τ_decay extracted from the Δτ sweep (§4.3) establishes the coherence window within which a QD photon must be injected to hybridize. This sets the timing jitter specification for the QD excitation pulse in H2 experiments.

**H1 result → H2 design implication table:**

| H1 outcome | H2 design change |
|------------|-----------------|
| τ_decay ≫ 10 ps | Wide timing tolerance; QD pulse jitter spec relaxed to ±2 ps |
| τ_decay < 5 ps | Sub-ps jitter required; QD must be resonantly driven; increases complexity |
| φ_spatial slope m ≥ 0.3 µm per π rad/µm | Spatial routing is viable; QD placement tolerance ± 0.5 µm acceptable |
| φ_spatial slope m < 0.05 µm per π rad/µm | Routing insufficient; H2 may need static lithographic splitter instead of dynamic phase control |
| H1 falsified (null) | H2 pump-shaping protocol must be redesigned; revisit material or geometry before H2 begins |

### 9.2 How H1 Enables H3

**H3 (working title):** *A network of N ≥ 2 coupled perovskite waveguides, each individually characterized by H1 and H2 protocols, can implement a delay-and-route function for sequential quantum-dot photon pulses with a total throughput latency controllable to within ±1 ps.*

H1's role in H3 is more distal but still load-bearing:

1. **Single-guide characterisation forms the network node model.** H3 uses the H1-measured polariton flow parameters (m*, τ_decay, disorder amplitude) as input to a network simulation. Without reliable single-guide numbers, the multi-guide network model is under-constrained.

2. **Falsification propagation.** If H1 shows that disorder prevents deterministic control in a single guide, H3's assumption of coherent inter-guide routing is invalidated and must be redesigned around incoherent (intensity-only) switching.

3. **Phase-control scaling.** H3 will require simultaneous SLM addressing of multiple pump spots. The number of independently addressable phase zones needed is:
   `N_zones = N_guides × (N_routing_states per guide)`.
   H1's phase resolution requirement (number of distinguishable φ_spatial steps) sets the lower bound on SLM pixel budget per guide, which propagates directly into the H3 optical layout.

### 9.3 Dependency Graph

```
[H1-A: Timing]──────────┐
                         ├──► [H2: QD-polariton routing] ──► [H3: Multi-guide delay network]
[H1-B: Phase mod.]──────┘
         │
         └──► [Material characterisation update in materials-log.md]
                    │
                    └──► [Feeds fab-notes.md iteration for H2 sample design]
```

---

## 10. Appendix: Measurement Checklist

Use this checklist at the start of every H1 measurement session. Initial each item.

```
Session date: ____________   Operator: ____________   Sample ID: ____________

[ ] Cryostat at target temperature; PID stable for ≥ 30 min
[ ] Laser wavelength verified against wavemeter
[ ] Pump power measured at objective entrance pupil (log value: _____ µW)
[ ] P_th measured or confirmed from previous session log (log value: _____ µW)
[ ] SLM phase calibration file loaded and spot-check passed
[ ] Interferometer fringe visibility checked (measured: _____ ; threshold: > 0.8)
[ ] Streak camera timing calibration verified
[ ] Accelerometer reading within normal range (log value: _____ mg RMS)
[ ] Baseline waveguide selected and PL uniformity variance confirmed < 5%
[ ] Data folder created with correct run_id and git_commit logged
[ ] Analysis script version matches expected hash in analysis/h1/README.md
```

---

*End of document. Next document in sequence: `h2-experimental-design.md` (to be drafted after H1 data collection is complete or after H1 go/no-go decision at first milestone).*
