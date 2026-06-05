---
tags:
  - technical-reference
  - process-units
---

# Process Units

**Summary:** Reference for every process unit type available in the WEST layout editor.

---

## Unit categories

WEST organises process units into six functional categories. Each unit is placed on the layout canvas and connected via flow ports to form a complete plant model.

| Category | Units | Purpose |
|---|---|---|
| Inlet / Outlet | Influent, Effluent, Splitter, Combiner | Define system boundaries and flow routing |
| Biological | CSTR, Plug Flow Reactor (PFR) | Biological treatment — growth, decay, nutrient removal |
| Settling | Secondary clarifier (1D Takács), Ideal settler | Solids separation and return/waste sludge routing |
| Sludge handling | Thickener, Digester | Sludge concentration and stabilisation |
| Flow management | Splitter, Combiner, Pump | Divide or merge streams; variable-flow elements |
| Sensors / Controllers | Sensor, Actuator, PID Controller | Closed-loop control of aeration, dosing, recycle rates |

Each unit exposes a set of **inlet ports** (carrying state variable vectors) and **outlet ports**. The process model assigned to a unit determines which state variables are transported and transformed.

---

## Influent unit

The **Influent** block is the entry point for wastewater into a WEST model. It does not perform any biological transformation — it solely defines the flow rate and composition of the incoming stream at each simulation time step.

### Role
- Sets the volumetric flow rate (Q, m³/d).
- Provides concentration values for every state variable required by the assigned process model (e.g. S_S, X_BH, S_O2, S_NO, X_TSS for ASM1).
- Acts as a boundary condition: the solver treats the Influent outputs as fixed inputs regardless of what happens downstream.

### Configuration parameters

| Parameter | Unit | Description |
|---|---|---|
| Flow (Q) | m³/d | Volumetric flow rate; can be a constant, a time-series file, or linked to a controller |
| Wastewater characterisation vector | g COD/m³ etc. | Concentrations for all model state variables |
| Temperature | °C | Bulk liquid temperature passed to kinetic rate expressions |

### Wastewater characterisation
Concentrations are entered as model-specific fractions (see [Influent Characterisation](influent-characterisation.md)). For ASM1 this means providing S_I, S_S, X_I, X_S, X_BH, X_BA, X_P, S_O2, S_NO, S_NH, S_ND, X_ND, and S_ALK. Raw lab measurements (total COD, BOD5, TKN) must be converted to these fractions before entry.

### Time-varying influent
Flow and composition can vary over time by linking the Influent block to a `.csv` or `.txt` time-series file via **Data Input**. The solver interpolates between rows. Typical use cases are diurnal load profiles and storm-event scenarios.

### Known limitations
- The Influent unit has no volume; it introduces no hydraulic delay.
- A single Influent block carries one composite vector. To model blended streams (e.g. primary effluent + industrial discharge), use a **Combiner** downstream of two separate Influent blocks.

---

## CSTR (bioreactor)

The **Continuously Stirred Tank Reactor (CSTR)** is the standard biological reactor unit in WEST. It models a completely mixed, single-volume reactor in which biological reactions — defined by the assigned process model — occur continuously.

### Assumptions
- Perfect mixing: concentration of every state variable is uniform throughout the reactor volume and equal to the outlet concentration.
- The liquid volume is constant (no free-surface dynamics).
- Dissolved oxygen (S_O2) is either controlled via a PI/PID aeration loop or set directly as a fixed value.

### Configuration parameters

| Parameter | Unit | Description |
|---|---|---|
| Volume (V) | m³ | Reactor liquid volume — primary determinant of hydraulic retention time |
| Number of compartments | — | Subdivides the volume into N equal CSTRs in series, approximating plug-flow behaviour |
| Process model | — | Selects the kinetic model (ASM1, ASM2d, ASM3, …) governing transformations |
| Aeration mode | — | Fixed DO setpoint, off (anoxic), or controlled by an external Actuator/PID |
| Temperature | °C | Kinetic rates are corrected using an Arrhenius-type temperature coefficient |
| K_L·a | 1/d | Oxygen transfer coefficient (when aeration mode is manual) |

### Compartment-in-series shortcut
Setting **compartments = N** replaces N individual CSTR units with a single block, reducing layout complexity. HRT per compartment = V_total / N. This is a common way to model plug-flow reactors (oxidation ditches, channel reactors) when a full PFR unit is not required.

### Biological reactions
All reactions defined in the selected process model are active simultaneously. Each process rate (e.g. aerobic heterotrophic growth, nitrification) is evaluated at every integration step using current state variable concentrations and kinetic parameters. Net transformation rates are multiplied by stoichiometric coefficients from the Petersen matrix (see [Components and Processes](components-processes.md)).

### Known limitations
- True plug-flow dynamics require the PFR unit or a large number of CSTR compartments (typically ≥ 10).
- Settler-return sludge enrichment must be modelled explicitly via a recycle connection from the clarifier underflow.

---

## Secondary clarifier (Takács model)

The **Secondary clarifier** in WEST implements the one-dimensional (1D) flux theory model published by Takács et al. (1991). It simulates solids settling as a layered column, capturing the behaviour of both the sludge blanket and the clarified effluent zone.

### Model description
The settler is discretised into **N horizontal layers** (default: 10) of equal depth. Each layer carries its own suspended solids concentration (X_TSS, g TSS/m³). Solids move between layers according to:

- **Gravity settling flux** (downward): driven by the double-exponential settling velocity function `v_s(X) = v_0 · (exp(-r_h · X) - exp(-r_p · X))`.
- **Bulk flow** (upward in the clarification zone, downward in the thickening zone): determined by the applied surface overflow rate.

Mass balances are solved for each layer at every time step. The bottom layer accumulates the thickened sludge returned to the bioreactor (RAS) or wasted (WAS).

### State variables
Each layer tracks `X_TSS` (total suspended solids). When an ASM model is active, the particulate fractions (X_BH, X_BA, X_I, X_P, X_S) are partitioned proportionally within each layer.

### Configuration parameters

| Parameter | Unit | Description |
|---|---|---|
| Surface area (A) | m² | Determines surface overflow rate (Q / A) |
| Total depth (H) | m | Total settler depth; divided equally among layers |
| Number of layers | — | Vertical resolution; 10 is typical, increase for finer blanket tracking |
| v_0 (max settling velocity) | m/d | Upper bound of the double-exponential settling function |
| r_h (hindered settling parameter) | m³/g | Controls settling velocity reduction at high solids concentrations |
| r_p (flocculation parameter) | m³/g | Controls the rise in settling velocity at very low concentrations |
| f_ns (non-settleable fraction) | — | Fraction of TSS that does not settle (exits in effluent) |
| RAS flow (Q_r) | m³/d | Return activated sludge flow; typically set via a Splitter or Actuator |

### Typical parameter values (municipal activated sludge)

| Parameter | Typical range |
|---|---|
| v_0 | 200 – 500 m/d |
| r_h | 0.000576 m³/g |
| r_p | 0.002880 m³/g |
| f_ns | 0.001 – 0.003 |

### Known limitations
- The 1D model cannot represent short-circuiting, density currents, or inlet turbulence.
- Performance degrades at very high solids loading rates (> 6–8 kg TSS/m²/h); an ideal settler is preferable for initial sizing studies.
- Layer count affects numerical stability; increasing layers improves accuracy but raises simulation time.

---

## Related

- [Process Models](process-models.md)
- [Quick Start Tutorial](../starter-kit/quick-start.md)
