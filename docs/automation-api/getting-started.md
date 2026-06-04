---
tags:
  - automation-api
---

# Getting Started with the API

**Summary:** Prerequisites, installation, and a first working script that connects to WEST.

**Prerequisites:**

- WEST installed and licensed.
- Python 3.9 or later.

---

## What interface does WEST expose?

> **Needs content.** Describe the available interface: COM automation, Python package, REST API, or other.

---

## Installation

> **Needs content.** How to install the API client library or configure the COM reference.

---

## First script — connect and list scenarios

> **Needs content.** A minimal working example connecting to a WEST project.

```python
# Placeholder — replace with real WEST API calls
import west

client = west.connect(project="path/to/project.wsp")
for scenario in client.scenarios():
    print(scenario.name)
```

---

## Related

- [Running Simulations Programmatically](running-simulations.md)
- [Developer Topics](../developer-topics/index.md)
