---
tags:
  - block-reference
  - flow
---

# Flow Management

**Summary:** Combiners, splitters, pumps, and loop breakers.

**Source:** WEST Models Guide — Combiners (pp. 121–124), Splitters (pp. 125–129), Pumps (pp. 133–150), Miscellaneous (pp. 453–456).

---

## Combiners

Mix two or more inflow streams.

![Flow combiner/mixer — description](../assets/images/modelica-p294-img1.png)

| Model | Inlets |
|---|---|
| `Mix_02` | 2 inlets |
| `Mix_03` | 3 inlets |
| `Mix_03s` | 3 inlets with settling |

![Flow combiner — parameters](../assets/images/modelica-p296-img1.png)

---

## Splitters

Divide one stream into two outflows.

![Flow splitter — description](../assets/images/modelica-p290-img1.png)

| Model | Description |
|---|---|
| `SplitAbs02` | Absolute flow split — `Q_Out2` sets the second outflow rate in m³/d |
| `SplitRel02` | Relative flow split — `Q_Out2` sets the fraction to the second outlet |
| `SplitRel02w` | Relative split with wastage |
| `SplitRel02s` | Relative split with settling |

![Flow splitter — parameters](../assets/images/modelica-p292-img1.png)

**TwoASU example:** `SplitAbs02` is used for the internal recycle (`Q_Out2 = 55 338 m³/d`) and wastage (`Q_Out2 = 385 m³/d`) splitters.

---

## Pumps

![Pump block — description](../assets/images/modelica-p305-img1.png)

| Model | Description |
|---|---|
| `Pumps.SimpleQ` | Constant-flow pump |
| `Pumps.CF_Q_Throttle` | Centrifugal pump, throttle-controlled |
| `Pumps.CF_Q_VFD` | Centrifugal pump, VFD-controlled |
| `Pumps.CF_HQ_VFD` | Centrifugal pump with H-Q characteristic |
| `Pumps.CF_HN_VFD` | Centrifugal pump with speed control |

![Pump block — parameters](../assets/images/modelica-p308-img1.png)

### Valve block

The Valve block modulates flow through a pipe or process connection based on a dimensionless control signal in the range 0–1. The output flow is proportional to the signal: `Q_out = signal × Q_in`, up to a configurable maximum. It is used wherever a controller or timer needs to throttle or divert a stream rather than switch it fully on or off.

**Parameters:**

| Parameter | Description | Units |
|---|---|---|
| `Q_max` | Maximum allowable flow through the valve when fully open | m³/d |
| `signal` | Control input from a controller or timer (dimensionless, 0–1) | — |

**Typical use — flow diversion:** Connect the output of an On/Off controller or a PID controller to the `signal` terminal. The valve then opens or closes proportionally, diverting the desired fraction of flow to a downstream block (e.g. bypassing a reactor, controlling RAS flow, or modulating dosing). A signal of 0 closes the valve completely; a signal of 1 passes the full `Q_max` flow.

![Valve block — description](../assets/images/modelica-p311-img1.png)

![Valve — parameters](../assets/images/modelica-p314-img1.png)

---

## Loop breakers

Loop breakers must be placed on recycle connections that create algebraic loops (circular dependencies that prevent the Newton solver from converging).

| Model | Description |
|---|---|
| `LoopBreakers.MainDiff` | Differential loop breaker — recommended default |
| `LoopBreakers.MainExplicit` | Explicit loop breaker |
| `LoopBreakers.SignalDiff` | Signal-level loop breaker |

In the TwoASU example, two Loop Breakers are used: one on the internal recycle and one on the return sludge line.

---

## Related

- [Building a Plant Layout](../how-to/building-layouts.md)
- [Quick Start Tutorial](../getting-started/quick-start.md)
