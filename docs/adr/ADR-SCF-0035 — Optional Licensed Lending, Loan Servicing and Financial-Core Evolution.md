# ADR-SCF-0035 — Optional Licensed Lending, Loan Servicing and Financial-Core Evolution

**Status:** Proposed — future-conditional and deferred activation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Prerequisites:** Proposed SCF charter/architecture, separate signed partner/legal market authority, existing Shared/CP/IAM/ERP/Payments/Trade Docs boundaries  

> Documentation does not grant lending, funder, payment, bank, transferable-record, collateral custody or market rights; no new mandatory stack is approved.


## 1. Decision and non-approval
SCF is **partner-led by default** under ADR-SCF-0001. A future internal lender, factor principal, arranger or servicer could be a separately authorised **legal entity and regulated business**, but this ADR **does not approve any lending, servicing, deposit-taking, regulated brokerage, loan ledger or payment activity**. Entry requires board-level economics, capital, licensing/registration where required, independent governance and qualified market-specific legal opinions.

## 2. Possible future businesses
| Activity | New authority and operational requirement |
|---|---|
| Lending originator | credit underwriting, contract, pricing, borrower protection, regulated finance mandate |
| Factoring principal | real receivable purchase, risk capital, legal assignment, recourse and collection |
| Licensed arranger | market permission, compensation/conflicts, customer/funder disclosures |
| Loan servicer | authoritative outstanding loan ledger, accruals, repayments, delinquency, recovery |
| Guarantor | contingent liability, regulatory capital/claims and accounting |
| Deposit/payment operator | specialised licence, safeguarding, prudential, bank reconciliation and payment rails |
| Portfolio distributor | risk participations, securities/intermediary rules and custody |
| Credit bureau/data processor | specific lawful credit data access, privacy, reporting and correction |

These are not interchangeable by product naming. Finance subsidiaries must have independent CP tenant/legal entity scope, contracts and limits. Nabhold parent company is not automatically guarantor of any subsidiary's funded obligations, and an internally owned lender may not receive competitor funder information by default.

## 3. Boundary of optional finance core
~~~mermaid
flowchart TD
 C["Existing partner-led SCF case orchestration"] --> P["Qualified provider-neutral interface"]
 P --> E["External finance providers (default)"]
 P -.-> L["Future separately authorised finance entity"]
 L --> K["Independent origination/servicing domain and financial core"]
 K --> R["ERP statutory accounting / bank reconciliation"]
 K --> A["Risk, audit and regulator reporting"]
~~~

A lender needs origination, servicing state, amortisation/interest calculation, collection allocation, fee/tax, impairment, recovery, contract, treasury, reserve/capital and reporting policies. FinancingCase alone is NOT a loan account. If an OSS financial core becomes relevant, separately evaluate build/adopt/partner options and licences; Apache Fineract/JVM is **not** automatically selected or a mandatory stack.

## 4. Licence, independence and bankability
**GO** only with legal entity and funding capital, licensing decisions, product/market scope, qualified staff, risk committee, governance, regulatory reporting, underwriter controls, AML/KYC, collections powers, audited financial accounting, liquidity, security and operational resilience. **NO-GO** if only code exists, a model suggests high market demand, the company is CIPC incorporated without financial permissions, or actual capital/partner appetite is unknown.

## 5. Conflicts and customer safeguards
An internal lender competing with external partner banks creates conflicts: routing neutrality, disclosure of own offer and fees, no cross-use of rival bank risk data, data firewall, independent complaints, privacy and anti-steering requirements. A borrower choosing independent funder must not be forced into internal financing to use SCF.

## 6. Financial core development condition
Plan a separate domain and technology ADR *only after* the legal business case. Compare Python/Go family, qualified OSS loan core or bank-hosted servicing systems against local law, functionality, trust, auditing, migration, license, disaster recovery, measured cost and future portability. New technology family requires approved exception, not implicit Fineract insertion.

## 7. Implementation gates (future and blocked pending legal authority)
| Gate | Evidence |
|---|---|
| SCF-LEND-01 | Board-approved feasibility, business model and risk capital |
| SCF-LEND-02 | Market regulator/licensing, entity scope, permitted activity and legal opinion |
| SCF-LEND-03 | Separate accepted loan-origination and servicing authority ADR |
| SCF-LEND-04 | Independent risk/underwriting/compliance, lending policies and consumer protection |
| SCF-LEND-05 | Audited financial core, ledger/ERP separation and bank reconciliation |
| SCF-LEND-06 | Penetration, stress/DR, liquidity, governance and regulatory acceptance |
| SCF-LEND-07 | CP/Shared per-capability certification, conflict controls and constrained pilot |

## 8. Reopen only on evidence
[Uganda UMRA overview](https://umra.go.ug/service/regulation/) and [South Africa National Credit Act](https://www.justice.gov.za/mc/vnbp/act2005-034.pdf) are **research starting points**, not advice or licence. Reopen when a genuine legal/financial institution, funder capital, regulator and board decisions exist.

## 9. Cross-cutting implementation and review protocol

IAM-authenticated principal and caller-bound CP tenant/legal entity/market context precede all cases, partner actions and data disclosures. Use source-owned cross-engine references, immutable external observations, source time/revision, purpose-scoped permissions, safe idempotency and reconciliation for UNKNOWN outcomes. Never treat funder approval, instrument issuance, received bank money, accounting balance and regulatory permission as the same status. Shared alone governs canonical capability/event semantics; CP/EA-09 registers and certifies only proven provider scopes.

**Revisit procedure:** date and cite changed law/partner/technical standards, classify the assertion as verified fact vs design inference, identify impacted old cases and contracts, map privacy/technical/financial risks, and file a superseding or amended ADR with evidence and rollback/grandfathering policy. Future gates require implementation tests and independent legal, business, security and partner approvals. No gate passes by merging this document.
