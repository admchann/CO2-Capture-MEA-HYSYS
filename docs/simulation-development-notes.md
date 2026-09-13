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

---

## 11. Stripper Column (T-101) & Preheater (E-100) Convergence

Following the successful baseline determination for the 40-stage absorber (T-100), the solvent regeneration and preheating systems were integrated into the flowsheet.

### 11.1 Booster Pump (P-101) & Decoupled Preheating
To feed the stripper safely, the rich solvent exiting T-100 had to be pressurized and heated:
* **Booster Pump (P-101):** Elevated the rich solvent pressure from 130 kPa to 250 kPa. This physical pressure increase is critical to suppress vaporization and line-flashing of dissolved CO₂ inside the preheater lines.
* **Lean/Rich Cross-Exchanger (E-100):** Replaced the temporary heater bypass. E-100 was modeled using the `Simple End Point` framework, with a 10 kPa pressure drop specified across both the tube and shell sides. 
* **Solver Specification:** The tube-side outlet temperature was set to **95.0°C**, serving as a stable, preheated liquid feed to the top stage of the stripper.

### 11.2 Stripper Configuration (T-101) & Thermal Sanity Check
The stripper was modeled using a **Reboiled Absorber** column with 8 equilibrium stages:
* **Operating Pressures:** Top Stage = 150 kPa; Reboiler = 190 kPa.
* **Active Specifications (Degrees of Freedom = 2):**
  1. `Column Component Fraction`: Liquid CO₂ mole fraction in the reboiler bottoms = **0.027** (targeting our base lean loading of 0.25 mol CO₂/mol MEA).
  2. `Column Boilup Ratio`: Initialized as a starting estimate of **1.0**.
* **Convergence & Solvent Purity:** T-101 successfully converged. The reboiler bottom temperature settled at **119.2°C**, providing a safe thermodynamic margin below the **122°C (395 K) MEA thermal degradation threshold** while maintaining complete solvent regeneration.

---

## 12. Recycle Loop Closure & Numerical Convergence Challenges

With both columns solved in an open-loop state, work proceeded to close the physical solvent recycle loop.

### 12.1 Trim Cooler (E-102) & Let-down Valve (VLV-100)
To prepare the regenerated lean amine for re-entry into T-100, the temperature and pressure profiles had to be matched to the absorber feed conditions:
* **Trim Cooler (E-102):** Cooled the lean solvent from its post-exchange temperature down to exactly **40.0°C** (10 kPa pressure drop).
* **Let-down Valve (VLV-100):** Dropped the regenerated solvent pressure from ~170 kPa to **120.0 kPa** to match the absorber inlet.
* **System Solvent Mismatch:** Due to water evaporation and trace MEA vapor losses in the absorber clean gas and stripper overheads, the return stream flow rate settled at **2,529 kgmol/h** (a deficit of 109 kgmol/h from the baseline 2,638 kgmol/h feed). 

### 12.2 Integration of the Makeup & Adjust Loops
To balance the solvent inventory, a pure water `Makeup` stream (40°C, 120 kPa) and a `Mixer` were added. An **Adjust Block (ADJ-1)** was configured to manipulate the `Makeup` flow rate to target a mixed `Lean Amine to Recycle` flow of exactly **2,638 kgmol/h**. This sub-system successfully converged.

### 12.3 Mathematical Circularity & Consistency Conflicts
Upon introducing the final physical **Recycle Block (RCY-1)**, the simulation encountered a severe **numerical consistency conflict** across the absorber boundaries. 

In a closed loop, HYSYS attempts to solve the composition of the absorber feed (`Lean Amine Feed`) dynamically based on what returns from the stripper. However, because both columns utilize highly sensitive, non-linear chemical solvent thermodynamics (`Acid Gas - Chemical Solvents`), the solver encountered a mathematical contradiction between its internal column stage-by-stage calculations and the hard-coded composition limits of the recycle boundary. 

This resulted in a localized solver halt (columns reverting to yellow/unconverged status). To prevent numerical runaway, the physical loop was left open at the recycle block boundary. The current flowsheet stands as a fully verified, stage-by-stage open-loop simulation, with the exact recycle parameters calculated and ready for future loop-tuning iterations.

---

## 13. Closed-Loop Integration & Multi-Shell Preheater Optimization

### 13.1 Closed-Loop Inventory Balance
To successfully close the solvent recycle loop without numerical drift, a customized makeup sub-system was integrated:
* **The Mass Deficit:** In open-loop operations, the solvent loop experienced a continuous mass loss of approximately **109 kgmol/h** (primarily water vapor lost via T-100 clean gas and T-101 overheads).
* **Closed-Loop Convergence:** A fresh solvent `Dummy` makeup stream was mixed with the returning `Lean Amine Recycled` stream in `MIX-100`. By matching the feed composition and flow rate, the **Recycle Block (RCY-1)** was successfully converged using **Successive Substitution (Wegstein Q Max = 0.0)** to stabilize numerical iterations. The entire flowsheet is now 100% closed, steady-state, and fully converged.

### 13.2 Preheater F_t Correction Factor & Multi-Shell Solution
Initial attempts to increase heat recovery in the Lean/Rich Exchanger (E-100) by raising the rich preheat target to 105.0°C triggered a localized **"Temperature Cross"** warning and dropped the LMTD correction factor ($F_t$) to unacceptable levels. 
* **The Solution:** The flow geometry was optimized by increasing the **Shells in Series to 4** in a counter-current pass configuration. 
* **The Result:** This multi-shell arrangement physically resolved the localized temperature cross, restoring a highly efficient $F_t$ correction factor (>0.85) while successfully delivering the rich amine to T-101 at an optimized temperature of **105.0°C**.

### 13.3 Process Key Performance Indicators (KPIs) & Energy Analysis
With the closed-loop fully optimized at a 105.0°C preheat, the process performance metrics were extracted:
* **Total Reboiler Duty ($Q_{\text{reboiler}}$):** $1.146 \times 10^7 \text{ kJ/h}$
* **$\text{CO}_2$ Captured Mass Flow:** $1,754.87 \text{ kg/h}$
* **Specific Reboiler Duty (SRD):** **6.53 MJ/kg $\text{CO}_2$ captured**

* **Performance Interpretation:** This optimized configuration achieved a **6.2% energy reduction** from the 95°C open-loop baseline (6.96 MJ/kg). The gap between this result and the commercial coal-fired baseline (3.6–4.0 MJ/kg) is attributed to the low thermodynamic driving force of the dilute NGCC flue gas (4 mol% $\text{CO}_2$), which necessitates a high solvent-to-gas ratio, and the conservative nature of equilibrium-stage modeling.
