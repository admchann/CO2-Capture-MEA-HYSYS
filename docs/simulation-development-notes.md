# Stage 4: Aspen HYSYS Model Development Log

> **Project:** CO₂ Capture from NGCC Flue Gas Using MEA Absorption  
> **Status:** Completed (Stage 4 — Absorber Diagnostics & Base Case Finalization)  

---

## 1. Context

After building the flue gas and lean amine feed streams per the design basis and placing a 10-stage equilibrium absorber (`Acid Gas – Chemical Solvents` property package, Efficiency Modeling / equilibrium-stage mode), the column converged on the first attempt. However, the converged result showed CO₂ capture efficiency far below the 90% design target. This document records the diagnostic process used to investigate and partially resolve the shortfall.

> **Note on Data Origin:** All values in this log are simulation outputs from this project's own HYSYS model (derived/simulated data) and are not published literature values.

---

## 2. Initial Result (Base Case)

* **Absorber Configuration:** 10 equilibrium stages
* **Lean Amine Feed:** 1,319 kgmol/h (base-case circulation rate calculated in the design basis, assuming a rich loading target of 0.50 mol CO₂/mol MEA)
* **Result:** Sweet Gas CO₂ mole fraction 0.0264 (26.459 kgmol/h), corresponding to **33.9% capture efficiency** — far short of the 90% target.
* **Mass Balance Check:** CO₂ removed from gas phase (13.54 kgmol/h) closely matched CO₂ gained by the liquid phase (13.15 kgmol/h), confirming the model itself was solving correctly — the shortfall was a genuine process result, not a convergence or balance error.
* **Rich Loading Achieved:** 0.341 mol CO₂/mol MEA, well below the 0.50 assumed during the original stoichiometric sizing calculation in the design basis.

---

## 3. Diagnosis: Minimum Liquid Rate / Finite-Stage Pinch

The original circulation rate calculation (Stage 5, design basis) implicitly assumed the column could drive absorption to a rich loading of 0.50 mol CO₂/mol MEA — an assumption that is only valid in the limit of a very large (effectively infinite) number of equilibrium stages, analogous to minimum reflux in a distillation column. 

With a finite 10-stage column, the liquid cannot physically be driven to that theoretical endpoint before exiting the bottom stage. This is a genuine finite-stage pinch, not a modeling error.

---

## 4. Liquid Rate Sensitivity (Diagnostic Test)

To test whether the shortfall was liquid-rate-limited, the Lean Amine Feed flow rate was increased at a fixed stage count (10 stages):

| Lean Amine Flow | Multiplier | Capture Efficiency | Rich Loading (mol/mol) |
| :--- | :--- | :--- | :--- |
| **1,319 kgmol/h** | 1.0× | 33.9% | 0.341 |
| **1,979 kgmol/h** | 1.5× | 39.1% | — |
| **2,638 kgmol/h** | 2.0× | 42.8% | 0.310 |

**Observation:** Capture efficiency gains diminished as liquid rate increased (a proportionally smaller improvement from 1.5× to 2.0× than from 1.0× to 1.5×), and rich loading achieved actually decreased at higher liquid rates (more solvent dilutes the CO₂ picked up per mole of MEA). This diminishing-returns pattern indicated the column was not purely liquid-rate-limited — stage count was also a binding constraint.

---

## 5. Stage Count Sensitivity (Diagnostic Test)

Liquid rate was reset to 1,979 kgmol/h (1.5× base case) and the absorber stage count was increased incrementally:

| Absorber Stages | Lean Amine Flow | Capture Efficiency | Δ from Previous | Rich Loading (mol/mol) |
| :--- | :--- | :--- | :--- | :--- |
| **10** | 1,979 kgmol/h | 39.1% | — | — |
| **15** | 1,979 kgmol/h | 53.9% | +14.7 pp | 0.348 |
| **20** | 1,979 kgmol/h | 64.3% | +10.4 pp | 0.367 |
| **25** | 1,979 kgmol/h | 72.0% | +7.7 pp | — |
| **30** | 1,979 kgmol/h | 78.0% | +6.0 pp | — |

**Observation:** Increasing stage count at a fixed liquid rate produced substantially larger capture efficiency gains than increasing liquid rate alone, confirming that stage count was the dominant limitation at the original 10-stage design. However, the gain per additional 5 stages is itself shrinking (14.7 → 10.4 → 7.7 → 6.0 percentage points), indicating the column is asymptotically approaching a ceiling — consistent with approaching the theoretical equilibrium limit for this liquid rate, rather than being able to reach 90% through stage count increases alone within a reasonable, realistic column size.

---

## 6. Engineering Interpretation

This behavior is a direct demonstration of a limitation identified in this project's literature review (`literature-review.md`, Section 5): **equilibrium-stage absorption models are documented in the literature to require disproportionately more stages — or to underperform relative to rate-based models — specifically for dilute, low-CO₂-partial-pressure feeds such as NGCC flue gas.** 

Rather than being a flaw in this project's model, this stage-count sensitivity is treated as an expected and reportable consequence of the modeling choice made deliberately in Stage 2 (equilibrium-stage over rate-based, for scope reasons).

---

## 7. Initial Status & Decision Point

As of the 30-stage test (78% capture), two options were considered:
1. Continue increasing stage count further to approach 90%, accepting a larger column design.
2. Accept a lower base-case capture efficiency (70–80% range) and document the gap explicitly.

**Decision Executed:** Proceeded with combined sensitivity testing to achieve the targeted ~90% capture while analyzing system trade-offs.

---

## 8. Follow-Up: Combined Liquid Rate and Stage Count Investigation

Following the 30-stage / 1.5×-liquid-rate result (78% capture), the investigation continued by testing whether liquid rate and stage count compound favorably when increased together, rather than testing either variable in isolation at a fixed value of the other.

### **8.1 Combined Effect at Fixed 30 Stages**

Liquid rate was increased from 1,979 kgmol/h (1.5×) to 2,638 kgmol/h (2.0×) while holding stage count fixed at 30:

| Stages | Liquid Rate | Capture Efficiency | Rich Loading (mol/mol) |
| :--- | :--- | :--- | :--- |
| **30** | 1.5× (1,979 kgmol/h) | 78.0% | — |
| **30** | 2.0× (2,638 kgmol/h) | 83.1% | 0.363 |

This produced a +5.1 percentage point gain, notably larger than the same liquid rate increase produced back at 10 stages (Section 4), where it was largely wasted on dilution rather than additional absorption. This confirmed that with more stage capacity available, the same liquid rate increase becomes meaningfully more effective — the two variables compound rather than acting independently.

### **8.2 Ancillary Observation: Column Operating Pressure**

A brief exploratory test was also run, varying the absorber's bottom-stage pressure drop (130 to 145 kPa) at fixed 30 stages / 2,638 kgmol/h. Capture efficiency rose modestly and monotonically (83.7% to 85.0%) as pressure increased, consistent with Henry's law (higher pressure increases CO₂ equilibrium solubility). This is a physically valid result, but pressure was not one of the three independent variables locked into this project's research question (`research-question.md`), so it was not pursued further and is not part of the base case. It is noted here as an observed, unexplored lever for potential future work.

### **8.3 Further Stage Count Increases at Fixed 2.0× Liquid Rate**

| Stages | Liquid Rate | Capture Efficiency | Δ from Previous |
| :--- | :--- | :--- | :--- |
| **30** | 2,638 kgmol/h (2.0×) | 83.07% | — |
| **35** | 2,638 kgmol/h (2.0×) | 86.76% | +3.69 pp |
| **40** | 2,638 kgmol/h (2.0×) | **89.40%** | +2.64 pp |

---

## 9. Final Base Case (Closing This Investigation)

The trial was closed at **40 absorber stages** and **2,638 kgmol/h lean amine circulation rate** (2.0× the original stoichiometric base-case flow), achieving **89.4% CO₂ capture efficiency** — effectively at the 90% design target.

This is adopted as the base case for the remainder of the project (Stage 6 validation and Stage 7 sensitivity analysis), in place of the originally literature-sourced 10-stage / 1,319 kgmol/h configuration from the design basis.

---

## 10. Engineering Discussion: Why This Result Matters

Reaching ~90% capture required approximately 4× the stage count (40 vs. the literature-sourced base case of 10) and 2.0× the liquid circulation rate originally calculated from stoichiometry. This is interpreted as a direct, self-generated demonstration of the equilibrium-stage model limitation identified in the literature review (`literature-review.md`, Section 5): **equilibrium-stage models compensate for the absence of explicit mass-transfer and reaction kinetics by requiring substantially more theoretical stages to match performance that a rate-based model, or a real column, would achieve with fewer stages.**

### **CAPEX vs. OPEX Trade-Off (Identified, Not Resolved)**

Both effective levers used to raise capture efficiency carry different cost implications that this project's scope does not permit quantifying:

* **Stage count is primarily a capital cost (CAPEX) driver:** A taller column is a larger one-time fabrication cost.
* **Liquid circulation rate is primarily an operating cost (OPEX) driver:** More solvent circulated means more solvent that must be regenerated, directly increasing reboiler duty (this project's second response variable, per `research-question.md`).

This project does not attempt to determine which trade-off is more economical, as doing so requires cost data (e.g., column fabrication cost per stage, steam/utility cost per unit reboiler duty) that is outside this project's scope. This trade-off is identified here as a specific, concrete motivation for a future techno-economic analysis project, rather than resolved within this Level 1 project.

---

*This log reflects the completed absorber diagnostic and sizing process. Values above are simulation outputs from this project's own HYSYS model (Acid Gas – Chemical Solvents property package, equilibrium-stage mode) and are not independently validated published data. The final base case (Section 9) supersedes the stage count and lean amine flow rate originally specified in `design-basis.md`; this deviation and its cause are treated as a primary engineering finding of this project rather than a correction to be silently applied.*
