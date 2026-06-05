---
tags:
  - getting-started
  - interface
---

# The WEST Interface

**Summary:** An orientation to the WEST graphical environment — windows, panels, sheets, and menus.

**Source:** WEST User Guide, Chapter 2.

---

## The WEST environment

The WEST interface consists of:

| Element | Purpose |
|---|---|
| **Getting Started Sheet** | Shown at start-up; browse Projects, Samples, Templates, Tutorials, Generators |
| **Layout Sheet** | The drawing canvas where blocks are placed and connected |
| **Dashboard Sheets** | Contain input widgets (sliders, fields) and output widgets (plots, tables, gauges) |
| **Interface Sheets** | Generate input files (influent fractionation) and output defractionation models |
| **System Windows** | Dockable panels: Block Library, Control Center, Model Explorer, Block Details, Properties, Logging, Layers, Overview |
| **Application Menu** | New, Open, Save, WEST Options — accessed via the top-left button |
| **Menu Bar** | Home, Insert, Project, View, Tools, Format tabs |
| **Status Bar** | Current experiment type, mode, zoom |

> **Needs content.** Add screenshots of each area and detailed panel descriptions from User Guide ch 2.

---

## System windows

### Block Library
The palette of all available blocks, organised by category. Drag blocks from here onto the Layout Sheet to build a plant layout.

### Control Center
The run panel. Select experiment type (Steady-state or Dynamic), configure and start/stop simulations, view run progress.

### Model Explorer
A tree view of the entire plant model. Navigate from project root → blocks → sub-models → variables. Drag variables from here onto a Sheet to plot them.

### Block Details
Shows the **Parameters** and **Variables** of the selected block. Edit parameters here before a run; read variable values after a run.

### Properties Window
Shows and edits graphical properties of the selected layout object (position, size, colour, label font).

### Logging Window
Shows solver messages, warnings, and errors during a simulation.

---

## Sheets

A project can have one **Layout Sheet** and multiple **Dashboard Sheets** and **Interface Sheets**.

- Add a Sheet: **Insert → Sheets Group → Add Sheet**
- Add a Plot: drag a variable from Model Explorer or Block Details onto a Dashboard Sheet
- Add a Slider: drag a manipulated variable from Block Details onto a Dashboard Sheet — WEST creates a slider widget

---

## Key menus

| Menu | Relevant items |
|---|---|
| **Insert → Input Group** | Add Influent Generator, Data Input, Vector Input |
| **Insert → Output Group** | Add Effluent Generator, Data Output, Vector Output |
| **Project → Properties Group** | Simulation Properties (solver settings), Analysis (objectives) |
| **Project → Virtual Experiments** | Add Steady-state, Dynamic, LSA, GSA, Parameter Estimation, Scenario, Uncertainty experiments |
| **Project → Miscellaneous** | Calculator Variables, Reports |
| **View → Windows** | Show/hide any system window |

---

## Related

- [Quick Start Tutorial](quick-start.md)
- [Building a Plant Layout](../how-to/building-layouts.md)
- [Running Simulations](../how-to/running-simulations.md)
