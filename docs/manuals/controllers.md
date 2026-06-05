---
tags:
  - manuals
  - control
---

# Controllers

**Summary:** Adding and configuring controller blocks in WEST — connecting sensors to manipulated variables, understanding interface links, and working with top-level parameters.

**Prerequisites:** A plant layout with at least one process unit. See [Building a Plant Layout](plant-layout.md).

---

## What controllers do in WEST

Controllers adjust **manipulated variables** (e.g. airflow rate, recycle flow, dosing rate) in response to **sensor signals** (e.g. measured DO, NH4, NO3). They close the control loop between a measurement and an actuator without requiring manual parameter changes between runs.

Controllers connect to the rest of the model via **data terminals** (shown as red squares), which carry signal values. These are distinct from **mass flux terminals** (shown as blue arrows), which carry flows of material between process units.

---

## Connecting a controller

1. Drag a controller block from the component library onto the plant layout canvas.
2. Draw a connection line from the **sensor output terminal** (red square on the sensor block) to the **input terminal** of the controller.
3. Draw a second connection line from the **controller output terminal** to the **manipulated variable terminal** on the target process unit.
4. When you complete a connection, the **Interface Links** dialog opens automatically.

![Controllers — PID block on canvas](../assets/images/userguide-p067-img1.png)

![Controllers — interface links dialog](../assets/images/userguide-p067-img2.png)

---

## Interface Links dialog

The Interface Links dialog maps signals across the connection. Use the two dropdowns to specify:

| Field | What to select |
|---|---|
| **From** | The sensor output signal being passed to the controller (e.g. `DO_measured`, `NH4_effluent`). |
| **To** | The manipulated variable on the receiving block that will be driven by this signal (e.g. `kLa`, `Qr`, `Qdos`). |

Multiple links can be configured on a single connection if the blocks expose more than one signal. Click **OK** to confirm.

---

## Controller types overview

| Type | Description |
|---|---|
| **PID** | Proportional-Integral-Derivative controller. Drives the manipulated variable to maintain a setpoint. Configure Kp, Ki, Kd, output limits, and setpoint. |
| **On/Off** | Switches the manipulated variable between two fixed values based on threshold crossings. |
| **Feed-forward** | Adjusts the manipulated variable based on a measured disturbance rather than a feedback error. Often combined with PID. |

---

## PID controller

Configure PID controller parameters and review its response.

![PID controller parameter dialog](../assets/images/userguide-p068-img1.png)

![Controller response plot](../assets/images/userguide-p068-img2.png)

---

## On/Off controller

The On/Off controller switches the output between two fixed values when the measured signal crosses threshold bounds.

![On/Off controller setup](../assets/images/userguide-p069-img1.png)

---

## Timer block

Use a Timer block to generate scheduled on/off or step signals independent of sensor feedback.

![Timer block configuration](../assets/images/userguide-p070-img1.png)

---

## Feed-forward controller

A feed-forward controller adjusts the manipulated variable based on a measured disturbance before the effect reaches the controlled variable.

![Feed-forward controller](../assets/images/userguide-p071-img1.png)

---

## Parameters vs manipulated variables

WEST distinguishes between two types of adjustable values on a block:

| Type | Description |
|---|---|
| **Parameters** | Fixed values set before a run. Edited in the **Block Details** window via an Input Field or a Slider. Cannot receive live signals during a simulation. |
| **Manipulated variables** | Values that can change during a simulation. Can receive data from **Data Input** blocks (file-driven schedules), **controllers**, or **timers**. |

When building a control scheme, always connect to a manipulated variable — not a parameter — otherwise the controller signal will be ignored.

---

## Top-level parameters tip

**Top-level parameters** are shared parameter values that propagate to multiple blocks simultaneously. For example, a single `kLa_top` parameter can drive the aeration constant in every aeration zone, so changing one value updates all zones.

**Two ways to create a top-level parameter:**

- **Easy Create:** Right-click a parameter in Block Details and choose **Create Top-Level Parameter**. WEST generates the shared parameter and links the block automatically.
- **Manual:** Right-click on an empty area of the canvas and choose **Top-Level Parameters**, then add the parameter and manually assign it to blocks via their Block Details windows.

Top-level parameters appear in the top-level Block Details window and can be varied in Scenario Analysis and Parameter Estimation experiments like any other parameter.

---

## Related

- [Running Simulations](running-simulations.md)
- [Results and Output](results-output.md)
- [Scenario Analysis](../experiment-types/scenario-analysis.md)
