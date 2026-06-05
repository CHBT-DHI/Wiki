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

> **Needs content.** Describe the plant-wide layout blocks and connections from Tutorial ch 14 (pp. 95–100).

---

## Setting up the PWM_SA instance

When creating a new project, select the **PWM_SA** instance instead of ASM1Temp. This bundles:
- ASM2dMod for the liquid train (biological N and P removal)
- ADM1-based anaerobic digestion for the sludge train

> **Needs content.** Step-by-step from Tutorial ch 14.

---

## Related

- [Choosing a Biological Model](../block-reference/choosing-a-model.md)
- [Sludge Treatment](../block-reference/sludge-treatment.md)
- [Advanced Processes](../block-reference/advanced-processes.md)
