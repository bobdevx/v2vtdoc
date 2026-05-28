# 30 — Domain model

The Vehicle Trade subsystem reuses five existing X2 entities. No new top-level entity is introduced; the distinctive behaviour comes from *how* these entities are populated and linked during a trade.

| Entity                            | Role in Vehicle Trade                                                                       |
| --------------------------------- | ------------------------------------------------------------------------------------------- |
| [Equipment](equipment.md)         | The vehicle being traded. One row per individual vehicle purchase.                          |
| [Job Card](job-card.md)           | Work performed on an Equipment — pre-sale repair, sale preparation, or warranty work.       |
| [Product](product.md)             | The catalogue lines used on purchase and sales documents. Two flavours: inventory and sales.|
| [Lot](lot.md)                     | Per-vehicle inventory sub-record; serial number is the Equipment code.                      |
| [Inventory](inventory.md)         | Aggregated stock-on-hand for an Equipment Inventory Product.                                |

## How the entities fit together

A single trade typically produces:

1. **One Equipment row** — created in `Draft` while evaluating the purchase, promoted to `Stock` on purchase, and finally `Sold` on sale.
2. **One Lot row** — created when the purchase document is posted; serial number = Equipment code; quantity 1 while in stock, 0 once sold.
3. **One Inventory row update** — the relevant `BILLAG-xx` product's inventory and value move up on purchase, down on sale, and may be revalued mid-life (capitalised repairs, write-downs).
4. **Zero or more Job Cards** — for any repair, preparation, or warranty work, with the workshop as customer while the vehicle is in stock.
5. **A purchase document and a sales document** — see [Processes](../50-processes/README.md).
