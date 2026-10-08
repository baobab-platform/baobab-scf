# ADR-SCF-0004 — Canonical FinancingCase, Programme, Offer and Position Domain Model

**Status:** Proposed — not automatically Accepted by PR merge  
**Date:** 2026-10-08  
**Repository:** `baobab-platform/baobab-scf`  
**Programme:** Foundation and product architecture; ADR-SCF-0001 remains the controlling **Proposed** foundational charter  
**Operating premise:** Partner-led, headless, self-hosted and provider-neutral; no lending, underwriting, funds custody, payment execution or financial ledger authority for SCF  
**Shared authority:** `baobab-platform/shared` controls wire contracts, canonical capability/event semantics; CP controls tenant/binding/certification; IAM controls identity.  

> **Decision status vs evidence:** this is an engine-local architecture *proposal*. No source runtime, licensed finance activity, external funding relationship, market permission, capability certification or production-readiness claim follows from this ADR.


## 1. Decision
Mint SCF-local opaque identities for **FinancingCase**, **ProgrammeParticipation**, **FundingApplication**, **FunderDecisionObservation**, **FinancingOffer**, **AcceptanceEvidenceReference**, **FundingObservation**, **PositionProjection**, **FinancingException** and **Claim/EncumbranceObservation**. SCF does not mint ERP invoice/receivable IDs, Trade orders, payment IDs, TradeDocument IDs, lender loan IDs or CP canonical organisations. Original issuer/provider native numbers belong in ExternalReference.

~~~mermaid
flowchart TD
  O["ERP obligation and Trade source"] --> C["FinancingCase"]
  C --> E["Pinned EvidenceManifest"]
  C --> P["FinancingProgramme reference"]
  P --> A["FundingApplication attempt(s)"]
  A --> D["FunderDecision observation"]
  D --> F["FinancingOffer revision"]
  F --> X["Consent/acceptance evidence"]
  X --> M["External funding/payment observation"]
  M --> S["Reconciled PositionProjection"]
  C --> I["Exception and conflict history"]
~~~

## 2. Identity/cardinality table
| SCF object | Scope / cardinality | Non-equivalence |
|---|---|---|
| FinancingCase | case_id, tenant/legal entity, product profile, obligation allocation, participants; many revisions | A loan/underwriting decision |
| FinancingProgramme | programme_id, buyer/funder and market participation, terms profile/version | CapabilityProvider or funding guarantee |
| ObligationAllocation | subset of authoritative ERP obligation, amount, currency, due date and effective state | ERP balance or invoice record |
| EvidenceManifest | immutable manifest_id/version, pinned external reference digests and check results | Original TradeDocument |
| FundingApplication | case/attempt/provider/programme, requested amount/terms, submitted snapshot/digest | A credit offer |
| FunderDecisionObservation | provider message/assurance/decision/time/conditions, exact source | SCF-computed underwriting |
| FinancingOffer | offer_id, funder external ID, version, terms, validity, recourse, fees, currency | Accepted or settled contract |
| AcceptanceEvidence | actor, authority, document version, signature/consent and time | legal agreement by merely clicking |
| Funding/SettlementObservation | exact provider/payment/ERP reference, amount, currency, time, status/provenance | SCF disbursement ledger |
| PositionProjection | aggregate view reconstructed from source terms/payments/ERP/partner confirmations | Authoritative outstanding loan ledger |

## 3. Independent lifecycle axes
Case: INITIATED -> EVIDENCE_GATHERING -> ELIGIBILITY_READY -> OFFER_SEARCH -> OFFER_PRESENTED -> ACCEPTANCE_PENDING -> AWAITING_FUNDING -> OBSERVED_FUNDED -> RECONCILING -> CLOSED, with REJECTED/WITHDRAWN/EXCEPTION. Each subordinate axis is independent: funder decision PENDING/APPROVED/DECLINED/REVIEW, offer VALID/EXPIRED/WITHDRAWN/ACCEPTED, acceptance UNCONFIRMED/CONFIRMED/DISPUTED, payout REQUESTED/PROCESSING/UNKNOWN/SUCCEEDED/RETURNED, ERP reconciliation MATCHED/UNMATCHED/DISPUTED. **Never** set case "FUNDED" simply on offer acceptance or payment initiation.

## 4. Allocation, concurrency and history
An obligation can support multiple possible applications only under explicit programme semantics; attempts/offer competition must not create multiple financeable allocations without conflict checks. Preserve portion financed, retained, repaid, disputed and released with decimal-safe currency. Position projection uses partner source confirmations and ERP records; negative balances or conflicting sources create exceptions, not a fabricated balance. Corrections append observations. Current-state projection must be replayable by pinned algorithm version.

## 5. Commands versus external facts
Illustrative SCF-local commands: create-case, attach-evidence, submit-funder-application, accept-offer-intent, record-third-party-acceptance, reconcile-funding. Candidate events and capability names **not canonical** until Shared reviews stewardship and contract. A command is not an outcome: external partner receipt is not approval, approval is not funding, funding is not final settlement.

## 6. Failure and rejected shortcuts
Unknown external partner result, same event ID/different digest, invoice partially credited, offer expiry between consent and callback, cross-tenant reference and dual offer accept race must be explicit. Reject one mutable `status` string, a single universal "loan" root, inferred loan account based on case ID and last-write-wins funder webhooks.

## 7. Independent implementation gates
| Gate | Evidence |
|---|---|
| SCF-DOM-01 | Domain identities and state machine specs with shared-reference owner alignment |
| SCF-DOM-02 | Multi-offer/multi-attempt and partial obligation allocation fixtures including concurrency |
| SCF-DOM-03 | Append-only funder/payment facts and deterministic position replay |
| SCF-DOM-04 | Tenant context and cross-obligation permission isolation |
| SCF-DOM-05 | Synthetic approval-but-unfunded, payout-unknown and returned-payment full journey |

## 8. Revision triggers
Review if portfolio servicing, multi-funder participation or securitisation requires new aggregate boundaries; do not rewrite core IDs. Retain ADR-SCF-0001 authority and use [Shared cross-engine reference semantics](https://github.com/baobab-platform/shared/tree/main/contracts/cross-engine-reference/v1).

## 9. Non-negotiable cross-engine controls and acceptance

SCF must authenticate caller via IAM and redeem caller-bound CP context, authorise each source object/counterparty and store tenant/legal-entity scope in durable state and workers. Exact external references remain source-owned; no direct writes to ERP, Trade, Payments, Trade Docs, TMS, Regulations or partner databases. Sensitive evidence is purpose-minimised, encrypted, auditable and non-replicated across sibling tenants. Provider callbacks are independently authenticated, replay-safe and provenance-bearing; an HTTP acknowledgement is not funding. Canonical `financing.*` or `scf.*` keys and events are **illustrative until Shared-approved** and must never be declared as implemented or active by merging this ADR.

**Review protocol:** when evidence, market rules or provider terms change, add an explicit dated amendment/superseding ADR recording the triggering fact, impacted rules, consumer migration and backward-compatibility plan. Keep the original accepted source facts and issued financial documents immutable. Implementation gates require exact code, fixtures, failure tests, known deferrals and review sign-off, followed by independent EA-09/Control Plane activation only for proven product/market/provider operations.
