# ADR-SCF-0029 — Documentary Trade Finance, Bank Guarantees, Letters of Credit and Forfaiting

**Status:** Proposed — conditional product, unimplemented  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Scope:** Partner-led supply-chain finance; market/product-specific legal, operational and economic authority  
**Baseline:** ADR-SCF-0001 remains Proposed, no lender/servicer/bank/funds custody authority, no mandatory third-party finance core  

> **Future intent is not deployment evidence.** Source partner schemes, legally effective financial rights, live eligibility and actual markets require distinct verification and approval before any product-facing activation.


## 1. Product decision
SCF can coordinate **external bank-issued documentary trade-finance instruments**—letters of credit (LCs), standby LCs, guarantees, documentary collections, approved bank payment obligations and forfaiting—without itself acting as a bank, negotiator, confirming bank, guarantee issuer or exporter of a sovereign credit undertaking. Each instrument type is a distinct **FinancialInstrumentReference** and workflow, not a generic financed invoice.

## 2. Instruments and authority
| Instrument | Relevant primary actors | Distinct documentary rule |
|---|---|---|
| Documentary LC | issuing bank, advising/confirming/nominated bank, applicant and beneficiary | source issued instrument, amendments, presentation, discrepancy and honour |
| Standby LC | issuing bank, beneficiary, confirming/claiming parties | demand/presentation conditions, expiry and claim rules |
| Bank guarantee | guarantor, obligee, applicant | guarantee scope, governing rules, valid demand and release |
| Documentary collection | remitting/collecting banks and principals | D/P or D/A instructions and bank response; not unconditional guarantee |
| Forfaiting | holder/forfaiter/guarantor and payment instruments | purchase and transfer of eligible medium-long receivables without recourse where contracted |
| Cash against documents | commercial parties, bank intermediaries and Trade Docs | document release and payment not identical |
| Export credit insurance | insurer, insured party, broker/funder | policy eligibility and independent claim authority |

ICC documentary rulebooks such as UCP 600, URDG 758, ISP98, URC 522, eUCP and URDTT may apply **only where chosen in the actual instrument/contract and effective revision**. Never assert UCP automatically governs every letter of credit or that software validation implies a compliant legal presentation.

## 3. Document and milestone flow
~~~mermaid
flowchart TD
 A["Trade signed sale and party roles"] --> B["External bank instrument issued"]
 B --> C["Trade Docs exact signed instrument/DocumentVersion"]
 C --> D["Trade/TMS/Customs documentary performance evidence"]
 D --> E["Bank-specific presentation and discrepancy handling"]
 E --> F["Bank source acceptance, honour/claim or refusal"]
 F --> G["Payments/ERP actual proceeds and financial reconciliation"]
~~~

Document presentation can be compliant or discrepant independent of physical goods quality; a shipment arriving does not prove bank honour. "LC issued" is not "bank funds received." Store amendment history, presenting party, required exact versions, time zones, expiration, value and source bank reference; never rewrite original instrument. Trade Docs owns documentary record and presentation evidence; bank owns credit undertaking, presentation decision and payment; SCF observes product case and sources.

## 4. Source trust and risk
Forgery, amended LC, mismatch of applicant/beneficiary, overshipment, prohibited partial shipments, late presentation, expired guarantee, non-recourse misclassification, documentary discrepancies and bank-country/correspondent risk all require source-qualified handling. A platform trade score or generic document checksum cannot independently clear discrepancies.

## 5. Rejected alternatives
SCF acting as LC issuing bank, programmatically generating an enforceable guarantee in-app, treating LC PDF as proof of available funds, auto-honour after TMS delivery and treating collections as settlement.

## 6. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-DOCFIN-01 | Distinct instrument legal roles/rulebooks and bank source contract matrix |
| SCF-DOCFIN-02 | Trade Docs pinned instruments/amendments, presentation package and issuer verification |
| SCF-DOCFIN-03 | LC/guarantee/collection/forfaiting simulation with discrepancies/expiry |
| SCF-DOCFIN-04 | Qualified bank adapters, signer authority and external decision observations |
| SCF-DOCFIN-05 | Payment/ERP reconciliation, returned/rejected honour and source audit |
| SCF-DOCFIN-06 | One qualified correspondent/issuer sandbox and local counsel approval |

## 7. Revision triggers
ICC rule revisions, digital instruments, bank onboarding, export guarantee programme or jurisdiction-specific electronic presentation; no product is activated by this ADR.

## 8. Common governance and implementation evidence

Every market/product extension requires a verifiable legal-role assessment under ADR-SCF-0003; CP context and IAM user/workload trust; versioned Shared cross-engine source references (ERP financial truth, Trade commerce, Trade Docs evidence, TMS physical facts, Regulations where contracted); exact funder consent/offer and financial rights; Payments/external bank source payment observations; and no SCF shadow ledger. No new SCF or finance capability/event is registered by this local ADR. A funder sandbox is not evidence of production certification.

**Review checklist:** date and source any future market standard/law/funder evidence, distinguish known conditions from hypotheses, define bounded first product/corridor and unsupported states, record contract changes, privacy/security impacts, measurable business case, adverse-case tests and backward compatibility for accepted historical cases. Pass each named gate only in later source/contract PRs with independent business, security, counsel and EA-09/CP certification evidence. No vendor, tokenisation mechanism or cross-border payment rail is a new mandatory stack by default.
