---
tags:
  - how-to
  - controllers
---

# Controllers

**Summary:** How to set up feedback control loops in a WEST layout.

**Source:** WEST User Guide, Chapter 5.2.15; WEST Models Guide — Controllers section.

**Prerequisites:** [Building a Plant Layout](building-layouts.md)

---

## Overview

WEST implements control loops by connecting:
1. A **sensor** (or direct block output) → controller input (`y_m`)
2. A **controller block** → computes control signal `u`
3. Controller output `u` → manipulated variable of the process block (e.g. `kLa`)

This is wired through **Interface Links** on each connection.

---

## Available controller types

From the WEST Models Guide (Controllers category):

| Model | Type | Description |
|---|---|---|
| `Controllers.PI_Saturation` | PI | PI with anti-windup saturation |
| `Controllers.PID_Saturation` | PID | PID with saturation |
| `Controllers.PID_SaturationAW` | PID | PID with anti-windup |
| `Controllers.OnOff` | On/Off | Simple bang-bang control |
| `Controllers.OnOff_Band` | On/Off | On/Off with dead-band |
| `Controllers.Ratio` | Ratio | Output = Gain × input |
| `ControllerOp.PID_SRT` | SRT | Sludge retention time controller |

---

## Step-by-step: DO control loop

The TwoASU example uses a PI controller to maintain dissolved oxygen (DO) in the aerobic tank:

1. In the Block Library, find **PI controller** (Control category) and drag it onto the Layout.
2. Draw a connection from the aerobic tank's **DO sensor terminal** to the controller's input terminal.
3. Draw a connection from the controller's output terminal to the aerobic tank's **aeration data terminal** (`kLa` input).
4. Double-click the first connection → **Interface Link** dialogue → map `S_O` (measured DO) → `y_m`.
5. Double-click the second connection → **Interface Link** dialogue → map `u` → `kLa`.
6. In the controller's Block Details → Parameters: set `y_S` (set-point), `K_P` (gain), `T_I` (integral time).

> **Needs content.** Add parameter values for DO control from Tutorial ch 7 and expand from User Guide 5.2.15.

---

## Using a slider for manual control

For interactive set-point adjustment during a simulation, drag a manipulated variable (e.g. `y_S`) from Block Details onto a Dashboard Sheet. WEST creates a Slider widget.

---

## Related

- [Running Simulations](running-simulations.md)
- [Controllers & Timers reference](../block-reference/controllers-timers.md)
- [Quick Start Tutorial](../getting-started/quick-start.md)
