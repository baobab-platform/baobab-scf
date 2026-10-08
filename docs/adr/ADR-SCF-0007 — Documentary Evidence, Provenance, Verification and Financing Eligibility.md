# ADR-SCF-0007 — Documentary Evidence, Provenance, Verification and Financing Eligibility

**Status:** Proposed — not automatically Accepted by PR merge  
**Date:** 2026-10-08  
**Repository:** `baobab-platform/baobab-scf`  
**Programme:** Foundation and product architecture; ADR-SCF-0001 remains the controlling **Proposed** foundational charter  
**Operating premise:** Partner-led, headless, self-hosted and provider-neutral; no lending, underwriting, funds custody, payment execution or financial ledger authority for SCF  
**Shared authority:** `baobab-platform/shared` controls wire contracts, canonical capability/event semantics; CP controls tenant/binding/certification; IAM controls identity.  

> **Decision status vs evidence:** this is an engine-local architecture *proposal*. No source runtime, licensed finance activity, external funding relationship, market permission, capability certification or production-readiness claim follows from this ADR.


## 1. Decision
SCF builds an immutable, versioned **FinancingEvidenceManifest** of permitted references and source-derived documentary facts. Trade Docs owns TradeDocument/DocumentVersion/ContentArtifact and issuer/verification/validity projections; Regulations owns legal requirement satisfaction; TMS owns actual transport and custody; Pulse may provide optional intelligence. **SCF does not equate an uploaded PDF, valid checksum, OCR result or AI score to a genuine financeable obligation.**

## 2. Manifest semantics
| Fact class | Example source and control | SCF permitted interpretation |
|---|---|---|
| Commercial | Trade contract/order buyer/parties/terms | Pin snapshot; Trade remains authority |
| Financial | ERP invoice, credit note, open balance and due date | Pin observation; reconcile before financing |
| Documentary | Trade Docs issuer assertion, exact DocumentVersion, verification and validity | Represent assertion and observed verification **separately** |
| Physical | TMS delivery, cargo handoff and POD evidence reference | Physical event does not prove buyer accepted payable |
| Regulatory | Regulations decision/evidence assessment with scope/effective date | SCF may enforce contract requirement, not independently rule legality |
| Risk intelligence | Pulse observations/market indicators | Advisory and labelled, not funder underwriting |
| Legal rights | External assignment/security registry, mandate, notice and lender confirmation | Treat as external evidence, not globally definitive ownership |

A manifest item has typed Shared reference, source observed_at, source change version, provenance/actor, evidence digest, rights to disclose, purposes, retention/expiry and verification/unknown outcome. **Exact DocumentVersion reference must not silently resolve to "latest"**; historical offers preserve the evidence bundle actually shared with a funder.

## 3. Manifest lifecycle
COLLECTING -> SNAPSHOT_LOCKED -> DISCLOSURE_APPROVED -> SUBMITTED -> SUPERSEDED/ARCHIVED. Verification is an **independent axis** (PENDING / VERIFIED_METHOD_SPECIFIC / DISCREPANCY / UNKNOWN / INVALID). A later source change emits a new evidence-impact observation and may require resubmission; never rewrite what funder saw. "Completeness for partner application" is distinct from Regulations' legal sufficiency.

~~~mermaid
flowchart LR
 D["Trade Docs exact versions"] --> M["Pinned evidence manifest"]
 E["ERP/Trade/TMS snapshots"] --> M
 R["Regulations decision refs"] --> M
 M --> V["Technical checks and disagreements"]
 V --> C["Purpose-limited disclosure consent"]
 C --> F["Funder provider adapter"]
 F --> A["External eligibility/underwriting authority"]
~~~

## 4. Discrepancy handling
If invoice says quantity 100 but delivery says 90, preserve both source assertions, issue type, materiality threshold/contract basis and reviewer state; do not rewrite ERP invoice or infer fraud. If documentation is corrupt, issuer unknown, signature expired or supplier revoked disclosure permission, block sensitive sharing and explicitly record what facts were already legally delivered to the funder. Funder-specific completeness rules are programme-contract rules, not embedded regulatory law.

## 5. Privacy and future model use
Consent is audience/purpose/version-specific and revocable for future processing to extent legally possible; previous legally required retention remains governed. Do not feed raw documents to external LLMs without legal processor agreement, tenancy isolation and prior permission. A feature proposed by Pulse must preserve source, model version and confidence and not silently become issuer-certified.

## 6. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-EVD-01 | Manifest schema and owner/typed reference/identity-pinning contract tests |
| SCF-EVD-02 | Source attribution, signature/checksum vs issuer validity and OCR caution tests |
| SCF-EVD-03 | Source change/credit-note/revocation impact and immutable funder disclosure history |
| SCF-EVD-04 | Disclosure scopes, foreign tenant and competitor-funder privacy negative tests |
| SCF-EVD-05 | Trade Docs RTD-06 where applicable and ERP/TMS/Regulations conformant consumer fixtures |

## 7. Rejected designs / revision triggers
Reject local TradeDocument clone, global "document_verified" boolean, all-or-nothing evidence score, perpetual presigned file URLs and Pulse-as-Regulations. Revisit when documentary standards or transferability controls evolve. [Shared TradeDocument v2](https://github.com/baobab-platform/shared/tree/main/contracts/trade-document/v2), [RTD-05 cross-engine refs](https://github.com/baobab-platform/shared/tree/main/contracts/cross-engine-reference/v1).

## 8. Non-negotiable cross-engine controls and acceptance

SCF must authenticate caller via IAM and redeem caller-bound CP context, authorise each source object/counterparty and store tenant/legal-entity scope in durable state and workers. Exact external references remain source-owned; no direct writes to ERP, Trade, Payments, Trade Docs, TMS, Regulations or partner databases. Sensitive evidence is purpose-minimised, encrypted, auditable and non-replicated across sibling tenants. Provider callbacks are independently authenticated, replay-safe and provenance-bearing; an HTTP acknowledgement is not funding. Canonical `financing.*` or `scf.*` keys and events are **illustrative until Shared-approved** and must never be declared as implemented or active by merging this ADR.

**Review protocol:** when evidence, market rules or provider terms change, add an explicit dated amendment/superseding ADR recording the triggering fact, impacted rules, consumer migration and backward-compatibility plan. Keep the original accepted source facts and issued financial documents immutable. Implementation gates require exact code, fixtures, failure tests, known deferrals and review sign-off, followed by independent EA-09/Control Plane activation only for proven product/market/provider operations.
