---
tags:
  - block-reference
  - iuws
  - river
---

# River and Receiving Water Blocks

Blocks for modelling rivers, streams, and receiving water bodies in integrated urban water system (IUWS) models.

**Source**: WEST Models Guide — IUWS section.

---

### Overview

River blocks model the quality and hydraulics of receiving water bodies downstream of WWTP effluent and CSO discharge points. They are used in IUWS models to assess the impact of wet-weather discharges on river water quality.

---

### Available river blocks

| Block | Description |
|---|---|
| `River.CSTRReach` | Single well-mixed river reach (CSTR assumption) |
| `River.PlugFlowReach` | Plug-flow river reach with dispersion |
| `River.Streeter_Phelps` | Classic DO sag model (BOD + reaeration) |
| `River.MultiReach` | Multiple river reaches in series |

---

### River.CSTRReach — parameters

| Parameter | Description | Typical value |
|---|---|---|
| `V` | Volume of reach | m³ (length × width × depth) |
| `Q_upstream` | Upstream boundary flow | m³/d |
| `kLa_river` | Reaeration rate coefficient | 1–5 /d (depends on depth and velocity) |
| `DO_sat` | Saturation DO at ambient temperature | 9–10 mg/L at 15°C |
| `T_river` | River water temperature | °C |
| `k_BOD` | BOD decay rate | 0.1–0.5 /d |

---

### Streeter-Phelps DO sub-model

The Streeter-Phelps model tracks the dissolved oxygen deficit in the river:

- **BOD exertion**: consumes DO at rate `k_BOD × BOD`
- **Reaeration**: replenishes DO at rate `kLa_river × (DO_sat - DO)`
- **DO sag**: minimum DO occurs at the critical distance downstream of the discharge point

The critical DO deficit D_c and critical time t_c are:

```
t_c = (1 / (k_r - k_d)) × ln[(k_r/k_d) × (1 - D_0(k_r-k_d)/(k_d×L_0))]
D_c = (k_d × L_0 / k_r) × exp(-k_d × t_c)
```

Where k_d = BOD decay rate, k_r = reaeration rate, L_0 = initial BOD, D_0 = initial deficit.

---

### Connecting river blocks to a WWTP model

1. Place a `River.CSTRReach` downstream of the WWTP **Effluent** block.
2. Connect the WWTP effluent outlet to the river reach inlet.
3. Add a second inlet for any CSO discharge points.
4. Set upstream boundary conditions (flow, DO, BOD, NH4).
5. Run a dynamic simulation with a rainfall event to observe the DO sag response.

See [Integrated Urban Water System](../worked-examples/iuws.md) for a complete example.

---

### Related

- [Worked Example: IUWS](../worked-examples/iuws.md)
- [Flow Management](flow-management.md)
