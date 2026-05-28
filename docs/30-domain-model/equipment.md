# Entity: Equipment

## Purpose

Represents a single, individual vehicle being traded by the workshop. One Equipment row exists per vehicle per *trade cycle* — buying back the same physical vehicle later creates a new row.

## Trade Equipment vs Customer Equipment

|                          | Trade Equipment                                  | Customer Equipment                       |
| ------------------------ | ------------------------------------------------ | ---------------------------------------- |
| Owner of the vehicle     | The workshop (in stock) or the buyer (after sale)| The customer                             |
| Equipment code           | System-generated serial — `EQ000001`, `EQ000002`,…| Vehicle registration number              |
| Created by               | The Vehicle Trade purchase flow                  | Normal service / customer onboarding     |
| Lifecycle                | Draft → Stock → Sold                             | (none defined here)                      |

If a buyer of a previously traded vehicle later returns to the workshop for unrelated service or repair, a **new Customer Equipment row is created** for that customer's instance of the vehicle. The original Trade Equipment row remains in its `Sold` state as a historical record.

## Key attributes

(Initial set — to expand)

- Equipment code (generated, pattern `EQ######`)
- Equipment status — `Draft` | `Stock` | `Sold` | `Reverted`
- Vehicle identity — registration number, VIN, make, model, model year, first-registration date
- Purchase type (links to [Purchase types](../40-purchase-types/README.md))
- Acquisition cost
- Carrying value (cost + capitalised repairs − write-downs)
- Sale price (when sold)
- Linked purchase document, sales document, lot, and inventory product

## Status lifecycle

```
                  +-------+   purchase posted   +-------+   sale posted   +-------+
   create  ---->  | Draft | ------------------> | Stock | --------------> | Sold  |
                  +-------+                     +-------+                 +-------+
                                                    |
                                                    | purchase reversed
                                                    v
                                                +----------+
                                                | Reverted |
                                                +----------+
```

| From    | To    | Trigger                                              | Constraint                                              |
| ------- | ----- | ---------------------------------------------------- | ------------------------------------------------------- |
| (new)   | Draft | User starts a purchase evaluation                    | A draft purchase document is normally created alongside |
| Draft   | Stock | Purchase document (invoice or credit note) is posted | Inventory and Lot rows are created at this point        |
| Stock   | Sold  | Sales document is posted                             | Lot quantity goes to 0; inventory decremented           |

### Reversal / cancellation

Each forward transition can be undone by posting an offsetting document. Any equipment row that has ever had a purchase document posted against it is **retained for audit** — reversing the purchase moves the row into the terminal `Reverted` state rather than back to `Draft` or out of the database.

| From    | To        | How                                                                                                                                                                                                                                                                                          |
| ------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sold    | Stock     | Post a sales credit note against the original sales invoice. The Lot quantity is restored to 1 and the inventory aggregate is incremented.                                                                                                                                                  |
| Stock   | Reverted  | Reverse the original purchase document — a purchase credit against a purchase invoice, or a sales invoice against an *afregningsbilag* sales credit note. The Lot quantity goes to 0 and the inventory aggregate is decremented; the equipment row itself is retained as a permanent audit record. |
| Draft   | (deleted) | A `Draft` has no posted purchase document and can simply be deleted (including the draft purchase document).                                                                                                                                                                                                                         |

A `Reverted` row is terminal: it cannot be returned to `Stock`, cannot be re-purchased in place, and cannot be deleted. To trade the same physical vehicle again, copy the row into a new `Draft` and start over.

## Rules

- Only a `Stock` equipment can be sold. A `Draft`, `Sold`, or `Reverted` row cannot be sold.
- A `Sold` equipment cannot be re-purchased. To trade the same physical vehicle again, **copy** the `Sold` row into a new `Draft`. (A sale can, however, be undone — see Reversal / cancellation.)
- A `Reverted` equipment cannot be re-purchased **and cannot be deleted** — the row is retained to preserve the audit link to the original, now-reversed purchase documents. To buy the same physical vehicle, **copy** the `Reverted` row into a new `Draft`.
- Work performed on a `Stock` equipment goes on a Job Card with the **workshop** as customer. Non-warranty work after sale goes on a Job Card with the **buyer** as customer (warranty work after sale is still a workshop cost — see [Job Card](job-card.md) and [Warranty costs](../60-accounting/README.md)).

## Open questions / to expand

- Whether a reversal-then-redo cycle should leave audit traces visible in the UI (and how they're rendered).
- Required vs optional vehicle-identity fields at each status.
- Integration with DMR (Motorregister) for registration-status lookups.
