# ADR-SCF-0030 — Multi-Funder Marketplace, Risk Participation and Portfolio Financing

**Status:** Proposed — future-conditional, deferred activation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Scope:** Partner-led supply-chain finance; market/product-specific legal, operational and economic authority  
**Baseline:** ADR-SCF-0001 remains Proposed, no lender/servicer/bank/funds custody authority, no mandatory third-party finance core  

> **Future intent is not deployment evidence.** Source partner schemes, legally effective financial rights, live eligibility and actual markets require distinct verification and approval before any product-facing activation.


## 1. Strategic status and decision
**Strategic option — defer activation.** Preserve a product-neutral **FinancingDistributionPort** for lawful multi-funder offer comparison, syndication, participation and portfolio risk transfer, without making Baobab a marketplace operator, arranger, securities broker, asset manager or pooled capital custodian by default. The operating model's financial-services perimeter changes materially when Baobab markets, brokers, allocates, sells or services financial assets; adoption requires **a separate business/legal authority decision** beyond standard case orchestration.

## 2. Distinct transaction types
| Type | Legal/economic substance to distinguish |
|---|---|
| Competing offers | alternative external funder bids for one supplier case, consented recipient disclosure |
| Co-funding/syndicated loan | two or more lender commitments under one or coordinated agreement |
| Risk participation | funded/unfunded risk transfer or subparticipation among financial institutions |
| Receivables portfolio sale | transfer of a set of eligible claims with buyer/priority and cutover |
| Warehouse facility | bank-backed credit line for programme funder, not public SCF pool |
| Insurance/guarantee | third-party credit risk mitigation with exclusions/claims process |
| Securitisation/tokenised position | regulated asset/security structure, not simply ledger token issuance |

## 3. Hypothetical domain extensions
ProgrammeDistributionMandate (legal permissions/contract); FunderInvitation (specific data and allowed scope); PortfolioEligibilitySnapshot (pinned assets/claims); BidEnvelope (signed terms and time); AllocationIntent (non-binding negotiation unless certified); FunderCommitmentObservation (source binding commitment); ParticipationShareReference (economic/legal allocation), ServicerMandateRef, CashflowWaterfallEvidence and ConcentrationPolicySnapshot. These are **reserved model seams** only, not new canonical Shared types or APIs.

~~~mermaid
flowchart TD
 A["Consented eligible source cases"] --> B["External structuring and legal authority"]
 B --> C["Funder invitation, terms and data-room rights"]
 C --> D["Independent qualified investor/lender commitments"]
 D --> E["Signed allocation / participation agreements"]
 E --> F["External servicer/fund cashflow authority"]
 F --> G["SCF source-attributed portfolio observations"]
~~~

## 4. Economic and concentration controls
No investor returns, guaranteed yield, aggregate funded limits, portfolio diversification or default-probability measures should be claimed without actual source data and approvals. Concentration dimensions may include buyer/supplier sector, related parties, country, currency, maturity, funding partner, collateral type and climate risk—subject to lawful data access. A programme that routes suppliers to multiple funders must prevent unintended duplicate financing and unequal disclosure of confidential pricing.

## 5. Activation triggers and blockers
**Trigger:** signed demand from multiple qualified funders, proven ADR-0023/0024 pilots, documented regulatory role, market-limited contracts, servicing partner and auditable portfolio feed. **Blockers:** no applicable licence/intermediary permission, unclear transfer title, absent servicer/cashflow settlement truth, unapproved investor access, no default/recovery handling or unmeasured concentration exposure.

## 6. Rejected alternatives
Building a public investing marketplace into the first Django service, pooling supplier proceeds in a Baobab bank account, auto-securitising invoices, creating a token to avoid securities regulation or implying syndication by having two quotes.

## 7. Implementation gates (future, not currently authorised)
| Gate | Evidence |
|---|---|
| SCF-MKT-01 | Commercial demand and activity/market regulatory perimeter memo |
| SCF-MKT-02 | Distinct economic/legal rights model and contractual issuer/servicer RACI |
| SCF-MKT-03 | Portfolio data minimisation, issuer consent, funder segregation and fairness tests |
| SCF-MKT-04 | Nonbinding two-provider allocation simulator with duplicate-claim/race test |
| SCF-MKT-05 | Independently audited risk/servicing/cashflow reconciliation with bank partners |
| SCF-MKT-06 | Separate architecture/business approval and partner/legal certification |

## 8. Revision policy
Maintain this ADR as **Proposed — deferred conditional strategy** until triggers are documented. Revision must list new market, funds-flow, investor class and legal analysis; do not promote based on projected continental financing gaps alone.

## 9. Common governance and implementation evidence

Every market/product extension requires a verifiable legal-role assessment under ADR-SCF-0003; CP context and IAM user/workload trust; versioned Shared cross-engine source references (ERP financial truth, Trade commerce, Trade Docs evidence, TMS physical facts, Regulations where contracted); exact funder consent/offer and financial rights; Payments/external bank source payment observations; and no SCF shadow ledger. No new SCF or finance capability/event is registered by this local ADR. A funder sandbox is not evidence of production certification.

**Review checklist:** date and source any future market standard/law/funder evidence, distinguish known conditions from hypotheses, define bounded first product/corridor and unsupported states, record contract changes, privacy/security impacts, measurable business case, adverse-case tests and backward compatibility for accepted historical cases. Pass each named gate only in later source/contract PRs with independent business, security, counsel and EA-09/CP certification evidence. No vendor, tokenisation mechanism or cross-border payment rail is a new mandatory stack by default.
