---
tags:
  - block-reference
  - aeration
---

# Aeration

**Summary:** Aeration block models for dissolved oxygen transfer.

**Source:** WEST Models Guide — Aeration section (pp. 332–343).

---

## Overview

Aeration blocks are connected to bioreactor blocks via the aeration terminal. They translate an aeration input signal (e.g. air flow rate, blower speed, or DO set-point) into an oxygen transfer rate (`kLa`).

| Model | Description |
|---|---|
| `Aeration.Ideal` | `kLa` is directly the manipulated input (`y_S` = DO set-point) |
| `Aeration.IdealMulti` | Multiple zones with independent set-points |
| `Aeration.IdealMulti2` | Two-compartment ideal aeration |
| `Aeration.Surface` | Surface aeration model |
| `Aeration.FineBubble1` | Fine-bubble diffuser model (single-zone) |
| `Aeration.FineBubble2` | Fine-bubble diffuser model (two-zone) |

---

## Aeration.Ideal

The simplest and most commonly used aeration model. The controller drives the DO set-point `y_S`, and the aeration block computes `kLa` to maintain that set-point.

**Key parameters:**

| Parameter | Description |
|---|---|
| `y_S` | DO set-point (mg O₂/l) — typically 1.5–2.5 |
| `K_La_max` | Maximum oxygen transfer coefficient (h⁻¹) |

In the TwoASU example, a PI controller sets `y_S` and the `Aeration.Ideal` block (inside the Activated Sludge block) maintains it.

---

## Blowers

For energy modelling, replace `Aeration.Ideal` with a blower-connected model. Blower blocks simulate the physical delivery of air to diffusers, computing power consumption and translating air flow rate into the `Q_air` input expected by the aeration terminal of a bioreactor.

### Blower model overview

| Model | Description |
|---|---|
| `Blowers.CF_Q_VFD` | Centrifugal fan, flow-controlled via variable-frequency drive (VFD) |
| `Blowers.CF_HQ_VFD` | Centrifugal fan with H-Q (pressure–flow) characteristic curve, VFD speed control |
| `Blowers.CF_Q_IGV` | Centrifugal fan with inlet guide vane (IGV) throttle control |
| `Blowers.PD_Q_VFD` | Positive-displacement (Roots-type) blower, VFD-controlled |

### CF_Q_Throttle blower

![CF_Q_Throttle blower — description](../assets/images/modelica-p136-img1.png)

![CF_Q_Throttle — parameters table](../assets/images/modelica-p136-img2.png)

### CF_Q_VFD blower

![CF_Q_VFD blower — description](../assets/images/modelica-p140-img1.png)

![CF_Q_VFD — parameters table](../assets/images/modelica-p140-img2.png)

### CF_HQ_VFD blower

![CF_HQ_VFD blower — description](../assets/images/modelica-p145-img1.png)

![CF_HQ_VFD — parameters table](../assets/images/modelica-p145-img2.png)

### CF_Q_IGV blower

![CF_Q_IGV blower — description](../assets/images/modelica-p147-img1.png)

![CF_Q_IGV — parameters](../assets/images/modelica-p147-img2.png)

### PD_Q_VFD blower

![PD_Q_VFD blower — description](../assets/images/modelica-p149-img1.png)

### Parameters

| Parameter | Description | Unit | Typical value |
|---|---|---|---|
| `Q_air` | Nominal air flow rate delivered to the tank | m³/d | 5 000–50 000 |
| `Q_air_min` | Minimum allowable air flow rate | m³/d | 500–2 000 |
| `Q_air_max` | Maximum allowable air flow rate | m³/d | 10 000–80 000 |
| `P_blower` | Rated shaft power of the blower | kW | 15–500 |
| `eta` | Overall blower efficiency (motor + mechanical) | – (0–1) | 0.60–0.75 |
| `p_out` | Discharge pressure (absolute) | kPa | 115–160 |
| `p_in` | Inlet air pressure (atmospheric) | kPa | 101.3 |
| `T_in` | Inlet air temperature | °C | 15–20 |
| `n_blowers` | Number of blowers operating in parallel | – | 1–10 |

For `CF_HQ_VFD` and `CF_HN_VFD` models, the H-Q characteristic curve is defined by additional polynomial coefficients (`a0`, `a1`, `a2`) that describe pressure as a function of flow.

### Connecting blowers to a bioreactor

The blower block outputs `Q_air` (m³/d) on its air-supply terminal. This terminal must be connected to the `Q_air` input port on the aeration sub-model of an Activated Sludge (or similar bioreactor) block. The aeration sub-model then converts `Q_air` into `kLa` using the selected diffuser transfer model (`FineBubble1`, `FineBubble2`, etc.).

Typical connection path:

```
BlowerController --> Blowers.CF_Q_VFD --> [Q_air port] --> Aeration.FineBubble1 (inside ASU block)
```

A blower controller block (e.g. `BlowerControllers.Common10_CFHQVFD`) sits upstream and outputs a flow or speed set-point to the blower. Multiple blowers can be staged by the controller using the `n_blowers` signal.

### Energy calculation

Blower power consumption is computed from the isentropic compression equation adjusted by efficiency:

```
P_blower = (Q_air_actual / 86 400) × (p_out − p_in) / eta   [kW]
```

where `Q_air_actual` is in m³/s and pressures are in kPa. WEST integrates `P_blower` over the simulation horizon to report total energy use (kWh), which feeds into the Energy block for plant-wide energy accounting.

### Typical values

| Configuration | Q_air (m³/d) | P_blower (kW) | eta |
|---|---|---|---|
| Small WWTP (< 10 000 PE) | 3 000–8 000 | 15–45 | 0.62 |
| Medium WWTP (10 000–100 000 PE) | 8 000–30 000 | 45–200 | 0.68 |
| Large WWTP (> 100 000 PE) | 30 000–80 000 | 200–500 | 0.72 |

---

## Aeration diffuser blocks

### Aeration FineBubble1

![Aeration FineBubble1 — description](../assets/images/modelica-p150-img1.png)

![Aeration FineBubble1 — parameters](../assets/images/modelica-p150-img2.png)

### Aeration FineBubble2

![Aeration FineBubble2 — description](../assets/images/modelica-p153-img1.png)

![Aeration FineBubble2 — parameters](../assets/images/modelica-p153-img2.png)

### Aeration CoarseBubble

![Aeration CoarseBubble — description](../assets/images/modelica-p154-img1.png)

![Aeration CoarseBubble — parameters](../assets/images/modelica-p154-img2.png)

### Surface aerator

![Surface aerator — description](../assets/images/modelica-p156-img1.png)

![Surface aerator — parameters](../assets/images/modelica-p156-img2.png)

### Jet aerator

Jet aerators inject pressurised liquid mixed with air into the tank through nozzles, creating high-velocity jets that entrain and disperse fine bubbles throughout the mixed liquor. The combined momentum and turbulence from the jets provide both oxygen transfer and mixing in a single unit, making them well suited to deep tanks or retrofit installations where diffuser grids are impractical.

**Key parameters:**

| Parameter | Description | Units |
|---|---|---|
| `Q_jet` | Jet liquid flow rate | m³/d |
| `alpha_jet` | Jet transfer efficiency factor (process water correction) | — |

Jet aerators typically achieve higher KLa values than fine-bubble diffusers at equivalent energy input in high-mixed-liquor-suspended-solids conditions, because the alpha factor (`alpha_jet`) degrades less with increasing MLSS. Typical standard oxygen transfer efficiency (SOTE) per metre of submergence is comparable to coarse-bubble systems, but the high mixing intensity reduces fouling risk on the nozzles.

![Jet aerator — description](../assets/images/modelica-p157-img1.png)

![Jet aerator — parameters](../assets/images/modelica-p157-img2.png)

---

## Related

- [Activated Sludge Tanks](activated-sludge-tanks.md)
- [Controllers & Timers](controllers-timers.md)
- [Advanced Processes](advanced-processes.md)
