---
tags:
  - technical-reference
  - process-models
---

# Process Models

**Summary:** The biological process models available in WEST, when to use each, and their key assumptions.

---

## Overview

Activated sludge models (ASMs) are mathematical descriptions of the biological and chemical transformations that occur in wastewater treatment. WEST ships with the three IWA standard models plus several extensions. All models follow the Petersen matrix convention (see [Components and Processes](components-processes.md)).

| Model | Primary use | N removal | P removal | Components | Processes |
|---|---|---|---|---|---|
| ASM1 | Carbon oxidation, nitrification/denitrification | Yes | No | 13 | 8 |
| ASM2d | Biological nutrient removal including EBPR | Yes | Yes | 19 | 21 |
| ASM3 | Storage-based model, improved N dynamics | Yes | No | 13 | 12 |

For models outside the IWA standard set (e.g. ADM1 for anaerobic digestion, Barker & Dold), consult the WEST Model Library.

---

## ASM1

ASM1 (Henze et al., IWA, 1987) is the most widely used activated sludge model. It describes carbon oxidation, nitrification, and denitrification by a mixed heterotrophic/autotrophic biomass culture.

### State variables (13 components)

| Symbol | Description |
|---|---|
| S_I | Soluble inert organics |
| S_S | Readily biodegradable COD |
| X_I | Particulate inert organics |
| X_S | Slowly biodegradable COD |
| X_BH | Active heterotrophic biomass |
| X_BA | Active autotrophic (nitrifying) biomass |
| X_P | Endogenous decay products |
| S_O2 | Dissolved oxygen |
| S_NO | Nitrate + nitrite nitrogen |
| S_NH | Ammonium nitrogen |
| S_ND | Soluble biodegradable organic N |
| X_ND | Particulate biodegradable organic N |
| S_ALK | Alkalinity |

### Processes (8)

1. Aerobic growth of heterotrophs (S_S + O2 → X_BH)
2. Anoxic growth of heterotrophs (S_S + NO3 → X_BH, denitrification)
3. Decay of heterotrophs
4. Aerobic growth of autotrophs (nitrification: NH4 + O2 → NO3 + X_BA)
5. Decay of autotrophs
6. Ammonification of soluble organic N
7. Hydrolysis of particulate organics (X_S → S_S)
8. Hydrolysis of particulate organic N (X_ND → S_ND)

### Key kinetic parameters

| Parameter | Symbol | Typical value |
|---|---|---|
| Max heterotrophic growth rate | mu_H | 4–6 d⁻¹ |
| Heterotrophic yield | Y_H | 0.67 g COD/g COD |
| Max autotrophic growth rate | mu_A | 0.5–0.8 d⁻¹ |
| Autotrophic yield | Y_A | 0.24 g COD/g N |
| Heterotrophic decay rate | b_H | 0.3–0.6 d⁻¹ |

### Typical applications
- Municipal activated sludge plants with nitrification/denitrification.
- Scenario analysis and process optimisation where P removal is not relevant.
- Calibration studies where data availability is limited (fewer parameters than ASM2d).

### Limitations
- No phosphorus removal processes.
- Decay represented as simple lysis; does not distinguish endogenous respiration from lysis products.
- Does not model intracellular storage — can underestimate O2 demand in SBR or intermittently fed systems.

---

## ASM2d

ASM2d (Henze et al., IWA, 1999) extends ASM1 with enhanced biological phosphorus removal (EBPR) by polyphosphate accumulating organisms (PAOs) and chemical phosphorus precipitation.

### Extensions over ASM1

- **New components (6 added, 19 total):** S_F (fermentable substrate), S_A (acetate/VFAs), X_PAO (PAO biomass), X_PP (stored polyphosphate), X_PHA (intracellular PHA storage), S_PO4 (soluble phosphate).
- **New process groups:**
  - PAO growth on S_PHA under aerobic and anoxic conditions.
  - Polyphosphate storage and release.
  - PAO decay.
  - Simultaneous chemical precipitation of phosphorus with metal salts.
- **Fermentation:** S_S is split into S_F and S_A to model VFA production under anaerobic conditions (pre-anaerobic selector zone).

### Key additional parameters

| Parameter | Symbol | Description |
|---|---|---|
| Max PAO growth rate | mu_PAO | Growth rate on intracellular PHA |
| PP storage rate constant | q_PP | Rate of polyphosphate uptake |
| PHA storage rate constant | q_PHA | Rate of PHA synthesis from acetate |
| PAO decay rate | b_PAO | Combined endogenous decay |

### Typical applications
- Plants with a biological P removal stage (UCT, A2O, Bardenpho configurations).
- Design and optimisation of anaerobic selector zones.
- Effluent total phosphorus compliance studies.

### Limitations
- Requires more detailed influent characterisation (S_F vs. S_A split, soluble phosphate).
- Higher parameter count increases calibration effort.
- Chemical precipitation sub-model requires metal dose data.

---

## ASM3

ASM3 (Gujer et al., IWA, 1999) was developed to address structural weaknesses in ASM1, particularly the representation of substrate storage and oxygen demand dynamics in non-steady-state conditions.

### Storage-based approach
The key conceptual difference from ASM1 is that heterotrophs do not grow directly on external substrate (S_S). Instead:

1. S_S is first **stored** as intracellular storage products (X_STO) — a rapid process that occurs even under feast conditions.
2. Growth occurs on X_STO — a slower process that is decoupled from external substrate availability.

This two-step mechanism improves prediction of:
- Oxygen uptake rate (OUR) profiles in batch tests.
- Respiration dynamics in SBR and intermittently aerated systems.
- Sludge production under variable loading.

### Differences from ASM1

| Aspect | ASM1 | ASM3 |
|---|---|---|
| Substrate uptake | Direct growth on S_S | Storage of S_S as X_STO |
| Decay model | Lysis with inert products | Endogenous respiration (no decay products) |
| Nitrogen | Organic N fractions explicit | Simplified N model |
| Phosphorus | Not included | Not included |

### Typical applications
- SBR (sequencing batch reactor) modelling.
- Dynamic loading studies where OUR profile accuracy matters.
- Research into storage-growth kinetics and feast-famine behaviour.

### Limitations
- Not suitable for plants requiring P removal modelling — use ASM2d.
- Less field validation than ASM1 for continuous-flow systems.
- Additional parameters for storage kinetics require dedicated batch experiments to calibrate.

---

## Selecting a model

Use the table below as a starting point. The guiding principle is **minimum complexity**: select the simplest model whose structure covers your treatment objectives and effluent requirements.

| Effluent requirements | Recommended model | Rationale |
|---|---|---|
| BOD / TSS only (no nutrient limits) | ASM1 | Simplest; adequate for carbon removal |
| Nitrification only | ASM1 | Autotrophic growth well-represented |
| Nitrification + denitrification | ASM1 | Standard application; widely calibrated |
| Biological N + P removal (EBPR) | ASM2d | PAO and polyphosphate processes required |
| N + P with VFA / anaerobic zone | ASM2d | Fermentation and PAO selector modelling |
| SBR or dynamic OUR accuracy | ASM3 | Storage-growth decoupling essential |
| Anaerobic digestion | ADM1 | Separate model; available in WEST library |
| Research / custom kinetics | Custom WML model | Define your own Petersen matrix in WEST |

**Calibration effort increases significantly from ASM1 to ASM2d.** If the plant does not have biological P removal and effluent TP is met by chemical dosing only, ASM1 + chemical P model is often sufficient and easier to calibrate.

---

## Related

- [Components and Processes](components-processes.md)
- [Calibration](../advanced-topics/calibration.md)
- [Glossary](../starter-kit/glossary.md)
