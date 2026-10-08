# ADR-SCF-0028 — Inventory, Warehouse Receipt and Commodity-Backed Finance

**Status:** Proposed — conditional product, unimplemented  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Scope:** Partner-led supply-chain finance; market/product-specific legal, operational and economic authority  
**Baseline:** ADR-SCF-0001 remains Proposed, no lender/servicer/bank/funds custody authority, no mandatory third-party finance core  

> **Future intent is not deployment evidence.** Source partner schemes, legally effective financial rights, live eligibility and actual markets require distinct verification and approval before any product-facing activation.


## 1. Product decision
Reserve a provider-led **inventory/commodity-secured financing** technique. Source assets, title, warehouse custody, collateral perfection, valuation, inspection, insurance and lender enforcement are **separate authorities**. SCF coordinates collateral evidence, qualified storage/provider links, loan offer and source observations, but does not create a legally perfected pledge, warehouse operator certificate or inventory asset ledger.

## 2. Collateral graph
| Concept | Source of truth / evidence |
|---|---|
| CommodityLot / HandlingUnit | ERP/WMS stock records and TMS physical custody source refs |
| WarehouseReceipt | recognised warehouse/Trade Docs issuer, exact DocumentVersion and content |
| LegalTitleObservation | owner/warehouse/lawful transfer evidence, jurisdiction and time |
| CollateralSecurityInterest | lender/registry contract and perfection/priority source |
| Custodian/CollateralManager | separately qualified warehouse operator and valid mandate |
| Insurance/QualityCertificate | authorised insurer/inspection agency reference and effective coverage |
| BorrowingBaseObservation | lender rule, appraised eligible value, currency/haircut, timestamp |
| ReleaseOrder/Control | qualified lender/warehouse authorised release, not SCF UI setting |
| MarginCall/Default | lender source notification, valuation change and remedy evidence |
| WarehouseDiscrepancy | stock count, seal, shrinkage, damage, custody/valuation mismatch |

Inventory value is not face value of an issued receivable. A TMS shipment event, Trade Docs receipt or ERP stock status does not by itself prove exclusive possession or first-ranking pledge. Goods may be commingled or pledged under a floating charge, with consequences governed by law. Warehouse custody, commercial ownership and lender control must be independently modelled.

~~~mermaid
flowchart TD
 A["ERP/WMS stock and TMS custody"] --> B["Qualified warehouse receipt / inspection"]
 B --> C["External legal title, lien and insurance proof"]
 C --> D["Lender borrowing base/advance decision"]
 D --> E["Source-confirmed funding"]
 E --> F["Ongoing stock/value/custody control"]
 F --> G["Lender-approved release or margin action"]
~~~

## 3. Valuation and monitoring
Commodity grade, volume, weight, packaging, storage location, FX, appraiser, quoted market source, haircut, spoilage and price date must be recorded with provenance. Pulse price insights may inform advisory warning, but do not set lender collateral values or remove source legal risk. Market prices fluctuate; a previously issued loan may require covenant/margin action determined externally.

## 4. Example and constraints
ZuriBeans could eventually finance verified vanilla/coffee lots under independent warehouse custody. No direct financing against an invented "warehouse receipt" created only in Baobab. Fraud/double pledge, warehouse insolvency, transit loss, quality degradation, commodity seizure, false title, unauthorized release and cross-border movement must be tested.

## 5. Rejected alternatives
Inventory count alone as collateral, mutable warehouse receipt, ledger token as legal title, borrower-controlled physical release and a generic collateral lock without warehouse/registry enforcement.

## 6. Implementation gates
| Gate | Objective evidence |
|---|---|
| SCF-INV-01 | Custody/title/receipts/lien/valuer/insurance authority map by country |
| SCF-INV-02 | Exact warehouse receipt version, lot allocation, source quantity and quality models |
| SCF-INV-03 | Double pledge, split lots, damaged goods, stale valuation and release guard tests |
| SCF-INV-04 | Qualified custodian/lender collateral control and reviewable appraisal evidence |
| SCF-INV-05 | One actual warehouse/scheme/funder market-law agreement before limited pilot |
| SCF-INV-06 | Recovery/revaluation/residue and maturity exception operational tests |

## 7. Review triggers
Warehouse receipt law, collateral registry, cross-border storage, tokenised collateral, external inspector or commodity mix change. A source scheme's genuine availability, not architecture, is activation prerequisite.

## 8. Common governance and implementation evidence

Every market/product extension requires a verifiable legal-role assessment under ADR-SCF-0003; CP context and IAM user/workload trust; versioned Shared cross-engine source references (ERP financial truth, Trade commerce, Trade Docs evidence, TMS physical facts, Regulations where contracted); exact funder consent/offer and financial rights; Payments/external bank source payment observations; and no SCF shadow ledger. No new SCF or finance capability/event is registered by this local ADR. A funder sandbox is not evidence of production certification.

**Review checklist:** date and source any future market standard/law/funder evidence, distinguish known conditions from hypotheses, define bounded first product/corridor and unsupported states, record contract changes, privacy/security impacts, measurable business case, adverse-case tests and backward compatibility for accepted historical cases. Pass each named gate only in later source/contract PRs with independent business, security, counsel and EA-09/CP certification evidence. No vendor, tokenisation mechanism or cross-border payment rail is a new mandatory stack by default.
