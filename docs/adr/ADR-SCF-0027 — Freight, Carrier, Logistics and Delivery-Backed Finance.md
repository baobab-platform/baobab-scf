# ADR-SCF-0027 — Freight, Carrier, Logistics and Delivery-Backed Finance

**Status:** Proposed — product-specific financing architecture; no live product authority  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Product family:** Conditional partner-led financing, not default lending or a bank account  
**Prerequisites:** Proposed ADR-SCF-0001..0022 foundations, especially legal market authority (0003), source facts (0006/0007), claims/priority (0008), external provider (0010), contract (0012), payments/ERP (0013/0014) and certification (0022)  

> **Publication is not activation.** This ADR creates a product design, not a verified funder, entitlement, customer financing approval, lawful collateral arrangement or available funding line.


## 1. Product decision
Enable qualified funders to offer **working capital against eligible logistics receivables** (freight invoices, approved carrier payables, accepted delivery services), not against truck GPS events alone. TMS owns physical transport, custody and delivery observations; ERP owns carrier/subcontractor payables and freight service financial obligations; Trade Docs owns POD versions and other documentary proof. SCF coordinates permitted evidence and finance offers; the contracting parties and qualified factor own funding and recourse.

## 2. Eligible source/evidence matrix
| Type | Source fact | Not sufficient alone |
|---|---|---|
| Freight service obligation | ERP posted carrier/subcontractor invoice, debtor, amount and currency | dispatch created |
| Carrier contract | Trade/commercial agreement and service rate reference | driver app profile |
| Transport proof | TMS movement/leg/call with source attribution and delivery attempt | raw GPS point |
| Delivery evidence | Trade Docs POD DocumentVersion and verified issuer/recipient evidence | unsigned PDF/image |
| Customer/buyer approval | authorised freight buyer confirms payability/dispute status | arrival at warehouse |
| Assignment/priority | external factoring/receivable purchase evidence | local allocation only |
| Payout/collection | funder/payment/bank/ERP source observations | request submitted |

## 3. Scenario
Thamani contracts a carrier for a delivered intra-market movement. Only when the customer approves the service payable and ERP recognises the relevant obligation may SCF offer an evidence manifest to an approved funder. The factor confirms eligible amount and issues a disclosed offer; carrier elects it. A late proof-of-delivery or cargo-damage dispute can change eligible amount and must be handled before submission or through a post-financing dilution process.

~~~mermaid
flowchart LR
 T["TMS movement/delivery"] --> D["Trade Docs POD evidence"]
 D --> A["ERP approved freight payable"]
 A --> S["SCF eligible case and funder consent"]
 S --> F["Qualified factor decision/offer"]
 F --> P["External finance/payment"]
 P --> R["ERP and funder position reconciliation"]
~~~

## 4. Mode and operational risk
Road-only first; air/sea transport invoice financing may need carrier booking, bill of lading, freight forwarder rights and independent terminal evidence. Avoid claiming multi-modal transport execution is available because an architecture ADR exists. Proof of delivery may be disputed; physical arrival and signed recipient acceptance are separate. Carrier employment and driver payment arrangements are not automatically credit activities in SCF.

## 5. Risk and legal boundaries
Not all freight invoices are assignable; subcarrier and principal-carrier contracts differ. Carrier qualification, subcontracting permission, insurance, actual debtor identity and rights of setoff need contract-specific review. Public carrier score must not be inferred from a few route delays or shared without legal basis.

## 6. Rejected alternatives
Loan from live GPS, pay carrier automatically on dispatch, assume POD legally proves receivable title, use TMS rate estimate as posted ERP invoice, and attach another carrier's evidence across tenants.

## 7. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-FRT-01 | Eligible freight payable/receivable definition and TMS/ERP ownership map |
| SCF-FRT-02 | TMS delivery vs POD vs buyer-approved freight obligation separation |
| SCF-FRT-03 | Wrong carrier/tenant, partial service, damage, dispute and late POD tests |
| SCF-FRT-04 | Funder/assignment and invoice discount mechanics, bank payout/ERP match |
| SCF-FRT-05 | Road-market carrier finance sandbox with actual qualified partner |
| SCF-FRT-06 | Expand sea/air only after executable TMS and legal-contract proof |

## 8. Review trigger
New transport modes, subcontracting rights, courier/driver legal status, freight payment practice or partner offering requires dated review.

## 9. Required governance and future revision protocol

All cross-engine source facts use Shared canonical typed references with applicable scope and historical pinning, not foreign SQL writes. Require IAM caller, CP trusted tenant/legal entity context, supplier/buyer/funder contract/mandate, consent and privacy controls. Verify actor and external-provider provenance, economic terms/recourse, amount/currency precision, applicable regulatory classification and explicit UNKNOWN outcomes before any consequential external action. Separate documentation review, source implementation, partner sandbox, independent legal approval, Shared canonical capability/event admission and CP/EA-09 production certification.

**When new evidence develops:** record a versioned product assumption ledger (contract/jurisdiction/partner/date/source), decision owner, superseded assumptions, replay impact on accepted obligations, migration/rollback and whether live cases must be grandfathered. Do not retrofit new finance terms onto old accepted offers or disclose source data to new parties without lawful authority. No gate passes by merging this document.
