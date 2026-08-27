# Stage 4: Aspen HYSYS Model Development Log

> **Project:** CO₂ Capture from NGCC Flue Gas Using MEA Absorption  
> **Status:** In Progress (Stage 4)  

---

## 📅 Build Log: Initial Absorber Implementation

### **Work Completed**
* **Fluid Package Setup:** Configured property package (`Acid Gas - Chemical Solvents`) for the MEA-CO₂-H₂O system.
* **Stream Specification:** Defined the simulated NGCC flue gas feed stream and lean MEA solvent feed stream.
* **Column Setup:** Implemented initial absorber column (`T-100`) operating under steady-state conditions.
* **Convergence:** Successfully converged initial absorber column simulation (Solver state: Active/Green).

---

## 📊 Initial Baseline Performance
* **Observed CO₂ Capture Efficiency:** ~39%
* **Target Reference Baseline:** ~90%

---

## 🔍 Initial Observations & Next Troubleshooting Steps
The initial baseline flowsheet converged cleanly, but yields an initial capture efficiency of ~39%, falling short of the ~90% reference target. 

### **Potential Factors Under Evaluation:**
1. **Liquid-to-Gas (L/G) Ratio:** Solvent flow rate relative to flue gas throughput may be insufficient for targeted mass transfer.
2. **Column Stage Count:** Stage configuration may require optimization to reach targeted performance.
3. **Lean Amine Temperature & Loading:** Operating conditions entering `T-100` require sensitivity verification against baseline design specs.

---

## 📌 Next Action Items
* Investigate solvent circulation rate adjustments to evaluate effect on baseline capture efficiency.
* Fine-tune stage configuration and column operating conditions.
* Complete stripper loop integration once absorber baseline parameters are finalized.
