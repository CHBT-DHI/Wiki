---
tags:
  - getting-started
  - tutorial
---

# Quick Start Tutorial

**Summary:** Build and run a Modified Ludzack-Ettinger (MLE) activated sludge model — two tanks, a clarifier, and a PI controller — in WEST from scratch. Estimated time: 45–60 minutes.

**Prerequisites:**

- WEST is installed and licensed. See [Installation](../how-to/installation.md) if not.
- No prior WEST experience required.

---

## What you will build

The **TwoASU** sample project: a standard MLE layout for nitrification–denitrification, consisting of an anoxic tank (2 000 m³), an aerobic tank (4 000 m³), a secondary clarifier (Takács model), internal recycle, sludge wastage, and a PI controller maintaining a dissolved-oxygen set-point. By the end you will have run a steady-state simulation and examined effluent quality results.

---

## Step 1 — Create a new project

1. Open WEST.
2. Click the **Application Menu** (top-left) and choose **New**.
3. In the **New Project** dialogue, select the **Templates** tab and choose **Create blank Project**.
4. Enter the name `TwoASU` and pick a save location.
5. Under **Instance**, select `ASM1Temp` (ASM1 with temperature correction — the standard municipal model).
6. Click **OK**. WEST creates the project and opens an empty Layout Editor.

!!! tip
    The **Instance** sets which biological model library is used by every bioreactor block in this project. You can change it later via **Project | Properties**.

---

## Step 2 — Set up the plant layout

The Layout Editor works like a drag-and-drop flowsheet. Open the **Block Library** panel on the left.

Drag the following blocks onto the canvas and arrange them roughly left-to-right:

| Block name | Library location | Role |
|---|---|---|
| Municipal wastewater | Influent | Influent generator |
| Three combiner | Flow | Mixes primary + recycles |
| Activated sludge unit (×2) | Biological | Anoxic tank, aerobic tank |
| Two flow splitter (×2) | Flow | Internal recycle split, wastage split |
| Secondary clarifier | Settling | Takács 1-D clarifier |
| PI controller | Control | DO control |
| Loop breaker (×2) | Utilities | Breaks algebraic loops in recycle streams |
| Effluent (×2) | Outlet | Treated effluent, wasted sludge |

**Connect the blocks** by hovering over an output terminal (blue triangle) and dragging to an input terminal (green triangle). The flow path is:

```
Influent → Combiner → Anoxic tank → Aerobic tank → Clarifier → Effluent
                ↑                                        |
          Loop breaker ← Internal recycle splitter ←────┘
                                                         |
                                                 Wastage splitter → Effluent (sludge)
```

The PI controller connects: sensor output from the aerobic tank's DO terminal → controller input; controller output → aerobic tank's `kLa` input terminal.

---

## Step 3 — Create the influent input file

Double-click the **Municipal wastewater** block to open the **Influent Generator** dialogue.

1. On the **General** tab, set the fractionation layout file to `WEST.ASM1.Input.Layout.xml`.
2. On the **Data Import** tab, select `WEST.BODCOD.Month.Influent.txt` as the data source. This file contains one month of typical municipal influent data.
3. Click the **Generate** button to produce the steady-state input file. WEST writes `Influent_Municipality_1.Steady State.in.txt` to the project folder.
4. Click **OK**.

---

## Step 4 — Initialise the model

Set the key design parameters for each block. Double-click a block to open its **Block Details** pane, then edit the **Parameters** tab.

**Anoxic tank (Activated sludge unit 1)**

| Parameter | Value | Unit |
|---|---|---|
| Volume (`V`) | 2 000 | m³ |
| Temperature (`Temp`) | 20 | °C |

**Aerobic tank (Activated sludge unit 2)**

| Parameter | Value | Unit |
|---|---|---|
| Volume (`V`) | 4 000 | m³ |
| Temperature (`Temp`) | 20 | °C |
| DO set-point (`y_S`) | 1.5 | mg O₂/l |

**Secondary clarifier**

| Parameter | Value | Unit |
|---|---|---|
| Underflow (`Q_Under`) | 18 831 | m³/d |

**Internal recycle splitter**

| Parameter | Value | Unit |
|---|---|---|
| Recycle flow (`Q_Out2`) | 55 338 | m³/d |

**Wastage splitter**

| Parameter | Value | Unit |
|---|---|---|
| Wastage flow (`Q_Out2`) | 385 | m³/d |

---

## Step 5 — Set up the effluent output

Double-click the **Effluent** block (treated water) to open the **Effluent Generator** dialogue.

1. On the **General** tab, set the defractionation layout file to `WEST.ASM1.Output.Layout.xml`.
2. Click **OK**. This tells WEST how to map ASM1 state variables to reportable effluent quality parameters (COD, TKN, TSS, etc.).

---

## Step 6 — Prepare output sheets

WEST displays results on **Sheets** — tabbed panels containing plots and tables.

1. Right-click the **Sheets** area (bottom panel) and choose **Add Sheet**. Name it `ASU Tanks`.
2. In the **Model Explorer**, expand the aerobic tank node. Drag the variables `S_O` (dissolved oxygen) and `S_NH` (ammonia) onto the sheet. WEST creates a time-series plot automatically.
3. Add another sheet called `Effluent`. From the effluent block's **Block Details**, drag `COD`, `TKN`, `TSS`, `Outflow(S_NO)`, and `Outflow(S_NH)` onto the sheet.

---

## Step 7 — Run a steady-state simulation

1. Open the **Control Center** (toolbar or **Project** menu).
2. Select the **Steady-state** tab.
3. Click **Start**.

WEST iterates to a steady-state solution using the Newton solver. A green status bar indicates convergence. This usually takes less than 30 seconds.

Once complete, the sheet plots show flat horizontal lines — these are the steady-state concentrations.

!!! note "Typical steady-state effluent results"
    For the TwoASU sample with default parameters, expect approximately:
    COD ≈ 46 mg/l · NHx ≈ 7.6 mg/l · NOx ≈ 6.5 mg/l · TSS ≈ 15.9 mg/l

---

## Step 8 — Run a dynamic simulation

1. In the Control Center, switch to the **Dynamic** tab.
2. Set the simulation duration to **30 days**.
3. Click **Start**.

The plots update in real-time as the simulation progresses.

**Improving speed:** The default integrator is `RK4ASC`. For this stiff system, switching to `VODE` gives a 3–4× speed improvement:

- Go to **Project | Simulation Properties → Solver** tab.
- Change **Integrator** to `VODE`, confirm **Is a Stiff Solver** is checked.
- Re-run the dynamic simulation.

---

## Step 9 — View and interpret results

Open the **Effluent** sheet. The plots show the time-varying effluent concentrations driven by the monthly influent variation.

To read a specific value, right-click a plot and choose **Crosshairs**. Hover over any point to see its exact value and time.

**Adding compliance objectives:** You can compute statistics (mean, percentiles, % time above a limit) over the simulation period:

1. Select the dynamic experiment in the Control Center.
2. Go to **Project | Properties → Analysis** button.
3. From the effluent block's Block Details, drag the variables COD, TKN, TSS, `Outflow(S_NO)`, `Outflow(S_NH)` onto the Analysis Properties dialogue.
4. On the **Time series Criteria** tab, enable **Upper Percentile** and **Percentage of Time in Violation** for ammonia (upper bound 5 mg/l) and nitrate (upper bound 10 mg/l).
5. Re-run the simulation. Results appear on the **Runs** tab.

---

## What next?

- Run a parameter study: [Advanced Simulations](../experiment-types/index.md)
- Add a controller: [Controllers](../how-to/controllers.md)
- Understand the biological model used: [Process Models](../block-reference/biological-models.md)
- Automate this workflow: [Getting Started with the API](../automation-api/getting-started.md)
