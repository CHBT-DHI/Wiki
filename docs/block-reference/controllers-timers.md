---
tags:
  - block-reference
  - controllers
  - timers
---

# Controllers & Timers

**Summary:** Controller block types available in the WEST model library.

**Source:** WEST Models Guide — Controllers (pp. 345–363), Timers (pp. 364–378).

---

## Standard controllers

| Model | Type | Notes |
|---|---|---|
| `Controllers.P_Saturation` | P | Proportional only |
| `Controllers.PI_Saturation` | PI | PI with anti-windup saturation — recommended for most loops |
| `Controllers.PI05_Saturation` | PI | PI with 0.5× output scaling |
| `Controllers.PID_Saturation` | PID | Full PID with saturation |
| `Controllers.PID_SaturationAW` | PID | Anti-windup PID |
| `Controllers.OnOff` | On/Off | Simple bang-bang |
| `Controllers.OnOff_Band` | On/Off with dead-band | |
| `Controllers.Ratio` | Ratio | Output = K × input |
| `Ratio2` | Ratio | Two-input ratio |

### PID controller

![PID controller — description](../assets/images/modelica-p266-img1.png)

![PID controller — parameters table](../assets/images/modelica-p272-img1.png)

### On/Off controller

![On/Off controller — description](../assets/images/modelica-p278-img1.png)

---

## Operational controllers

| Model | Purpose |
|---|---|
| `ControllerOp.OnOff11` | On/Off with 11 operating points |
| `ControllerOp.PI11` | PI with 11 operating points |
| `ControllerOp.PID11` | PID with 11 operating points |
| `ControllerOp.PID11_aw` | PID with anti-windup and 11 points |
| `ControllerOp.PID_SRT` | Sludge retention time controller |
| `ControllerOp.Ratio` | Ratio controller |

---

## Pump and blower controllers

| Model | Controls |
|---|---|
| `Separate10_CFQThrottle` | 10 centrifugal pumps, throttle |
| `Separate10_CFQVFD` | 10 centrifugal pumps, VFD |
| `Common10_CFHQVFD` | Common header, 10 centrifugal pumps |
| `Common10_CFHNVFD` | Common header, speed control |
| `BlowerControllers.Common10_CFHQVFD` | 10 blowers, H-Q control |
| `BlowerControllers.Common10_PDHQVFD` | 10 positive displacement blowers |

---

## Timers

Timers generate a periodic signal for time-based control (e.g. SBR phase sequencing).

![Timer block — description and parameters](../assets/images/modelica-p282-img1.png)

| Model | Period outputs |
|---|---|
| `Timer21–Timer22` | 2-phase, 1 or 2 signals |
| `Timer31–Timer32` | 3-phase |
| `Timer41–Timer42` | 4-phase |
| `Timer51–Timer82` | 5–8 phase timers |

---

## Related

- [Controllers how-to](../how-to/controllers.md)
- [Activated Sludge Tanks](activated-sludge-tanks.md)
