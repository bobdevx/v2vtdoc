# 40 — Equipment purchase types

The **Purchase Type** of a trade equipment row is the central classification that drives:

- The VAT treatment on the purchase document.
- The carrying value of the vehicle in stock.
- The VAT treatment on the eventual sale (Danish rules require the sale's tax treatment to be consistent with how the vehicle was acquired).
- The ledger postings sent to the external accounting system.

For this reason the purchase type is **set at the moment of purchase and travels with the vehicle through its entire lifecycle** — sale treatment cannot be decoupled from acquisition treatment.

Five purchase types are recognised:

| Code | Type                                              | Typical seller                                       | Danish VAT treatment                                                          |
| ---- | ------------------------------------------------- | ---------------------------------------------------- | ----------------------------------------------------------------------------- |
| 1    | Domestic Used Vehicle — VAT inclusive             | Danish VAT-registered dealer or business             | Full input VAT deductible on purchase; full output VAT on sale                |
| 2    | Domestic Used Vehicle — VAT exclusive (Margin Scheme) | Danish private citizen, or another margin-scheme dealer | No input VAT on purchase; output VAT only on the profit margin            |
| 3    | EU Import — VAT exclusive                         | VAT-registered seller in another EU member state     | Reverse-charged EU acquisition (*EU-erhvervelse*); self-accounted Danish VAT  |
| 4    | Non-EU Import — VAT exclusive                     | Seller outside the EU                                | Import VAT and any customs duty at the border; *registreringsafgift* on first Danish registration |
| 5    | Other                                             | TBA                                                  | TBA                                                                           |

## 1. Domestic Used Vehicle — VAT inclusive

**When it applies.** Purchase from a Danish VAT-registered seller (typically another dealer or a business disposing of a fleet vehicle) where the seller is charging full Danish VAT on the sale price.

**Purchase side.** Input VAT on the purchase invoice is fully deductible, assuming the vehicle is bought for resale (not for the workshop's own use).

**Sale side.** Output VAT applies to the full sale price.

## 2. Domestic Used Vehicle — VAT exclusive (Margin Scheme)

**When it applies.** Purchase from a party who did not — and could not — charge VAT, typically a Danish private citizen, or from another dealer who is themselves selling under the margin scheme.

**Purchase side.** No input VAT is recoverable (none was charged). The purchase is documented via an *afregningsbilag* — a self-billing settlement note — modelled in this system as a sales credit note. See [Purchase via credit note](../50-processes/README.md).

**Sale side.** The vehicle is sold under the Danish second-hand goods margin scheme (*brugtmomsordningen*, Momsloven §§69–71). Output VAT is calculated on the **profit margin** (sale price − total purchase cost), not on the full sale price. The sales invoice must not show VAT as a separate line; instead it carries the statutory margin-scheme note.

**Constraint.** A vehicle bought under the margin scheme cannot subsequently be sold under full-VAT rules, and vice versa.

## 3. EU Import — VAT exclusive

**When it applies.** Purchase from a VAT-registered seller in another EU member state, who is invoicing without VAT under the EU intra-Community supply rules.

**Purchase side.** The workshop self-accounts for Danish acquisition VAT under the reverse-charge mechanism (*EU-erhvervelse*) — both an output and a matching input VAT entry are made in the same VAT return, typically netting to zero.

**Sale side.** Treated as a standard Danish sale once the vehicle is in stock — full output VAT, in most cases.

**Caution.** Vehicles that meet the EU definition of a *new means of transport* (under 6 months old or under 6,000 km at acquisition) fall under a special regime — VAT is always due in the country of destination regardless of the seller's status. This case is flagged here and to be documented in detail later.

**Registreringsafgift.** Due on first Danish registration if the vehicle has not previously been registered in Denmark. Treated as a **pass-through cost** — tracked separately on a dedicated JobCard (see [Job Card](../30-domain-model/job-card.md)), **not** absorbed into the equipment's carrying value, and re-billed to the buyer on a dedicated outlay line of the sales invoice (see [Product](../30-domain-model/product.md)).

## 4. Non-EU Import — VAT exclusive

**When it applies.** Vehicle imported from outside the EU customs territory.

**Purchase side.** Customs handling at the EU border: customs duty (where applicable) and **import VAT** are paid on the customs-cleared value. Both are recorded — duty as part of acquisition cost, import VAT as deductible input VAT.

**Sale side.** Standard Danish sale; full output VAT.

**Registreringsafgift.** Due on first Danish registration. For imported used vehicles this is typically a substantial amount. It is **not** absorbed into the equipment's carrying value — instead it is tracked as a **pass-through cost** on a dedicated JobCard (see [Job Card](../30-domain-model/job-card.md)) and re-billed to the buyer on a dedicated outlay line of the sales invoice (see [Product](../30-domain-model/product.md)). Not VAT, not deductible.

## 5. Other

Placeholder for purchase scenarios that do not fit the four above (e.g. demonstration vehicles, in-kind contributions, leased-vehicle buy-outs). To be documented.

## Open questions / to expand

- The mapping from each `BIL-LAGx` and `BIL-xx` SKU (see [Product](../30-domain-model/product.md)) to the purchase type above.
- A worked example per purchase type showing purchase document → stock value → sales document → ledger postings.
- Margin scheme: handling of period-based margin calculation vs item-by-item (Danish rules allow both under conditions).
