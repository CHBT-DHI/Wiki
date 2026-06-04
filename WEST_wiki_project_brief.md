# WEST Documentation Wiki — Project Brief for Claude

## How to use this brief

This document describes a project to build a scalable documentation wiki for WEST, with the longer term option of grounding an AI assistant on that same content. It is written for you, Claude, so that you have the full context before doing any work.

**Important: do not start building anything yet.** The person you are working with may be at any stage of this project, from "nothing started" to "wiki already running, now adding the assistant." Before taking any action, read this whole brief, then ask the person where they currently are using the checklist in the **Process stages** section below. Confirm what has already been decided and done, and only then propose the next concrete step. Do not assume a stage, and do not assume tooling choices that have not been confirmed.

## Project goal

Build a documentation wiki for WEST that:

1. Reduces the "hard to use" friction that current users report.
2. Scales cleanly as more pages and more contributors are added over time.
3. Stays in a clean format so it can later serve as the source for a grounded AI assistant, without rework.

The wiki is the deliverable. The AI assistant is a possible later phase that reuses the same content, not a separate project.

## Background and context

**What WEST is.** WEST is a wastewater process modelling tool from DHI. It sits in the same product category as SUMO by Dynamita, which is a useful reference (see below).

**The problem.** Users report that WEST is hard to use. The important clarification is that the simulation engine itself is sound. The complaint is about usability, meaning the gap between what a user wants to do and the steps the software requires to do it. It is not a reliability or bug problem. This matters because it means the right fix is to lower the learning and navigation barrier, which good documentation and later an assistant can do, rather than fixing the engine.

**Why a wiki first.** A well organised wiki directly addresses the usability complaint for users who can find and read the right page. It is cheap, low risk, and has no per query running cost. It is also the foundation for the assistant: an AI assistant grounded on the documentation is only as good as the documentation itself, so building clean, well structured content first is the prerequisite either way. Nothing put into the wiki is wasted if the assistant is added later.

**Reference model: the SUMO wiki.** Dynamita documents SUMO using a tiered wiki (starter kit, manuals, technical reference, advanced topics, developer topics) plus a Digital Twin Toolkit that exposes a Python API to the engine. This structure is proven in this exact product category and is the template this project should follow. The SUMO example also shows that exposing a documented programmatic interface to the simulator is a normal capability, which is the precondition for an assistant that does more than answer questions.

## The plan in two phases

**Phase 1: build the wiki (current focus).** Stand up a scalable documentation site, seed it from existing material, and get real users reading it. This is the priority and the part that delivers value on its own.

**Phase 2: add a grounded assistant (later, optional).** Only if the wiki alone does not fully close the usability gap. The assistant indexes the wiki content and answers user questions from it. The model is not trained or fine tuned; it is grounded on the content at query time through retrieval, so updating the wiki updates the assistant with no retraining.

## Design principles

These apply from day one because they serve both human readers and the later assistant.

1. **One topic per page.** Self contained, single purpose pages are easier to find, easier to own, and are the ideal unit for retrieval later.
2. **Plain Markdown as the source of truth.** Keep content in plain Markdown so it renders for humans now and indexes cleanly for the assistant later, with no conversion step.
3. **Substance in text, not only screenshots.** Screenshots help humans but carry no information for a text based assistant. Anything important must also be written out.
4. **Consistent page template.** A predictable shape makes the wiki coherent as it grows and easier for both readers and an assistant to parse.
5. **Review before merge.** Treat content like code, with review, so quality holds as contributors are added.

## Recommended tool stack

The picks are chosen so content flows from the wiki into the assistant without rework.

### Phase 1 (wiki)

* **Documentation engine: MkDocs with the Material theme** is the primary recommendation. Pages are plain Markdown, it renders a clean searchable site with built in search, it is cheap, and it keeps content in the format the assistant will want. Docusaurus is an alternative for a more application like site. **Wiki.js** is the fallback if editors are mostly non technical and need a browser based editor, at the cost of slightly messier source content.
* **Version control: Git with GitHub or GitLab.** Review and merge content the way code is handled, with full history.
* **Hosting: GitHub Pages or GitLab Pages, or an internal static host.** A MkDocs site is static files, so hosting is cheap and low maintenance. A Wiki.js setup would be self hosted on a small server instead.
* **Authoring: VS Code** for editing, and **Pandoc** for converting the existing documents and tutorials into Markdown. Expect to clean up the converted output.

### Phase 2 (assistant, later)

* **Indexing and retrieval: LlamaIndex** (Python), built for retrieval over your own documents.
* **Vector store: a lightweight option such as Chroma or LanceDB, or pgvector** if PostgreSQL is already in use. No large managed database is needed at the scale of one product's docs.
* **Model: a hosted LLM API, using a small model for most queries** to control cost. Grounding, not training.

### The deciding input

The platform choice hinges on **who will edit the wiki over the next year**. If mostly technical staff, MkDocs Material is the clear pick. If a broad, non technical group, lean toward Wiki.js. Confirm this with the person before recommending a platform.

## Proposed wiki structure

Follow the SUMO style tiering, adapted to WEST:

1. **Starter kit** — frequently asked questions, a quick tutorial, and a glossary of terms and acronyms. The lowest barrier entry point.
2. **Manuals** — operating the interface: installation, running simulations, advanced simulations, controllers, results, and worked examples.
3. **Technical reference** — the models and science: process models, components and processes, influent characterisation, and process units.
4. **Advanced topics** — productivity and power user features.
5. **Developer topics** — extending WEST and any custom modelling.
6. **Automation and API** — driving WEST programmatically, which is also the foundation for the Phase 2 assistant.

## Page template and conventions

Agree these before writing much content.

* Every page has a clear, descriptive title and a one line summary at the top.
* A suggested page shape: summary, prerequisites, steps, worked example, related links.
* Tag pages by topic so they can be grouped and filtered.
* Maintain a single glossary and link to it rather than redefining terms.
* Cross link related pages instead of duplicating content.

## Governance and handoff

This is the part most likely to undo the effort, so plan it early.

* The people building the wiki may become busy soon, so decide who owns it afterward and how new content is reviewed and merged.
* Write a short contribution guide covering the page template, the conventions above, and the review process, so the wiki keeps growing and stays consistent after the original builders move on.

## Seeding strategy

Do not start from a blank wiki. A large set of existing documentation and tutorials is available and is the biggest advantage. Convert it into the tiered structure, prioritising the workflows behind the "hard to use" feedback first, since those pages deliver the most value and double as the first test cases for the eventual assistant. Tutorials map naturally onto step by step pages.

## Division of labour

* **The person and their team** decide what to document and verify that it is technically correct for WEST. This needs domain expertise that you, Claude, do not have.
* **You, Claude** build and maintain the system around that content: scaffolding the site, running the conversions, wiring the pipeline, and handling the git workflow. You can also draft page content from supplied source material, but the team must review it for correctness.

## Process stages — ask the person which one they are at

Before doing anything, ask the person which of these best describes where they are now, and what has already been decided or done. Use their answer to pick the next step.

0. **Not started.** Still deciding on approach and tools.
1. **Platform decision.** Choosing between MkDocs Material and Wiki.js (depends on the editor audience).
2. **Tooling set up.** Repository created, MkDocs or Wiki.js installed, basic site builds locally.
3. **Structure and template defined.** Tiers, navigation, and the page template agreed.
4. **Existing content inventoried.** Existing docs and tutorials catalogued, gaps identified, formats and location known.
5. **Conversion in progress.** Running Pandoc conversions of existing material into Markdown and cleaning up.
6. **First tier seeded.** Starter kit and the highest friction workflows written and reviewed.
7. **Published and in use.** Site hosted, real users reading it, feedback being collected.
8. **Phase 2 started.** Beginning the grounded assistant over the wiki content.

## Open questions to ask the person

To locate them and plan the next step, find out:

1. Who will edit the wiki over the next year, technical staff only or a broader mix? This decides the platform.
2. Where does the existing documentation and tutorials live, and in what formats?
3. Which WEST workflows generate the most "hard to use" complaints, so they can be prioritised?
4. Are there hosting constraints, for example internal only versus cloud allowed?
5. What is the timeline and who will own the wiki after the initial build?

## Constraints and reminders

* Keep it simple and cost conscious. Prefer the lightweight, low cost option at each step.
* The engine is fine; the target is usability. Do not frame the work as fixing bugs.
* Keep content as plain Markdown so it stays ready for the Phase 2 assistant.
* Domain correctness is the team's responsibility. Always flag content that needs expert review rather than presenting it as verified.
