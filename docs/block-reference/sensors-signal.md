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
