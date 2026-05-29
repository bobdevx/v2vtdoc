# Notes

Structured, English-language interpretations of the corresponding `../sources/<id>.<ext>` file. One file per source, named `<id>.md`.

## Purpose

The source file may be in any language, written in any style, and may contain ambiguity. The notes file is a normalised version that:

- Translates the content into English.
- Tightens dense prose into discrete numbered points.
- Calls out anything the original left ambiguous.
- Lists the `docs/` areas each point is likely to touch — for the `../analysis/<id>.md` step to verify.

The notes file is not yet a recommendation; it's a faithful, clarified rendering of what the expert said. Recommendations live in `../analysis/<id>.md`.

## Template

```markdown
# Notes <id> — <short topic>

| Field             | Value                  |
| ----------------- | ---------------------- |
| Source            | sources/<id>.<ext>     |
| Received          | YYYY-MM-DD             |
| Expert            | <name, role, org>      |
| Original language | <e.g. Danish>          |

## Summary

Two or three sentences in English giving the gist of the expert's input.

## Points

1. **<short heading>.** Translated, tightened point. Where nuance matters, keep the original phrasing in *italics* alongside.
2. **<short heading>.** …
3. …

## Ambiguities

- Anything I couldn't resolve from the source alone — flagged for the user to clarify with the expert.

## Possibly touches

- Best-guess list of `docs/` files or sections each point relates to — e.g. `docs/40-purchase-types/README.md` (section on margin scheme). The analysis step verifies each.
```
