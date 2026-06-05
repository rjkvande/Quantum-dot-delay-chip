# Polariton-Mediated Delay and Routing in Perovskite Waveguides
## A Proposal for Experimental Collaboration

**Repository:** quantum-dot-delay-chip  
**Date:** June 2026  
**Contact:** *(fill in)*  
**Version:** 1.0 — for circulation to potential collaborators and funders  

---

## The Core Problem

Photonic quantum computing and quantum networking require components that can hold, route, and
re-time single photons with sub-picosecond precision — the photonic equivalent of a programmable
delay line. Classical optical delay solutions (fibre loops, ring resonators) are passive: they
store light but cannot reshape or redirect a wavepacket in response to an external control
signal without significant loss or added noise.

**Polaritons** — hybrid light-matter quasiparticles formed when photons strongly couple to
excitons in a semiconductor cavity — offer a different route. They propagate like photons but
interact like matter, making them sensitive to external optical control fields. If polariton flow
in a waveguide can be deterministically reshaped by a pump timing or phase signal, it becomes
possible to build a reconfigurable single-photon delay element that is *programmable at the
picosecond scale* without fibre or moving parts.

This project tests whether that is physically achievable using perovskite thin-film waveguides
— a material class whose room-temperature-accessible, tunable Rabi splittings (50–150 meV) make
it among the most promising platforms for integrated polaritonics.

---

## Research Architecture: Three Sequential Hypotheses

The project is structured around three falsifiable hypotheses with an explicit dependency chain.
Each must be established before the next is attempted.

### H1 — Controllable polariton flow in a single waveguide
*Can pump-timing or phase modulation deterministically reshape polariton spatial density or phase
in a single perovskite waveguide?*

This is the foundational question. H1 is tested by two independent sub-experiments:
- **Sub-Experiment A (Timing):** Two pump pulses separated by a variable delay Δτ inject
  interfering polariton wavepackets. The output centroid and momentum distribution are measured
  as a function of Δτ. A sinusoidally modulated centroid shift with the polariton oscillation
  frequency confirms that timing controls the output field.
- **Sub-Experiment B (Phase):** A spatial phase ramp applied across the pump spot imparts a
  transverse momentum kick to the polariton population. The output centroid should shift linearly
  with the ramp gradient, with a sign reversal on negative ramps.

Both sub-experiments include fully specified controls, pre-registered falsification criteria,
and decision rules for ambiguous outcomes, documented in `experimental-design.md`.

### H2 — Quantum-dot photon injection and polariton hybridisation
*Can a quantum dot evanescently coupled to the waveguide inject single photons that hybridise
with polaritons, with the resulting quasiparticle subject to the routing controls validated in H1?*

H2 is only pursued if H1 is supported. The timing and phase sensitivities measured in H1 directly
set the QD pulse-timing jitter specification and the spatial positioning tolerance for the QD.

### H3 — Multi-waveguide delay-and-route network
*Can a network of ≥ 2 coupled waveguides, each characterised by H1 and H2 protocols, implement
a delay-and-route function for sequential QD photon pulses with timing precision ≤ 1 ps?*

H3 is the architectural goal: a photonic delay element that is reconfigurable, loss-efficient,
and compatible with single-photon emitters. It is pursued only after H2 is established.

---

## Current Status

The theoretical framework, experimental design, and analysis infrastructure are fully
documented and version-controlled in the public repository:

| Document | Contents | Status |
|----------|----------|--------|
| `docs/physics-assumptions.md` | Full H1/H2/H3 hypothesis stack; demonstrated vs. assumed vs. falsifiable stratification | ✅ Complete |
| `experimental-design.md` | H1 protocol: geometry, variables, two sub-experiments, controls, falsification criteria, decision trees, H2/H3 implication table | ✅ Complete |
| `materials-log.md` | Sample tracking schema; batch index | ✅ Template ready |
| `analysis/h1/README.md` | Full analysis pipeline: ingest → HDF5 → centroid/phase/statistics → figures | ✅ Specification complete |

No data has been collected yet. The immediate need is experimental access.

---

## What Is Needed

### Materials and Fabrication
- High-quality perovskite thin films (CsPbBr₃ preferred for stability) on SiO₂/Si,
  with Rabi splitting Ω_R ≥ 50 meV and PL uniformity variance < 5% along the waveguide axis.
- Ridge waveguides (w = 3–5 µm, L = 50–100 µm) patterned by EBL + ion milling or FIB,
  with PMMA or SiN encapsulation to resist photobleaching.

### Optical Infrastructure
- Confocal cryogenic µ-PL setup with real-space and Fourier-plane (k-space) imaging.
- Pulsed Ti:Sapphire laser (80 MHz rep. rate) with a 4f pulse shaper (SLM or EOM) capable
  of: inter-pulse delay Δτ 0–20 ps; spatial phase ramp φ_spatial 0–π rad/µm.
- Stabilised Michelson interferometer arm (phase drift < λ/20 per acquisition).
- Streak camera with ≤ 0.5 ps timing resolution.
- Operating temperature: 4–77 K (closed-cycle or liquid-He cryostat).

### Timeline (estimated, contingent on material quality)
| Phase | Duration | Milestone |
|-------|----------|----------|
| Material pre-screening | 4 weeks | ≥ 2 H1-eligible waveguide batches identified |
| H1 Sub-Exp A (timing) | 6 weeks | Go/no-go decision on timing control |
| H1 Sub-Exp B (phase) | 6 weeks | Go/no-go decision on phase control |
| Analysis and write-up | 4 weeks | H1 result published / deposited |
| H2 design (if H1 supported) | 4 weeks | QD coupling geometry and specs defined |
| **Total to H1 conclusion** | **~5 months** | |

---

## Why This Matters

A successful H1 result would be the first demonstration that polariton flow in a
perovskite waveguide is *extrinsically controllable* at the picosecond scale — a prerequisite
for any polariton-based routing or delay device. Combined with the single-photon emitter
integration planned in H2, it would constitute a new class of quantum photonic component:
a programmable polariton delay element driven by a single optical control field.

The architectural endpoint (H3) targets a capability with no direct classical equivalent:
sub-picosecond-precision photon routing driven by coherent polariton switching, with a
footprint of tens of microns. This is relevant to quantum network synchronisation, boson
sampling architectures, and time-bin qubit manipulation.

---

## Collaboration and Resource Ask

This project is seeking:

1. **Fabrication partnership** — A group with established perovskite thin-film growth and
   waveguide patterning capability who can supply characterised samples meeting the H1
   material spec.

2. **Optical lab access** — Beam time on a cryogenic µ-PL setup with pulse-shaping and
   interferometric capability (see §What Is Needed above). We bring the experimental
   protocol, analysis pipeline, and personnel.

3. **QD source access (H2, deferred)** — A group with site-selectively grown or deterministically
   placed QDs compatible with perovskite waveguide evanescent coupling geometry.

All raw data, analysis scripts, and results will be shared openly via the GitHub repository
under MIT licence. Co-authorship on publications is expected for material fabrication and
instrument access contributions, proportional to contribution.

---

## Risk and Contingency Summary

The three highest-risk scenarios and their planned responses:

| Risk | Probability | Response |
|------|------------|----------|
| Perovskite disorder prevents deterministic control (H1 null) | Medium | Switch to CsPbBr₃ monocrystalline platelets or GaN-based polariton guides; H1 protocol is material-agnostic |
| Polariton lifetime < 2.5 ps (too short for Δτ sweep) | Medium | Shorten Δτ step to 0.5 ps; measure τ_pol by TRPL first and adjust before committing |
| Photobleaching degrades sample mid-run | High | Mandatory PMMA or SiN encapsulation; dose-limited acquisition; sample drift protocol |

Full risk register and failure-mode analysis: `experimental-design.md §8`.

---

## Contact

*(PI name, institution, email — fill in before circulating)*  
Repository: `https://github.com/rjkvande/Quantum-dot-delay-chip`
