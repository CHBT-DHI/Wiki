---
tags:
  - worked-examples
  - plant-wide
---

# Plant-Wide Model

**Summary:** A full WWTP model including the liquid train and anaerobic sludge digestion, enabling resource and energy balance calculations.

**Source:** WEST Getting Started Tutorial, Chapter 14.

**Prerequisites:** [TwoASU MLE Layout](twoasu-mle.md)

---

## What a plant-wide model adds

Standard activated sludge models (ASM1, ASM2d) focus on the liquid train. The Plant-Wide Model (PWM_SA) instance additionally models:

- Primary clarifier
- Anaerobic digester (biogas production)
- Sludge thickening and dewatering
- Heat exchangers
- Energy balance (biogas utilisation, blower energy)

This enables answering questions like: "Is this plant energy-neutral?" or "What is the net carbon footprint?"

---

## Layout overview

A typical plant-wide model in WEST contains two interconnected trains — the **liquid train** and the **sludge train** — plus biogas and energy accounting blocks.

**Liquid train (left to right):**

```
Raw influent
  → Primary Clarifier (Settler.Primary)
      ├─ Primary effluent → Combiner → Anoxic tank(s) → Aerobic tank(s)
      │                                                        |
      │                              Internal recycle (IR) ←──┘
      │                                                        |
      └─ Primary sludge ─────────────────────────────→ Thickener
                                                               |
Aerobic tank → Secondary Clarifier (Settler.Secondary)        |
                    ├─ Clarified effluent → Effluent point     |
                    ├─ RAS → Combiner (return activated sludge)|
                    └─ WAS → Thickener ────────────────────────┘
```

**Sludge train (top to bottom):**

```
Primary sludge + WAS
  → Sludge Thickener (SludgeTreatment.Thickener)
      → Anaerobic Digester (SludgeTreatment.AnaerobicDigester_ADM1)
          ├─ Biogas → Biogas handling block (energy recovery)
          └─ Digested sludge → Dewatering (SludgeTreatment.Dewatering)
                                    ├─ Dewatered cake → disposal
                                    └─ Reject water (filtrate) → back to liquid train influent
```

**Key stream connections:**

| Stream | From | To |
|---|---|---|
| Primary sludge | Primary Clarifier (underflow) | Thickener (feed) |
| WAS | Secondary Clarifier (wastage splitter) | Thickener (feed) |
| Thickened sludge | Thickener (underflow) | Anaerobic Digester |
| Biogas | Anaerobic Digester | Biogas/CHP block |
| Reject water | Dewatering (filtrate) | Influent Combiner or dedicated reject tank |
| RAS | Secondary Clarifier (underflow) | Anoxic tank inlet via Combiner |

The reject water return is important — it recycles nutrients (especially NH4 and PO4) back into the liquid train and can represent a significant internal load. In some configurations a dedicated reject water treatment step (e.g. SHARON/ANAMMOX) is inserted before the return point.

---

## Setting up the PWM_SA instance

When creating a new project, select the **PWM_SA** instance instead of ASM1Temp. This bundles:
- ASM2dMod for the liquid train (biological N and P removal)
- ADM1-based anaerobic digestion for the sludge train

### 1. Create the project with the PWM_SA model instance

1. In the WEST Project Manager, click **New Project**.
2. In the **Model Instance** drop-down, select **PWM_SA** (not ASM1Temp or ASM2dTemp).
3. Name the project (e.g. `PlantWideModel`) and click **OK**.

The PWM_SA instance pre-loads the interfacing transformers that convert ASM2dMod state variables (liquid train) into ADM1 state variables (digester) and back, so you do not need to configure these manually.

### 2. Build the layout

Add blocks in the Layout Editor following the train structure described above. Minimum required blocks:

| Block type | WEST library path | Suggested name |
|---|---|---|
| Primary clarifier | `Settler.Primary` | `PC` |
| Anoxic reactor | `Reactor.CSTR` | `Anoxic` |
| Aerobic reactor | `Reactor.CSTR` | `Aerobic` |
| Secondary clarifier | `Settler.Secondary` | `SC` |
| RAS splitter/combiner | `FlowSplitter`, `Combiner` | `RAS_split`, `Combiner1` |
| WAS splitter | `FlowSplitter` | `WAS_split` |
| Sludge thickener | `SludgeTreatment.Thickener` | `Thickener` |
| Anaerobic digester | `SludgeTreatment.AnaerobicDigester_ADM1` | `Digester` |
| Dewatering | `SludgeTreatment.Dewatering` | `Dewatering` |
| Biogas handling | `Energy.BiogasStorage` or `Energy.CHP` | `Biogas` |

Connect the blocks with streams as described in the layout diagram above.

### 3. Configure activated sludge reactor parameters

Open each reactor's **Block Editor** (double-click the block) and set:

| Parameter | Anoxic tank | Aerobic tank | Notes |
|---|---|---|---|
| `Volume` | 3 000–5 000 m³ | 6 000–10 000 m³ | Size to target SRT |
| `Temperature` | 15 °C (default) | 15 °C | Link to influent temperature variable for dynamic runs |
| `DO_setpoint` | 0.1–0.3 mg O₂/l | 2.0 mg O₂/l | Aerobic DO controlled by PI controller |
| `kLa` | calculated by controller | — | Leave as controlled variable |

To target a specific **SRT**, use the formula: `SRT = (Σ VSS in all tanks + VSS in clarifier sludge blanket) / (WAS VSS per day)`. The WAS flow splitter ratio is the primary tuning handle.

Typical starting values for a 100 000 PE plant:
- Anoxic volume: 4 000 m³ (HRT ≈ 2 h at Q = 50 000 m³/d)
- Aerobic volume: 8 000 m³ (HRT ≈ 4 h)
- Target SRT: 12–18 days

### 4. Link influent characterisation to PWM_SA state variables

The PWM_SA instance uses ASM2dMod state variables. Your influent data file (or `DWF2` generator) must supply concentrations for the following key variables — use the **Influent Characterisation** tool (Tools → Influent Fractionation) if starting from standard COD/BOD/TSS measurements:

| ASM2dMod variable | Description | Typical municipal value |
|---|---|---|
| `Sf` | Readily biodegradable fermentable COD | 50–80 mg COD/l |
| `Sa` | Acetate (VFA) | 20–40 mg COD/l |
| `Xi` | Inert particulate COD | 30–60 mg COD/l |
| `Xs` | Slowly biodegradable particulate COD | 100–200 mg COD/l |
| `Xh` | Heterotrophic biomass | 20–40 mg COD/l |
| `SNH4` | Ammonium nitrogen | 25–45 mg N/l |
| `SPO4` | Soluble phosphate | 5–10 mg P/l |
| `TSS` | Total suspended solids | 150–250 mg/l |

In the influent block properties, map each column of the input timeseries to the corresponding state variable using the **Variable Mapping** tab.

### 5. Configure the anaerobic digester

In the `AnaerobicDigester_ADM1` block editor, key parameters are:

| Parameter | Typical value | Notes |
|---|---|---|
| `Volume` | 2 000–5 000 m³ | Size for 20–30 day HRT |
| `Temperature` | 35 °C | Mesophilic operation |
| `HRT_target` | 20 days | Used by auto-sizing wizard |
| `TSS_feed` | from thickener outlet | Connected automatically via stream |

The ADM1 transformer (built into PWM_SA) handles conversion of ASM2dMod particulates and solubles to ADM1 components; no manual mapping is required.

### 6. Run steady-state, then dynamic

- Run **Steady State** first to initialise all tanks (especially the digester, which converges slowly).
- Switch to **Dynamic** with a 365-day timeseries to assess seasonal variation and energy balance.
- Check the **Energy Summary** output block for blower energy demand vs. biogas CHP output.

---

## Related

- [Choosing a Biological Model](../block-reference/choosing-a-model.md)
- [Sludge Treatment](../block-reference/sludge-treatment.md)
- [Advanced Processes](../block-reference/advanced-processes.md)
