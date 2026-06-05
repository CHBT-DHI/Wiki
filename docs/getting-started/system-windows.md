---
tags:
  - getting-started
  - interface
---

# System Windows

Reference for all dockable system windows in the WEST interface.

## Overview

System windows are dockable panels that can be shown or hidden via **View → Windows**. They provide information about the current project and model state.

---

## Block Library

The Block Library lists all available process blocks organized in palette groups (Input and Output, Biological, Clarifiers, Aeration, Flow, Controllers, Sensors, etc.).

- **Drag** a block from the library onto the Layout Sheet to place it.
- Use the **search box** at the top to filter by name.
- Right-click a block in the library → **Properties** to see a description before placing.
- Custom block libraries appear here once registered (see [Model Editor](../west-tools/model-editor.md)).

---

## Control Center

The Control Center shows the current simulation state: running, stopped, or initialised. It provides:

- **Run/Stop** buttons for quick simulation control
- **Progress bar** during a simulation run
- **Elapsed time** and estimated time remaining
- **Current simulation time** (for dynamic runs)
- Access to **Experiment Properties** for the active experiment

---

## Block Summary

Displays a summary table of all blocks in the current layout:

| Column | Description |
|---|---|
| Name | Block name |
| Model | Active model selection (e.g. `ASM2dMod`) |
| Category | Palette group |
| Layer | Assigned layer |
| Locked | Whether the block is locked from editing |

Use this to quickly audit what models are in use, rename blocks in bulk, or check which layer each block is assigned to.

---

## Model Explorer

The Model Explorer is the primary tool for browsing simulation variables. It shows a tree of all blocks and their variables:

- **Parameters**: input parameters (editable before run)
- **State variables**: dynamic variables tracked by ODE solvers
- **Interface variables**: inlet/outlet connector values
- **Calculator variables**: user-defined expressions
- **Top-level parameters**: parameters promoted to the project level

**Key actions:**
- Drag a variable from the Model Explorer onto a sheet to create a plot.
- Right-click → **Copy Variable Path** to use in expressions or Data Output blocks.
- Use **Ctrl+F** to search for a variable by name.
- Enable **Show all variables** to see internal variables not normally displayed.

---

## Block Details

Shows detailed information for the currently selected block:

- Full parameter table (editable)
- State variable initial conditions
- Interface variable current values (updates during a live run)
- Model description text

This is an alternative to double-clicking the block — useful when comparing multiple blocks side-by-side.

---

## Properties Window

A compact version of the block properties. Displays the selected object's most important attributes. Updates immediately when you click a different block. Useful for quick parameter inspection without opening the full properties dialog.

---

## Logging Window

Shows all WEST messages including:

- Simulation start/stop events
- Convergence warnings
- Compiler messages (when generating Modelica code)
- Error messages with line references

**Tip**: When a simulation fails, check the Logging Window first — it usually shows the exact variable or block that caused the issue.

| Message type | Colour |
|---|---|
| Info | Black |
| Warning | Orange |
| Error | Red |
| Debug (if enabled) | Grey |

---

## Layers Window

Manage visibility layers for the layout canvas. Blocks can be assigned to layers to organise complex layouts:

- Toggle layer visibility with the eye icon
- Lock a layer to prevent accidental editing
- Rename layers (e.g. "Liquid train", "Sludge train", "Controllers")
- Move all blocks on a layer together

---

## Overview Window

Shows a thumbnail of the entire layout canvas. A blue rectangle indicates the currently visible area. Drag the rectangle to pan the canvas, or click anywhere in the thumbnail to jump to that location. Useful for large plant-wide models that don't fit on screen.

---

## Related

- [The WEST Interface](interface-overview.md)
- [Building a Plant Layout](../how-to/building-layouts.md)
