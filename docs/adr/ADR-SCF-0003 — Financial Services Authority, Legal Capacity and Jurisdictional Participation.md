# ADR-SCF-0003 — Financial Services Authority, Legal Capacity and Jurisdictional Participation

**Status:** Proposed — not automatically Accepted by PR merge  
**Date:** 2026-10-08  
**Repository:** `baobab-platform/baobab-scf`  
**Programme:** Foundation and product architecture; ADR-SCF-0001 remains the controlling **Proposed** foundational charter  
**Operating premise:** Partner-led, headless, self-hosted and provider-neutral; no lending, underwriting, funds custody, payment execution or financial ledger authority for SCF  
**Shared authority:** `baobab-platform/shared` controls wire contracts, canonical capability/event semantics; CP controls tenant/binding/certification; IAM controls identity.  

> **Decision status vs evidence:** this is an engine-local architecture *proposal*. No source runtime, licensed finance activity, external funding relationship, market permission, capability certification or production-readiness claim follows from this ADR.


## 1. Decision
Treat every financial action as a **regulated/constrained activity whose legitimacy comes from market-specific legal analysis, contractual roles and actual permissions**, not from the existence of software or CP registration. SCF coordinates qualifying partners and evidence. It **does not** by default lend, arrange regulated credit, hold funds, issue securities, receive deposits, underwrite, create security interests, collect for a lender or execute bank transfers.

## 2. Authority matrix
| Activity | Expected authority/source | SCF's permissible default |
|---|---|---|
| Commercial invoice and term | Trade/ERP and contracting seller/buyer | Reference fact, do not change receivable or amount |
| Funder underwriting and risk appetite | Licensed/authorised funder under contract | Display source-stamped external determination |
| Broker/intermediary activity | Authorised market participant as applicable | No arranging/referral fees without review |
| Offer issuance and financing agreement | Authorised funder, borrower and binding contract | Orchestrate transmission/consent; preserve evidence |
| Assignment/perfection/security | Parties and legally competent registry/authority | Observe verified external assignment/encumbrance refs |
| Disbursement, collection and remittance | Funder and Payments' approved operator scope | Observe/ref payment operation, not execute from SCF |
| Loan servicing, arrears, restructuring | Qualified financial institution/servicer | Store source status, exceptions and workflow links |
| Credit reporting and credit bureau data | Licensed/legally permitted counterparties | No unlicensed credit-data brokerage |
| Data privacy, sanctions, AML duties | Actors required by jurisdiction and arrangement | Enforce assigned obligations and escalation; do not claim institutional compliance globally |

## 3. Country participation matrix
Begin with **South Africa and Uganda** as candidates; each LegalParticipationProfile must identify product, legal form, lenders/borrowers/resident status, domestic/cross-border scenario, entity role, intermediary status, credit/data/AML/payment regimes, consumer vs business treatment, compulsory notices, registration/licence scope, counsel approval, contractual capacity, effective date and monitoring owner. Do not assume a South African CIPC company may lend/arrange credit; incorporation and regulatory permission differ. Do not assume a Uganda UMRA money-lender licence covers banking, payment or securities products. If the financial activity falls outside a regime, record the reason/counsel source rather than declaring "unregulated".

~~~mermaid
flowchart TD
 A["Product + legal entity + jurisdiction + activity"] --> B["Counsel-approved legal participation profile"]
 B --> C{"Mandate/licence/partner authority current?"}
 C -->|No / Unknown| D["BLOCK external regulated operation"]
 C -->|Yes| E["CP/SCF entitlement, qualified partner and approved contract"]
 E --> F["Authorised product workflow"]
 F --> G["Periodic revalidation and audit"]
~~~

## 4. Review semantics and change
A permission is **time-, activity-, organisation-, market- and programme-scoped**. The system records external register evidence/licence status and verification time but does not certify the licence independently. If licensing, sanctions designation, partner mandate or product perimeter changes, close new applications, preserve existing rights/historical cases and route impacted cases to legal/partner review. Future subsidiaries are autonomous; Nabhold parent affiliation does not confer finance authority on ZuriBeans or Thamani.

## 5. Alternatives rejected
Reject a binary \`licensed=true\`, global "Africa eligible" flag, SCF admin granting itself permission to lend, one market's law copied to another, case acceptance used as legal contractual acceptance, and connecting to payment operator without appropriate onboarding.

## 6. Independent implementation gates
| Gate | Proof |
|---|---|
| SCF-LEG-01 | Legal-role and regulated-activity inventory per product/jurisdiction and external partner |
| SCF-LEG-02 | Effective-dated participation records, market/entity/mandate scope and expiry denial tests |
| SCF-LEG-03 | Ugandan and South African independent local counsel review for **actual proposed activity** |
| SCF-LEG-04 | No-authorisation, revoked-funder, wrong market/entity and role-escalation negative fixtures |
| SCF-LEG-05 | Live programme only after signed contracts, external regulated actor proof and independently approved admission |

## 7. Revision triggers and references
Reopen on new law, supervisory interpretation, licensing expansion, change in charging/referral model, cross-border servicing, customer class or new country. **References, not legal opinions:** [Uganda Microfinance Regulatory Authority](https://umra.go.ug/service/regulation/), [Uganda money-lender guidance](https://umra.go.ug/what-are-the-regulatory-dos-and-donts-for-money-lending/), [South Africa National Credit Act](https://www.justice.gov.za/mc/vnbp/act2005-034.pdf). Qualified counsel must verify current status and scope.

## 8. Non-negotiable cross-engine controls and acceptance

SCF must authenticate caller via IAM and redeem caller-bound CP context, authorise each source object/counterparty and store tenant/legal-entity scope in durable state and workers. Exact external references remain source-owned; no direct writes to ERP, Trade, Payments, Trade Docs, TMS, Regulations or partner databases. Sensitive evidence is purpose-minimised, encrypted, auditable and non-replicated across sibling tenants. Provider callbacks are independently authenticated, replay-safe and provenance-bearing; an HTTP acknowledgement is not funding. Canonical `financing.*` or `scf.*` keys and events are **illustrative until Shared-approved** and must never be declared as implemented or active by merging this ADR.

**Review protocol:** when evidence, market rules or provider terms change, add an explicit dated amendment/superseding ADR recording the triggering fact, impacted rules, consumer migration and backward-compatibility plan. Keep the original accepted source facts and issued financial documents immutable. Implementation gates require exact code, fixtures, failure tests, known deferrals and review sign-off, followed by independent EA-09/Control Plane activation only for proven product/market/provider operations.
