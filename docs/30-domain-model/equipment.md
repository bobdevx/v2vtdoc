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
- Equipment status — `Draft` | `Stock` | `Sold`
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
```

| From    | To    | Trigger                                              | Constraint                                              |
| ------- | ----- | ---------------------------------------------------- | ------------------------------------------------------- |
| (new)   | Draft | User starts a purchase evaluation                    | A draft purchase document is normally created alongside |
| Draft   | Stock | Purchase document (invoice or credit note) is posted | Inventory and Lot rows are created at this point        |
| Stock   | Sold  | Sales document is posted                             | Lot quantity goes to 0; inventory decremented           |

Each stage of the life cycle can also be cancelled by applying corresponding credits. A sale is reverted with a credit note returning the `Sold` equipment to `Stock`. A purchase is reverted with a purchase credit (or sales invoice is purchased with a credit note) returning the `Stock` equipment to `Draft`. A `Draft` purchase can only be deleted if it has no posted purchase document; otherwise, it is cancelled with a purchase credit.

returning the `Draft` equipment to (non-existent) `(new)`.

## Rules

- A `Draft` equipment cannot be sold.
- A `Sold` equipment cannot be re-purchased. To buy the same physical vehicle back, **copy** the `Sold` row into a new `Draft`.
- Only a `Stock` equipment can be sold.
- Work performed on a `Stock` equipment goes on a Job Card with the **workshop** as customer. Non-warranty work after sale goes on a Job Card with the **buyer** as customer (warranty work after sale is still a workshop cost — see [Job Card](job-card.md) and [Warranty costs](../60-accounting/README.md)).

## Open questions / to expand

- Cancellation / void path (e.g. a draft that is abandoned, or a posted purchase that needs to be reversed).
- Required vs optional vehicle-identity fields at each status.
- Integration with DMR (Motorregister) for registration-status lookups.
