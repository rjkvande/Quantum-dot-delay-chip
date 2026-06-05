# Materials Log — Quantum-dot-delay-chip

All sample batches used in H1, H2, and H3 experiments are recorded here.
One entry per fabrication batch. Within a batch, individual chips are tracked
in the per-run HDF5 metadata (see `/analysis/h1/README.md`).

**Schema version:** 1.0  
**Maintainer:** *(fill in)*  
**Last updated:** 2026-06-05  

---

## Entry Schema

Each entry is an H2-level heading: `## [BATCH-ID]`

Required fields are marked **bold**. Fill every field; use `N/M` (not measured)
if a value is genuinely unavailable — do not leave blanks.

| Field | Type | Description |
|-------|------|-------------|
| **batch_id** | string | Unique identifier: `MAT-YYYYMMDD-NNN` |
| **date_fabricated** | ISO date | Date the film or crystal was produced |
| **operator** | string | Who fabricated the sample |
| **material_class** | enum | `MAPbI3` / `MAPbBr3` / `CsPbBr3` / `FAPbI3` / `other` |
| **composition** | string | Exact stoichiometry or precursor ratio (e.g., `MA:Pb:I = 1:1:3`) |
| **substrate** | string | e.g., `SiO2/Si (300 nm oxide)`, `glass`, `Si` |
| **deposition_method** | enum | `spin-coat` / `CVD` / `MBE` / `solution-growth` / `exfoliation` / `other` |
| **film_thickness_nm** | float | Measured by profilometer or AFM; nominal if not measured |
| **waveguide_width_um** | float or range | e.g., `3–5` or `4.2` |
| **waveguide_length_um** | float | Nominal design length |
| **patterning_method** | string | e.g., `EBL + Ar-ion milling`, `FIB`, `photolithography` |
| **encapsulation** | string | e.g., `PMMA (100 nm)`, `SiN (ALD, 20 nm)`, `none` |
| **rabi_splitting_mev** | float | Ω_R from reflectance fit; N/M if not yet measured |
| **lpb_min_wavelength_nm** | float | LPB minimum from angle-resolved PL; N/M if not measured |
| **lasing_threshold_uW** | float | P_th at the measurement temperature; N/M if not measured |
| **pl_uniformity_variance_pct** | float | Shot-to-shot PL variance along guide; from pre-screening scan |
| **polariton_lifetime_ps** | float | τ_pol from time-resolved PL; N/M if not measured |
| **disorder_metric** | string | Qualitative: `low (<3%)` / `medium (3–10%)` / `high (>10%)` |
| **measurement_temperature_K** | float | Temperature at which optical parameters were measured |
| **h1_eligible** | bool | `true` if PL uniformity < 5% AND Ω_R ≥ 50 meV |
| **assigned_experiments** | list | Which experiment IDs used this batch (e.g., `[H1-RUN-001, H1-RUN-004]`) |
| **notes** | string | Degradation observations, handling incidents, storage conditions |
| **storage_location** | string | Physical location of sample (box, shelf, desiccator) |
| **disposed_date** | ISO date or `active` | When the sample was discarded |

---

## Entry Template

Copy this block and fill it for each new batch:

```
## MAT-YYYYMMDD-NNN

| Field | Value |
|-------|-------|
| **batch_id** | `MAT-YYYYMMDD-NNN` |
| **date_fabricated** | `YYYY-MM-DD` |
| **operator** | *(name)* |
| **material_class** | *(e.g., MAPbI3)* |
| **composition** | *(stoichiometry)* |
| **substrate** | *(e.g., SiO2/Si 300nm)* |
| **deposition_method** | *(e.g., spin-coat)* |
| **film_thickness_nm** | *(value or N/M)* |
| **waveguide_width_um** | *(value or range)* |
| **waveguide_length_um** | *(value)* |
| **patterning_method** | *(method)* |
| **encapsulation** | *(type or none)* |
| **rabi_splitting_mev** | *(value or N/M)* |
| **lpb_min_wavelength_nm** | *(value or N/M)* |
| **lasing_threshold_uW** | *(value or N/M)* |
| **pl_uniformity_variance_pct** | *(value or N/M)* |
| **polariton_lifetime_ps** | *(value or N/M)* |
| **disorder_metric** | *(low/medium/high)* |
| **measurement_temperature_K** | *(value)* |
| **h1_eligible** | *(true/false)* |
| **assigned_experiments** | `[]` |
| **notes** | *(any observations)* |
| **storage_location** | *(location)* |
| **disposed_date** | `active` |

**Measurement summary:**
- *(Free-form notes on characterization method, key results, caveats)*
```

---

## Batch History

*(Entries added chronologically below. Start with the template above.)*
