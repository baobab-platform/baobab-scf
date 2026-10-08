# ADR-SCF-0023 — Receivables Discounting, Factoring and Invoice Finance

**Status:** Proposed — product-specific financing architecture; no live product authority  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Product family:** Conditional partner-led financing, not default lending or a bank account  
**Prerequisites:** Proposed ADR-SCF-0001..0022 foundations, especially legal market authority (0003), source facts (0006/0007), claims/priority (0008), external provider (0010), contract (0012), payments/ERP (0013/0014) and certification (0022)  

> **Publication is not activation.** This ADR creates a product design, not a verified funder, entitlement, customer financing approval, lawful collateral arrangement or available funding line.


## 1. Product decision
Support a **seller-led eligible receivable financing** programme, where a qualified factor/funder may purchase a receivable or extend an advance against it under the actual contract. Receivables purchase, disclosed/non-notified factoring and a loan secured on receivables are **different legal forms**, even if all accelerate supplier liquidity. SCF offers case/evidence/offer/acceptance and observation services only, without pretending to buy the receivable itself.

## 2. Specific domain and finance source
| Concept | Required role and evidence |
|---|---|
| Seller/assignor | source legal entity creditor and programme enrolment |
| Debtor | independently verified owing party; debtor notice if required |
| Receivable | ERP posted/accepted invoice, outstanding amount and currency/version |
| Buyer confirmation | source verified delivery/acceptance or contractual approval; may be optional by product |
| Assignment evidence | external legal agreement, notice/registration and priority as governed ADR-0008 |
| Purchase/advance | source funder price/advance rate, reserve, recourse, fees and validity |
| Funding | provider and Payments/bank observations, distinct from obligation settlement |
| Collection/recourse | external servicer/debtor evidence, source ERP reconciliation and dispute |
| Dilution | returns, credits, non-performance, short deliveries and invoice corrections |

Do not treat a supplier-uploaded invoice PDF as proof of an ERP receivable. Do not finance an already paid, cancelled, materially disputed or fully assigned obligation unless a qualified funder explicitly accepts a legally supported structure.

## 3. Process and states
~~~mermaid
flowchart TD
 A["ERP eligible receivable"] --> B["Seller contract and KYB"]
 B --> C["Verify debtor/source docs and legal encumbrances"]
 C --> D["Funder offer: purchase or advance"]
 D --> E["Seller mandate + exact agreement"]
 E --> F["External assignment/purchase/advance confirmation"]
 F --> G["Funder bank payment and ERP reconciliation"]
 G --> H["Debtor collection / reserve release / recourse observation"]
~~~

Separate application APPROVED_BY_FUNDER from EXTERNAL_PURCHASE_COMPLETED, DISBURSEMENT_REPORTED, BENEFICIARY_CREDITED, COLLECTED and POSITION_RECONCILED. Recourse may create obligations to seller only under source contract/competent law, not by changing a case field.

## 4. Amount/priority and risk
Face value may differ from eligible receivable after tax/credit notes. Amount-to-finance and limit are currency-specific, decimal safe and revalidated before purchase. Parallel applications for the same invoice must carry local reservation and external priority evidence; supplier consent to one factor does not license disclosure to another. Confidential factoring can constrain debtor notice, but does not permit avoiding legal/contractual safeguards.

## 5. Market scenario
**ZuriBeans example:** supplier invoice for legitimately delivered vanilla exported under an approved B2B transaction. Evidence: ERP posted invoice+balance, Trade commercial agreement, pinned Trade Docs invoice/POD, TMS delivery where applicable, verified buyer acknowledgment, funder factor agreement. Funder—not ZuriBeans or SCF—decides and pays. If TMS delivery is unavailable, programme must explicitly define acceptable alternative evidence rather than fabricating it.

## 6. Failure cases / alternatives
Unverified invoice, duplicate purchase, payment before funder advance, debtor disputes quantity, recourse after buyer default, funder reserves withheld, invoice denominated foreign currency, historic invoice amended. Reject automatic credit-score-as-approval, SCF invoice creation, SCF lender ledger, and assumption all factoring requires exact same debtor notice model.

## 7. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-REC-01 | Receivables purchase vs advance legal/product semantic fixtures |
| SCF-REC-02 | ERP source and document source, buyer approval and eligibility case tests |
| SCF-REC-03 | Partial assignments/limits, duplicate/priority and multi-funder rejection tests |
| SCF-REC-04 | Offer/fees/recourse/reserve and legal acceptance conformance |
| SCF-REC-05 | Source-funded vs bank/ERP-reconciled lifecycle with reversal/dispute |
| SCF-REC-06 | Actual factor partner, debtor consent where required and market counsel sign-off |

## 8. Source and revisability
[GSCFF receivables definitions](https://supplychainfinanceforum.org/glossary/). Revisit on specific factoring law, permitted distributor role, factoring provider contract or new debt assignment regime. Initial approval scope is **only documented qualifying invoices**, not all receivables.

## 9. Required governance and future revision protocol

All cross-engine source facts use Shared canonical typed references with applicable scope and historical pinning, not foreign SQL writes. Require IAM caller, CP trusted tenant/legal entity context, supplier/buyer/funder contract/mandate, consent and privacy controls. Verify actor and external-provider provenance, economic terms/recourse, amount/currency precision, applicable regulatory classification and explicit UNKNOWN outcomes before any consequential external action. Separate documentation review, source implementation, partner sandbox, independent legal approval, Shared canonical capability/event admission and CP/EA-09 production certification.

**When new evidence develops:** record a versioned product assumption ledger (contract/jurisdiction/partner/date/source), decision owner, superseded assumptions, replay impact on accepted obligations, migration/rollback and whether live cases must be grandfathered. Do not retrofit new finance terms onto old accepted offers or disclose source data to new parties without lawful authority. No gate passes by merging this document.
