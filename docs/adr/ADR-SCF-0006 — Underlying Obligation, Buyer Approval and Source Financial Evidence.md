# ADR-SCF-0006 — Underlying Obligation, Buyer Approval and Source Financial Evidence

**Status:** Proposed — not automatically Accepted by PR merge  
**Date:** 2026-10-08  
**Repository:** `baobab-platform/baobab-scf`  
**Programme:** Foundation and product architecture; ADR-SCF-0001 remains the controlling **Proposed** foundational charter  
**Operating premise:** Partner-led, headless, self-hosted and provider-neutral; no lending, underwriting, funds custody, payment execution or financial ledger authority for SCF  
**Shared authority:** `baobab-platform/shared` controls wire contracts, canonical capability/event semantics; CP controls tenant/binding/certification; IAM controls identity.  

> **Decision status vs evidence:** this is an engine-local architecture *proposal*. No source runtime, licensed finance activity, external funding relationship, market permission, capability certification or production-readiness claim follows from this ADR.


## 1. Decision
**ERP is the source of financial receivables, payables, posted invoice status, credit memos and accounting balances.** Trade owns accepted orders, contracts and commercial terms. SCF is permitted to model a **FinancingObligationReference** and **ObligationEligibilitySnapshot** that point to exact source versions/periods; it cannot create a parallel invoice system, amend debtor balances or recognise income/expense.

## 2. Obligation components
| Item | Required source facts | Invalid inference |
|---|---|---|
| ObligationReference | source engine + typed immutable/pinned object ref, obligor, creditor, legal entity, market | PDF upload itself is a posted receivable |
| AmountSnapshot | gross/net, currency, tax, due date, outstanding balance at observation time, source/version | candidate case amount equals presently eligible amount |
| BuyerApprovalObservation | who confirmed what amount/services/invoice, when, authority/contract/assurance | supplier click certifies buyer acceptance |
| Dispute/CreditNoteObservation | source invoice adjustment, quantity dispute, effective date, debtor claim | old eligible amount persists after dilution |
| Assignment/EncumbranceRef | checked third-party ownership/priority evidence and scope | Baobab has globally verified no competing claims |
| Payment/SettlementReference | source Payments/ERP id, amount, currency, time, status | transfer initiated means receivable discharged |
| EligibilitySnapshot | evaluated source versions, programme criteria, freshness and reason | binding finance decision |

## 3. Evidence pinning and staleness
Read exact ERP invoice/receivable and Trade acceptance reference through supported APIs using Shared CrossEngineObjectReference; preserving owner, object_type, scope and the permitted pinning mode. The snapshot includes observed_at and max staleness policy. At **submission, offer acceptance and funding request** check relevant live source constraints again, without mutating already submitted funder evidence. Later credit notes and revised due dates trigger a reconciled exception and provider notification where contractually required.

**Buyer-approved payable** differs from posted invoice, seller claim, goods received or commercial order. The party with approval power must be verified under buyer's workflow and participant mandate. A buyer objection later is not erased because a funder previously accepted the application.

## 4. Concurrency and double-use
Reserve **within SCF's observable domain** a case/obligation/amount allocation that prevents local inconsistent duplicate submissions. Track source obligation total, funded portion, pending requests, retained amount and released/rejected allocations in decimal-safe currency. Local idempotency cannot prove another bank did not purchase the same receivable; ADR-0008 requires independent assignment/registry confirmations.

~~~mermaid
flowchart TD
 A["ERP authoritative obligation"] --> B["Source snapshot + exact revision"]
 C["Buyer approval evidence"] --> B
 B --> D["SCF eligibility and anti-conflict"]
 D --> E["External funder application"]
 E --> F["Change observation: paid/credited/disputed"]
 F --> G["Revalidate / notify / reconcile"]
~~~

## 5. Adverse conditions and policies
Cross-currency assumptions require source FX and explicit obligation vs financing currency. Unknown outstanding amount, stale ERP snapshot, unposted draft, disputed invoice, false buyer approval, wrong issuer, amended Trade terms and mismatched seller entity produce DENY/REVIEW, not conditional fictitious eligibility. Intra-group invoices demand extra related-party and legal authority review; parent/subsidiary are independent parties.

## 6. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-OBL-01 | ERP/Trade source schema, source-of-truth map and id/version pinning |
| SCF-OBL-02 | Buyer approval vs invoice/contract/receipt independent state fixtures |
| SCF-OBL-03 | Credit-note, partial payment, overdue, dispute, stale version and wrong-tenant tests |
| SCF-OBL-04 | Concurrent financeable allocation and duplicate application controls |
| SCF-OBL-05 | ERP/Trade consumer conformance and audit-proof no-write-to-source |
  
## 7. Review conditions
Revisit only if ERP shifts authority through accepted architecture, source credit/receivables model changes or new product creates a distinct contractual obligation. Reference [Baobab Trade ADR-0027](https://github.com/baobab-platform/baobab-trade/tree/main/docs/adr) and [ERP](https://github.com/baobab-platform/baobab-erp).

## 8. Non-negotiable cross-engine controls and acceptance

SCF must authenticate caller via IAM and redeem caller-bound CP context, authorise each source object/counterparty and store tenant/legal-entity scope in durable state and workers. Exact external references remain source-owned; no direct writes to ERP, Trade, Payments, Trade Docs, TMS, Regulations or partner databases. Sensitive evidence is purpose-minimised, encrypted, auditable and non-replicated across sibling tenants. Provider callbacks are independently authenticated, replay-safe and provenance-bearing; an HTTP acknowledgement is not funding. Canonical `financing.*` or `scf.*` keys and events are **illustrative until Shared-approved** and must never be declared as implemented or active by merging this ADR.

**Review protocol:** when evidence, market rules or provider terms change, add an explicit dated amendment/superseding ADR recording the triggering fact, impacted rules, consumer migration and backward-compatibility plan. Keep the original accepted source facts and issued financial documents immutable. Implementation gates require exact code, fixtures, failure tests, known deferrals and review sign-off, followed by independent EA-09/Control Plane activation only for proven product/market/provider operations.
