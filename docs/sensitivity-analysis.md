# Stage 6: Parametric Sensitivity Analysis

> **Project:** $\text{CO}_2$ Capture from NGCC Flue Gas Using MEA Absorption  
> **Simulation Engine:** Aspen HYSYS V14  
> **Property Package:** Acid Gas - Chemical Solvents (Electrolyte NRTL)  
> **Documentation Target:** `docs/stage-6-sensitivity-analysis.md`  
> **Current Status:** 🟢 Study 1 Complete ($L/G$ Ratio Parametric Sweep: $2,000\text{ to }3,400\text{ kgmol/h}$)

---

## 📌 1. Executive Summary & Scope

Following the successful steady-state closure and multi-shell preheater optimization of the closed-loop model in **Stage 5** ($\text{SRD} = 6.53\text{ MJ/kg }\text{CO}_2$, $\eta_{\text{capture}} = 90.1\%$), **Stage 6** focuses on evaluating the operational flexibility, parametric response, and thermodynamic sensitivity of the capture plant.

This document details the execution of **Study 1: Solvent Circulation Rate ($L/G$ Ratio Sweep)**. Using the automated Aspen HYSYS Case Study tool, the solvent circulation rate was systematically varied across eight discrete operating states from $2,000\text{ kgmol/h}$ to $3,400\text{ kgmol/h}$ ($L/G$ ratios of $2.00\text{ to }3.40\text{ mol/mol}$) to map the fundamental process trade-offs between $\text{CO}_2$ removal efficiency, reboiler energy consumption, and Specific Reboiler Duty ($\text{SRD}$).

---

## 🛠️ 2. Critical Troubleshooting & Degrees-of-Freedom Decoupling

During initial sensitivity runs with an active closed loop, automated Case Study sweeps triggered localized boundary consistency errors (`Comp Mole Frac - MEAmine Inconsistent in Rich Amine @COL1`).

### 2.1 Root-Cause Diagnosis
* **The Conflict:** In an open-loop model, setting an initial composition estimate on the absorber bottoms stream (`Rich Amine`) helps kick-start convergence. However, once integrated into a feedback loop with `E-100`, `T-101`, and `RCY-1`, manual specifications on `Rich Amine` over-constrain the column sub-flowsheet.
* **Numerical Impact:** When the Case Study engine perturbed the inlet flow rate, the column solver calculated the natural physical stage equilibrium composition ($x_{\text{MEA}} = 0.1088$), which conflicted with the retained boundary specification ($x_{\text{MEA}} = 0.1089$), halting the solver.

### 2.2 Engineering Resolution
1. **Decoupled Boundary Stream:** All user-specified mole fractions on the `Rich Amine` stream were deleted, returning full mathematical degrees of freedom to the absorber stage calculation.
2. **Column Reset:** Absorber `T-100` was reset and re-solved from the feed conditions, allowing the column solver to dynamically back-propagate composition profiles without solver halts.
3. **Outcome:** 100% convergence across all eight parametric Case Study points without solver interruption or oscillation.

---

## ⚙️ 3. Experimental Setup & Variable Mapping

The sensitivity study was configured in the Aspen HYSYS V14 **Case Study Manager** using a discrete automated sweep:

```text
+------------------------------------+          +--------------------------------------------+
|   INDEPENDENT (MANIPULATED) VAR    |          |        DEPENDENT (RESPONSE) METRICS        |
+------------------------------------+          +--------------------------------------------+
| • Lean Amine Feed - Molar Flow     |  =====>  | 1. Q-Reboiler - Heat Flow (kJ/h)           |
|   (2,000 to 3,400 kgmol/h)         |          | 2. Stripper Ovhd Vap - Mass Flow CO2 (kg/h)|
|   Step Size: 200 kgmol/h           |          | 3. Sweet Gas - Molar Flow CO2 (kgmol/h)    |
|   Baseline: 2,638 kgmol/h          |          | 4. Lean Amine from Stripper - Temp (°C)    |
+------------------------------------+          +--------------------------------------------+
