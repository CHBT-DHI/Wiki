---
tags:
  - block-reference
  - biological-models
---

# Biological Models

**Summary:** The biological process models available in WEST — state variables, processes, and key parameters.

**Source:** WEST Models Guide; WEST Getting Started Tutorial, ch 2–3.

---

## Model overview

| Model | Primary use | Nutrient removal | State variables |
|---|---|---|---|
| ASM1 | Carbon + nitrification/denitrification | N only | 13 |
| ASM2dMod | Biological N and P removal | N and P | 20 |
| ASM2dModNDHA | N, P + nitrous oxide | N, P, N₂O | 21 |
| ASM2dISS | N, P + inorganic suspended solids | N, P, mineral | 21 |
| PWM_SA | Plant-wide (WWTP + digester) | Full resource model | Multiple |

---

## ASM1

ASM1 is the IWA standard model for carbon removal, nitrification, and denitrification. It is the most widely calibrated biological model for municipal wastewater and the recommended starting point for most projects.

**Key assumptions:**
- Biomass divided into heterotrophs (X_BH) and autotrophs (X_BA)
- Oxygen and nitrate as electron acceptors
- No phosphorus dynamics

> **Needs content.** State variable table, process matrix summary, key kinetic parameters and typical values.

---

## ASM2dMod

ASM2dMod is a WEST modification of the IWA ASM2d model (Gernaey and Jørgenson, 2004). It extends ASM2d to make decay process rates electron-acceptor dependent.

**What it covers:** Carbon removal, nitrification, denitrification, and biological phosphorus removal (EBPR) via phosphate-accumulating organisms (PAOs).

### State variables

| Name | Description | Unit |
|---|---|---|
| Q | Water flow | m³/d |
| S_I | Inert soluble matter | g COD/m³ |
| S_O | Dissolved oxygen | g COD/m³ |
| S_N2 | Nitrogen gas | g N/m³ |
| S_F | Fermentable, readily biodegradable organic matter | g COD/m³ |
| S_A | Fermentation products (acetate) | g COD/m³ |
| S_NO | Nitrate + nitrite nitrogen | g N/m³ |
| S_PO | Inorganic soluble phosphorus (ortho-phosphates) | g P/m³ |
| S_NH | Ammonium nitrogen (NH4-N) | g N/m³ |
| S_ALK | Alkalinity | mol |
| X_I | Inert particulate matter | g COD/m³ |
| X_S | Slowly biodegradable matter | g COD/m³ |
| X_H | Heterotrophic organisms | g COD/m³ |
| X_PAO | Phosphorus-accumulating organisms | g COD/m³ |
| X_PP | Poly-phosphate | g P/m³ |
| X_PHA | Cell internal storage (PHA) | g COD/m³ |
| X_AUT | Autotrophic (nitrifying) biomass | g COD/m³ |
| X_TSS | Total suspended solids | g TSS/m³ |
| X_MeOH | Metal hydroxides | g COD/m³ |
| X_MeP | Metal phosphate | g COD/m³ |

### Key processes

| Process | Description |
|---|---|
| AerHydrol / AnHydrol / AnaerHydrol | Aerobic, anoxic, anaerobic hydrolysis of X_S |
| AerGrowthOnSf / AerGrowthOnSa | Aerobic growth of X_H on S_F and S_A |
| AnGrowthOnSfDenitrif / AnGrowthOnSaDenitrif | Anoxic growth (denitrification) |
| Fermentation | Conversion of S_F to S_A under anaerobic conditions |
| LysisOfHetero | Decay of X_H |
| StorageOfXPHA / AerStorageOfXPP / AnStorageOfXPP | PAO intracellular storage |
| AerGrowthOnXPHADenitrif / AnGrowthOnXPHADenitrif | PAO aerobic/anoxic growth |
| LysisOfXPAO / LysisOfXPP / LysisOfXPHA | PAO decay |
| GrowthOfAuto / LysisOfAuto | Autotrophic (nitrifier) growth and decay |
| Precipitation / Redissolution | Simultaneous phosphorus precipitation |
| Aeration | Oxygen transfer |

> **Needs content.** Add kinetic parameter table with typical values and temperature correction details.

---

## ASM2dModNDHA

Extension of ASM2dMod adding nitrous oxide (N₂O) as an intermediate in denitrification. Used for greenhouse gas modelling.

> **Needs content.** From Models Guide pp. 29–56.

---

## ASM2dISS

Extension of ASM2dMod adding inorganic suspended solids dynamics (mineral precipitation/dissolution). Relevant for plants with chemical P-removal.

> **Needs content.** From Models Guide pp. 57–83.

---

## PWM_SA

Plant-Wide Model combining ASM-based WWTP with anaerobic digestion, enabling full carbon, energy, and resource balances.

> **Needs content.** From Models Guide pp. 84–96.

---

## Related

- [Choosing a Biological Model](choosing-a-model.md)
- [Activated Sludge Tanks](activated-sludge-tanks.md)
- [Glossary](../getting-started/glossary.md)
