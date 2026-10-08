# ADR-SCF-0034 — Digital Collateral, Transferable Trade Records and Tokenisation Boundary

**Status:** Proposed — future-conditional and deferred activation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Prerequisites:** Proposed SCF charter/architecture, separate signed partner/legal market authority, existing Shared/CP/IAM/ERP/Payments/Trade Docs boundaries  

> Documentation does not grant lending, funder, payment, bank, transferable-record, collateral custody or market rights; no new mandatory stack is approved.


## 1. Strategic decision
Maintain a seam for **externally controlled electronic transferable records**, digital warehouse receipts, collateral registry references, assignment evidence and (conditionally) tokenised representations. **Do not** treat a PDF, hash, NFT or private token as automatically establishing legal title, ownership of goods, exclusive control, receivable assignment or collateral priority. No new blockchain, token ledger, crypto custody, electronic BL issuance service or securities platform is a baseline dependency.

## 2. Legal object distinctions
| Object | Competent/source authority | Non-equivalence |
|---|---|---|
| TradeDocument / DocumentVersion | Trade Docs and external issuer evidence | Legal holder/control not automatic |
| Electronic Transferable Record | Legally recognised issuer/qualified control scheme | Generic signed PDF is insufficient |
| Exclusive Control Observation | External controller, scheme, holder, receipt and time | SCF database permission |
| Warehouse Receipt | Recognised warehouse/issuer, verified lot/identity | Inventory proof not legal security |
| Assignment or Security Interest | Funder, debtor, registry, governing-law evidence | Local claimant hash or token transfer |
| Tokenised Representation | If future approved, regulated issuer/redemption/custodian scheme | Universal enforceability |
| FinancingPosition | Source lender/servicer state in SCF projection | Asset ownership or lender balance |

~~~mermaid
flowchart TD
 D["Issued TradeDocument version"] --> C["Qualified source exclusive-control provider"]
 C --> H["Holder / endorsement / surrender evidence"]
 H --> L["Separate receivable assignment or collateral priority"]
 L --> F["Qualified finance partner decision"]
 F --> S["SCF financing case and pinned source"]
 T["Optional token, if separately authorised"] -.-> C
~~~

## 3. MLETR, ETR and control constraints
UNCITRAL MLETR is a model law; jurisdiction-specific enactment, effective scope and instrument types matter. Do not assume all countries recognise electronic negotiability, nor Uganda/ZA recognition solely because another market has enacted it. Before any ETR financing, verify enforceable instrument, current legal holder/control, change-of-medium, duplicate-original prevention, endorser, title/security instrument and applicable scheme. DCSA document message conformity alone is not legal effect.

## 4. Financial and technical risks
Duplication of electronically transferable original, compromised holder credentials, fraudulent warehouse receipt, conflicting collateral registry, unauthorised endorsement, revoked issuer trust, on-chain privacy leaks, failed redemption, broken smart contract and sovereign/local title conflict require bounded source-trust handling. UNKNOWN_CONTROL must block proposed collateral-dependent funding unless funder explicitly documents lawful alternative.

## 5. Conditional business case
Require real buyer/funder demand, qualified control-scheme issuer, independent law/counsel confirmation, integrated Trade Docs TDOC-0017 proof, source-rights evidence, pricing/custodian operating model and insurance/incident recovery arrangements. Tokenisation adds a separate securities, digital-asset, custody, settlement, accounting and cyber-risk ADR if an actual viable use case emerges.

## 6. Rejected shortcuts
Mint invoice hash as token of title; blockchain prevents double finance automatically; token possession equal to lender lien; source PDF legally equivalent to eBL in all markets; SCF-issued electronic original; public finance claim explorer.

## 7. Implementation gates (future)
| Gate | Evidence |
|---|---|
| SCF-DCL-01 | Instrument, holder/control, registry, assignment and title authority matrix |
| SCF-DCL-02 | Trade Docs exact version/ref and qualified control provenance |
| SCF-DCL-03 | Synthetic transfer/surrender and conflicting-original negative tests |
| SCF-DCL-04 | Source control provider, funder and counsel jurisdiction sandbox |
| SCF-DCL-05 | Independent token/security/custody architecture only if real demand |
| SCF-DCL-06 | Specific instrument/corridor production certification and legal review |

## 8. References/review triggers
[UNCITRAL MLETR enactment status](https://uncitral.un.org/en/texts/ecommerce/modellaw/electronic_transferable_records/status) and [Trade Docs ADR programme](https://github.com/baobab-platform/baobab-trade-docs/tree/main/docs/adr). Reopen if domestic law, recognised provider or bank instrument changes.

## 9. Cross-cutting implementation and review protocol

IAM-authenticated principal and caller-bound CP tenant/legal entity/market context precede all cases, partner actions and data disclosures. Use source-owned cross-engine references, immutable external observations, source time/revision, purpose-scoped permissions, safe idempotency and reconciliation for UNKNOWN outcomes. Never treat funder approval, instrument issuance, received bank money, accounting balance and regulatory permission as the same status. Shared alone governs canonical capability/event semantics; CP/EA-09 registers and certifies only proven provider scopes.

**Revisit procedure:** date and cite changed law/partner/technical standards, classify the assertion as verified fact vs design inference, identify impacted old cases and contracts, map privacy/technical/financial risks, and file a superseding or amended ADR with evidence and rollback/grandfathering policy. Future gates require implementation tests and independent legal, business, security and partner approvals. No gate passes by merging this document.
