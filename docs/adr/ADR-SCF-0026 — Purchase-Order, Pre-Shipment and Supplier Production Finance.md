# ADR-SCF-0026 — Purchase-Order, Pre-Shipment and Supplier Production Finance

**Status:** Proposed — product-specific financing architecture; no live product authority  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Product family:** Conditional partner-led financing, not default lending or a bank account  
**Prerequisites:** Proposed ADR-SCF-0001..0022 foundations, especially legal market authority (0003), source facts (0006/0007), claims/priority (0008), external provider (0010), contract (0012), payments/ERP (0013/0014) and certification (0022)  

> **Publication is not activation.** This ADR creates a product design, not a verified funder, entitlement, customer financing approval, lawful collateral arrangement or available funding line.


## 1. Product decision
Support **pre-performance/pre-shipment financing coordination** only for a qualified partner prepared to underwrite **performance, sourcing and delivery risk** before a receivable necessarily exists. A purchase order or buyer intent is not an ERP receivable, nor secure repayment collateral by itself. SCF must not convert a draft Trade order into eligible invoice discounting or fabricate future bank exposure.

## 2. Evidence and contract model
| Evidence element | Authority / treatment |
|---|---|
| Confirmed purchase order | Trade contract/acceptance, quantities, products, buyer, cancellation terms |
| Production plan | supplier source, capacity, schedule, critical materials and milestones |
| Export/corridor requirements | Regulations applicability and Trade Docs documentation, as supported |
| Shipment plan | TMS transport intent, not actual performed shipment |
| Buyer credibility | independently authorised partner data/risk decision, not Pulse-issued credit limit |
| Use-of-proceeds | contracted funder restrictions and verified supplier drawdown evidence |
| Security/guarantee | source legal pledge/assignment, bank/guarantor proof |
| Milestone observation | ERP procurement, approved inspection, TMS loading/delivery |
| Repayment source | lender contract, later invoices/collections with source lineage |

## 3. Financing structure
Programme may require supplier co-investment, staged advances, direct supplier/material payment, buyer undertaking, export contract or guarantee; all defined externally. Partial drawdowns and repayment waterfall are funder/Payments-controlled. SCF tracks planned vs externally confirmed milestones and holds the next drawdown request if required production or regulatory proof is missing; it cannot issue disbursement instructions without permitted actor and partner mandate.

~~~mermaid
flowchart TD
 A["Trade signed PO / eligible buyer"] --> B["Supplier production/capacity and KYB"]
 B --> C["Funder performance-risk underwriting"]
 C --> D["Conditional financing terms and milestones"]
 D --> E["Documented approved drawdown conditions"]
 E --> F["External funder/payment source observations"]
 F --> G["Production/inspection/shipment/delivery milestone evidence"]
 G --> H["Funder-controlled next stage / repayment"]
~~~

## 4. Risk management
Non-shipment, poor harvest, contamination, logistics closure, export restrictions, buyer cancellation, cost overrun, exchange-rate movement, fraud, collateral unenforceability, damage and delays create differing consequences. Revisit buyer purchase obligation and actual shipment proof each drawdown. Commodity price forecasts from Pulse remain advice, not source valuation or credit approval.

## 5. Initial applicability
ZuriBeans vanilla or coffee supplier production financing could support verified export contracts; **this is a research/pilot hypothesis** contingent on real buyers, supplier track record, inspections and licensed funder appetite. Product should be blocked for fresh unsupported producers without qualified partner approval, rather than pretending a PO is cash collateral.

## 6. Alternatives rejected and safeguards
Reject PO treated as issued invoice, automated bank advance when TMS marks shipment "planned", data scraping as supplier credit score, using customer wallet as investment pool, and unconditional finance guarantee.

## 7. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-PRE-01 | Pre-shipment finance vs receivables purchase legal distinction and risk matrix |
| SCF-PRE-02 | Version-pinned PO, buyer commitment, supplier capability and planned milestones |
| SCF-PRE-03 | Drawdown condition/inspection, stage failure, changes and refund/recourse fixtures |
| SCF-PRE-04 | Funder-mandated usage, currency, security and legal agreements |
| SCF-PRE-05 | Independent funder underwriting sandbox and producer/supplier verified scenario |
| SCF-PRE-06 | Narrow legally authorised real-market pilot only after evidence of actual demand |

## 8. Revision conditions
New crop/corridor, agricultural guarantees, credit support programme or collateral law; no blanket entitlement for new supplier groups.

## 9. Required governance and future revision protocol

All cross-engine source facts use Shared canonical typed references with applicable scope and historical pinning, not foreign SQL writes. Require IAM caller, CP trusted tenant/legal entity context, supplier/buyer/funder contract/mandate, consent and privacy controls. Verify actor and external-provider provenance, economic terms/recourse, amount/currency precision, applicable regulatory classification and explicit UNKNOWN outcomes before any consequential external action. Separate documentation review, source implementation, partner sandbox, independent legal approval, Shared canonical capability/event admission and CP/EA-09 production certification.

**When new evidence develops:** record a versioned product assumption ledger (contract/jurisdiction/partner/date/source), decision owner, superseded assumptions, replay impact on accepted obligations, migration/rollback and whether live cases must be grandfathered. Do not retrofit new finance terms onto old accepted offers or disclose source data to new parties without lawful authority. No gate passes by merging this document.
