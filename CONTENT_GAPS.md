# Wiki Content Gaps — Work Queue

Generated 2026-06-05. Sections below need to be written or substantially expanded.
Priority: **High** = user-facing reference content | **Medium** = how-to / operational | **Low** = meta/contributing

---

## 1. BLOCK REFERENCE — Individual block documentation (HIGH)

These blocks appear only as a one-line table entry. Each needs its own subsection with: description, parameter table with typical values, typical use case, and any known limitations.

### `block-reference/sensors-signal.md`
Missing individual block entries for:
- `Sensors.NO3` — nitrate sensor (range, lag, typical calibration)
- `Sensors.TSS` / `Sensors.MLSS` — suspended solids sensor
- `Sensors.FlowMeter` — flow measurement block
- `Sensors.Temperature` — temperature sensor
- `Sensors.PO4` — phosphate sensor
- `Sensors.NH4_Ion` — ion-selective electrode variant
- `Data.Noise` section is only 29 words — expand with noise amplitude guidance

### `block-reference/advanced-processes.md`
- `## Anaerobic digestion (ADM1-based)` intro is 17 words — needs proper intro paragraph explaining when to use ADM1 vs simpler models, and the ASM–ADM1 interface requirement
- `## Membrane bioreactors (MBR)` intro is 19 words — expand with: when MBR is appropriate vs conventional clarifier, aeration strategy differences, MLSS ranges (8–15 g/L)
- Individual block entries needed for:
  - `MBR.SubmergedMembrane` — full parameter table (flux, TMP, fouling parameters, backwash cycle)
  - `MBR.SideStreamMembrane` — cross-flow velocity, energy premium vs submerged
  - `MBR.FoulingModel_Simple` — fouling rate constants, cleaning cycle parameters
  - `AGS_SBR` — SBR cycle configuration (fill/react/settle/draw times), granule size parameters
  - `MBBR_Aerobic` / `MBBR_Anoxic` — carrier type, fill fraction, typical removal rates
  - `IFAS` — suspended vs attached biomass ratio, carrier parameters
- `### Available blocks` (Tertiary/GAC) is 5 words — just a comma-separated list, no descriptions

### `block-reference/sludge-treatment.md`
- `ThickenerEfficiency` — only 40 words, needs parameter table (removal efficiency, TSS capture %)
- `## Aerobic digesters` — 65 words, no individual block entries; needs:
  - `AerobicDigester.CSTR` parameters (SRT, DO requirement, VSS reduction)
  - `AerobicDigester.AutothermalTAD` for thermophilic aerobic digestion
- `## Sludge drying` — 48 words, needs parameter table (drying temperature, energy per kg water removed)
- Missing block: `Dewatering.Centrifuge` / `Dewatering.BeltPress` — not documented at all

### `block-reference/controllers-timers.md`
- `## Operational controllers` — 99 words overview only, no individual block entries needed for:
  - `Controllers.Cascade` — cascade (outer/inner loop) configuration
  - `Controllers.FeedForward` — disturbance variable, gain tuning
  - `Controllers.Ratio` — ratio setpoint, use for RAS/influent pacing
- `## Pump and blower controllers` — 107 words overview, no individual entries for:
  - `ControllerOp.PID_SRT` — SRT control logic, WAS flow calculation

### `block-reference/flow-management.md`
Currently only category-level descriptions. Individual block entries needed for:
- `Mixer2`, `Mixer3`, `Mixer4` — when to use each, terminal layout
- `Splitter` vs `ControlledSplitter` — fixed ratio vs signal-driven, parameter table
- `SimplePump` vs `VariablePump` — parameter comparison (fixed Q vs head-flow curve)
- `LoopBreaker` — purpose (breaking algebraic loops), typical placement in recycle streams

### `block-reference/clarifiers.md`
- `PrimaryClarifier.BSM2` — only 36 words; needs parameter table and BSM2 context
- `SecondaryClarifier.BurgerDiehl30` — only 49 words; needs parameter table (30-layer model parameters)

### `block-reference/activated-sludge-tanks.md`
- `## VolumeConstant` intro is 35 words before sub-sections — adequate but could add a "When to use" note

### `block-reference/biological-models.md`
- `## ASM2dMod` section intro is 39 words — adequate
- `## ASM2dModNDHA` is 24 words — needs: what N₂O pathway is modelled, when emissions monitoring requires it
- `## ASM2dISS` intro is 20 words — adequate, subsections cover it
- `## PWM_SA` intro is 22 words — adequate, subsections cover it

---

## 2. HOW-TO GUIDES — Thin operational sections (MEDIUM)

### `how-to/installation.md`
- `## Licence configuration` — 18 words; needs: FlexNet vs CodeMeter vs dongle steps, grace period behaviour
- `## Visual Studio Build Tools` — 34 words; needs: exact download URL path, which components to select in installer (C++ build tools, Windows 10 SDK)

### `how-to/controllers.md`
- `## Step-by-step: DO control loop` intro is 32 words; subsections 3 and 4 are missing (steps jump from 2 to 5) — fill steps 3 (connect controller output to KLa) and 4 (set interface link)
- `## Using a slider for manual control` — 28 words; needs: how to add slider widget to a Sheet, how to set min/max range

### `how-to/results-and-output.md`
- `## Gauges` / `### Adding a gauge` — each ~23 words; needs: available gauge types (dial, bar, numeric), how to set alarm thresholds

### `how-to/west-product-editions.md`
- `### WEST Player` — 21 words; needs: what Player can and cannot do, who it is for (viewers, clients), how to export a Player file
- `### Upgrading` — 24 words; needs: upgrade path (Basic → WEST → WEST+), licence transfer process

### `how-to/effluent-defractionation.md`
- `### Viewing effluent quality` — 28 words; expand with: which output variables to check, typical acceptable fractionation check values

### `how-to/plot-customisation.md`
- `### Creating a plot` — 39 words (just 3 bullet steps); expand with axis labelling, legend placement, colour selection
- `### Saving and reusing plot layouts` — 31 words; expand with template file location, sharing templates across projects

### `how-to/building-layouts.md`
- `## Step 5 — Rotate a block` — 34 words; needs the actual keyboard shortcut (Ctrl+R or right-click menu path)
- `## Step 12 — Configure block parameters` — 22 words; expand with: opening Block Details, editing values, top-level parameters shortcut

### `how-to/running-simulations.md`
- `### Sliders` — 38 words; expand with: how to add slider, link to manipulated variable, set range limits

### `how-to/calculator-variables.md`
- `### Variable naming` — 39 words; add: naming constraints (no spaces, no reserved keywords, case-sensitive)
- `### Using calculator variables in experiments` — 34 words; expand with: using calc vars as PE calibration parameters, as SA sensitivity parameters

---

## 3. GETTING STARTED & GLOSSARY — Gaps (MEDIUM)

### `getting-started/glossary.md`
Thin letter sections (1–2 entries only):
- `## C` — 30 words; add: **Calculator variable**, **Clarifier**, **COD**, **Communication interval**
- `## L` — 18 words; add: **Layout**, **LSODA**, **Loop breaker**
- `## U` — 34 words; add: **Uncertainty Analysis**, **Unit editor**

### `getting-started/interface-overview.md`
- `## Results viewer` — 20 words; expand with: tabs (Plot / Table / Dashboard), variable tree, how to add variables to a plot

### `getting-started/system-windows.md`
- `## Overview` — 26 words; expand with: which windows are dockable, how to reset layout (View → Reset Layout)
- `## Properties Window` — 34 words; expand with: what metadata it shows (block name, model path, description), difference from Block Details

### `starter-kit/glossary.md`
- `## H`, `## L`, `## R`, `## V` — 18–36 words each; all need 2–3 additional entries

### `getting-started/quick-start.md` and `starter-kit/quick-start.md`
- `## What next?` — 29 words; expand with 3–5 concrete next-step links (calibration, SA, worked examples)

---

## 4. MANUALS — Thin sections (MEDIUM)

### `manuals/controllers.md`
- `## On/Off controller` — 21 words; needs parameter table (upper/lower threshold, on-value, off-value)
- `## Timer block` — 19 words; needs parameter table (start time, period, duty cycle, signal levels)
- `## Feed-forward controller` — 22 words; needs gain parameter, typical use (influent flow → aeration feed-forward)

### `manuals/results-output.md`
- `### Gauge widget` — 17 words; expand with gauge types, alarm threshold configuration
- `## Criteria types` — 16 words; expand with Time Series vs Run vs BVDF criterion definitions
- `### Time series criteria` — 27 words; expand with expression syntax examples
- `### Classification criteria` — 26 words; expand with pass/fail count example
- Export sections (CSV, Excel, runs matrix) — each ~28–36 words; could add column format details

### `manuals/advanced-simulations.md`
- `## Sensitivity analysis` — 26 words; expand with: LSA vs GSA choice guidance, minimum number of parameters
- `## Parameter estimation / calibration` — 8 words (just a cross-reference); either expand or remove the section entirely

### `manuals/worked-examples.md`
- Two `### Overview` sections — 38–39 words each (just under threshold); review and confirm adequate

### `manuals/installation.md`
- `### Floating windows disappear after monitor resolution change` — 22 words; needs fix steps
- `### WEST starts but Modelica compilation fails` — 24 words; needs compiler path fix steps

---

## 5. TECHNICAL REFERENCE — Gaps (MEDIUM)

### `technical-reference/process-models.md`
- All individual model sections (`## ASM1`, `## ASM2d`, `## ASM3`) have 1-line intros — adequate given the detailed sub-sections, but could add a "When to use this model" callout box at the top of each

### `technical-reference/process-units.md`
- `### Wastewater characterisation`, `### Time-varying influent`, `### Compartment-in-series shortcut`, `### Biological reactions`, `### State variables` — all 1-line entries before sub-sections; adequate but could be expanded if users report confusion

---

## 6. EXPERIMENT TYPES — Thin sections (MEDIUM)

### `experiment-types/parameter-estimation.md`
- `## Algorithm options` — 12 words intro + sub-sections; adequate
- `### Runs tab`, `### Error (convergence) plot`, `### Parameter trajectory plot` — each 1-line descriptions; could be expanded with what to look for / interpret

---

## 7. DEVELOPER TOPICS — Thin sections (LOW)

### `developer-topics/extending-west.md`
- `### Via the command line` — 7 words; needs actual CLI command syntax for registering a model
- `### Option C: Version-controlled repository` — 38 words; expand with Git workflow for model packages

### `developer-topics/custom-models.md`
- `### Process rate expressions` — 28 words; needs example Modelica expression syntax
- `### Stoichiometric coefficients` — 31 words; needs example Petersen matrix notation

---

## 8. AUTOMATION & API — Thin sections (LOW)

### `automation-api/batch-workflows.md`
- `### Saving to CSV` — 25 words; needs the actual pandas `to_csv` code snippet

### `automation-api/getting-started.md`
- `### 2. Install the Python COM client library` — 37 words; needs exact pip command and package name
- `## First script — connect and list scenarios` — 31 words; needs a complete runnable code example

---

## 9. CONTRIBUTING — Incomplete template (LOW)

### `contributing.md`
- `## Page template` — 14 words; needs the actual Markdown template (front matter + section structure) that contributors should copy
- `## Section heading` — 3 words (almost empty); needs guidance on H2 vs H3 usage
- `## Running the site locally` — 15 words; needs: `pip install mkdocs-material && mkdocs serve` commands

---

## Summary counts

| Priority | Area | Gaps |
|---|---|---|
| High | Block reference — individual block descriptions | ~25 missing block entries |
| Medium | How-to guides — thin operational sections | ~15 sections to expand |
| Medium | Getting Started / Glossary | ~8 sections to expand |
| Medium | Manuals — thin sections | ~12 sections to expand |
| Medium | Technical reference | ~5 sections (optional improvement) |
| Low | Developer topics / API / Contributing | ~8 sections |

**Total: ~73 items.** Most are expansions of existing stubs rather than entirely new pages.
