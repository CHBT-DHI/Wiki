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

The Control Center is the primary panel for managing and running simulations. It is divided into three main areas: the **Experiment selector** (top) where you choose the experiment type (Steady State, Dynamic, Sensitivity Analysis, etc.); the **Parameter panel** (middle) where you set experiment-specific settings such as simulation period, solver tolerances, and output configuration; and the **Run controls** (bottom) with Run, Pause, Stop, and Reset buttons. The status bar at the bottom of the Control Center shows the current simulation time, residual (for steady-state), and elapsed wall-clock time. During a dynamic run, a progress bar fills as simulation time advances toward the end time. You can also access **Experiment Properties** directly from the Control Center to adjust solver settings or output frequency without navigating the main menu.

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

The Logging Window records all simulation events, warnings, and errors in chronological order. Each entry has a timestamp, severity level (Info, Warning, Error), and a message. Info messages report normal progress (solver steps, run completion). Warning messages flag potential issues that did not stop the simulation (e.g. a variable hitting its bounds). Error messages indicate failures that stopped the run — always read these first when a simulation does not complete. The log can be cleared with the **Clear** button or saved to a text file with **Save Log**.

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
