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

Entry-level edition for steady-state plant sizing and design checks. Suitable for:
- Preliminary design calculations
- Training and education
- Users who do not need dynamic simulation

---

### WEST

The standard edition for dynamic simulation of WWTP models. Includes:
- Full dynamic integration (multiple solver options)
- Model Editor for custom Modelica models
- All block types (biological, clarifiers, sludge treatment, etc.)
- Dashboard and reporting tools

---

### WEST+

Adds advanced experiment types to WEST:
- **Sensitivity Analysis** (LSA and GSA) — see [Sensitivity Analysis](../experiment-types/sensitivity-analysis.md)
- **Parameter Estimation** — see [Parameter Estimation](../experiment-types/parameter-estimation.md)
- **Scenario Analysis** — see [Scenario Analysis](../experiment-types/scenario-analysis.md)
- **Uncertainty Analysis** — see [Uncertainty Analysis](../experiment-types/uncertainty-analysis.md)

Required for calibration workflows and uncertainty quantification.

---

### WEST SDK

Adds programmatic access via the WEST Automation API:
- COM interface for Python, MATLAB, R, VBA
- Batch workflow automation
- Integration with external optimisation tools
- See [Automation & API](../automation-api/index.md)

---

### WEST Player

Free view-only mode — open and inspect existing WEST projects without a full licence. Cannot run simulations or edit models.

---

### Upgrading

Contact DHI Sales to upgrade between editions. Licence files are delivered via the DHI Customer Portal and activated through the WEST Licence Manager.

---

### Related

- [Installation & Licensing](installation.md)
