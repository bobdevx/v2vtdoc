# Entity: Product

## Purpose

A Product is the catalogue row that backs every purchase and sales document line. It carries the SKU, description, default price, VAT classification, and — optionally — inventory tracking.

In normal circumstances a Product represents spare parts, consumables (oil, filters, etc.), or any other item the workshop regularly buys and records on job cards. Products are also used to record **labour** — the time spent performing a job.

A VAT rate is always associated with a Product. For an ordinary Product the same VAT rate applies to both the purchase and sale legs of any transaction it appears on. For an **Equipment Inventory Product** the VAT rate carried by the product is the default for the *purchase* leg only; the *sale* leg's VAT treatment is carried by a separate **Equipment Sales Product** with its own VAT rate. This split is what enables a vehicle bought under one VAT regime (e.g. an EU acquisition) to be sold under a different one (e.g. a domestic full-VAT sale) without losing audit clarity.

Two product families are used by Vehicle Trade:

## 1. Equipment Inventory Products — `BIL-LAGx`

- **Inventory tracked.** One Lot per vehicle; quantity goes up on purchase and down on sale.
- **One SKU per [purchase type](../40-purchase-types/README.md)** — e.g. one for domestic VAT-inclusive purchases, one for margin-scheme purchases, one for EU acquisitions, one for non-EU imports.
- Appears on **both the purchase and sales documents** for any trade.
- Holds the carrying value of the vehicle in stock.

| SKU       | Description                                    | VAT rate | VAT description |
| --------- | ---------------------------------------------- | -------- | --------------- |
| BIL-LAG1  | Varelager nye biler                            | 25%      | Moms            |
| BIL-LAG2  | Varelager brugte biler (Inklusiv moms)         | 25%      | Moms            |
| BIL-LAG3  | Varelager brugte biler (Difference Moms)       | 0%       | Differencemoms  |
| BIL-LAG4  | Varelager brugte biler (Erhvervelsesmoms)      | 25%      | Erhvervelsmoms  |
| BIL-LAG5  | Varelager brugte biler (Importmoms)            | 25%      | Importmoms      |

## 2. Equipment Sales Products — `BIL-xx`

- **Not inventory tracked.**
- **One SKU per sales-side tax / customer classification** — e.g. private buyer, business buyer, export, margin-scheme resale.
- Appears on the **sales document only**, alongside the inventory product. Its role is to carry the correct VAT treatment for the *sale leg* of the transaction, which can differ from the purchase leg.

| SKU    | Description                                       | VAT rate | VAT description |
| ------ | ------------------------------------------------- | -------- | --------------- |
| BIL-1  | Salg nye biler (DK Inklusiv Moms)                 | 25%      | Moms            |
| BIL-2  | Salg brugte biler (DK Inklusiv moms)              | 25%      | Moms            |
| BIL-3  | Salg brugte biler (Difference Moms)               | 0%       | Differencemoms  |
| BIL-4  | Salg brugte biler (Indenfor EU Eksklusiv Moms)    | 0%       | Ej moms         |
| BIL-5  | Salg brugte biler (Indenfor EU Inklusiv Moms)     | 25%      | Moms            |
| BIL-6  | Salg brugte biler (Udenfor EU)                    | 0%       | Ej moms         |

> **Why two products per sale?** Danish VAT treatment depends jointly on how the vehicle was acquired *and* to whom it is being sold. Splitting responsibilities — inventory product carries the cost / purchase classification, sales product carries the sale classification — keeps the data model orthogonal and lets the same stock vehicle be sold under whichever scheme is correct for the buyer.

## Open questions / to expand

- **Confirm** the VAT rates and descriptions filled into the `BIL-LAGx` and `BIL-xx` tables above — they are best-guess values based on the Danish VAT regime; in particular: BIL-LAG3 / BIL-3 (margin scheme) treat the line as `0%` because VAT is calculated on the margin rather than the gross, and BIL-LAG4 (EU acquisition) shows `25%` even though the reverse-charge mechanism nets it to zero in cash terms.
- The mapping from each `BIL-LAGx` inventory product to its [purchase type](../40-purchase-types/README.md), and from each `BIL-xx` sales product to its sale-side scenario.
- Whether **new cars** (BIL-LAG1) need a dedicated purchase type in [Purchase types](../40-purchase-types/README.md), or whether they sit under purchase type 1 (Domestic Used Vehicle — VAT inclusive).
- Default product attributes (account codes, default VAT codes per SKU).
