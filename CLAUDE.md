# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

Software Requirements Documentation for a new **Vehicle Trade** subsystem of the **X2 Automotive Workshop System** — a cloud-based, multi-tenant SaaS for automotive workshop management marketed across the Nordic countries.

Repository owner: **X2 Workshop AB** (Sweden).

This is a documentation-first repository. Most artefacts will be requirement-style: user stories, acceptance criteria, domain glossaries, process flows, and architecture/decision records — not application source code.

## Domain expertise to apply

When reasoning about content in this repo, act as a domain expert in:

- **Automotive repair and trade across the Nordic countries** (Sweden, Norway, Denmark, Finland, Iceland) — workshop processes, parts and labour conventions, statutory inspections, VAT/tax handling, and common Nordic regulatory framing.
- **Automotive vehicle trade in Denmark, specifically** — Danish registration tax (*registreringsafgift*), CO₂/green-tax basis, used-vehicle dealer obligations, Motorregister/DMR interactions, mandatory disclosures, and Danish VAT treatment of used vehicles (including the margin scheme where relevant).

Default vehicle-trade reasoning to **Denmark** when a document is silent on country, and explicitly flag where Sweden / Norway / Finland / Iceland would diverge.

## Conventions

- **Markdown for everything.** All files are `.md` unless they are explicitly generated outputs. `.gitignore` already excludes the generated `docs/published/output/` tree and a `.pandoc/` working directory, which implies a pandoc-based publishing pipeline for the markdown sources.
- **Any code samples or reference implementations target .NET / C# 10 or later.** Use language features appropriate to that version.
- Visual Studio is the assumed authoring environment for any C# (`.gitignore` excludes `*.suo`, `*.user`, `.vs/`).

## Repository layout

All documentation lives under `docs/`. Each top-level section is its own folder with a `README.md` landing page; some sections also have per-concept files. Numeric folder prefixes give a stable reading order and leave gaps for inserting new sections later.

```
docs/
  README.md                  # TOC and reading order
  10-overview/    README.md
  20-glossary/    README.md
  30-domain-model/           # entities — split into one file per entity
    README.md
    equipment.md
    job-card.md
    product.md
    lot.md
    inventory.md
  40-purchase-types/  README.md
  50-processes/       README.md
  60-accounting/      README.md
  70-user-interface/  README.md
  90-references/      README.md
```

When a section's single `README.md` grows large enough that distinct concepts are competing for space, split each concept into its own file in the same folder (the pattern used in `30-domain-model/`) and reduce the `README.md` to an index.

The pandoc publishing pipeline reads from this `docs/` tree; its generated output lands under `docs/published/output/` (gitignored).
