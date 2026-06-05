# Contribution Guide

How to add and improve content in the WEST Documentation Wiki.

---

## Who can contribute

Any DHI staff member or authorised contractor with access to the repository. Technical correctness review by a WEST domain expert is required before any page is merged.

---

## Page template

Every page should follow this shape:

```markdown
---
tags:
  - <tier>
  - <topic>
---

# Page Title

**Summary:** One sentence describing what this page covers.

**Prerequisites:** (if any) What the reader needs to know or have done first.

---

## Section heading

Content here.

---

## Related

- [Link to related page](path/to/page.md)
```

**Rules:**

- One topic per page.
- Substance in text, not only screenshots.
- Link to the [Glossary](starter-kit/glossary.md) rather than redefining a term.
- Cross-link to related pages instead of duplicating content.
- Mark unfinished sections with `> **Needs content.**` so they are easy to find.

---

## Adding a page

1. Create a new `.md` file in the appropriate tier folder under `docs/`.
2. Add it to the `nav:` section of `mkdocs.yml`.
3. Use the page template above.
4. Submit a pull request. A domain expert must review for technical correctness before merge.

---

## Running the site locally

```bash
pip install mkdocs-material
mkdocs serve
```

The site will be available at `http://127.0.0.1:8000`.

---

## Review process

All contributions go through a pull request (PR) review before merging to `main`. The review checks: (1) technical accuracy — content should be consistent with WEST behaviour and the source PDFs; (2) style — matches the existing page structure (front matter, H2/H3 headings, Related section); (3) images — are stored as binary PNG files in `docs/assets/images/` with the naming convention `{prefix}-p{page_num:03d}-img{count}.png`; (4) navigation — if a new page is added, it must be listed in `mkdocs.yml` under the correct section. Reviewers leave comments on the PR; address all blocking comments before requesting a re-review.

| Role | Responsibility |
|---|---|
| Wiki owner | Final say on structure, tooling, and merge policy |
| Domain reviewers | Technical correctness review before merge |
| Contributors | Writing, converting, and updating content |

---

## Governance

The WEST Documentation Wiki is maintained by DHI. Content decisions (what to include, how to structure sections, which PDFs are authoritative) are made by the DHI WEST product team. Community contributions are welcome for: correcting errors, improving clarity, adding worked examples, and expanding the glossary. Requests to add new top-level sections or change the navigation structure should be raised as GitHub Issues for discussion before submitting a PR.
