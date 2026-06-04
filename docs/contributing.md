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

- Every pull request requires at least one approval from a WEST domain expert.
- The reviewer confirms technical correctness, not just grammar.
- Merge only after approval.

---

## Governance

| Role | Responsibility |
|---|---|
| Wiki owner | Final say on structure, tooling, and merge policy |
| Domain reviewers | Technical correctness review before merge |
| Contributors | Writing, converting, and updating content |
