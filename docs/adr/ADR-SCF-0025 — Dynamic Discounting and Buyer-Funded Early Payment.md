# ADR-SCF-0025 — Dynamic Discounting and Buyer-Funded Early Payment

**Status:** Proposed — product-specific financing architecture; no live product authority  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Product family:** Conditional partner-led financing, not default lending or a bank account  
**Prerequisites:** Proposed ADR-SCF-0001..0022 foundations, especially legal market authority (0003), source facts (0006/0007), claims/priority (0008), external provider (0010), contract (0012), payments/ERP (0013/0014) and certification (0022)  

> **Publication is not activation.** This ADR creates a product design, not a verified funder, entitlement, customer financing approval, lawful collateral arrangement or available funding line.


## 1. Product decision
Model **buyer-funded early payment** as a commercial amendment to supplier payment timing in exchange for a consensual discount, **not automatically a third-party receivables sale or bank loan**. The buyer must actually have legal capacity and available funds; neither SCF nor Baobab Payments may infer free cash or move buyer money without authorised payout and ERP controls.

## 2. Domain model
| Concept | Owner and record |
|---|---|
| BuyerEarlyPaymentProgramme | buyer legal entity, eligible obligations, discount policy, terms, authority limits |
| SupplierOptIn | specific supplier/obligation/offer and corporate mandate; no blanket enrollment |
| EarlyPaymentOffer | amount, original vs accelerated due date, discount, amount payable, validity |
| TreasuryLiquidityObservation | buyer/ERP/treasury externally authorised available budget, as_of and scope |
| PaymentInstructionRef | Payments-authorised outbound payout if implemented/entitled |
| BuyerERPAdjustmentRef | ERP accounting of discount, payable, cash and invoice effects |
| EarlyPaymentReceipt | partner/bank/payment observed supplier credited and reconciliation |
| Dispute/Correction | disagreement over supplied goods, fees, or early payment reversal |

## 3. Calculation and treasury isolation
Possible reviewed structures include fixed discount for a set early date or sliding discount by payment date. Example is **illustrative only**: an invoice of 1,000 units with a contractually agreed 2% discount produces 980 units before any tax or FX effects. Legal discount base, tax treatment, invoice amendment and accounting are partner/ERP responsibilities. Quote expiry, business days and bank value date may affect price. Use decimal safe currency and explain exact calculation; do not substitute an interest rate APR automatically.

~~~mermaid
flowchart LR
 A["ERP approved payable and buyer treasury budget"] --> B["Reviewed discounted early payment offer"]
 B --> C["Supplier explicit election"]
 C --> D["Buyer authorised funds/payout instruction"]
 D --> E["Payments/bank confirmed settlement"]
 E --> F["ERP booking and SCF reconciliation"]
~~~

## 4. Capacity and fairness
The buyer's available discount programme liquidity can fluctuate; expiry or withdrawal must not modify accepted earlier contract terms. Multiple suppliers may race against a capped treasury budget; use optimistic reservations but require actual buyer financial approval. Ensure smaller suppliers are not coerced into discounting or paid late when they refuse. Anchor buyer cannot use SCF UI to make unapproved funds movements.

## 5. Limits and differences
Supplier early settlement without external funder can be commercially simpler but may still have legal/accounting consequences. It must not be mixed with payables finance using third-party factor funding or misreported as bank-facilitated finance. Participation under Nabhold subsidiaries remains legal-entity scoped.

## 6. Negative tests
Buyer budget stale, cash insufficient, supplier withdraws, invoice modified, duplicate early payment, offer expired, FX shift, buyer withholds without basis, payment returned and ERP payable still open. All remain explicit source observations.

## 7. Implementation gates
| Gate | Proof |
|---|---|
| SCF-DYN-01 | Separate buyer-funded from funder-led model and legal/customer agreement |
| SCF-DYN-02 | Reviewed fixed/sliding discount arithmetic with tax/FX caveats |
| SCF-DYN-03 | Treasury caps, concurrent supplier election and wrong-entity payout tests |
| SCF-DYN-04 | Payments payout and ERP payable adjustment/reconciliation source contracts |
| SCF-DYN-05 | Real approved buyer budget and supplier opt-in before activation |

## 8. Revisit
New country tax/accounting guidance, dynamic price mechanism or product funding from a third party requires review.

## 9. Required governance and future revision protocol

All cross-engine source facts use Shared canonical typed references with applicable scope and historical pinning, not foreign SQL writes. Require IAM caller, CP trusted tenant/legal entity context, supplier/buyer/funder contract/mandate, consent and privacy controls. Verify actor and external-provider provenance, economic terms/recourse, amount/currency precision, applicable regulatory classification and explicit UNKNOWN outcomes before any consequential external action. Separate documentation review, source implementation, partner sandbox, independent legal approval, Shared canonical capability/event admission and CP/EA-09 production certification.

**When new evidence develops:** record a versioned product assumption ledger (contract/jurisdiction/partner/date/source), decision owner, superseded assumptions, replay impact on accepted obligations, migration/rollback and whether live cases must be grandfathered. Do not retrofit new finance terms onto old accepted offers or disclose source data to new parties without lawful authority. No gate passes by merging this document.
