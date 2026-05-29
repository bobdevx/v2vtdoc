# Analysis

Cross-reference of each numbered point in `../notes/<id>.md` against the current `docs/` specification, with a verdict and a recommended action per point. One file per source, named `<id>.md`.

## Purpose

The analysis is the bridge between an expert opinion and the specification. For every point in the notes it answers:

- What does the current `docs/` say on this topic?
- Does the expert *confirm*, *contradict*, *extend*, or *fill a gap* in what we have?
- What — if anything — should change in `docs/` as a result?

The user then uses the **Open issues to fold into the spec** section at the bottom to decide what actually lands in `docs/`.

## Template

```markdown
# Analysis <id> — <short topic>

Source: [notes/<id>.md](../notes/<id>.md)

## Per-point findings

### Point 1 — <heading from notes>

- **Current spec:** what `docs/` says today, with a link like `docs/40-purchase-types/README.md#margin-scheme`.
- **Expert input:** what the expert says, in one line.
- **Verdict:** *confirms* / *contradicts* / *extends* / *fills a gap* / *out of scope*.
- **Recommended action:** specific edit — *add a paragraph to file X*, *change wording in file Y from "…" to "…"*, *raise as open question in section Z*, *no action*.

### Point 2 — <heading>

…

## Cross-cutting

Anything that affects multiple `docs/` areas at once, or any pattern across the expert's points worth flagging (e.g. consistent push-back on how warranty is modelled).

## Open issues to fold into the spec

A clean, copy-ready list intended for the relevant `## Open questions / to expand` blocks in the appropriate `docs/` files. Each item should be self-contained and reference the target file:

- **`docs/30-domain-model/equipment.md`** — Confirm whether warranty pre-sale is treated as repair or as a distinct event (per expert point 3).
- **`docs/40-purchase-types/README.md`** — Add a fifth purchase type for leased-vehicle buy-outs (per expert point 5).
```
