# Entity: Inventory

## Purpose

The Inventory row holds the **aggregate** stock position for an Equipment Inventory Product — the total number of vehicles of that purchase-type classification currently in stock, and their total carrying value.

## Relationship to Lot

|                | Lot                                  | Inventory                                |
| -------------- | ------------------------------------ | ---------------------------------------- |
| Granularity    | One per individual vehicle           | One per Equipment Inventory Product      |
| Quantity       | 0 or 1                               | Sum of lot quantities (≥ 0)              |
| Value          | Per-vehicle carrying value           | Sum of lot values                        |

## Inventory transactions

Inventory transactions record movements at the aggregate level. Every Lot transaction has a corresponding Inventory transaction that rolls the change up to the product level. The Inventory row's value at any point in time equals the sum of the carrying values of its currently-in-stock Lots.

| Event        | Lot                                                       | Inventory row                                                            |
| ------------ | --------------------------------------------------------- | ------------------------------------------------------------------------ |
| Purchase     | New Lot created with quantity 1 and value = acquisition cost | Quantity +1; value += acquisition cost                                |
| Sale         | Lot quantity → 0; value removed                           | Quantity −1; value −= the lot's then-current carrying value              |
| Revaluation  | Zero-quantity Lot transaction with a positive or negative value | Quantity unchanged; value moves by the same delta                  |

## Open questions / to expand

- Whether Inventory rows are persisted or derived on read from Lot data.
- How aggregate value is reconciled when individual lots are revalued.
