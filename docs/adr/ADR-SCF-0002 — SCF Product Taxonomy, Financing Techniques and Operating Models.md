# ADR-SCF-0002 — SCF Product Taxonomy, Financing Techniques and Operating Models

**Status:** Proposed — not automatically Accepted by PR merge  
**Date:** 2026-10-08  
**Repository:** `baobab-platform/baobab-scf`  
**Programme:** Foundation and product architecture; ADR-SCF-0001 remains the controlling **Proposed** foundational charter  
**Operating premise:** Partner-led, headless, self-hosted and provider-neutral; no lending, underwriting, funds custody, payment execution or financial ledger authority for SCF  
**Shared authority:** `baobab-platform/shared` controls wire contracts, canonical capability/event semantics; CP controls tenant/binding/certification; IAM controls identity.  

> **Decision status vs evidence:** this is an engine-local architecture *proposal*. No source runtime, licensed finance activity, external funding relationship, market permission, capability certification or production-readiness claim follows from this ADR.


## 1. Question, decision and product strategy
Which techniques can SCF coordinate without representing every arrangement as a loan? **Adopt a typed, extensible FinancingTechnique and OperatingModel**, including RECEIVABLES_PURCHASE, APPROVED_PAYABLES_FINANCE, BUYER_FUNDED_DYNAMIC_DISCOUNT, ADVANCE_AGAINST_RECEIVABLES, PRE_SHIPMENT_FINANCE, INVENTORY_BACKED_FINANCE, LOGISTICS_RECEIVABLE_FINANCE and DOCUMENTARY_INSTRUMENT_REFERENCE. Each technique has separate **FinancingProductProfile**, proof requirements, pricing basis, legal form, recourse and authorised provider coverage; the generic case does not dictate lending semantics. Only separately reviewed profiles may be activated.

## 2. Taxonomy and authority
| Technique | Typical trigger/counterparty | Nature of finance and source authority | Initial choice |
|---|---|---|---|
| Receivables discounting/factoring | Verified ERP receivable; seller/factor/debtor | Purchase or financing of receivable; external factor decides legal transfer | Candidate first slice |
| Buyer-led approved payables | Buyer-approved ERP payable; supplier/anchor/funder | Funder offers supplier early payment against eligible approved payable | **Preferred if anchor buyer commits** |
| Dynamic discounting | Buyer-funded early payment | Buyer and supplier agree modified commercial payment terms; not automatically a third-party loan | Conditional |
| Pre-shipment/PO finance | Purchase order, supplier production contract | External funder assumes performance/sourcing risks, not yet earned receivable | Later |
| Inventory/warehouse receipt | Verified quantity/custody/security interests | Collateral/loan or inventory facility under market-specific law | Later |
| Logistics invoice | Accepted freight obligation and TMS verified delivery | Freight receivable finance, no GPS-as-collateral fiction | Second wave |
| Letter of credit/guarantee/forfaiting | Bank-issued documentary instrument | Bank/issuer authority and scheme-specific rules | Later provider adapter |

**OperatingModel** is independent from product type: PARTNER_LED_ORCHESTRATION (default), BUYER_FUNDED_DISCOUNT, LICENSED_THIRD_PARTY_MARKETPLACE, and FUTURE_AUTHORISED_INTERNAL_FINANCE. Selecting a technique does not imply an operating-model licence. Recognise recourse/nonrecourse, disclosed/confidential factoring, maturity, debtor notification, seller vs buyer fee responsibility and product applicability as separate dimensions.

## 3. Eligibility vs funding
A case with correct document evidence is not fundable unless qualified partner capacity, buyer/supplier mandate, contract/assignment status, specific market and consent permit. Source Trade payment terms are not a credit decision. An estimated advance rate is not a binding offer; an offer is not funded; funded is not settled. Avoid promise of guaranteed funding or automatic capital availability.

~~~mermaid
flowchart LR
  A["Eligible commerce/ERP obligation"] --> B["SCF product-profile + partner programme"]
  B --> C["Evidence/consent and risk controls"]
  C --> D["Qualified external funder decision"]
  D --> E["Versioned offer"]
  E --> F["Acceptance and payment observation"]
  F --> G["ERP/Payments reconciliation"]
~~~

## 4. Product configuration
A product profile pins technique, terms vocabulary, fee basis, permitted currencies, advance/discount parameters, recourse, affected legal entities/markets, provider capability matrix, policy version, eligibility facts, mandatory documents, data-sharing limits, disclosure/consent, and lifecycle template. Configure as **reviewed rules, not arbitrary tenant scripting**. Future additions require schema-evolution conformance and cross-owner authority review; do not mutate historical case product versions after programme upgrade.

## 5. Alternative decisions
Reject a universal `Loan` aggregate, one generic interest formula, marketplace routing before funder onboarding, any product that demands platform escrow by default, and a user-selected "Financing type" string that bypasses provider/market eligibility. Commercial SaaS billing, referral compensation, transaction fees and funder revenue-sharing are distinct business/legal arrangements, not assumed rights.

## 6. Independent implementation gates
| Gate | Objective exit evidence |
|---|---|
| SCF-PROD-01 | Technique dictionary with GSCFF terminology mapping and explicit funding/legal nature |
| SCF-PROD-02 | Versioned ProductProfile, OperatingModel and qualified provider eligibility fixtures |
| SCF-PROD-03 | Distinct payables/receivables/PO/inventory negative tests; no synthetic loan authority |
| SCF-PROD-04 | Partner/buyer/supplier consent, recourse, fee responsibilities and term comparison fixtures |
| SCF-PROD-05 | One documented pilot commercial proposition with real external interest and legal review |

## 7. Revision triggers and references
Reconsider classification if GSCFF definitions, operating laws, agreed partner contracts, IFRS accounting interpretation or an approved new product require a distinct legal right or risk structure. **Source:** [GSCFF standard definitions](https://supplychainfinanceforum.org/glossary/) and [ICC terminology](https://iccwbo.org/news-publications/policies-reports/standard-definitions-techniques-supply-chain-finance/). Product economics, demand and market availability remain **hypotheses**, not verified SCF revenue.

## 8. Non-negotiable cross-engine controls and acceptance

SCF must authenticate caller via IAM and redeem caller-bound CP context, authorise each source object/counterparty and store tenant/legal-entity scope in durable state and workers. Exact external references remain source-owned; no direct writes to ERP, Trade, Payments, Trade Docs, TMS, Regulations or partner databases. Sensitive evidence is purpose-minimised, encrypted, auditable and non-replicated across sibling tenants. Provider callbacks are independently authenticated, replay-safe and provenance-bearing; an HTTP acknowledgement is not funding. Canonical `financing.*` or `scf.*` keys and events are **illustrative until Shared-approved** and must never be declared as implemented or active by merging this ADR.

**Review protocol:** when evidence, market rules or provider terms change, add an explicit dated amendment/superseding ADR recording the triggering fact, impacted rules, consumer migration and backward-compatibility plan. Keep the original accepted source facts and issued financial documents immutable. Implementation gates require exact code, fixtures, failure tests, known deferrals and review sign-off, followed by independent EA-09/Control Plane activation only for proven product/market/provider operations.
