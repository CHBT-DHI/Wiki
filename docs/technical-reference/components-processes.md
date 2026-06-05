---
tags:
  - technical-reference
---

# Components and Processes

**Summary:** State variables, stoichiometry, and kinetics for the process models in WEST.

---

## What are components?

In WEST, **components** are the state variables that describe the composition of a liquid stream or reactor contents at any point in time. Every component is a concentration value (typically g/m³ or mol/m³) carried in the state vector that flows between units.

Components are divided into two groups by their physical phase:

- **Soluble components** — prefix `S_`. Dissolved species that are not removed by settling (e.g. dissolved oxygen, nitrate, soluble substrate).
- **Particulate components** — prefix `X_`. Suspended or colloidal species that are subject to settling in clarifiers (e.g. active biomass, inert solids, slowly biodegradable substrate).

### ASM1 components (example)

| Symbol | Description | Phase |
|---|---|---|
| S_I | Soluble inert organic matter | Soluble |
| S_S | Readily biodegradable substrate | Soluble |
| X_I | Particulate inert organic matter | Particulate |
| X_S | Slowly biodegradable substrate | Particulate |
| X_BH | Active heterotrophic biomass | Particulate |
| X_BA | Active autotrophic (nitrifying) biomass | Particulate |
| X_P | Products from biomass decay | Particulate |
| S_O2 | Dissolved oxygen | Soluble |
| S_NO | Nitrate and nitrite nitrogen | Soluble |
| S_NH | Ammonium and ammonia nitrogen | Soluble |
| S_ND | Soluble biodegradable organic nitrogen | Soluble |
| X_ND | Particulate biodegradable organic nitrogen | Particulate |
| S_ALK | Alkalinity (molar units) | Soluble |

ASM2d adds components for phosphorus (S_PO4, X_PP, X_PAO, X_PHA) and ASM3 adds intracellular storage products (X_STO).

### Component naming in WEST
WEST follows the IWA naming convention. When building a custom model in the WEST Model Library (WML / Modelica), components are declared as `Real` variables inside the **WasteWater** connector type. The connector is the typed port that carries all component concentrations plus the volumetric flow rate Q between units.

---

## What are processes?

**Processes** are the biological or chemical transformation reactions that change component concentrations over time. Each process is characterised by:

1. A **kinetic rate expression** (ρ_j, units: g COD/m³/d or g N/m³/d) that quantifies how fast the transformation occurs under current conditions.
2. A row of **stoichiometric coefficients** that specify how much of each component is consumed or produced per unit of process rate.

### Common process types in ASM models

| Process | Type | Key driver |
|---|---|---|
| Aerobic growth of heterotrophs | Growth | S_S consumption, S_O2 required |
| Anoxic growth of heterotrophs | Growth | S_S consumption, S_NO as electron acceptor |
| Aerobic growth of autotrophs (nitrification) | Growth | S_NH oxidation, S_O2 required |
| Decay of heterotrophs | Decay | Endogenous respiration or lysis |
| Decay of autotrophs | Decay | Endogenous respiration or lysis |
| Hydrolysis of X_S | Hydrolysis | Slow enzymatic breakdown of particulate organics to S_S |
| Ammonification | Mineralisation | Conversion of S_ND to S_NH |

Kinetic rate expressions combine Monod saturation terms, inhibition terms, and switching functions. For example, aerobic heterotrophic growth in ASM1:

```
rho_1 = mu_H · (S_S / (K_S + S_S)) · (S_O2 / (K_OH + S_O2)) · X_BH
```

---

## The stoichiometric (Petersen) matrix

The relationship between components and processes is organised as a **stoichiometric matrix**, also called the **Petersen matrix** after its originator. Rows represent processes; columns represent components. Each cell holds the stoichiometric coefficient ν_{i,j} for component i in process j.

The net rate of change of component i is:

```
dC_i/dt = Σ_j (ν_{i,j} · ρ_j)
```

where the sum runs over all active processes j. This compact notation makes it straightforward to add new processes or components without restructuring the model equations.

### Example (ASM1, partial)

|  | S_S | S_O2 | S_NO | X_BH | S_NH |
|---|---|---|---|---|---|
| Aerobic heterotrophic growth | −1/Y_H | −(1−Y_H)/Y_H | 0 | +1 | −i_XB |
| Anoxic heterotrophic growth | −1/Y_H | 0 | −(1−Y_H)/(2.86·Y_H) | +1 | −i_XB |
| Decay of heterotrophs | 0 | 0 | 0 | −1 | 0 |

Positive coefficients indicate production; negative coefficients indicate consumption. Y_H is the heterotrophic yield coefficient; i_XB is the nitrogen content of biomass.

---

## How components and processes are defined in WEST

WEST models are written in a Modelica-based language called the **WEST Model Language (WML)**. The key structural elements are:

1. **WasteWater connector** — declares all component variables and Q. All units share a common connector type for a given process model, ensuring type-safe connections.

2. **ProcessModel block** — contains:
   - Parameter declarations (kinetic constants, stoichiometric coefficients).
   - Process rate equations (`equation` section).
   - A `for` loop that accumulates stoichiometric contributions: `dC[i] = sum(nu[i,j] * rho[j] for j)`.

3. **Reactor unit** — instantiates a ProcessModel and integrates the mass balance `V · dC[i]/dt = Q_in · C_in[i] − Q_out · C_out[i] + V · dC[i]_reaction`.

Custom models can be created in the WEST Model Library editor by defining new components, extending the WasteWater connector, and implementing the Petersen matrix equations in WML.

---

## Related

- [Process Models](process-models.md)
- [Glossary](../starter-kit/glossary.md)
