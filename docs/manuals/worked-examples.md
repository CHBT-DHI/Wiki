---
tags:
  - manuals
  - examples
---

# Worked Examples

**Summary:** End-to-end walkthroughs for common plant configurations.

| Example | Description | Status |
|---|---|---|
| Simple activated sludge | Single reactor + settler, steady-state | [Quick Start Tutorial](../starter-kit/quick-start.md) |
| BNR plant | Biological nutrient removal with anoxic and aerobic zones | Below |
| MBR configuration | Membrane bioreactor layout | Below |
| Step-feed control | Aeration control with DO feedback | Below |

!!! note
    Each example should follow the [page template](../contributing.md#page-template).

---

## Example 1 — BNR Plant (Biological Nutrient Removal)

A conventional BNR layout with anaerobic, anoxic, and aerobic zones in series followed by a secondary clarifier. The example demonstrates: setting up the ASM2d model, configuring EBPR by enabling the PAO sub-model, adding a DO controller, and comparing steady-state vs dynamic effluent quality. Source files are included in the WEST installation under `Examples\BNR_Plant`.

### Overview

A BNR plant achieves simultaneous removal of nitrogen and phosphorus by combining anoxic and aerobic zones with controlled recirculation. This example uses the **ASM2dMod** model, which includes phosphorus-accumulating organisms (PAOs), and targets effluent quality suitable for sensitive receiving waters.

### Layout

```
Influent → [Pre-anoxic zone] → [Aerobic zone] → [Secondary clarifier] → Effluent
                 ↑                                         |
                 |←——— Internal Recirculation (IR, 3×Q) ←—|
                 |←——— Return Activated Sludge (RAS) ←————|
                                                           ↓
                                                        WAS (waste)
```

**Blocks required:**

| Block | Model class | Notes |
|---|---|---|
| Influent | Municipal | ASM2d output category |
| Pre-anoxic zone | CSTR | ASM2dMod; aeration off |
| Aerobic zone | CSTR | ASM2dMod; KLa controlled |
| Secondary clarifier | Settler1D | Takács settling model |
| Internal recirculation | Splitter / Recycle | From aerobic zone outlet to pre-anoxic inlet |
| RAS | Splitter / Recycle | From clarifier underflow to pre-anoxic inlet |
| WAS | Waste | From clarifier underflow |
| Effluent | Output | De-fractionation to quality parameters |

### Key parameters

| Parameter | Value |
|---|---|
| Pre-anoxic zone volume | 1 000 m³ |
| Aerobic zone volume | 3 000 m³ |
| Internal recirculation ratio (IR) | 3 × Q_influent |
| RAS flow ratio | 1 × Q_influent |
| Target SRT | 15 days |
| Design temperature | 15 °C |

### Simulation procedure

1. Build the layout following the block table above.
2. Open the **Influent** block and load your wastewater data (or use the default municipal influent).
3. Set the **Pre-anoxic CSTR** volume to 1 000 m³ and ensure KLa = 0 (no aeration).
4. Set the **Aerobic CSTR** volume to 3 000 m³ and set KLa to 240 d⁻¹ (≈ 2 mg/L DO at steady state).
5. Configure the **Internal Recirculation** splitter: set flow fraction = 3 × influent flow.
6. Configure the **RAS** splitter from the clarifier underflow: set RAS flow = influent flow.
7. Set **WAS** flow to achieve an SRT of 15 days (WAS = total biomass / SRT).
8. Run a **Steady-State** simulation to initialise concentrations.
9. Run a **Dynamic** simulation (minimum 30 days) to confirm steady performance.

### Expected results

| Effluent parameter | Target |
|---|---|
| NH₄-N | < 1 mg/L |
| NO₃-N | < 10 mg/L |
| PO₄-P | < 1 mg/L |
| TSS | < 15 mg/L |

If targets are not met, review: SRT (increase for better nitrification), IR ratio (increase for better denitrification), or anaerobic contact time (add an anaerobic zone upstream of the pre-anoxic zone for enhanced EBPR).

---

## Example 2 — MBR Configuration (Membrane Bioreactor)

An MBR layout replaces the secondary clarifier with a membrane tank. The example covers: selecting the MBR block, setting permeate flux and TMP parameters, linking aeration to the membrane tank, and running a fouling scenario. Demonstrates how MBR allows higher MLSS concentrations and smaller footprint.

### Overview

A Membrane Bioreactor (MBR) replaces the secondary clarifier with a submerged or external membrane module. The membrane provides complete solids retention, allowing operation at much higher MLSS concentrations (8–12 g/L) and producing a particle-free effluent suitable for reuse.

### Layout

```
Influent → [Bioreactor (MBR)] → [MBR Membrane block] → Permeate (Effluent)
                ↑                        |
                |←——— RAS (concentrate) ←|
                                         ↓
                                      WAS (waste)
```

**Blocks required:**

| Block | Model class | Notes |
|---|---|---|
| Influent | Municipal | ASM2d or ASM1 output category |
| Bioreactor | CSTR | High MLSS (8–12 g/L); aerobic |
| MBR Membrane | MBR block | Replaces secondary clarifier — see [Advanced Processes](../block-reference/advanced-processes.md) |
| WAS | Waste | Direct waste from bioreactor or concentrate stream |
| Effluent (permeate) | Output | Particle-free; high quality |

### Key differences from a conventional ASP

| Aspect | Conventional ASP | MBR |
|---|---|---|
| Solids separation | Gravity settler | Membrane filtration |
| MLSS | 2–5 g/L | 8–12 g/L |
| Footprint | Large (clarifier) | Compact |
| Effluent TSS | 5–15 mg/L | < 1 mg/L |
| Energy | Lower | Higher (membrane aeration/pumping) |

### Key parameters

| Parameter | Description |
|---|---|
| Membrane flux | Operating permeate flux (L m⁻² h⁻¹); must remain below the critical flux to prevent irreversible fouling |
| Permeability | Ratio of flux to transmembrane pressure; declines as fouling progresses |
| Critical flux | The flux below which membrane fouling is reversible; a key operating constraint |
| Bioreactor volume | Sized for the required SRT at the elevated MLSS |

### Simulation procedure

1. Build the layout with a bioreactor CSTR connected to the MBR membrane block (not a settler).
2. Set bioreactor volume to achieve the target SRT at design MLSS (8–12 g/L).
3. Configure the MBR membrane block parameters (flux, permeability, critical flux threshold). See [Advanced Processes](../block-reference/advanced-processes.md) for parameter descriptions.
4. Set WAS flow from the bioreactor or concentrate stream to control SRT.
5. Run steady-state then dynamic simulations. Monitor transmembrane pressure — if it increases unboundedly, reduce the operating flux.

---

## Example 3 — Step-Feed Aeration Control

A step-feed layout distributes influent across multiple aerobic zones. Aeration is controlled by DO sensors feeding PID controllers on each zone blower. The example shows: configuring the step-feed splitter, tuning PID gains, and comparing energy consumption with and without control using the Scenario Analysis experiment type.

### Overview

This example demonstrates closed-loop aeration control: a dissolved oxygen (DO) sensor in the aerobic zone feeds back to a PID controller, which adjusts blower output (KLa) to maintain a DO setpoint of 2 mg/L. This is the simplest and most common control loop in a WWTP and is a good starting point for learning about WEST's controller blocks.

### Layout

```
Influent → [Anoxic zone] → [Aerobic zone (ASU)] → Settler → Effluent
                                    ↑  ↑
                             [DO Sensor] [PID Controller] [Blower / KLa input]
                                    |______________|
                                    (feedback loop)
```

**Blocks required:**

| Block | Model class | Notes |
|---|---|---|
| Influent | Municipal | Standard ASM1 or ASM2d |
| Anoxic zone | CSTR | KLa = 0 |
| Aerobic zone (ASU) | CSTR | KLa = manipulated variable |
| DO sensor | Sensor | Measures S_O in the aerobic zone |
| PID controller | PIDController | Setpoint = 2 mg/L DO |
| Settler | Settler1D | Takács |
| Effluent | Output | |

### Control configuration

| Controller property | Value |
|---|---|
| Measured variable (PV) | S_O in aerobic zone (from DO sensor block) |
| Setpoint (SP) | 2 mg/L |
| Manipulated variable (MV) | KLa of aerobic zone CSTR |
| MV minimum | 0 d⁻¹ (blower off) |
| MV maximum | 480 d⁻¹ (maximum aeration) |
| Controller action | Reverse (increase KLa when DO < SP) |

For detailed PID tuning (Kp, Ti, Td) and controller wiring steps, see [Controllers](../how-to/controllers.md).

### Simulation procedure

1. Build the layout and connect the DO sensor output to the PID controller's measured-variable input.
2. Connect the PID controller's output to the KLa interface variable of the aerobic CSTR.
3. Set the PID setpoint to 2 mg/L.
4. Run a **Steady-State** simulation with manual KLa first to initialise concentrations.
5. Switch to **Dynamic** simulation with the PID active. Observe the DO and KLa time-series on a Dashboard plot.
6. Vary influent load (e.g. apply a diurnal flow pattern) and observe the controller's response.

### Expected behaviour

- During low-load periods, KLa decreases (energy saving).
- During peak-load periods, KLa increases to maintain the 2 mg/L DO setpoint.
- Nitrification performance (effluent NH₄) remains stable despite varying load.

For more complex control strategies (cascade, feedforward, model-based), see [Controllers](../how-to/controllers.md).
