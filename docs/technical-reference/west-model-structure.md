---
tags:
  - technical-reference
  - model-structure
---

# The WEST Model Structure

How WEST organises the mathematical model — input model, plant model, output model, flattening, and top-level parameters.

## Three-layer model architecture

Every WEST project has three model layers that transform influent data into simulation results:

```
Influent data → [Input Model] → ASM state variables → [Plant Model] → ASM state variables → [Output Model] → Bulk effluent parameters
```

---

## Input model (fractionation)

The **Input Model** converts bulk wastewater parameters (COD, TKN, TSS, flow) into the state variable vector required by the biological model.

- Defined by the **Fractionation** tab of the Influent block
- Applies COD fractionation ratios (S_S/COD, X_S/COD, etc.)
- Converts total TKN into S_NH, S_ND, X_ND
- Outputs a complete ASM component vector at each time step

See [Influent Characterisation](influent-characterisation.md) for details.

---

## Plant model

The **Plant Model** is the set of differential and algebraic equations describing all process units and their connections. It consists of:

### Flattened model

When WEST compiles a project, it "flattens" the hierarchical block structure into a single system of equations. Each block contributes its state equations; connections define mass flux between blocks. The flattened model is what the ODE solver integrates.

### Parameters

Model parameters are constants in the equations (kinetic rates, volumes, etc.). They can be edited via block properties or promoted to Top-Level Parameters.

### Top-level parameters

Parameters promoted to project level are accessible without navigating into each block. They appear in the **Properties Window** and in the parameter tree of sensitivity analysis and parameter estimation experiments. Promote a parameter via: right-click a parameter in Block Properties → **Promote to Top-Level**.

### Top-level interface variables

Variables promoted to the project level (e.g. key effluent concentrations, DO set points) for easy access in dashboards and experiments.

### Calculator variables

User-defined expressions over model variables. See [Calculator Variables](../how-to/calculator-variables.md).

---

## Output model (de-fractionation)

The **Output Model** converts ASM state variables at the effluent point back to measurable bulk parameters (COD, BOD, TSS, TN, TP). Defined by the **De-fractionation** tab of the Effluent Output block. See [Effluent De-fractionation](../how-to/effluent-defractionation.md).

---

## Model compilation

When you first run a project (or after changing the model), WEST compiles the Modelica equations to optimised C++ code using the Visual Studio Build Tools. The compiled model is cached; subsequent runs reuse the cache unless the model changes. Compilation takes 5–30 seconds depending on model complexity.

---

## Related

- [Process Models](process-models.md)
- [Components and Processes](components-processes.md)
- [Calculator Variables](../how-to/calculator-variables.md)
