# Physics Assumptions: Exciton-Polariton Temporal Computing

This document stratifies the claims underlying the temporal computing architecture into three layers: **demonstrated behaviors**, **primary speculative mechanism**, **secondary speculative mechanism**, and **architectural synthesis**.

Each layer is testable independently, though the full timing-engine concept requires partial success of the lower layers.

---

## **Section 1: Experimentally Demonstrated Behaviors**

These are facts in the current literature (as of 2024–2025).

### 1.1 Room-Temperature Exciton-Polaritons in Perovskite Systems
- **What:** Exciton-polariton waveguides in perovskite crystals/films sustain coherent polariton states at 25–35 °C without cryogenic cooling.
- **Evidence:** Recent room-temperature polariton neural networks in perovskite microwires and waveguides (arXiv:2412.10865, Nature 2024).
- **Relevance:** Substrate viability confirmed; no fundamental barrier to room-temp operation.

### 1.2 Multi-Channel Propagation
- **What:** Polaritons propagate over distances of 1–100 μm in single or multiple waveguide modes/channels.
- **Evidence:** Long-distance exciton-polariton transfer in TMDC+microcavity systems; multi-mode dynamics in perovskite waveguides.
- **Relevance:** Spatial multiplexing (multiple channels) is physically feasible.

### 1.3 Pump-Controlled Dynamics and Interference
- **What:** Time-dependent pump fields (intensity, phase, timing) reshape polariton populations, effective potentials, and interference patterns.
- **Evidence:** Standard technique in polariton physics; used extensively in coherent control and pump-probe experiments.
- **Relevance:** Pump modulation is a real control knob; we can reliably inject energy and phase into the system.

### 1.4 Localized Pump Regions as Sources/Sinks
- **What:** Spatially confined pump spots create localized polariton populations and can establish effective potential barriers and wells.
- **Evidence:** Standard in cavity polariton experiments; used to create artificial potential landscapes.
- **Relevance:** Pump geometry can be engineered to define "regeneration zones" or control regions.

---

## **Section 2: Primary Speculative Mechanism — Jitter as Control Signal**

### **Hypothesis H1: Structured Pump-Timing/Phase Modulation as a Reliable Control Axis**

**Core Question:**  
Can structured fluctuations in pump timing, phase, or intensity (collectively: "jitter") reliably reshape polariton flow, phase patterns, and density distributions in a controlled, repeatable way?

---

### **2.1 What is Known**

- **Time-dependent pumping works:** Modulated pump fields are standard in nonlinear optics and polariton physics.
- **Polaritons respond to phase:** Phase-sensitive processes (interference, locking) are well-established in polariton systems.
- **Noise can affect dynamics:** Fluctuations in pumping alter polariton statistics and transport.

---

### **2.2 What is Assumed (H1)**

1. **Controllability:** Pump-timing and phase modulation can be applied with sufficient precision and repeatability to act as a *steering field*, not just a source of decoherence.

2. **Predictability:** The response of polariton flow/phase to structured jitter follows a sufficiently deterministic relationship that we can model it (e.g., via drift-diffusion, rate equations, or Gross-Pitaevskii formalism) and predict outcomes.

3. **Signal-to-Noise:** Intentional jitter (the control signal) can be engineered to dominate over stochastic background noise, so the effect is measurable and exploitable.

4. **Locality:** Jitter applied at one location/time primarily affects polaritons in that region; effects are not randomized globally.

---

### **2.3 What Would Falsify or Weaken H1**

- **Strong falsification:** Controlled jitter produces no detectable or repeatable effect on polariton trajectories or phase patterns above noise level.
- **Weak falsification:** Jitter *does* produce effects, but they are:
  - Highly nonlinear or chaotic (unpredictable mapping between input and output)
  - Dominated by random scattering (effect size too small or too noisy to control)
  - Irreproducible (significant shot-to-shot variation independent of input)
- **Partial success:** Jitter reshapes flow in specific regimes (e.g., low density) but fails at high density or in certain geometries. (This would refine H1, not kill it.)

---

### **2.4 Experimental Signature of H1 (Minimal Test)**

- **Setup:** Single-channel perovskite polariton waveguide, no regeneration nodes.
- **Measurement:** Real-space or spectral imaging of polariton distribution under modulated pump (e.g., sinusoidal jitter in timing or phase).
- **Expected outcome:** Observable correlation between jitter parameters and polariton position/phase/density pattern.
- **Falsification threshold:** No significant correlation, or correlation too small/noisy to exploit.

---

## **Section 3: Secondary Speculative Mechanism — Regeneration Nodes**

### **Hypothesis H2: Localized Pump Regions Reduce Phase/Density Drift**

**Core Question:**  
Can localized pump spots (regeneration nodes) act as repeatable phase/density reset or correction points, such that polaritons downstream show reduced accumulated temporal drift compared to unpumped propagation?

---

### **3.1 What is Known**

- **Localized pumping works:** Pump spots create source regions; well-established in cavity polariton physics.
- **Phase can be reset:** Coherent pumping can drive or re-phase systems; used in lasers, masers, and coherent control.
- **Density regeneration is possible:** Pumping replenishes populations.

---

### **3.2 What is Assumed (H2)**

1. **Phase Stability:** A regeneration node (localized pump) can imprint a stable, repeatable phase on downstream polaritons, overwriting prior phase drift.

2. **Coherence Preservation:** The reset does not destroy the quantum or classical coherence properties of the polariton field; the system maintains phase information through the regeneration event.

3. **Repeatability:** Multiple regeneration cycles (pump → propagate → pump → propagate) yield consistent phase/density corrections, not degradation with each cycle.

4. **Drift Correction:** Polaritons exiting a regeneration node show measurably less accumulated phase/temporal drift downstream than polaritons that bypass it.

---

### **3.3 What Would Falsify or Weaken H2**

- **Strong falsification:** Regeneration nodes produce no detectable phase or density correction; downstream polaritons accumulate drift at the same rate as unpumped paths.
- **Weak falsification:** Nodes *do* produce some reset, but:
  - It decays rapidly (phase re-drifts on ps timescales, making it unreliable)
  - It is lossy (each cycle introduces significant dephasing or population loss)
  - It is irreproducible (shot-to-shot variation is large)
- **Partial success:** Nodes work at low density but break down at high density, or work in specific geometries but not others.

---

### **3.4 Experimental Signature of H2 (Conditional on H1)**

- **Setup:** Two-channel or single-channel geometry. Upstream pump (regeneration node) + downstream channel. Compare phase/density drift with and without regeneration.
- **Measurement:** Time-resolved phase interferometry or streak-camera density imaging.
- **Expected outcome:** Observable reduction in phase smear or density spread downstream of regeneration node vs. unpumped control.
- **Falsification threshold:** No measurable difference, or differences explainable by simple re-excitation without phase correction.

---

## **Section 4: Architectural Synthesis — Temporal Computing Medium**

### **Hypothesis H3: Multi-Channel Geometry with H1+H2 Yields Emergent Self-Stabilizing Temporal Behavior**

**Core Question:**  
When jitter-as-control (H1) and regeneration-as-reset (H2) are arranged in a multi-channel, multi-node platter geometry, does the system exhibit emergent global coherence, temporal alignment, or self-correcting behavior that constitutes "temporal computing" (i.e., the system maintains or optimizes a timing/phase reference without explicit external feedback)?

---

### **4.1 What is Known**

- **Self-organization in polariton systems:** Driven-dissipative nonlinear systems can exhibit pattern formation, synchronization, and phase-locking under certain conditions (literature: Carusotto & Ciuti, etc.).
- **Multi-channel coherence is possible:** Photonic and polariton systems routinely maintain phase relationships across multiple modes or waveguides.

---

### **4.2 What is Assumed (H3)**

1. **Coupling via Jitter:** The jitter modulation (H1) can couple information between channels, allowing one channel's state to influence another through controlled temporal fluctuations.

2. **Global Phase Reference:** When H1 and H2 operate in concert across multiple channels with regeneration nodes, the system converges toward a stable global phase reference (a "timing manifold") rather than drifting chaotically.

3. **Self-Correction:** Small perturbations to timing or phase are automatically corrected by the system's internal dynamics, without requiring external intervention, through the interplay of jitter steering and regeneration resets.

4. **Scalability:** The self-correcting behavior persists as the number of channels increases (at least up to 5–10 channels; the limit is not yet known).

---

### **4.3 What Would Falsify or Weaken H3**

- **Strong falsification:** When multiple channels with regeneration are connected, the system does not show enhanced coherence or reduced drift compared to isolated single-channel tests. Instead, phase becomes increasingly chaotic or incoherent.
- **Weak falsification:** The system *does* show some multi-channel behavior, but:
  - It only occurs in a very narrow parameter range (fragile, not robust)
  - It breaks down at high exciton density or under realistic noise
  - It requires fine-tuning across channels (not scalable)
- **Partial success:** Self-stabilization works for 2–3 channels but fails for 5; indicates the mechanism works but needs refinement for full architecture.

---

### **4.4 Experimental Signature of H3 (Conditional on H1+H2)**

- **Setup:** Full 5-channel (or N-channel) perovskite polariton platter with distributed regeneration nodes, jitter modulation applied.
- **Measurement:** Global phase coherence (e.g., via multi-point interferometry or spectral analysis) and temporal drift statistics across all channels.
- **Expected outcome:** Coherence and phase alignment persist over many regeneration cycles; global phase reference is maintained or self-corrects.
- **Falsification threshold:** No measurable global coherence; channels evolve independently or become chaotically dephased; no evidence of self-correction.

---

## **Section 5: Dependency and Contingency**

| Hypothesis | Depends On | Status |
|-----------|-----------|--------|
| H1 | Demonstrated section 1 | Primary, independently testable |
| H2 | H1 (but can be tested locally) | Secondary, builds on H1 |
| H3 | H1 + H2 (both needed for full effect) | Architectural, emergent property |

**Progression:**
- If H1 fails: Stop. Jitter-as-control doesn't work; architecture is null.
- If H1 succeeds, H2 fails: Jitter modulation is real, but regeneration mechanism needs rethinking. Architecture persists but must be revised.
- If H1 + H2 succeed but H3 fails: Two mechanisms work individually, but they don't self-organize into a timing engine. Architecture needs new layer or different geometry.
- If all three succeed: Core architecture is validated; proceed to scaling and applications.

---

## **Section 6: Undefined Boundaries (Honest Unknowns)**

The following remain unsettled and are candidates for explicit investigation:

1. **Jitter magnitude:** What range of modulation depth (timing, phase, intensity jitter) is optimal? Too much noise destroys control; too little may not overcome background fluctuations.

2. **Regeneration spacing:** What density of regeneration nodes is required? Empirical question.

3. **Timescale matching:** Must the timescale of jitter modulation match the polariton lifetime? The decay time of phase correlations? Not yet clear.

4. **Density regime:** Do H1, H2, H3 hold only at low exciton density (single/few excitons per channel) or can they extend to high-occupancy regimes? This has major implications for "computing" throughput.

5. **Temperature sensitivity:** Room temp is targeted, but what is the temperature sensitivity of each mechanism? Does the architecture require ±0.5 °C control, or is broader tolerance okay?

---

## **Section 7: Relationship to Published Work**

- **Directly relevant:** Polariton coherence control, nonlinear polariton dynamics, driven-dissipative systems literature (e.g., Carusotto & Ciuti, Kasprzak et al., Amo et al.).
- **Tangentially relevant:** Quantum metrology (phase sensing), photonic feedback control, optical lattice engineering (different platforms, similar concepts).
- **Not yet in literature:** A polariton system explicitly framed as a "jitter-actuated, self-correcting temporal computing medium" does not appear in current publications. This architecture is a novel synthesis.

---

## **Revision History**

- **2026-06-05:** Initial draft. H1/H2/H3 structure established. Status: ready for experimental design phase.
