# Vehicle Trade — Documentation

Software Requirements Documentation for the **Vehicle Trade** subsystem of the **X2 Automotive Workshop System**.

The Vehicle Trade subsystem handles the purchase, reconditioning, and resale of vehicles ("Equipment") by automotive workshops operating in Denmark. It addresses three intertwined concerns:

1. **Transaction processing** — purchase costs, sales proceeds, inventory tracking, repairs, and warranty work.
2. **Accounting and tax treatment** — Danish VAT (including the second-hand goods margin scheme), profit/loss recognition, and posting to an external accounting system.
3. **User experience** — a workflow that handles the common cases by default while still supporting the less common ones (imports, private-party trades, post-sale warranty, fleet onboarding).

## Reading order

| #   | Section                                            | Purpose                                                              |
| --- | -------------------------------------------------- | -------------------------------------------------------------------- |
| 10  | [Overview](10-overview/README.md)                  | Scope, terminology, and product context                              |
| 20  | [Glossary](20-glossary/README.md)                  | Canonical definitions of the domain terms                            |
| 30  | [Domain model](30-domain-model/README.md)          | The entities: Equipment, Job Card, Product, Lot, Inventory           |
| 40  | [Purchase types](40-purchase-types/README.md)      | The five tax/valuation classifications that drive everything else    |
| 50  | [Processes](50-processes/README.md)                | Purchase, repair, warranty, sale, stock onboarding                   |
| 60  | [Accounting](60-accounting/README.md)              | Ledger postings to the external accounting system                    |
| 70  | [User interface](70-user-interface/README.md)      | Buy / repair / sell / inventory / trade-in screens                   |
| 90  | [References](90-references/README.md)              | External regulations, APIs, and standards                            |

## Conventions

- All source files are Markdown.
- Code samples, when present, target .NET / C# 10 or later.
- Generated artefacts (e.g., the pandoc-published output under `docs/published/output/`) are out of source control.
