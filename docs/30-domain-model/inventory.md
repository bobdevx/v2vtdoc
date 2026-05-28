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

Inventory transactions record movements at the aggregate level. Every Lot transaction has a corresponding Inventory transaction that rolls the change up to the product level.

The value of the aggregated inventory is the sum of the carrying values of the individual lots. When a vehicle is purchased, a new lot is created with the acquisition cost as its value, and the inventory quantity and value are increased accordingly. When a vehicle is sold, its lot quantity goes to 0 and its value is removed from inventory.

## Open questions / to expand

- Whether Inventory rows are persisted or derived on read from Lot data.
- How aggregate value is reconciled when individual lots are revalued.
