# ADR-SCF-0024 — Approved Payables Finance and Buyer-Anchored Programmes

**Status:** Proposed — product-specific financing architecture; no live product authority  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Product family:** Conditional partner-led financing, not default lending or a bank account  
**Prerequisites:** Proposed ADR-SCF-0001..0022 foundations, especially legal market authority (0003), source facts (0006/0007), claims/priority (0008), external provider (0010), contract (0012), payments/ERP (0013/0014) and certification (0022)  

> **Publication is not activation.** This ADR creates a product design, not a verified funder, entitlement, customer financing approval, lawful collateral arrangement or available funding line.


## 1. Product decision
Implement **buyer-led payables finance** as an optional partner-led arrangement where an anchor buyer affirmatively approves a payable, an enrolled supplier may elect early payment, and a qualified funder purchases/finances the eligible receivable under source contract. Buyer approval, funder approval, supplier election, legal assignment and payment remain **separate facts**. Buyer generally remains obliged to pay at maturity subject to actual contract, but SCF cannot assert a universal legal treatment.

## 2. Programme actors and constraints
| Party | Boundaries |
|---|---|
| Anchor buyer/debtor | authoritative approval of amount, due date and payable under ERP/Trade contract |
| Supplier | opt-in to early payment, exact invoice and funder disclosure/offer |
| Funding partner | credit appetite, buyer/programme limit, finance pricing and external disbursement |
| Programme sponsor | participation terms, permitted entities/markets and programme coordination |
| SCF | source approval references, case state, offers, consents and funder/payment reconciliation |
| ERP | buyer payable/obligation, invoice and accounting effects; financial statement classification |

A payable approval must identify the effective corporate signatory/approved buyer workflow, source invoice, quantity/acceptance criteria and amount. Trade purchase order, goods receipt, approved payable, funder-confirmed finance and buyer's final settlement are not interchangeable statuses.

## 3. Protocol
~~~mermaid
sequenceDiagram
 participant E as ERP/Trade buyer source
 participant S as SCF
 participant U as Supplier
 participant F as Qualified funder
 participant P as Payments/bank
 E->>S: Approved payable + version
 S->>U: Consent-approved early-pay opportunity
 U->>S: Authorised opt-in
 S->>F: Approved payable evidence and exact snapshot
 F-->>S: Funder-specific binding offer/decline
 U->>S: Select and authorise exact terms
 F->>P: External disbursement authorisation
 P-->>S: Source payment result; reconcile via ERP
 E-->>S: Buyer settlement/maturity status
~~~

A funder may refuse even approved invoices. Terms may be anchored in buyer credit, but SCF does not calculate or certify its rating. Disputes, setoff, credit notes, buyer insolvency or supplier revocation require source evaluation and external partner legal disposition.

## 4. Anchor finance governance
Anchor and funder limits are distinct. Buyer liability is not assumed to move off balance sheet; IFRS IAS7/IFRS7 supplier finance disclosure obligations may apply and are accounted by ERP/financial reporting legal entity. Buyer-funded early payment without third-party funder is ADR-0025, not a hidden second variant.

## 5. Pilot scenario
ZuriBeans and an established buyer agree to finance only **posted, buyer-approved** vanilla purchase obligations within a contract-specific amount/currency and approved partner bank market. Supplier opts in voluntarily, sees costs/recourse and consents before sending documentary evidence. Test a supplier declining finance and remaining paid under standard terms.

## 6. Failure and alternate paths
Revoked buyer approval, buyer claims returned goods, duplicated invoice, programme limit consumed, early supplier acceptance after offer expiry, funder payout returned, buyer pays supplier despite assignment. Reject autoenrolment without consent and converting buyer approval into funder credit approval.

## 7. Implementation gates
| Gate | Objective evidence |
|---|---|
| SCF-PAYF-01 | ERP buyer-approved payable source contract and signatory/authority verification |
| SCF-PAYF-02 | Supplier consent, voluntary opt-out and programme limit/eligibility tests |
| SCF-PAYF-03 | Partial payable, credit note, changed due date, dispute and assignment fixtures |
| SCF-PAYF-04 | Funder binding offer, fees, repayment and double-payment protection |
| SCF-PAYF-05 | Supplier/buyer/funder/ERP end-to-end synthetic programme |
| SCF-PAYF-06 | One signed anchor buyer and qualified funder sandbox/market legal approval |

## 8. Review source and triggers
[GSCFF payables finance](https://supplychainfinanceforum.org/techniques/payables-finance/), [IFRS supplier finance disclosures](https://www.ifrs.org/news-and-events/news/2023/05/iasb-increases-transparency-of-companies-supplier-finance/). Revisit if anchor's approval is nonbinding, funder unavailable or legal/accounting classification changes.

## 9. Required governance and future revision protocol

All cross-engine source facts use Shared canonical typed references with applicable scope and historical pinning, not foreign SQL writes. Require IAM caller, CP trusted tenant/legal entity context, supplier/buyer/funder contract/mandate, consent and privacy controls. Verify actor and external-provider provenance, economic terms/recourse, amount/currency precision, applicable regulatory classification and explicit UNKNOWN outcomes before any consequential external action. Separate documentation review, source implementation, partner sandbox, independent legal approval, Shared canonical capability/event admission and CP/EA-09 production certification.

**When new evidence develops:** record a versioned product assumption ledger (contract/jurisdiction/partner/date/source), decision owner, superseded assumptions, replay impact on accepted obligations, migration/rollback and whether live cases must be grandfathered. Do not retrofit new finance terms onto old accepted offers or disclose source data to new parties without lawful authority. No gate passes by merging this document.
