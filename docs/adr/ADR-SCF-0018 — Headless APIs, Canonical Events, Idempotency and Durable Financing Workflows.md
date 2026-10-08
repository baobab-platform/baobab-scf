# ADR-SCF-0018 — Headless APIs, Canonical Events, Idempotency and Durable Financing Workflows

**Status:** Proposed — no production or financial permission by documentation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Decision class:** Platform API/runtime, secure cross-engine orchestration and independent provider acceptance  
**Precedence:** Proposed ADR-SCF-0001 for default partner-led operating model; applicable accepted Shared, CP, IAM, ERP, Trade, Trade Docs, Payments and TMS boundary decisions  

> This repository is a Foundation-0 architecture scaffold. No application runtime, SCF capability declaration, licensed funder integration or market activation is established by these decisions.


## 1. Decision
Expose a **headless, versioned, context-bound** command/query API for case orchestration, eligible evidence resolution, programme participation and external funder workflows. SCF uses **local PostgreSQL transactions + transactional outbox/inbox** for material source-derived facts, not distributed ACID across Trade, ERP, Payments, Trade Docs, CP or external banks. Canonical event types/capability keys are **not established by this local ADR**. All proposed SCF event/command names are provisional until separate Shared capability census, contract semantics, event stewardship and consumer sign-off.

## 2. Bounded context API proposal (non-canonical)
| Port | Intended resource / response | Side-effect and trust |
|---|---|---|
| CreateFinancingCase | validated source obligation ref, product profile and case ID | requires CP redeemed context and idempotency |
| AttachEvidenceManifest | exact pinned sources, scope and version, results | never accepts unsourced issuer facts |
| EvaluateLocalProgrammeEligibility | local checked programme prerequisites and UNKNOWN outcomes | not credit underwriting |
| SubmitFunderApplication | exact signed consent, evidence snapshot, amount and provider | external unknown-outcome reconciliation |
| ReadFunderOffer | trusted external proposal and validity/recourse terms | not approval or cash |
| RecordSelectionAndConsent | exact terms, authorised legal representative | legal acceptance only when provider confirms |
| ObserveFundingAndPosition | source provider/payment/ERP observations and timestamps | not SCF ledger |
| ResolveFinancingCase | scoped read projection, source freshness and watermark | no cross-tenant funding visibility |

HTTP 202 acknowledges command acceptance, not bank submission or disbursement. Public finance operations are restricted to entitled legal entities and roles; no SCF superuser bypassing business mandates.

## 3. Durable workflow semantics
~~~mermaid
sequenceDiagram
  participant U as Entitled estate/workload
  participant S as SCF API
  participant D as SCF PostgreSQL
  participant O as Outbox/reconciler
  participant F as Qualified funder
  U->>S: Command + CP context + idempotency key
  S->>D: Atomic domain case/revision + outbox + request digest
  D-->>U: Stable case/operation ID
  O->>F: Signed, provider-scoped application snapshot
  F-->>O: ACK or timeout or later binding decision
  O->>D: Source-attributed observation, retry/reconcile/DLQ
~~~

State machine phases: created, evidence-ready, partner-submission-pending, submission-unknown, provider-confirmed-received, externally-decided, offer-presented, acceptance-confirmation-pending, funder-confirmed, funding-observed, reconciled/exception. Subordinate decision/payment/contract statuses are separate axes. Workflow workers re-check current funder participation and issuer consent before **new irreversible** submission; preserving original authorisations and case provenance.

## 4. Idempotency and integrity
Scope write keys by tenant/legal entity, caller/audience, command, aggregate and input digest. Same key/same digest returns original committed outcome; same key/different digest returns conflict. Inbox dedupe on source provider + event ID + digest; conflicting duplicate quarantined. Delivery is at-least-once; no false exactly-once or global message ordering promise. Outbox lease and retry metadata survive restart, with DLQ and controlled replay. A provider timeout can have irrevocable side effects; status lookup precedes reissue or alternate provider routing.

## 5. Consumer and compatibility boundaries
Read-only current projections expose watermark/freshness/uncertainty, not fabricated finality. Shared contract evolution follows major/minor compatibility, pinned revision/consumer fixture and producer transition policies. ERP payment/statement, TradeDocument and Regulations references remain source-owned and may not be converted into SCF custom copied schemas.

## 6. Negative testing and alternatives
Bad actor audience, stale context, wrong tenant, parent access, duplicated application, concurrent offer acceptance, wrong bank callback signature, unresolved assignment and schema drift must fail safe. Reject synchronous chain that makes money move on a UI request or blindly retries across providers.

## 7. Implementation gates
| Gate | Proof |
|---|---|
| SCF-API-01 | OpenAPI command/query proposal and source/actor/CP context matrix |
| SCF-API-02 | Exact Shared contract governance proposal for accepted capability/event semantics |
| SCF-API-03 | Postgres atomic case + outbox/inbox, crash, duplicate and conflicting-digest tests |
| SCF-API-04 | Provider timeout/unknown outcome, safe status inquiry and workflow recovery |
| SCF-API-05 | Cross-tenant/role, revoked permission, schema-compatibility, DLQ/replay tests |
| SCF-API-06 | Independently tested partner sandbox and measured operations before activation |

## 8. Revision triggers
New canonical event, payment partner transport or legal signing workflow requires a dated contract impact analysis; green CI is not certified capability support.

## 9. Review, evidence and compatibility protocol

Keep an ADR conformance index linking each normative statement to future source paths, unit/contract/negative tests, legal-review evidence, provider protocol version, failure/reconciliation runbook and known unsupported conditions. Any changed accepted contract or market assumption must be dated and explicitly reviewed; preserve historical financing submissions and accepted external agreements, not overwrite them. No direct writes to foreign authority databases and no unregistered finance/SCF events.

A documentation PR establishes **reviewable intentions**, not source implementation, authenticated provider availability, real-money approval, customer lending eligibility, CP registration or production readiness. Future implementation gates should each produce bounded PRs with reproducible CI and independent evidence.
