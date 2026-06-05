# CLAUDE.md — WEST Documentation Wiki

This file gives Claude Code everything it needs to work on this project without prior context.

---

## What this project is

A **MkDocs Material** documentation wiki for **WEST** — DHI's wastewater process modelling software. The wiki is hosted on **GitHub Pages** and deploys automatically on every push to `main` via GitHub Actions.

- **Repo**: `chbt-dhi/wiki`
- **Live site**: deploys from `main` branch
- **Dev branch**: `claude/beautiful-fermi-e3HOT` (always develop here, push to `main` via MCP or PR)
- **Stack**: MkDocs Material, GitHub Pages, GitHub Actions

---

## Repository structure

```
Wiki/
├── docs/                        # All wiki content (81 .md files)
│   ├── index.md                 # Home page
│   ├── getting-started/         # Interface overview, glossary, FAQ, system windows
│   ├── how-to/                  # 17 how-to guides (installation → troubleshooting)
│   ├── experiment-types/        # SA, PE, scenario analysis, uncertainty analysis
│   ├── block-reference/         # Every block category (11 pages)
│   ├── worked-examples/         # TwoASU, plant-wide, IUWS
│   ├── west-tools/              # Model/Block/Data/Designer/Unit editors
│   ├── automation-api/          # Python API docs
│   ├── technical-reference/     # Process models, influent characterisation, etc.
│   ├── advanced-topics/         # Calibration, scenario mgmt, performance
│   ├── developer-topics/        # Custom models, extending WEST
│   ├── manuals/                 # Installation, running sims, results, controllers
│   ├── starter-kit/             # Quick start, glossary, FAQ (entry-level section)
│   ├── contributing.md
│   └── assets/images/           # 322 PNG images extracted from source PDFs
├── source-docs/                 # Source PDFs (authoritative reference)
│   ├── WEST_GettingStarted_Tutorial.pdf
│   ├── WEST_UserGuides.pdf
│   └── WEST_Modelica_Library.pdf
├── mkdocs.yml                   # Navigation, theme, plugins
├── CONTENT_GAPS.md              # Prioritised list of remaining content work
└── CLAUDE.md                    # This file
```

---

## Source of truth

All technical content must be consistent with the three source PDFs in `source-docs/`. When writing content about a specific block, parameter, or workflow, cross-reference:

| PDF | Covers |
|---|---|
| `WEST_GettingStarted_Tutorial.pdf` | Tutorial workflow, quick start, basic experiments |
| `WEST_UserGuides.pdf` | Full interface reference, all tools, controllers, output, options |
| `WEST_Modelica_Library.pdf` | Every block: parameters, state variables, process equations |

Image filenames encode their source: `tutorial-p{page:03d}-img{n}.png`, `userguide-p{page:03d}-img{n}.png`, `modelica-p{page:03d}-img{n}.png`.

---

## How to push content

### Option A — MCP (text content, no binary files)
Use `mcp__github__get_file_contents` to read the current file and get its SHA, then `mcp__github__create_or_update_file` to push. **Always fetch the SHA immediately before pushing** — stale SHAs cause 409 errors.

```
1. mcp__github__get_file_contents(path, repo=chbt-dhi/wiki, branch=main)  → get SHA
2. mcp__github__create_or_update_file(path, content, sha, branch=main)
```

### Option B — git (for binary files or bulk changes)
```bash
git config user.email noreply@anthropic.com
git config user.name Claude
git fetch origin main && git merge origin/main --no-edit
# make changes
git add <files>
git commit -m "docs: description"
git push -u origin claude/beautiful-fermi-e3HOT
```

**Stop hook requirement**: The repo has a stop hook that requires `user.email = noreply@anthropic.com`. If a commit is rejected, fix with:
```bash
git config user.email noreply@anthropic.com && git config user.name Claude
git commit --amend --no-edit --reset-author
git push --force-with-lease origin claude/beautiful-fermi-e3HOT
```

### ⚠️ Never use MCP to push binary files (images)
`mcp__github__create_or_update_file` double-encodes binary content — it will store base64 text instead of a real PNG. Always push images via git.

---

## Image extraction (if needed again)

All 322 images are already extracted and committed. If you ever need to re-extract or add more:

```python
import fitz, os
out_dir = "/home/user/Wiki/docs/assets/images"
pdfs = {
    "tutorial": "/home/user/Wiki/source-docs/WEST_GettingStarted_Tutorial.pdf",
    "userguide": "/home/user/Wiki/source-docs/WEST_UserGuides.pdf",
    "modelica": "/home/user/Wiki/source-docs/WEST_Modelica_Library.pdf",
}
for prefix, path in pdfs.items():
    doc = fitz.open(path)
    for page_idx, page in enumerate(doc):
        page_num = page_idx + 1
        img_count = 0
        for img in page.get_images(full=True):
            xref, _, w, h = img[0], img[1], img[2], img[3]
            if w <= 200 or h <= 150:
                continue  # skip logos/watermarks
            img_count += 1
            base_img = doc.extract_image(xref)
            pix = fitz.Pixmap(base_img["image"])
            if pix.n > 3:
                pix = fitz.Pixmap(fitz.csRGB, pix)
            fname = f"{prefix}-p{page_num:03d}-img{img_count}.png"
            pix.save(os.path.join(out_dir, fname))
```

Naming convention: `{prefix}-p{page_num:03d}-img{count}.png`

---

## Page format conventions

Every wiki page should follow this structure:

```markdown
---
tags:
  - <section-name>
  - <topic>
---

# Page Title

**Summary:** One sentence describing what this page covers and who it is for.

**Source:** WEST User Guide §X.X  (or Tutorial ch X, or Modelica Library p. XX)

**Prerequisites:** [Link to prerequisite page](../path/to/page.md)  (if applicable)

---

## Section heading

Content...

![Caption](../assets/images/prefix-p000-img1.png)

---

## Related

- [Related page](../path/to/page.md)
- [Another page](../path/to/page.md)
```

---

## Navigation — mkdocs.yml

The navigation is defined in `mkdocs.yml`. Current top-level sections (in order):
Home → Getting Started → How-To Guides → Experiment Types → Block Reference → Worked Examples → WEST Tools → Automation & API → Technical Reference → Advanced Topics → Developer Topics → Manuals → Starter Kit → Contributing

When adding a new page:
1. Create the `.md` file in the correct `docs/` subdirectory
2. Add it to the `nav:` section of `mkdocs.yml` in the right position
3. Fetch the current SHA of `mkdocs.yml` before pushing the updated version

---

## Content gaps

See **`CONTENT_GAPS.md`** for the full prioritised work queue. Summary:

### High priority — individual block descriptions missing
- `block-reference/sensors-signal.md` — NO3, TSS, flow meter, temperature, PO4 sensors not documented
- `block-reference/advanced-processes.md` — MBR blocks (SubmergedMembrane, SideStreamMembrane, FoulingModel_Simple), AGS_SBR, MBBR_Aerobic, MBBR_Anoxic, IFAS need individual parameter tables
- `block-reference/sludge-treatment.md` — ThickenerEfficiency, aerobic digester blocks, dewatering blocks (Centrifuge, BeltPress) undocumented
- `block-reference/controllers-timers.md` — Cascade, FeedForward, Ratio, PID_SRT controllers need individual entries
- `block-reference/flow-management.md` — Mixer2/3/4, SimplePump vs VariablePump, LoopBreaker need individual entries
- `block-reference/clarifiers.md` — PrimaryClarifier.BSM2 and SecondaryClarifier.BurgerDiehl30 need parameter tables

### Medium priority — thin how-to and manual sections
- Licence configuration steps in `how-to/installation.md`
- Controller setup steps 3–4 missing in `how-to/controllers.md`
- Slider widget guide in `how-to/running-simulations.md`
- Gauge types and alarm thresholds in `how-to/results-and-output.md`
- WEST Player edition description in `how-to/west-product-editions.md`
- On/Off, Timer, Feed-forward controller parameter tables in `manuals/controllers.md`
- Glossary letters C, L, U (getting-started) and H, L, R, V (starter-kit)

### Low priority
- Developer topics: CLI model registration syntax, Petersen matrix examples
- API: complete runnable code examples
- Contributing: actual page template, `mkdocs serve` command

---

## Parallel agent workflow

When filling many sections at once, launch multiple agents in parallel (one per file or group of related files). Each agent should:
1. `mcp__github__get_file_contents` — read the file and capture the SHA
2. Write the new content (preserving all existing content)
3. `mcp__github__create_or_update_file` — push with the captured SHA

Agents push directly to `main`. After all agents complete, sync the local branch:
```bash
git fetch origin main && git merge origin/main --no-edit
git push -u origin claude/beautiful-fermi-e3HOT
```

---

## Quality checklist for new content

- [ ] Technically accurate (consistent with source PDFs)
- [ ] Parameter values include units and typical ranges, not just names
- [ ] Images referenced with correct path `../assets/images/prefix-pNNN-imgN.png`
- [ ] `## Related` section at the bottom with 2–4 cross-links
- [ ] Page added to `mkdocs.yml` nav if it's a new file
- [ ] Tags in front matter match the section name
- [ ] No "Needs content" or "TODO" placeholders left
