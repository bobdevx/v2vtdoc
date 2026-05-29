# Expert Input

Workflow for processing expert input received from outside the project — PDFs, text files, email exports, in any language — and folding it into the Vehicle Trade specification under `../docs/`.

## Layout

```
expert-input/
├── sources/    # Raw expert input files as received. Immutable.
├── notes/      # Translated, structured, clarified version. One .md per source.
└── analysis/   # Cross-reference against docs/ with recommended actions. One .md per source.
```

Each piece of expert input is assigned a numeric ID that ties the three files together:

| Stage    | File                                  |
| -------- | ------------------------------------- |
| Source   | `sources/100.pdf` (or `.txt`, `.eml`, …) |
| Notes    | `notes/100.md`                        |
| Analysis | `analysis/100.md`                     |

## Workflow

1. **You drop a file** into `sources/` — any format, any language. Assign the next free numeric ID.
2. **Notes are produced** in `notes/<id>.md` — a structured, English-language version of the expert's points: translation, light interpretation, explicit ambiguity flags, and a best-guess list of `docs/` areas affected.
3. **Analysis is produced** in `analysis/<id>.md` — for each point in the notes, what the current `docs/` says, the expert's position, a verdict (*confirms / contradicts / extends / fills a gap*), and a recommended action.
4. **You review** the analysis and decide which items make it into `docs/`.

The intent is that nothing from an expert lands in `docs/` *directly* — it always passes through `notes/` and `analysis/` first, so the provenance is preserved and the integration decision stays with you.

## Templates

Templates for the notes and analysis files live in each subfolder's `README.md`.
