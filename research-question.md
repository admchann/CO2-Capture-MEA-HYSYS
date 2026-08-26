# Stage 2: Research Question, Objectives, Scope & Limitations

> **Project:** CO₂ Capture from NGCC Flue Gas Using MEA Absorption  
> **Author:** admchann  
> **Status:** Complete (Pre-Simulation Definition)  

---

## 📌 Methodology Statement
This research question, project scope, and initial limitations were defined **before** simulation work began, consistent with a stage-gated process engineering methodology. Any material deviations required during model development will be documented explicitly alongside the engineering reasoning.

---

## ❓ Main Research Question
> **How do inlet CO₂ concentration, lean amine (MEA) loading, and flue gas flow rate independently affect CO₂ capture efficiency and specific reboiler duty in a simplified equilibrium-stage MEA absorption process treating simulated natural gas combined cycle (NGCC) flue gas?**

This question is intentionally scoped to be directly answerable through steady-state process simulation. The independent variables can be systematically varied in Aspen HYSYS, and the response metrics (CO₂ capture efficiency and specific reboiler duty) serve as standard performance indicators in post-combustion carbon capture literature.

---

## 🎯 Supporting Objectives
1. **Model Development:** Develop a steady-state Aspen HYSYS flowsheet representing a simplified MEA-based absorber–stripper system treating simulated NGCC flue gas.
2. **Model Verification:** Verify mass and energy balance closure and assess the reasonableness of key model outputs against published literature benchmark ranges.
3. **Parametric Sensitivity (CO₂ Concentration):** Quantify the independent effect of inlet CO₂ concentration on capture efficiency and specific reboiler duty.
4. **Parametric Sensitivity (Lean Loading):** Quantify the independent effect of lean amine CO₂ loading on capture efficiency and specific reboiler duty.
5. **Parametric Sensitivity (Throughput):** Quantify the independent effect of flue gas flow rate on capture efficiency and specific reboiler duty under defined assumptions.
6. **Engineering Evaluation:** Discuss the operational implications of the simulation results and explicitly analyze the limitations of using an equilibrium-stage model for dilute feeds.

---

## 📐 Scope

### In-Scope
* **System Boundary:** A single absorber–stripper absorption train operating strictly under steady-state conditions.
* **Solvent Selection:** Monoethanolamine (MEA) as the sole chemical solvent.
* **Modeling Approach:** Equilibrium-stage column model implemented in Aspen HYSYS.
* **Feed Basis:** Generic NGCC flue gas containing ~4 mol% CO₂ (based on literature baselines rather than a specific power plant).
* **Sensitivity Design:** Three One-at-a-Time (OAT) sensitivity studies varying inlet CO₂ concentration, lean MEA loading, and flue gas flow rate independently around a baseline case.

### Out-of-Scope
* Dynamic simulation, process control, and transient response.
* Alternative chemical/physical solvents (e.g., MDEA, DEA, piperazine blends).
* Rate-based column modeling (reserved for potential future advanced projects).
* Multi-variable full factorial sensitivity designs.
* Techno-economic assessment, capital expenditure (CAPEX), and operating cost (OPEX) estimations.

---

## ⚠️ Key Modeling Limitations & Simplifications

### 1. Equilibrium-Stage Modeling Accuracy
An equilibrium-stage model assumes vapor-liquid equilibrium on each stage rather than solving mass-transfer kinetics. Its predictions may deviate from detailed rate-based models, particularly for dilute, low-CO₂-partial-pressure streams like NGCC flue gas. Results are interpreted as directional engineering trends rather than predictive field performance.

### 2. Absence of Physical Experimental Validation
No physical experimental or pilot plant data will be collected. Model validity relies on internal mass/energy balance closure and validation against established literature benchmarks.

### 3. Simplified Flue Gas Composition
The simulated feed ignores trace contaminants (e.g., SOₓ, NOₓ, fly ash, oxygen degradation products). Mechanisms such as solvent degradation, reclaiming, and corrosion are outside the model scope.

### 4. One-at-a-Time (OAT) Sensitivity Limitations
Varying one parameter at a time does not capture non-linear interaction effects (e.g., how flow rate variations interact with lean loading). 

---

## 💡 Engineering Rationale for Project Level
* **Workload & Feasibility:** Executable within 4–6 hours per week over a 3–5 week timeline while maintaining academic rigor.
* **Complementary Metrics:** Pairing separation efficiency (capture %) with energy intensity (MJ/kg CO₂) provides a holistic thermodynamic overview of column behavior.
* **Foundational Baseline:** Establishes the necessary modeling foundation for future work in process optimization and techno-economic modeling.
