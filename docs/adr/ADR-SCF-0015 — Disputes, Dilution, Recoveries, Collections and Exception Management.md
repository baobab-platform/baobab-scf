# ADR-SCF-0015 — Disputes, Dilution, Recoveries, Collections and Exception Management

**Status:** Proposed — pending independent acceptance  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Scope:** Partner-led SCF evidence/orchestration; separate external finance, Payments, ERP and source-domain authority  
**Dependencies:** Proposed ADR-SCF-0001; relevant prior SCF decisions; accepted Shared/CP/IAM/Payments/ERP/Trade/Trade Docs/TMS and Regulations contracts where applicable  

> No executable SCF system, financial licence, bank account, qualified partner, legal credit decision, artificial intelligence product or production acceptance is demonstrated by this ADR.


## 1. Decision
SCF coordinates discrepancy/dispute cases and observes **legally authorised external** recovery/collections actions; it does not create its own collections agency, repossess collateral or autonomously collect debt. Represent commercial disputes (buyer-supplier), document discrepancy, finance-provider dispute, credit-note/dilution, buyer nonpayment, fraud allegation and money movement exception as distinct categories and roles.

## 2. Case and event model
| Concept | Who can assert and decide |
|---|---|
| CommercialDisputeRef | Trade commercial parties, source case and amount |
| ERPAdjustmentRef | ERP credit note, deduction, invoice correction and posted effect |
| DocumentaryDiscrepancyRef | Trade Docs mismatch and exact DocumentVersion |
| FunderClaim/RecourseNotice | externally issued notice under actual contract |
| ServicerCollectionStatus | qualified external servicer, obligation and time |
| RecoveryObservation | source provider/ERP money receipt, amount/currency, allocation |
| ExceptionResolution | source outcome, reviewer, remediation, precedence and audit |

A buyer rejecting damaged goods is not automatically a defaulted loan. A supplier credit note may dilute receivables purchase value under contract, but SCF cannot unilaterally reprice the legal finance agreement. Insurance coverage, security enforcement, judicial claims and collections require their own competent actors.

## 3. Workflows
~~~mermaid
flowchart TD
 A["Discrepancy or adverse financial observation"] --> B["Classify source, obligation, participants and materiality"]
 B --> C["Preserve original source facts and freeze unsafe new actions"]
 C --> D["Notify entitled partner and commercial owner"]
 D --> E["Source-owner/servicer/bank review"]
 E --> F["Corrective source decision or new version"]
 F --> G["SCF reconciliation and explicit closure"]
~~~

Statuses: OPEN -> TRIAGED -> AWAITING_SOURCE -> PLAN_ACCEPTED -> RECONCILING -> RESOLVED, with DISPUTED/ESCALATED/CLOSED_UNRESOLVED as explicit outcomes. Exceptions can coexist with valid funding and must not rewrite contracts, funder decisions or original invoices.

## 4. Privacy, legal and human controls
Claims of fraud are allegations until evidence supports an authorised conclusion; do not auto-flag public supplier profile as fraudulent. Communication must follow permitted parties and contracts. Debtor contact, collections conduct, bankruptcy, consumer protection and nonpayment notices are jurisdiction-specific. A pending dispute may block new funding submissions without stopping lawful payments due under an existing agreement absent source legal authority.

## 5. Failure cases and rejected shortcuts
Duplicate collections notification, credit note after financing, supplier insolvency, returned payout, wrong assignment holder, undelivered claim notice, disputed fee, corrupted POD, conflicting debtor statement. Reject automatic debt collection, exposure write-off in SCF, direct mutation of ERP receivable, and rule "delivery late => borrower default".

## 6. Implementation gates
| Gate | Acceptance |
|---|---|
| SCF-DSP-01 | Exception categories and authority matrix, link to source owner |
| SCF-DSP-02 | Partial credit, dilution, recourse, debtor objection and source follow-up fixtures |
| SCF-DSP-03 | Human adjudication/dual-control, retention, notice and privilege tests |
| SCF-DSP-04 | Funder/ERP/Trade Docs/TMS repair and idempotent reconciliation |
| SCF-DSP-05 | Real partner and market-specific collections/claim rights review |

## 7. Review trigger
New regulated collections actor, insurer, product-specific recourse or insolvency regime requires separate review.

## 8. Cross-cutting safeguards, alternatives and review practice

Every consequential operation must bind IAM subject and CP trusted tenant, legal entity, market and relationship; never trust client-supplied actor or tenant labels. Protect commercial and personal finance data through scoped disclosures, encryption, classifications and retention review. Use precise source-owned CrossEngineObjectReference, immutable observations with original occurred_at/recorded_at, and auditable versioned rules. For external financial side effects, preserve uncertain outcomes and reconcile rather than retry blindly. Reject treating SCF as a loan ledger, credit committee, customs agency, payment processor, document issuer or hidden cross-subsidiary information channel.

**Review requirements:** record actual external legal/partner evidence with dated URL and contract revision; separate current approved reality from target architecture; preserve previously issued source facts; perform security and source-authority tests. Acceptance of this Proposed ADR does not register canonical capabilities/events or establish CP certification. Each gate must map to future code, tests, deployment and independent legal/business approval, with explicit unsupported operations and revisit triggers.
