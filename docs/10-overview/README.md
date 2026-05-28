# 10 — Overview

## Scope

This documentation describes how vehicle trades are handled inside the X2 Automotive Workshop System for workshops operating in Denmark. The aim is to simplify what is, in practice, a layered transaction:

- **Processing the trade** — purchase cost, sales proceeds, inventory carrying value, pre-sale repairs, and warranty obligations.
- **Accounting for the trade** — Danish VAT rules (full-VAT vs the second-hand goods margin scheme / *brugtmomsordningen*), valuation of inventory, recognition of profit or loss, and posting to an external accounting system.
- **Capturing the transaction in the UI** — a short record path for the common cases, with the controls needed for the less common ones (private-party purchase, EU acquisition, non-EU import, fleet-purchase onboarding, etc.).

## Terminology: Equipment = Vehicle

The X2 platform models physical assets through a generic **Equipment** entity. In the Vehicle Trade subsystem an Equipment row is *always* a road vehicle, so the terms *Equipment* and *Vehicle* are used interchangeably in this documentation. The canonical entity name in the data model remains *Equipment*.

The same Equipment table is also used elsewhere in X2 to track **customer vehicles** (vehicles owned by a workshop's customer, brought in for service). The Vehicle Trade subsystem introduces a second use of the entity — **trade equipment** — which is the workshop's own inventory of vehicles for resale. The two are distinguished by how the row is created and by what its code looks like; see [Equipment](../30-domain-model/equipment.md).

## Out of scope

- General workshop service operations, which are covered elsewhere in the X2 product documentation.
- Accounting bookkeeping itself — X2 *posts* ledger entries through an external accounting API but does not maintain ledgers. See [Accounting](../60-accounting/README.md).
- Markets outside Denmark. The other Nordic markets (Sweden, Norway, Finland, Iceland) will reuse much of the data model but require their own tax-rule overlays; those overlays are out of scope for this document.
