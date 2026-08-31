# Stage 3: Process Design Basis

> **Project:** CO₂ Capture from NGCC Flue Gas Using MEA Absorption  
> **Author:** admchann  
> **Status:** Complete (Pre-Simulation Baseline)  

---
> **⚠️ Design Basis Evolution Note:**  
> The preliminary stoichiometric calculations initially suggested a 10-stage absorber at 1,319 kgmol/h lean amine feed. However, detailed Aspen HYSYS model convergence and diagnostic sensitivity testing (documented in [simulation-development-notes.md](./simulation-development-notes.md)) demonstrated a finite-stage pinch under equilibrium modeling. To achieve the target ~90% CO₂ capture efficiency, the final base case was updated to **40 equilibrium stages** and **2,638 kgmol/h** lean amine circulation rate. This update supersedes the initial preliminary sizing values below.

## 1. Purpose
This document defines the preliminary design basis for the base-case Aspen HYSYS simulation. It specifies the feed composition and conditions, solvent specification, preliminary operating conditions, product definitions, major unit operations, and key modelling assumptions.

Each parameter is identified as either:
* **Published/Reference-Based Value:** Selected based on relevant literature or reference sources; or
* **Engineering Assumption:** Selected to establish a reproducible educational simulation basis.

Where applicable, values may be revised during model development. Any material revision will be explicitly documented together with its engineering justification.

---

## 2. Feed Composition: Simulated NGCC Flue Gas
The base-case flue gas represents a simplified generic NGCC exhaust stream rather than a specific named power plant.

| Component | Base-Case Composition | Status | Basis |
| :--- | :--- | :--- | :--- |
| **CO₂** | 4 mol% | Published/reference-based | Representative value for NGCC flue gas identified in literature review |
| **H₂O** | To be defined | Engineering/model assumption | Must be specified consistently with feed temperature & phase condition |
| **O₂** | ~12 mol% | Engineering assumption | Represents excess combustion air in simplified NGCC flue gas |
| **N₂** | Balance | Derived | Calculated to ensure total feed composition equals 100 mol% |

### Key Feed Assumptions
* Trace contaminants such as SOₓ and NOₓ are excluded.
* O₂ is treated as non-reactive within the simplified process model.
* The model does not represent solvent degradation caused by contaminants or long-term oxidative degradation.
* The feed composition is intended for process simulation and sensitivity analysis rather than representation of a specific commercial power plant.

> **Important Modelling Note:** Water content and feed temperature must be finalized together based on whether the flue gas is represented as saturated, partially condensed, or another clearly defined condition to avoid thermodynamic inconsistency.

---

## 3. Feed Conditions

| Parameter | Preliminary Base-Case Value | Status | Basis |
| :--- | :--- | :--- | :--- |
| **Temperature** | 40 °C | Published/reference-based | Representative condition for cooled flue gas entering absorption |
| **Pressure** | 1.15 bar | Published/reference-based | Representative near-atmospheric pressure supported by literature |
| **Total Flow Rate** | To be defined | Engineering assumption | Required to establish a reproducible simulation base case |

### Feed Flow Basis
The base-case flue gas flow rate will be selected as a clearly defined engineering basis rather than representing a specific commercial power plant. This allows the project to investigate the relative effect of feed throughput on process performance without claiming to represent a specific plant capacity. Once selected, this baseline flow rate will remain documented as the reference condition for sensitivity analysis.

---

## 4. Solvent Specification

| Parameter | Base-Case Value | Status | Basis |
| :--- | :--- | :--- | :--- |
| **Solvent** | Aqueous monoethanolamine (MEA) | Published/reference-based | Widely used reference solvent for post-combustion capture |
| **MEA Concentration** | 30 wt% | Published/reference-based | Common baseline concentration used in reference MEA studies |
| **Lean Solvent Loading** | 0.25 mol CO₂/mol MEA | Published/reference-based | Representative baseline selected for sensitivity analysis |
| **Capture Objective** | ~90% | Performance target | Literature-relevant reference target to establish base case |

> **Important Modelling Principle:** The ~90% capture value is used to establish and evaluate the initial base case. However, capture efficiency will **not** be fixed at 90% during sensitivity studies. Operating parameters will be held according to the defined methodology, allowing capture efficiency to respond naturally as a genuine response variable.

---

## 5. Preliminary Column Operating Conditions

| Parameter | Preliminary Base-Case Value | Status | Basis |
| :--- | :--- | :--- | :--- |
| **Absorber Pressure** | ~1.1–1.2 bar | Published/reference-based | Near-atmospheric post-combustion absorption conditions |
| **Absorber Stage Count** | Preliminary value | Engineering/model assumption | Simplified equilibrium-stage representation |
| **Stripper Pressure** | ~1.5–1.9 bar | Published/reference-based | Representative pressure range for simplified MEA regeneration |
| **Stripper Stage Count** | Preliminary value | Engineering/model assumption | Simplified equilibrium-stage representation |
| **Column Pressure Drop** | To be defined | Engineering/model assumption | Required for reproducible model specification |
| **Lean/Rich HX Approach** | ~10 °C | Published/reference-based | Representative approach used in comparable configurations |

*Note: The selected equilibrium-stage counts represent a simplified modelling configuration and should not be interpreted as detailed industrial column design. Column diameter, packing selection, hydraulic design, and mass-transfer kinetics are outside the scope of this Level 1 project.*

---

## 6. Product Definitions

### Treated Flue Gas
The absorber overhead stream is evaluated primarily using:
* CO₂ capture efficiency (%)
* Residual CO₂ concentration

The base-case model will initially be configured to achieve approximately 90% CO₂ capture where feasible, after which efficiency will act as a variable model response during sensitivity testing.

### CO₂-Rich Product Stream
The stripper overhead stream will be evaluated and its composition reported. CO₂ purity is not specified as a strict constraint because downstream processing (compression, dehydration, and transport specifications) is outside the scope of this project.

---

## 7. Major Unit Operations
The simplified base-case flowsheet is expected to include:
* **Absorber:** Simplified equilibrium-stage column
* **Heat Exchanger:** Lean/rich solvent heat exchanger
* **Stripper/Regenerator:** Simplified equilibrium-stage column
* **Reboiler:** Column bottom heat supply
* **Condenser System:** Overhead condenser and reflux accumulator
* **Cooler:** Lean solvent cooler
* **Pumps:** Solvent circulation pump(s)
* **Makeup Streams:** Water/MEA makeup to maintain solvent inventory balance

*Advanced configurations (e.g., split-stream absorption, vapor recompression, intercooling) are explicitly outside the scope of this Level 1 project.*

---

## 8. Key Modelling Assumptions

| Assumption | Engineering Rationale |
| :--- | :--- |
| **Simplified Feed Composition** | Focuses model on CO₂ capture behavior while excluding complex degradation |
| **Non-Reactive O₂** | Avoids introducing complex degradation kinetics outside steady-state scope |
| **No Solvent Reclaiming Loop** | Not required for a foundational steady-state process model |
| **Equilibrium-Stage Columns** | Provides a manageable representation while acknowledging accuracy limitations |
| **Fixed Configuration Baseline** | Enables controlled comparison during one-at-a-time (OAT) sensitivity studies |
| **Generic NGCC Feed Basis** | Avoids claiming representation of proprietary or site-specific operations |
| **No Equipment Sizing/Hydraulics** | Keeps focus on process behavior rather than detailed mechanical design |

---

## 9. Sensitivity Analysis Variables

| Independent Variable | Base Case | Sensitivity Study |
| :--- | :--- | :--- |
| **Inlet CO₂ Concentration** | 4 mol% | Yes |
| **Lean MEA CO₂ Loading** | 0.25 mol CO₂/mol MEA | Yes |
| **Flue Gas Flow Rate** | Defined base case | Yes |

Detailed ranges, increments, and response metrics will be defined in Stage 6 using a One-at-a-Time (OAT) methodology.

---

## 10. Design Basis Revision Policy
This document establishes the preliminary design basis before Aspen HYSYS model construction begins. Some parameters may require revision during model development due to thermodynamic consistency, model convergence, physically unrealistic results, or reproducibility requirements.

Any material change will be explicitly documented with:
1. The original value or assumption
2. The revised value
3. The technical reason for the revision
4. The expected impact on subsequent analysis

No design-basis changes will be made silently after simulation work begins.
