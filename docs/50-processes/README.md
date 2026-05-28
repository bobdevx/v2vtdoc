# 50 — Processes

Each subsystem process is a sequence of events that mutate the entities described in [Domain model](../30-domain-model/README.md). The processes listed here are the ones unique to Vehicle Trade; standard workshop processes (booking, service, parts) are inherited from the broader X2 platform.

| #   | Process                                  | Trigger                                                          |
| --- | ---------------------------------------- | ---------------------------------------------------------------- |
| 1   | Purchase equipment via purchase invoice  | Buying from a VAT-registered seller                              |
| 2   | Purchase equipment via credit note       | Buying from a private citizen (*afregningsbilag*)                |
| 3   | Perform repair                           | Pre-sale reconditioning                                          |
| 4   | Perform warranty                         | Warranty obligation, before or after sale                        |
| 5   | Sell equipment via sales invoice         | Vehicle sold to a buyer                                          |
| 6   | Add equipment to stock                   | Onboarding / migration / fleet purchase                          |

---

## 1. Purchase equipment via purchase invoice

**When.** Standard route for buying a vehicle from a VAT-registered seller — another dealer, a business disposing of a vehicle, an EU seller, or a non-EU seller (in the last two cases via the appropriate import variant of the purchase-type classification).

**Outcome.**
- A new `Draft` Equipment is created, or an existing draft is finalised.
- A purchase invoice is posted, with at least one line on the relevant `BIL-LAGx` inventory product.
- A Lot row is created with serial = equipment code, quantity = 1.
- The Equipment Inventory aggregate is incremented.
- Equipment status moves to `Stock`.
- VAT and inventory ledger postings are generated. See [Accounting](../60-accounting/README.md).

## 2. Purchase equipment via credit note

**When.** Standard route for buying from a Danish private citizen, who cannot issue a VAT invoice. The workshop issues a self-billing settlement note (*afregningsbilag*), modelled in this system as a **sales credit note** (a negative sales document).

**Outcome.** As for the purchase-invoice route, except:
- The purchase document is a credit note rather than a purchase invoice.
- The purchase is necessarily classified under the **margin scheme** purchase type.
- No input VAT is recoverable.

## 3. Perform repair

**When.** Any work performed on a `Stock` Equipment to ready it for sale.

**Outcome.**
- A Job Card is opened with the workshop as customer.
- Labour and parts costs are recorded against the Job Card.
- When the Job Card is closed, the equipment's Lot value (and the aggregate Inventory value) is increased by the capitalised portion of the repair cost — recorded as a Lot transaction of quantity `0` and positive value.
- Ledger postings reclassify the cost from expense to inventory. See [Repair costs](../60-accounting/README.md).

## 4. Perform warranty

**When.** A warranty obligation needs to be honoured. Can occur:

- **Pre-sale.** Less common; usually treated like an ordinary repair.
- **Post-sale.** The Job Card's customer remains the **workshop** (warranty is the workshop's cost), even though the vehicle is by then a *Customer Equipment* owned by the buyer.

**Outcome.**
- Job Card recorded against the appropriate equipment row.
- Cost recognised as warranty expense — **does not** capitalise into inventory post-sale (the vehicle is no longer in stock). Pre-sale warranty work that closes a known defect may be treated as repair instead — to be specified.
- Realised profit on the original sale is reduced by the warranty cost in management reporting.

## 5. Sell equipment via sales invoice

**When.** A `Stock` equipment is sold to a buyer.

**Outcome.**
- A sales invoice is posted with two product lines:
  - The matching `BIL-LAGx` inventory product (relieves stock, carries the acquisition-side classification).
  - A `BIL-xx` sales product (carries the sale-side tax classification — private / business / export / margin).
- Lot quantity goes to 0; the serial Lot row remains as historical record.
- Inventory aggregate decremented in both quantity and value.
- Equipment status moves to `Sold`.
- VAT and revenue ledger postings generated; treatment depends jointly on the original purchase type and the sales product chosen.

## 6. Add equipment to stock

**When.** Used during system startup, data migration, or to record a fleet purchase where many vehicles are taken in under a single arrangement and itemised internally.

**Outcome.**
- One or more Equipment rows are created directly in `Stock` status, with corresponding Lot and Inventory entries.
- No purchase document is posted within X2; the user supplies the carrying value and purchase type directly.
- An accounting journal entry may still be needed externally to recognise the inventory — to be specified.

## Open questions / to expand

- Cancellation / reversal of a posted purchase or sale (return-to-seller, cancelled sale).
- Handling of part-exchange / trade-in as part of a sale (covered by the [Vehicle trade-in](../70-user-interface/README.md) UI flow, but the entity-level process is not yet specified).
- Whether a single Job Card can span multiple equipment rows (e.g. a fleet pre-sale inspection batch).
