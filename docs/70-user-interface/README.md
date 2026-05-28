# 70 — User interface

The goal of the Vehicle Trade UI is to make the **common case fast** and the **uncommon case possible**:

- The simplest, most common trades should require only the minimum information, with sensible defaults filled in.
- The UI must also expose the controls needed for less common trades — imports, margin-scheme handling, export sales, fleet onboarding, post-sale warranty.
- For every completed trade the UI must surface links to every document generated (purchase invoice, credit note, Job Cards, sales invoice, accounting postings) so an accountant can review the full chain.

## UI areas

| #   | Area                                  | Purpose                                                                              |
| --- | ------------------------------------- | ------------------------------------------------------------------------------------ |
| 1   | Buy equipment                         | Capture a purchase, choose purchase type, generate purchase document                 |
| 2   | Perform repair or warranty work       | Open and complete a Job Card against a stock or sold equipment                       |
| 3   | Sell equipment                        | Pick a stock equipment, choose the sales tax classification, generate sales invoice  |
| 4   | Review inventory                      | List, filter, and inspect vehicles currently in stock; drill into per-vehicle history|
| 5   | Vehicle trade-in                      | Combined workflow when the buyer is trading in a vehicle as part-payment             |

---

## 1. Buy equipment

(To expand.) Entry point for all purchase types. The user first identifies the seller as VAT-registered or private; the system chooses between the purchase-invoice route and the credit-note (*afregningsbilag*) route accordingly. The selected purchase type — domestic / EU import / non-EU import / margin / other — drives the rest of the form.

## 2. Perform repair or warranty work

(To expand.) Opens a Job Card against a stock equipment (workshop is customer) or against a sold equipment for warranty (workshop is still customer). For non-warranty work on a sold vehicle the UI should redirect the user into the standard customer-equipment service workflow.

## 3. Sell equipment

(To expand.) Lists `Stock` equipment. The sale form requires the buyer, the sale price, and the sale-side classification (private / business / export / margin) — the latter selects the appropriate `BIL-xx` sales product. Constraints from the original purchase type (e.g. margin-scheme vehicles cannot be sold under full-VAT) must be enforced.

## 4. Review inventory

(To expand.) Aggregate view (counts and total value per purchase-type bucket), drill-down to the per-vehicle list, and a further drill-down to a single vehicle's full history — purchase document, repairs, current carrying value, days in stock, expected margin.

## 5. Vehicle trade-in

(To expand.) Combined workflow when a buyer is trading their old vehicle in part-payment for one of the workshop's stock vehicles. Produces a sales invoice for the outgoing vehicle and a purchase document for the incoming vehicle (credit note for a private trade-in, purchase invoice for a business trade-in), with the net cash amount as the balancing entry.

## Open questions / to expand

- Wizard-style vs single-form UX for the buy and sell flows.
- Permissions and approval steps (who can approve a margin-scheme purchase, who can void a draft, etc.).
- Bulk operations for the "Add equipment to stock" onboarding case.
