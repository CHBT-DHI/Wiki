---
tags:
  - how-to
  - layout
---

# Building a Plant Layout

**Summary:** How to create, arrange, and connect process blocks on the Layout Sheet.

**Source:** WEST User Guide, Chapter 5.2.

**Prerequisites:** [The WEST Interface](../getting-started/interface-overview.md)

---

## Placing blocks

1. Open the **Block Library** panel.
2. Drag a block onto the **Layout Sheet**.
3. Rename it by double-clicking its label (or via Properties).

> **Needs content.** Describe block layers, resize, rotate, flip options (User Guide 5.2.2–5.2.6).

---

## Connecting blocks

Click an output terminal (blue triangle) and drag to an input terminal (green triangle). A connection is drawn automatically.

- Connections carry the **wastewater vector** (all state variables) between blocks.
- To inspect a connection's properties, double-click it.

> **Needs content.** Expand from User Guide 5.2.9, including bend points and routing options.

---

## Changing a block's model

To swap the underlying process model without redrawing the layout:

1. Right-click the block → **Replace Block (and underlying model)**.
2. Select the new model from the library.

> **Needs content.** From User Guide 5.2.11.

---

## Defining top-level parameters

Parameters shared across multiple blocks can be promoted to project level:

1. Right-click the Layout → **Top-level Parameters**.
2. Add a new row: Name, Value, Unit, Description.
3. In any block's Parameters tab, reference the top-level parameter by name.

> **Needs content.** From User Guide 5.2.12.

---

## Setting up a controller

> **Needs content.** From User Guide 5.2.15. Cross-reference [Controllers](controllers.md).

---

## Related

- [Running Simulations](running-simulations.md)
- [Controllers](controllers.md)
- [Quick Start Tutorial](../getting-started/quick-start.md)
