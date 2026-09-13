# $\text{CO}_2$ Capture from NGCC Flue Gas Using MEA Absorption

> **Level 1 Process Engineering Project**  
> **Simulation Engine:** Aspen HYSYS V14  
> **Property Package:** Acid Gas - Chemical Solvents (Electrolyte NRTL)  
> **Status:** 🟢 Stage 5 Complete — Closed-Loop Convergence & Preheater Optimization Achieved

---

## 📌 Project Overview

This project investigates the simulation, thermal optimization, and process analysis of post-combustion carbon dioxide capture from Natural Gas Combined Cycle (NGCC) flue gas using a **30 wt% monoethanolamine (MEA)** aqueous solvent in Aspen HYSYS. 

The goal of this study is to progressively demonstrate rigorous chemical and process engineering methodology—moving from literature benchmarks and process design basis to full closed-loop flowsheet convergence, energy integration, and engineering evaluation against commercial capture benchmarks.

---

## 🎯 Project Objectives

* **Foundational Modeling:** Develop a robust thermodynamic and chemical absorption framework for dilute $\text{CO}_2$ streams using the Acid Gas property package.
* **Process Simulation:** Construct a rigorous, multi-column simulation (absorber, stripper, cross-exchanger, pumps, coolers, and let-down valves) in Aspen HYSYS.
* **Closed-Loop Convergence:** Resolve solvent mass balance deficits and achieve stable steady-state recycle closure without numerical composition drift.
* **Thermal Optimization:** Optimize the lean/rich cross-exchanger configuration to eliminate LMTD temperature crosses and maximize sensible heat recovery.
* **Benchmark Evaluation:** Quantify key performance indicators (Specific Reboiler Duty, solvent circulation rate, capture efficiency) and contrast NGCC capture energetics against coal-fired baselines.

---

## 🛣️ 9-Stage Project Roadmap

- [x] **Stage 1:** Literature Review, Background & Technology Selection
- [x] **Stage 2:** Research Question, Objectives, Scope & Limitations
- [x] **Stage 3:** Process Design Basis
- [x] **Stage 4:** Aspen HYSYS Model Development (Open-Loop Base Case)
- [x] **Stage 5:** Model Verification, Closed-Loop Convergence & Heat Optimization
- [ ] **Stage 6:** Parametric Sensitivity Analysis (Case Study Sweeps)
- [ ] **Stage 7:** Results & Comparative Engineering Discussion
- [ ] **Stage 8:** Techno-Economic & Process Safety Considerations
- [ ] **Stage 9:** Final Project Release & Portfolio Publication

---

## ⚙️ Process Flowsheet Overview

The flowsheet models an industrial absorption-regeneration closed loop:

```text
                        +-------------------------------------------------------+
                        |                                                       | (Lean Amine Recycle)
                        v                                                       |
Flue Gas Feed ---> [ T-100 Absorber ] ---> Sweet Gas (Purified Gas Vented)      |
                        |                                                       |
                   Rich Amine                                                   |
                        v                                                       |
                   [ P-100 Pump ]                                               |
                        v                                                       |
               [ E-100 Lean/Rich HX ] (4 Shells in Series, Counter-Current)     |
                        v                                                       |
                 Rich to Stripper (105.0°C)                                     |
                        v                                                       |
                 [ T-101 Stripper ] ---> Stripper Ovhd Vap                      |
                        |                 (CO2 Product + H2O Vapor)             |
                   Lean Amine                                                   |
                        v                                                       |
               [ E-100 Tube/Shell ]                                             |
                        v                                                       |
              [ E-102 Trim Cooler ] (Cooled to 40.0°C)                          |
                        v                                                       |
             [ VLV-100 Let-down Valve ] (Depressurized to 120 kPa)              |
                        v                                                       |
                   [ RCY-1 Block ]                                              |
                        v                                                       |
          Dummy Makeup ---> [ MIX-100 ] ----------------------------------------+
```

---

## 📋 Key Design & Operating Parameters

| Unit / Parameter | Specification | Value | Engineering Notes |
| :--- | :---: | :---: | :--- |
| **Flue Gas Flow Rate** | $\text{kgmol/h}$ | 1,000 | Typical scaled NGCC flue gas stream |
| **Flue Gas $\text{CO}_2$ Content** | $\text{mol\%}$ | ~4.0% | Dilute concentration typical of combined cycle gas turbines |
| **Solvent Composition** | $\text{wt\%}$ | 30% MEA / 70% $\text{H}_2\text{O}$ | Standard industrial concentration limit to prevent corrosion |
| **Solvent Circulation Rate** | $\text{kgmol/h}$ | 2,638 | Required $L/G$ to hit $\sim 90\%$ capture with dilute feed |
| **Absorber Column (T-100)** | Stages | 40 ideal stages | Counter-current chemical absorption column |
| **Regenerator / Stripper (T-101)** | Stages | 8 equilibrium stages | Reboiled absorber with bottom reboiler at 190 kPa |
| **Lean/Rich Cross-Exchanger (E-100)** | Shells | **4 in Series** | Resolves LMTD temperature cross; rich outlet = **$105.0^\circ\text{C}$** |
| **Trim Cooler (E-102)** | Outlet Temp | $40.0^\circ\text{C}$ | Conditions lean amine for optimal absorption kinetics |
| **Let-down Valve (VLV-100)** | Outlet Press | $120.0\text{ kPa}$ | Matches absorber top operating pressure |

---

## 📊 Process Key Performance Indicators (KPIs)

Thermal performance comparison before and after preheater optimization:

| Metric | Initial Baseline (95°C Preheat) | Optimized Case (105°C Preheat, 4 Shells) | Delta / Improvement |
| :--- | :---: | :---: | :---: |
| **Stripper Reboiler Duty ($Q_{\text{reboiler}}$)** | $1.185 \times 10^7\text{ kJ/h}$ | **$1.146 \times 10^7\text{ kJ/h}$** | **$-390,000\text{ kJ/h}$ ($-3.3\%$)** |
| **$\text{CO}_2$ Mass Flow Recovered** | $1,703.6\text{ kg/h}$ | **$1,754.9\text{ kg/h}$** | **$+51.3\text{ kg/h}$ ($+3.0\%$)** |
| **Specific Reboiler Duty (SRD)** | **6.96 MJ/kg** | **6.53 MJ/kg** | **$-0.43\text{ MJ/kg}$ ($-6.2\%$)** |
| **$\text{CO}_2$ Capture Efficiency** | 89.4% | **90.1%** | Target achieved ($\ge 90\%$) |

### Specific Reboiler Duty Calculation
$$\text{SRD} = \frac{Q_{\text{reboiler}}\text{ [kJ/h]}}{\dot{m}_{\text{CO}_2}\text{ [kg/h]} \times 1,000} = \frac{11,460,000}{1,754.87 \times 1,000} \approx \mathbf{6.53\text{ MJ/kg }\text{CO}_2}$$

---

## 🔬 Engineering & Thermodynamic Insights

### 1. Dilute Feed Penalty (NGCC vs. Coal Benchmark)
* **Commercial Baseline:** Conventional coal-fired capture plants (e.g., Fluor Econamine benchmark) report Specific Reboiler Duties of **3.6–4.0 MJ/kg $\text{CO}_2$**.
* **NGCC Reality:** In this simulation, the SRD settles at **6.53 MJ/kg $\text{CO}_2$**. 
* **Thermodynamic Reason:** NGCC flue gas is dilute ($\sim 4\text{ mol\% }\text{CO}_2$) compared to coal exhaust ($\sim 12\text{–}14\text{ mol\% }\text{CO}_2$). Capturing dilute $\text{CO}_2$ demands a significantly higher Liquid-to-Gas ($L/G$) solvent ratio. The extra volume of water in the circulating loop demands large amounts of sensible heat during regeneration, inherently increasing reboiler duty per kilogram of $\text{CO}_2$ stripped.

### 2. Multi-Shell Preheater Optimization ($F_t$ Correction Factor)
* Elevating rich solvent preheat to $105^\circ\text{C}$ reduced reboiler duty and flashed an extra $51.3\text{ kg/h}$ of $\text{CO}_2$ before the column reboiler.
* In a standard 1-shell exchanger, this narrow temperature approach triggered an internal **temperature cross** and an invalid $F_t$ correction factor ($F_t < 0.75$).
* Configuring **4 Shells in Series** divided the heat exchange into 4 discrete counter-current steps, eliminating the temperature cross and restoring an efficient $F_t > 0.85$.

### 3. Closed-Loop Inventory Balance
* Evaporative losses across the absorber clean gas and stripper overhead lead to a mass deficit of $\sim 109\text{ kgmol/h}$ (mainly water vapor).
* Incorporating **`MIX-100`** with a dynamic **`Dummy`** makeup stream enables Aspen HYSYS to back-propagate the required makeup rate and composition, converging **`RCY-1`** using Successive Substitution without composition drift.

---

## 📁 Repository Structure

```text
├── CO2_Capture_MEA_ClosedLoop_Optimized.hsc   # Converged closed-loop HYSYS simulation model
├── README.md                                  # Project overview, KPIs, and engineering summary
├── LICENSE                                    # MIT License
├── docs/
│   ├── literature-review.md                   # Stage 1: State of the art & solvent comparison
│   ├── research-question.md                   # Stage 2: Scope, objectives & constraints
│   ├── process-design-basis.md                # Stage 3: Thermodynamic models & stream specifications
│   └── model-construction-log.md              # Stages 4 & 5: Detailed simulation construction log
└── figures/
    └── flowsheet_closed_loop.png              # Flowsheet layout and convergence screenshots
```

---

## 📖 Project Documentation & Progress

* **Stage 1 — Literature Review & Background:**  
  👉 [`docs/literature-review.md`](./docs/literature-review.md)
* **Stage 2 — Research Question & Project Scope:**  
  👉 [`docs/research-question.md`](./docs/research-question.md)
* **Stage 3 — Process Design Basis:**  
  👉 [`docs/process-design-basis.md`](./docs/process-design-basis.md)
* **Stage 4 & 5 — Model Construction & Optimization Log:**  
  👉 [`docs/model-construction-log.md`](./docs/model-construction-log.md)

---

## 🚀 How to Run the Simulation Model

1. **Software Requirement:** Aspen HYSYS V14 (or compatible version) with the **Acid Gas - Chemical Solvents** property package installed.
2. Open `CO2_Capture_MEA_ClosedLoop_Optimized.hsc`.
3. Verify that the master solver toggle on the top ribbon is set to **Solver Active** (Green).
4. Inspect the **`RCY-1`** block: status should show **Converged (Green)** with 0 degrees of freedom.
5. Review stream conditions, column profiles, and reboiler duties via the **Workbook** or PFD callouts.

---

## 🛠️ Tools & Technologies

* **Process Simulation:** Aspen HYSYS V14 (Electrolyte NRTL / Acid Gas package)
* **Documentation & Typesetting:** Markdown, $\LaTeX$
* **Version Control:** Git & GitHub
