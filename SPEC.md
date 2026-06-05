# SPEC.md — WEST Documentation Wiki Specification

This document defines **what** the WEST Documentation Wiki is, **who** it serves, and the **requirements** it must satisfy. It is the contract against which content and structure are judged. For *how to work on* the project (tooling, push workflow, commands), see `CLAUDE.md`. For the *outstanding work queue*, see `CONTENT_GAPS.md`.

---

## 1. Purpose

Provide a comprehensive, accurate, and navigable online reference for **WEST** — DHI's wastewater process modelling software — derived from and consistent with the official source documentation.

The wiki replaces scattered PDF manuals with a searchable, cross-linked, continuously deployable web resource.

---

## 2. Audience

| Audience | Needs |
|---|---|
| **New users** | Quick start, interface orientation, glossary, FAQ |
| **Practising modellers** | How-to guides, block parameter references, worked examples |
| **Advanced users** | Calibration, scenario management, performance tuning, uncertainty analysis |
| **Developers / integrators** | Custom model creation, extending WEST, Python automation API |

A reader should be able to enter from any page (via search or deep link) and find the context and cross-links needed to be productive.

---

## 3. Scope

### In scope
- All standard WEST interface features, tools, and workflows
- Every block category in the WEST model library, with parameters, state variables, and typical values
- All experiment types (steady-state, dynamic, sensitivity analysis, parameter estimation, scenario analysis, uncertainty analysis)
- Process model theory (ASM family, ADM1, plant-wide models)
- Worked examples reproducing the official tutorials
- Python automation API reference

### Out of scope
- Licensed source code of WEST itself
- Content that contradicts or supersedes official DHI documentation
- Marketing material, sales information, or pricing
- Support ticketing or user forums

---

## 4. Sources of truth

All technical content **must** be consistent with the three authoritative PDFs in `source-docs/`:

| PDF | Authoritative for |
|---|---|
| `WEST_GettingStarted_Tutorial.pdf` | Tutorial workflows, quick start, basic experiments |
| `WEST_UserGuides.pdf` | Interface reference, tools, controllers, output, options |
| `WEST_Modelica_Library.pdf` | Block parameters, state variables, process equations |

Where the wiki and a PDF disagree, the PDF wins. Content not traceable to a source PDF must be clearly general-knowledge background (e.g. standard ASM theory) and must not invent WEST-specific behaviour.

---

## 5. Functional requirements

### FR-1 — Deployment
The site **must** build with MkDocs Material and deploy to GitHub Pages automatically on every push to `main` via GitHub Actions. A broken build (invalid `mkdocs.yml`, missing nav target) is a defect.

### FR-2 — Navigation
Every published `.md` page **must** be reachable from the `mkdocs.yml` navigation tree. No orphan pages. Top-level section order is fixed (see §7).

### FR-3 — Search
All content **must** be full-text searchable (provided by the `search` plugin). Page titles and headings must be descriptive enough to surface in search.

### FR-4 — Cross-linking
Every content page **must** end with a `## Related` section containing 2–4 links to adjacent pages. Internal links use relative paths (`../section/page.md`).

### FR-5 — Images
Figures **must** render as real binary PNGs from `docs/assets/images/`, referenced with relative paths. Images must never be pushed via the MCP file API (it corrupts binary content — see `CLAUDE.md`).

### FR-6 — Tagging
Every page **must** carry YAML front matter `tags:` matching its section, enabling the `tags` plugin index.

---

## 6. Content requirements

### CR-1 — Page structure
Every page follows the canonical template:
```markdown
---
tags: [section, topic]
---
# Page Title
**Summary:** One sentence — what this covers and for whom.
**Source:** <PDF reference>
**Prerequisites:** <link>   (if applicable)
---
## Sections...
---
## Related
- links
```

### CR-2 — Parameter documentation
When documenting a block or model, parameters **must** include **name, unit, and a typical value or range** — not just a name. A bare parameter list with no units/values is incomplete.

### CR-3 — No placeholders
Published pages **must not** contain `TODO`, `Needs content`, or empty sections. A heading with no body (prose, table, or list) is a defect.

### CR-4 — Accuracy over completeness
It is better to omit a block than to document it with invented parameters. Uncertain content must be flagged in `CONTENT_GAPS.md`, not guessed into a page.

### CR-5 — Consistent terminology
Use WEST's own terminology (e.g. "Model Instance", "manipulated variable", "top-level parameter", "Block Details"). The glossary is the canonical reference for terms.

---

## 7. Information architecture

Fixed top-level navigation order:

1. **Home** — landing page
2. **Getting Started** — interface, glossary, FAQ, system windows
3. **How-To Guides** — task-oriented procedures (installation → troubleshooting)
4. **Experiment Types** — SA, PE, scenario, uncertainty
5. **Block Reference** — every block category with parameters
6. **Worked Examples** — TwoASU, plant-wide, IUWS
7. **WEST Tools** — Model/Block/Data/Designer/Unit editors
8. **Automation & API** — Python API
9. **Technical Reference** — process models, influent characterisation, model structure
10. **Advanced Topics** — calibration, scenario management, performance
11. **Developer Topics** — custom models, extending WEST
12. **Manuals** — long-form installation, simulation, results, controllers
13. **Starter Kit** — entry-level quick start, glossary, FAQ
14. **Contributing** — how to contribute to the wiki

Page categories by intent:
- **Tutorial** (Getting Started, Starter Kit, Worked Examples) — learning-oriented
- **How-To** (How-To Guides, Manuals) — task-oriented
- **Reference** (Block Reference, Technical Reference, Automation API) — information-oriented
- **Explanation** (Advanced Topics, Developer Topics) — understanding-oriented

---

## 8. Technical constraints

| Constraint | Value |
|---|---|
| Static site generator | MkDocs + Material theme |
| Hosting | GitHub Pages (`chbt-dhi/wiki`) |
| Deploy trigger | Push to `main` (GitHub Actions) |
| Development branch | `claude/beautiful-fermi-e3HOT` |
| Markdown extensions | admonition, pymdownx (details, superfences, tabbed, highlight), attr_list, def_list, tables, toc |
| Image format | PNG, binary, in `docs/assets/images/` |
| Image naming | `{tutorial\|userguide\|modelica}-p{page:03d}-img{n}.png` |
| Commit author | `Claude <noreply@anthropic.com>` (enforced by stop hook) |

---

## 9. Quality acceptance criteria

A page is considered **done** when all of the following hold:

- [ ] Content is traceable to and consistent with a source PDF
- [ ] All parameters include units and typical values/ranges
- [ ] Images render correctly (binary PNG, relative path)
- [ ] `## Related` section present with 2–4 valid internal links
- [ ] Front matter `tags:` match the section
- [ ] Page is listed in `mkdocs.yml` nav
- [ ] No empty sections, `TODO`, or `Needs content` placeholders
- [ ] MkDocs builds without warnings for this page

The wiki as a whole is **healthy** when:
- [ ] `mkdocs build` succeeds with no broken nav references
- [ ] Every `.md` file is in the nav (no orphans)
- [ ] No genuinely empty sections remain (tables/lists count as content)
- [ ] `CONTENT_GAPS.md` accurately reflects outstanding work

---

## 10. Current state (2026-06-05)

| Metric | Value |
|---|---|
| Content pages | 81 `.md` files |
| Total content | ~10,200 lines |
| Images | 322 PNGs (25.7 MB) |
| Top-level sections | 14 |
| Empty sections | 0 (category headings with sub-sections excepted) |
| Outstanding gaps | ~73 expansion items (see `CONTENT_GAPS.md`) |

The wiki is **complete and deployable**. Remaining work is depth — individual block parameter tables and expansion of thin operational sections — not breadth. No page is missing; some blocks are documented at category level rather than per-block.

---

## 11. Non-goals

- This is **not** a replacement for DHI's official support channels.
- This is **not** a place for unverified community modelling tips.
- The wiki does **not** track WEST version-specific changelogs (it documents the version covered by the source PDFs).
- Automated content generation must **not** trade accuracy for coverage (see CR-4).
