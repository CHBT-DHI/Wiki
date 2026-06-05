---
tags:
  - how-to
  - licensing
---

# WEST Product Editions

Overview of WEST licence tiers and which features are available in each.

**Source**: WEST Getting Started Tutorial ch 2.

---

### Edition comparison

| Feature | WEST Basic | WEST | WEST+ | WEST SDK |
|---|---|---|---|---|
| Steady-state simulation | ✓ | ✓ | ✓ | ✓ |
| Dynamic simulation | — | ✓ | ✓ | ✓ |
| Scenario Analysis | — | — | ✓ | ✓ |
| Sensitivity Analysis (LSA + GSA) | — | — | ✓ | ✓ |
| Parameter Estimation | — | — | ✓ | ✓ |
| Uncertainty Analysis | — | — | ✓ | ✓ |
| Model Editor (custom models) | — | ✓ | ✓ | ✓ |
| Automation API (COM/SDK) | — | — | — | ✓ |
| WEST Player (view-only) | ✓ | ✓ | ✓ | ✓ |

---

### WEST Basic

WEST Basic is the entry-level edition aimed at students and users who need to run pre-built plant models without modifying the underlying process models. It includes the full layout editor, all standard block types, and the Dynamic and Steady State experiment types. WEST Basic does not include the Model Editor, so users cannot create or modify process models; they are limited to the block library shipped with the product. Licensing is typically individual (per-seat) and available at an educational price tier.

---

### WEST

The standard WEST edition is suitable for practising engineers carrying out routine plant modelling, calibration, and scenario analysis. It includes everything in WEST Basic plus the Model Editor (for creating and modifying process models), Sensitivity Analysis and Parameter Estimation experiment types, and the Influent Tool for influent characterisation. Most consulting and utility modellers use this edition for day-to-day work.

---

### WEST+

WEST+ adds advanced modelling capabilities on top of the standard edition: the full ADM1-based plant-wide modelling (PWM) framework, the Uncertainty Analysis experiment type, advanced biofilm and granular sludge models, and the integrated urban water system (IUWS) module for catchment-to-receiving-water modelling. It is suited to research groups and advanced consulting work requiring full plant-wide mass and energy balances. Required experiment types for calibration workflows and uncertainty quantification — including **Sensitivity Analysis** (LSA and GSA), **Parameter Estimation**, **Scenario Analysis**, and **Uncertainty Analysis** — are all included.

---

### WEST SDK

The WEST Software Development Kit edition includes everything in WEST+ plus the Python automation API (WESTforAUTOMATION), batch workflow tools, and full source access to the model library for developing and distributing custom model packages. The SDK is used by model developers, software integrators, and large utilities that need programmatic control of simulations in automated workflows or digital-twin applications. It exposes a COM interface accessible from Python, MATLAB, R, and VBA, enabling integration with external optimisation tools and automated reporting pipelines. See [Automation & API](../automation-api/index.md) for usage examples.

---

### WEST Player

Free view-only mode — open and inspect existing WEST projects without a full licence. Cannot run simulations or edit models.

---

### Upgrading

Contact DHI Sales to upgrade between editions. Licence files are delivered via the DHI Customer Portal and activated through the WEST Licence Manager.

---

### Related

- [Installation & Licensing](installation.md)
