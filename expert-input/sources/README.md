# Sources

Raw expert input files exactly as received. Treat these as **immutable source-of-truth** — do not edit them. If an expert sends a revised version, save it under a new ID rather than overwriting.

## What goes here

- **PDFs** — whitepapers, dealer manuals, regulatory extracts, expert reports, scanned correspondence.
- **Plain text** files.
- **Email exports** — `.eml`, or copy-pasted into `.txt` / `.md`.
- Files in **any language**; translation happens in `../notes/<id>.md`, not here.

## Naming

Use a plain numeric ID with the original file's extension:

```
100.pdf
101.txt
102.eml
```

IDs are assigned on drop and must be unique across this folder. Three digits gives plenty of room; bump up if it ever runs out.

If the original file is enormous or scanned at low quality, it's fine to keep both the original and a cleaned copy here under the same ID (e.g. `100.pdf` and `100.cleaned.pdf`) — only one needs to be the canonical source referenced from `notes/100.md`.
