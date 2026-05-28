# Entity: Product

## Purpose

A Product is the catalogue row that backs every purchase and sales document line. It carries the SKU, description, default price, VAT classification, and — optionally — inventory tracking.

In normal cirumstances, a Product is used for spare parts, consumables (oil, filters, etc.), and other items that are regularly purchased and sold. Products can also be used for labor recording time used to perform a job.

A VAT rate is always associated with a Product. For normal products, the VAT rate is the one applied to both purchase and sale legs of the transaction. For equipment inventory-tracked products, the VAT rate is the default applied on the purchase leg; a separate sales product with its own VAT rate can be used to carry the correct VAT treatment on the sale leg.

Two product families are used by Vehicle Trade:

## 1. Equipment Inventory Products — `BILLAG-xx`

- **Inventory tracked.** One Lot per vehicle; quantity goes up on purchase and down on sale.
- **One SKU per [purchase type](../40-purchase-types/README.md)** — e.g. one for domestic VAT-inclusive purchases, one for margin-scheme purchases, one for EU acquisitions, one for non-EU imports.
- Appears on **both the purchase and sales documents** for any trade.
- Holds the carrying value of the vehicle in stock.

## 2. Equipment Sales Products — `BIL-xx`

- **Not inventory tracked.**
- **One SKU per sales-side tax / customer classification** — e.g. private buyer, business buyer, export, margin-scheme resale.
- Appears on the **sales document only**, alongside the inventory product. Its role is to carry the correct VAT treatment for the *sale leg* of the transaction, which can differ from the purchase leg.

> **Why two products per sale?** Danish VAT treatment depends jointly on how the vehicle was acquired *and* to whom it is being sold. Splitting responsibilities — inventory product carries the cost / purchase classification, sales product carries the sale classification — keeps the data model orthogonal and lets the same stock vehicle be sold under whichever scheme is correct for the buyer.

## Open questions / to expand

- The full list of `BILLAG-xx` SKUs and the purchase type each maps to.
- The full list of `BIL-xx` SKUs and the sale-side classifications each maps to.
- Default product attributes (account codes, default VAT codes per SKU).
