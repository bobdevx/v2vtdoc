# Entity: Job Card

## Purpose

Records work performed on an Equipment. The same Job Card entity is used across the whole X2 platform; in Vehicle Trade it is used to record:

- **Pre-sale repair** — bringing a stock vehicle up to saleable condition.
- **Pre-sale preparation** — valeting, inspection, documentation.
- **Warranty work** — performed before *or* after the sale to honour a warranty commitment.

## Customer on the Job Card

| Job Card scenario                                       | Customer field                                          |
| ------------------------------------------------------- | ------------------------------------------------------- |
| Work on a `Stock` Trade Equipment                       | The workshop itself (effectively an internal job)       |
| Warranty work after sale                                | The workshop (warranty is a cost the workshop bears)    |
| Non-warranty work after sale                            | The new owner — recorded against their *Customer Equipment* row, not the original Trade Equipment |

Note the asymmetry: warranty obligations stay with the workshop even after the vehicle has been sold, so the Job Card's customer remains the workshop. Ordinary post-sale service moves to the buyer-as-customer flow on a fresh Customer Equipment row.

The vehicle VIN is used to track work across the same vehicle from customer or workshop ownership.

## Effect on equipment value

Repair Job Cards on a `Stock` equipment can **increase** its carrying value (capitalised reconditioning) via a zero-quantity Lot revaluation transaction. Warranty Job Cards generally **do not** capitalise — they're a cost of sale that reduces realised profit. See [Repair costs](../60-accounting/README.md) and [Warranty costs](../60-accounting/README.md).

## Open questions / to expand

- The rule that distinguishes capitalised repair from expensed repair (threshold, work type, materiality).
- Whether external sublet costs (sub-contracted bodywork, paint, etc.) follow the same capitalisation rule as internal labour.
- Linking warranty Job Cards back to the original sale document for reporting.
