---
tags:
  - block-reference
  - sensors
---

# Sensors & Signal Treatment

**Summary:** Sensor and signal processing blocks for instrumentation and control.

**Source:** WEST Models Guide — Sensors (pp. 380–390), Signal Treatment (pp. 396–403), Samplers (pp. 404–410).

---

## Sensors

Sensors measure one or more state variables and output them as data signals for use by controllers or data loggers.

| Model | Measures |
|---|---|
| `ASM1.Multiprobe` | Multiple ASM1 variables (DO, NH4, NO3, TSS, etc.) |
| `ASM1.MultiSampler_kVol` | Volume-proportional sampler for ASM1 |
| `ASM2dISS.Multiprobe` | Multiple ASM2dISS variables |
| `ASM2dISS.MultiSampler_kVol` | Volume-proportional sampler for ASM2dISS |
| `ASM2dMod.Multiprobe` | Multiple ASM2dMod variables |
| `ASM2dMod.MultiSampler_kVol` | Volume-proportional sampler for ASM2dMod |
| `PWM_SA.Multiprobe` | Plant-wide model probe |

### DO Sensor

![DO Sensor — description](../assets/images/modelica-p254-img1.png)

![DO Sensor — parameters](../assets/images/modelica-p254-img2.png)

### NH4 Sensor

![NH4 Sensor — description and parameters](../assets/images/modelica-p255-img1.png)

---

## Signal treatment

Signal treatment blocks modify a data signal before it reaches a controller — adding noise, delay, saturation, or sample-and-hold behaviour. Used to simulate real sensor imperfections.

| Model | Effect |
|---|---|
| `Data.Delay` | Adds a time delay to the signal |
| `Data.Noise` | Adds Gaussian noise |
| `Data.NoiseFromFile` | Adds noise from a file |
| `Data.ResponseTime` | First-order lag (sensor response time) |
| `Data.SampleHold` | Sample-and-hold at fixed interval |
| `Data.Saturation` | Clips signal to min/max range |

---

## Signal treatment blocks

### Data.Delay

![Data.Delay block — introduces delay between input and output signal](../assets/images/modelica-p397-img1.png)

Introduces a fixed time delay between the input and output signal. Use to simulate measurement lag or transport delay in a pipe or sample line.

### Data.Noise

![Data.Noise block — adds random noise to signal](../assets/images/modelica-p398-img1.png)

Adds Gaussian random noise to the input signal, simulating sensor measurement error or electrical interference. Configure amplitude and seed for reproducibility.

### Data.ResponseTime

![Data.ResponseTime — dynamic response behaviour (first-order filter)](../assets/images/modelica-p400-img1.png)

Applies a first-order dynamic filter to the input signal, mimicking the finite response time of a physical sensor (e.g. a DO probe with a membrane). Specify the time constant (s) to control how quickly the output tracks a step change.

### Data.SampleHold

![Data.SampleHold — discontinuous/sampled signal](../assets/images/modelica-p402-img1.png)

Samples the input signal at a fixed interval and holds the value until the next sample, producing a staircase (zero-order hold) output. Useful for simulating PLC-based or SCADA-polled measurements with a finite scan rate.

### Data.Saturation

![Data.Saturation — limits output signal range](../assets/images/modelica-p403-img1.png)

Clips the output signal to a defined minimum and maximum range, simulating sensor range limits (e.g. a DO probe that reads 0–20 mg O₂/l). Values outside the range are clamped at the boundary.

---

## Samplers

Samplers aggregate a signal over a time or volume interval, simulating composite sampling.

| Model | Method |
|---|---|
| `Grab` | Instantaneous grab sample |
| `kTime` | Time-proportional composite |
| `kVolume` | Volume-proportional composite |

---

## Related

- [Controllers & Timers](controllers-timers.md)
- [Controllers how-to](../how-to/controllers.md)
