# Stage 1: Literature Review & Technology Selection

> **Project:** CO₂ Capture from NGCC Flue Gas Using MEA Absorption  
> **Author:** admchann  
> **Status:** Complete  

---

## 1. Project Context
This document establishes the background, technology selection rationale, and known modeling limitations for a Level 1 process simulation project: CO₂ capture from natural gas combined cycle (NGCC) flue gas via monoethanolamine (MEA) absorption, simulated in Aspen HYSYS.

---

## 2. The Problem Being Addressed
Natural gas combined cycle plants represent a large and growing share of global power generation, but they are a comparatively difficult CO₂ capture target. Because gas turbines are operated with a large excess of combustion air to protect turbine blades from thermal damage, NGCC exhaust gas is far more dilute in CO₂ than coal-fired flue gas.

Multiple independent sources converge on this figure:
* **U.S. DOE/NETL:** NGCC flue gas typically contains approximately **4–5% CO₂ by volume**, compared to 12–15% for coal-fired flue gas.
* **U.S. Patent 8,137,444:** Reports NGCC flue gas typically contains **3.9–4.2 vol% CO₂**, with essentially no SOₓ.
* **OSTI Technical Report:** Notes gas turbine exhaust contains a "relatively low concentration of CO₂ (3–4%) compared to coal flue gas (10–14%)" due to excess combustion air.
* **U.S. Patent 10,393,018:** Reports GT exhaust with **4–5 vol% CO₂** and 13–14 vol% O₂, versus 12–13% CO₂ and 5–6% O₂ for coal plants.

This dilute CO₂ concentration, combined with higher flue gas flow rates and temperatures relative to coal plants, is the central engineering challenge that makes NGCC capture more costly and technically demanding than coal capture.

---

## 3. Selected Technology and Engineering Principles
MEA (monoethanolamine) absorption is the reference solvent for post-combustion CO₂ capture and the most extensively documented capture technology in the simulation literature.

### Reaction Mechanism
* Primary amines such as MEA react with CO₂ via the **zwitterion mechanism** (Caplow, 1968; Danckwerts, 1979): CO₂ first forms an unstable zwitterion intermediate with the amine, which is then deprotonated by a base (commonly a second amine molecule) to form a stable carbamate.
* Confirmed via ¹³C NMR spectroscopy: CO₂ absorption into MEA begins with carbamate formation via the zwitterion mechanism, followed by hydration of dissolved CO₂ to bicarbonate/carbonate, accompanied by carbamate hydrolysis (Ma et al., 2015).
* Quantum-chemical and molecular dynamics studies show that both capture and solvent regeneration follow a zwitterion-mediated two-step mechanism, with proton transfer occurring primarily through hydrogen-bonded water bridges.

### Process-Level Consequence
Because MEA forms a comparatively stable carbamate, solvent regeneration is energy-intensive. The reboiler duty in the stripper is consistently identified in the literature as the dominant energy cost of the whole process — this is the parameter our future sensitivity and techno-economic work will center on.

---

## 4. Industrial Relevance
MEA absorption on gas turbine exhaust specifically is commercially demonstrated:
* **Bellingham, Massachusetts (1991–2005):** Operating on gas turbine flue gas using Fluor's Econamine FG Plus (EFG+) technology. Logged >120,000 hours of capture operation over 14+ years with an on-stream factor exceeding 98% in later years (processing a 40 MW equivalent slipstream, 85–95% recovery).
* **Peterhead, Scotland (FEED-stage):** Post-combustion capture facility designed for retrofit to a 385 MW combined-cycle gas turbine, targeting ~90% CO₂ capture using Shell/Cansolv amine technology.
* **DOE/NETL Reference Designs:** Baseline studies model NGCC-with-capture at commercial scale using Shell's CANSOLV amine solvent system, targeting 90% and 95% CO₂ capture cases.
* **Simulation-Methodology Precedent:** Peer-reviewed Aspen HYSYS study of a simplified NGCC power plant with MEA-based CO₂ removal reports a calculated heat consumption of **3.7 MJ/kg CO₂ removed** at 85% removal (close to the literature benchmark of 4.0 MJ/kg CO₂).

---

## 5. Limitations of the Simplified Model
Absorber/stripper columns can be simulated using either an **equilibrium-stage model** or a **rate-based model**. Rate-based models are more deterministic and accurate for amine absorption, particularly for low CO₂ concentration sources like NGCC (accuracy improvement of up to 8 percentage points).

### Practical Implication for This Project
As a beginner-level HYSYS user, we will build an **equilibrium-stage absorber/stripper model**. Rate-based modeling requires kinetic/mass-transfer parameter data and advanced convergence handling. This is a legitimate, well-precedented simplification for a foundational project, but it is a known, quantified limitation specifically for dilute feeds that will be disclosed explicitly in project reporting.

---

## 6. Summary: Published Data vs. Engineering Assumptions

| Item | Status | Source |
| :--- | :--- | :--- |
| **NGCC flue gas CO₂ concentration (~3–5 mol%)** | Published data | NETL; US Patents 8,137,444 & 10,393,018; OSTI |
| **MEA zwitterion → carbamate mechanism** | Published data | Peer-reviewed NMR and DFT/MD studies |
| **Reboiler duty dominates energy demand** | Published consensus | Multiple simulation and mechanistic studies |
| **Equilibrium models less accurate for dilute CO₂** | Published, quantified data | ScienceDirect open-access MEA model paper (2026) |
| **Bellingham and Peterhead industrial precedent** | Published data | Fluor Corp.; Power Technology; MIT; UK Gov't CCS |
| **Benchmark reboiler duty (~3.6–4.0 MJ/kg CO₂)** | Published data | Multiple independent HYSYS/Aspen studies |
| **Specific feed flow, composition, geometry** | Engineering assumption | To be defined in Design Basis (Stage 3) |

---

## 7. References
1. **NETL.** "Point Source Carbon Capture from Power Generation Sources." *netl.doe.gov/carbon-capture/power-generation*
2. **U.S. Patent 8,137,444.** "Systems and methods for processing CO2."
3. **U.S. Patent 10,393,018.** "Power plant methods and apparatus."
4. **OSTI.** "CO2 capture from natural gas power plants using selective exhaust gas recycle membrane designs." *osti.gov/servlets/purl/1538331*
5. **Ma, S. et al. (2015).** "Mechanisms of CO2 Capture into Monoethanolamine Solution with Different CO2 Loading during the Absorption/Desorption Processes." *Environmental Science & Technology*.
6. **University of Texas at Austin.** "A combined quantum chemical and molecular dynamics study of MEA-CO2 reaction mechanism."
7. **Fluor Corporation.** "Fluor Econamine FG Plus – Carbon Capture Technology."
8. **JWN Energy (2022).** "Carbon capture and commercially proven technologies."
9. **zeroco2.no.** "Bellingham Project Profile."
10. **Power Technology.** "Peterhead Carbon Capture and Storage (CCS) Project, Scotland."
11. **MIT Sequestration Project.** "Peterhead Project Fact Sheet."
12. **UK Government.** "Peterhead CCS Project Front Matter."
13. **ScienceDirect (2026).** "Open-access steady-state MEA CO2 capture model paper."
14. **PMC.** "Impact of thermodynamics and kinetics on the carbon capture performance of the amine-based CO2 capture system."
15. **Linköping University Electronic Press.** "Aspen HYSYS simulation of CO2 removal by amine absorption from a gas-based power plant." *ep.liu.se/ecp/027/008/ecp072708.pdf*

