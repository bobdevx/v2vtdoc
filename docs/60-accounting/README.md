# 60 — Accounting for equipment trade

The X2 Automotive Workshop System does **not** maintain ledgers. Vehicle Trade events generate accounting transactions that are posted through to an **external accounting system** via its API. The responsibilities of this subsystem are therefore to:

1. Generate the correct set of ledger entries for each event.
2. Map each entry to the right account in the configured external accounting system.
3. Push the entries reliably (with retry, idempotency, and audit trail).
4. Surface the resulting documents for accountant review.

## Accounting-relevant events

| Event                       | Triggered by                                                | Postings broadly cover                            |
| --------------------------- | ----------------------------------------------------------- | ------------------------------------------------- |
| Inventory valuation change  | Purchase, sale, repair capitalisation, write-down           | Inventory asset accounts                          |
| VAT on purchases            | Purchase invoice / credit note / EU acquisition / import    | Input VAT, acquisition VAT, import VAT            |
| VAT on sales                | Sales invoice                                               | Output VAT (full or margin-scheme)                |
| Repair costs                | Job Card (repair) closure                                   | Reclassification of cost into inventory           |
| Warranty costs              | Job Card (warranty) closure                                 | Warranty expense                                  |

The exact ledger entries depend on the [Equipment Purchase Type](../40-purchase-types/README.md), since the Danish VAT treatment differs by type.

## TBA — ledger entries by purchase type

A worked matrix of "for each event × each purchase type, the debit / credit accounts and amounts" is the heart of this section, and will be filled in as the chart of accounts and target accounting integrations are confirmed.

| Event ↓ \ Purchase type → | 1. Domestic VAT-incl. | 2. Margin scheme | 3. EU import | 4. Non-EU import | 5. Other |
| ------------------------- | --------------------- | ---------------- | ------------ | ---------------- | -------- |
| Purchase                  | TBA                   | TBA              | TBA          | TBA              | TBA      |
| Repair (capitalised)      | TBA                   | TBA              | TBA          | TBA              | TBA      |
| Write-down                | TBA                   | TBA              | TBA          | TBA              | TBA      |
| Sale                      | TBA                   | TBA              | TBA          | TBA              | TBA      |
| Warranty (post-sale)      | TBA                   | TBA              | TBA          | TBA              | TBA      |

## Integration model

(To expand.) The integration is API-based, asynchronous, and idempotent. Each X2 document (purchase invoice, sales invoice, credit note, Job Card with capitalisable cost) produces a posting batch identified by a stable external reference. Re-sends must not double-post.

## Open questions / to expand

- The target external accounting systems and the per-system account mappings.
- Treatment of *registreringsafgift* in the chart of accounts — separate pass-through account vs absorbed into inventory cost.
- Reconciliation reports between X2 inventory and the external ledger.
- Handling of foreign-currency purchases (EU import / non-EU import) — FX rate, FX gain/loss recognition.
