---
tags:
  - west-tools
  - model-editor
---

# Model Editor

**Summary:** The Model Editor allows writing, editing, and generating optimised code for WEST process models.

**Source:** WEST User Guide, Chapter 10.

---

## What the Model Editor provides

- **Code Editor**: syntax-aware editor for the MSL (WEST model specification language)
- **Matrix Editor (Gujer Matrix)**: graphical interface for defining biokinetic models as a stoichiometry/kinetics matrix — no code required
- **Code generation**: WEST automatically compiles model code to optimised C++ for maximum simulation speed

---

## The WEST model library

The model library is organised into:
- **Categories** — groups of related model variants (e.g. `ASM2dMod` category contains `VolumeConstant`, `VolumeVariable`, etc.)
- **Instances** — a category bundled with a temperature correction model (e.g. `ASM2dModTemp`)

---

## Creating a new model

> **Needs content.** From User Guide 10.3: creating a Block Library, creating an Instance, creating a Category, importing an existing Category, generating code.

Key steps:
1. Open Model Editor
2. Create or open a Block Library
3. Add a new Category (new model) or import an existing one
4. Edit in Code Editor or Matrix Editor
5. Generate code

---

## MSL language basics

> **Needs content.** Brief MSL syntax overview — variable declarations, process rates, Gujer matrix format.

---

## Related

- [Block Editor](block-editor.md)
- [Block Reference — Biological Models](../block-reference/biological-models.md)
- [Developer context](../contributing.md)
