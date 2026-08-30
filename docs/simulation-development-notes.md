# Stage 4: Aspen HYSYS Model Development Log

> **Project:** CO₂ Capture from NGCC Flue Gas Using MEA Absorption  
> **Status:** In Progress (Stage 4 — Absorber Diagnostics & Tuning)  

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
| **2,638 kgmol/h** | 2.0× | 42.8% | 0.31 |

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

## 7. Status & Open Decision

As of the 30-stage test (78% capture), a decision has not yet been finalized on how to proceed. Two options remain under consideration:

1. **Continue increasing stage count further** to approach 90%, accepting a progressively larger and less realistic column design.
2. **Accept a lower base-case capture efficiency** (in the 70–80% range) as the honest result of this simplified equilibrium-stage model, and document the gap to the 90% industrial benchmark explicitly as a central engineering finding of this project, rather than continuing to add stages purely to hit a target number.

This decision will be made in the next work session and recorded here as a follow-up entry once finalized.
