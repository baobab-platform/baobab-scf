# ADR-SCF-0014 — Financing Positions, Exposure, Reconciliation and Financial Reporting

**Status:** Proposed — pending independent acceptance  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Scope:** Partner-led SCF evidence/orchestration; separate external finance, Payments, ERP and source-domain authority  
**Dependencies:** Proposed ADR-SCF-0001; relevant prior SCF decisions; accepted Shared/CP/IAM/Payments/ERP/Trade/Trade Docs/TMS and Regulations contracts where applicable  

> No executable SCF system, financial licence, bank account, qualified partner, legal credit decision, artificial intelligence product or production acceptance is demonstrated by this ADR.


## 1. Decision
SCF maintains **read-only, provenance-bearing FinancingPositionProjection** for case/offer/obligation/programme reporting. It must not replace external funder servicing books, ERP accounts receivable/payable, general ledger, impairment/accounting rules, tax books or Payments settlement. Every position value has an as_of timestamp, origin, calculation method and independent reconciliation status.

## 2. Position dimensions
| Dimension | Required distinction |
|---|---|
| Source obligation | ERP balance, debtor, creditor, currency, due date and revision |
| Applied/approved | funder application/decision amounts, not committed funding |
| Contracted | legally accepted finance contract and assignment evidence |
| Outstanding advance | funder confirmed financing exposure, not SCF estimate |
| Funded amount | external provider payment observation and/or bank confirmation |
| Supplier receipts | beneficiary account credited vs payout requested |
| Collections | lender/servicer receipts, allocation across obligation tranches |
| Deductions | reserves, dilution, credit notes, fees and disputes |
| Recourse | accepted contract-specific potential seller liability |
| Programme utilisation | provider limit observation and exact outstanding source |
| Reporting | currency and period-specific reconciled measures, ERP-only accounting decisions |

Position version must specify which funder holds financial position, scope and contract; multiple funders with tranches cannot be collapsed into one loan balance.

## 3. Reconciliation process
~~~mermaid
flowchart LR
 F["Funder/servicer position observation"] --> R["SCF reconciler"]
 P["Payments status and bank ref"] --> R
 E["ERP obligation and journal snapshot"] --> R
 R --> S["Matched / Divergent / Unknown"]
 S --> A["Provenance-backed position projection"]
 S --> X["Exception with owner and repair workflow"]
~~~

Reconcile invoice face/credit notes, finance purchase amount, fees/reserve, actual payout, funding date, debtor collection, FX difference and serviced balance. A difference can arise from timing, price terms, settlement delay, source scope, or error; **do not** classify every difference as fraud or impairment. Record reconciler version, currency, allowed tolerance by product and resolved-by authority.

## 4. IFRS and reporting boundary
Supplier-finance arrangement disclosures under IAS 7 and IFRS 7 can require details about terms, carrying amounts and liquidity risk; **ERP and the reporting legal entity decide actual classification and financial statement treatment**. SCF may export consented source observations: approved-payables portfolio, maturity band, opening/closing funded obligations and related finance-provider payment status. Never self-classify IFRS 9 derecognition or IFRS 7 risk, and do not turn SCF projection into financial statements.

## 5. Historical and operational rules
Store snapshot as-of, source record time, provider effective time, exchange rate source and reconciliation watermark. A late credit note after collection creates a new fact and adjustment workflow, not past-state rewrite. Expose staleness and incomplete evidence on every dashboard. Parent Nabhold consolidated totals are permitted only through explicit cross-subsidiary grants or governed anonymisation.

## 6. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-POS-01 | Dimensioned projection schema, funder source mapping and non-ledger proof |
| SCF-POS-02 | Partial invoice/tranches/recourse/reserves/FX example fixtures |
| SCF-POS-03 | ERP bank/payment/funder divergent status and delayed event reconciliation |
| SCF-POS-04 | IFRS reporting export evidence with accounting-team review and non-classification tests |
| SCF-POS-05 | Source lineage, freshness, privacy and historical replay under two tenant cases |

## 7. Review triggers and sources
Change when finance products, accounting/reporting regime or ERP source contract changes. [IFRS supplier-finance disclosure amendments](https://www.ifrs.org/news-and-events/news/2023/05/iasb-increases-transparency-of-companies-supplier-finance/) effective 1 January 2024. These are accounting reporting requirements, not SCF's own authority to post entries.

## 8. Cross-cutting safeguards, alternatives and review practice

Every consequential operation must bind IAM subject and CP trusted tenant, legal entity, market and relationship; never trust client-supplied actor or tenant labels. Protect commercial and personal finance data through scoped disclosures, encryption, classifications and retention review. Use precise source-owned CrossEngineObjectReference, immutable observations with original occurred_at/recorded_at, and auditable versioned rules. For external financial side effects, preserve uncertain outcomes and reconcile rather than retry blindly. Reject treating SCF as a loan ledger, credit committee, customs agency, payment processor, document issuer or hidden cross-subsidiary information channel.

**Review requirements:** record actual external legal/partner evidence with dated URL and contract revision; separate current approved reality from target architecture; preserve previously issued source facts; perform security and source-authority tests. Acceptance of this Proposed ADR does not register canonical capabilities/events or establish CP certification. Each gate must map to future code, tests, deployment and independent legal/business approval, with explicit unsupported operations and revisit triggers.
