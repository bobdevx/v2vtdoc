# Entity: Lot

## Purpose

A Lot tracks an individual identifiable item inside an inventory-tracked product. The platform supports several lot types; Vehicle Trade uses only the **Serial** lot type.

## Behaviour for Vehicle Trade

- **Lot type:** `Serial`.
- **Serial number:** the Equipment code (`EQ######`).
- **Quantity:** `1` (purchased, in stock) or `0` (sold). Never any other value.
- **Value:** the carrying value of the individual vehicle — purchase cost plus capitalised repairs, less any write-downs.
- Exactly one Lot row exists for each Equipment Inventory Product unit currently or historically in stock.

> **Invariant.** For any Equipment Inventory Product with a positive inventory quantity *n*, there are exactly *n* Lot rows with quantity = 1 under that product.

## Lot transactions

Lot transactions record changes to a single lot. For Vehicle Trade the transaction quantity is one of:

| Quantity | Meaning                                                                                                                                       |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `+1`     | Purchase — vehicle taken into stock                                                                                                           |
| `−1`     | Sale — vehicle leaves stock                                                                                                                   |
| `0`      | Revaluation — no change in physical quantity, but the lot's value moves (e.g. capitalised repair, write-down, capitalised reconditioning cost)|

## Open questions / to expand

- Costing method when capitalising repairs (specific-identification is implied by serial lots but the value-update rules should be made explicit).
- Audit / traceability requirements on revaluation transactions.
