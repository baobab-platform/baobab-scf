# ADR-SCF-0031 — Agricultural, Cooperative and Inclusive Supplier Finance

**Status:** Proposed — future-conditional, deferred activation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Scope:** Partner-led supply-chain finance; market/product-specific legal, operational and economic authority  
**Baseline:** ADR-SCF-0001 remains Proposed, no lender/servicer/bank/funds custody authority, no mandatory third-party finance core  

> **Future intent is not deployment evidence.** Source partner schemes, legally effective financial rights, live eligibility and actual markets require distinct verification and approval before any product-facing activation.


## 1. Strategic decision
Reserve a programme variant for **cooperatives, producer aggregators, agricultural SMEs, seasonal sourcing and small suppliers**, using partner finance and source-backed evidence without making land, harvest predictions or membership cards automatic collateral. Inclusion is a programme objective, not a reason to weaken consent, licensed lender requirements, identity or anti-fraud controls.

## 2. Actor and rights graph
| Role | Required distinction |
|---|---|
| Producer/farmer | individual or legally documented business rights and consent, not always a company |
| Cooperative/aggregator | independently verified legal form, collection/marketing mandate and representation |
| Buyer/exporter | actual commercial contract and product acceptance from Trade |
| Finance partner | authorised borrower/lender contract, credit decision and recourse |
| Input provider | material supply obligations, not automatically funder |
| Warehouse/inspector | independent quantity/quality/custody and receipt provenance |
| Insurance/guarantee provider | externally issued climate/crop/credit protection instrument |
| SCF | consented programme, financing-case evidence, funder interaction and observation |

A cooperative may be supplier, agent or borrower in different transactions; a producer may receive funds or guarantee obligations only under verified legal entitlement. Agricultural offtake contract and farmer-membership records do not automatically grant consent to borrow.

## 3. Seasonal product modelling
~~~mermaid
flowchart TD
 A["Buyer sourcing/offtake contract"] --> B["Supplier/cooperative role verification"]
 B --> C["Historical delivery and verified produce evidence"]
 C --> D["Qualified funder seasonal product/guarantee"]
 D --> E["Conditional advance / input finance"]
 E --> F["Inspection, aggregation, quality and actual delivery"]
 F --> G["Funder repayment and ERP reconciliation"]
~~~

Track crop cycle, expected harvest/production window, perishability/storage conditions, production-location privacy, purchase quantity/price formula, grade/quality, delivery and beneficiary type. Expected production/price from Pulse is an estimate, not a financial receivable. Smallholder mobile-payment destinations belong Payments or qualified funder. Land tenure and security interests require actual local law and title evidence; do not pretend electronic farm plots are registered collateral.

## 4. Inclusion, protection and fairness
Simplified onboarding may mean progressive evidence with thresholds, not skipping legally mandatory identification/sanctions. Explain actual cost, recourse and total repayable obligations in accessible channels/languages approved by the partner. Consent must be granular for cooperative/individual data; group approval cannot silently bind a member's personal debt. Ensure opt-out, complaint routes, grievance processes and vulnerable-user protection. No automated risk penalty based solely on geographic or demographic proxies.

## 5. Risk scenarios
Crop failure, extreme weather, storage contamination, exporter rejects grade, seasonal prices move, cooperative funds misallocated, disputes over member entitlement, weak connectivity, missing records, delayed cross-border export and insurance claim rejected. An observation of poor harvest cannot automatically trigger a funder's legal acceleration or recover insured losses.

## 6. Activation and blockers
**Trigger:** a real buyer/offtake arrangement, verified aggregator rights, qualified partner with product appetite, source evidence and accessible supplier consent/complaint infrastructure. **Blocker:** unclear producer capacity/identity, unrecognised guarantee, unsupported crop grade and missing independent inspection/custody.

## 7. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-AGR-01 | Agrifinance role/crop/season/contract and market legal matrix |
| SCF-AGR-02 | Cooperative representative and member consent/ownership tests |
| SCF-AGR-03 | Crop delivery/quality/inspection, partial lot and weather scenario fixtures |
| SCF-AGR-04 | Cost transparency, inclusion/fairness and accessible dispute process |
| SCF-AGR-05 | Partner/funder off-take pilot contract plus insurance/collateral review |
| SCF-AGR-06 | Reproducible follow-up monitoring before adding regions or crop products |

## 8. Review and alternative
Reject instant farmer score from satellite image, using all members as joint guarantors by default, platform lending to unverified persons and farm data resale. Reconsider with new agricultural sector, producer protection rule or qualified financial programme.

## 9. Common governance and implementation evidence

Every market/product extension requires a verifiable legal-role assessment under ADR-SCF-0003; CP context and IAM user/workload trust; versioned Shared cross-engine source references (ERP financial truth, Trade commerce, Trade Docs evidence, TMS physical facts, Regulations where contracted); exact funder consent/offer and financial rights; Payments/external bank source payment observations; and no SCF shadow ledger. No new SCF or finance capability/event is registered by this local ADR. A funder sandbox is not evidence of production certification.

**Review checklist:** date and source any future market standard/law/funder evidence, distinguish known conditions from hypotheses, define bounded first product/corridor and unsupported states, record contract changes, privacy/security impacts, measurable business case, adverse-case tests and backward compatibility for accepted historical cases. Pass each named gate only in later source/contract PRs with independent business, security, counsel and EA-09/CP certification evidence. No vendor, tokenisation mechanism or cross-border payment rail is a new mandatory stack by default.
