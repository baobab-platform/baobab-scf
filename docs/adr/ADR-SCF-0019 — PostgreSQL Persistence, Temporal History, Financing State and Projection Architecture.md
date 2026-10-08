# ADR-SCF-0019 — PostgreSQL Persistence, Temporal History, Financing State and Projection Architecture

**Status:** Proposed — no production or financial permission by documentation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Decision class:** Platform API/runtime, secure cross-engine orchestration and independent provider acceptance  
**Precedence:** Proposed ADR-SCF-0001 for default partner-led operating model; applicable accepted Shared, CP, IAM, ERP, Trade, Trade Docs, Payments and TMS boundary decisions  

> This repository is a Foundation-0 architecture scaffold. No application runtime, SCF capability declaration, licensed funder integration or market activation is established by these decisions.


## 1. Decision
Adopt an **SCF-owned PostgreSQL 17 transactional storage boundary** when implementation is chosen. Persist canonical SCF identities, workflow state, immutable source observations, versioned applications/offers/consent/manifest, transactional inbox/outbox, reproducible projections and operator reconciliation. SCF will **never** replicate a funder's loan ledger or use ERP/Postgres cross-database writes to pretend it has banking authority. Python 3.14/Django 6.0 versus existing-stack Go remains a separately reviewed SCF-TECH-01 choice.

## 2. Aggregates and consistency boundaries
| Aggregate | Own transaction invariants | Cross-domain link |
|---|---|---|
| FinancingCase | case identity, tenant/party/technique, version and state | referenced ERP obligation and Trade contract |
| FinancingProgrammeParticipation | qualified partner roles, scoped terms, eligibility/activation dates | CP legal entity, funder contract refs |
| EvidenceManifest | fixed source references/digests and version | Trade Docs/ERP/TMS/Regulations |
| FundingApplication / Attempt | exact submitted digest, provider external IDs, idempotency and outcome | provider decision source |
| FinancingOffer | funder-issued version, price/recourse, expiry, identity | partner native offer ID |
| Consent/AcceptanceEvidence | legal actor, exact terms and document references | IAM/CP and Trade Docs |
| ClaimReservation / EncumbranceObservation | allocated amount/concurrency and source legal claims | external registry/assignee |
| Funding/RepaymentObservation | source payment ID, amount/currency, occurred/received times | Payments/funder and ERP |
| FinancingException/Resolution | discrepancy type, owner, evidence, next action | source-domain decisions |
| Outbox/Inbox/ProjectionCheckpoint | durable idempotency, leases, errors/replay | Shared event/consumer IDs |

## 3. Temporal model
Keep **source_effective_at**, **occurred_at**, **received_at**, **recorded_at** and **superseded_at** where relevant. A snapshot represents what SCF knew when an application was sent; later corrected ERP balance or funder status must not rewrite the original application/offer. Version-pin underlying ERP obligation, Trade contract, TradeDocument and provider-native source and show uncertainty where source allows no historical versioning. Explicitly distinguish current case projection from historical source events.

~~~mermaid
flowchart LR
 E["Immutable external observations and SCF events"] --> J["Append-oriented event journal"]
 J --> P["Deterministic projection reducer (versioned)"]
 P --> C["Current FinancingCase/Position view"]
 J --> R["Audit and historical as-of reconstruction"]
~~~

## 4. Isolation, concurrency and integrity
Tenant/legal-entity foreign keys and constraint indexes are mandatory. Postgres RLS may be used as defence-in-depth only with proven session context and background-worker safety. Claim amount reservations require transactional locking/serialisable or exclusion-constrained concurrency tests to avoid double local booking. Monetary values are decimal-safe with explicit currency. No idempotency scope may be cross-tenant global without privacy review. Sensitive financing data requires encryption, retention, backup and data residency approval.

## 5. Migration, retention and recovery
Forward/back compatible expand-migrate-contract schema rollout, durable outbox/inbox replay, partial restore and projection rebuild are required before staging admission. Restore must preserve case, manifest, source observations, provider unknown-outcome tasks and immutable legal evidence links. Retention for financial/legal claims may outlast operational case closure, subject to market privacy/legal hold approval. Never delete legal document evidence merely because a supplier closes an account.

## 6. Alternatives rejected
One giant case JSON blob as sole truth, cross-engine foreign keys, globally mutable last offer field, default float for money, event-only persistence without recoverable idempotency, and optimistic paid flag with no funder/ERP source.

## 7. Implementation gates
| Gate | Acceptance |
|---|---|
| SCF-DB-01 | Versioned PostgreSQL schema, migration and business-reference scoping |
| SCF-DB-02 | Competing concurrent offers and claim allocation conflict tests |
| SCF-DB-03 | Outbox/inbox crash/replay, source attribution and bitemporal observation fixtures |
| SCF-DB-04 | Rebuild deterministic position projections and as-of snapshot audit |
| SCF-DB-05 | Backup/PITR, retention/legal hold and tenant isolation proof |
| SCF-DB-06 | Upgrade/backfill/rollback and data classification review |

## 8. Revision triggers
Measured scaling/region needs, a production financial-services mandate or Shared contract evolution; new persistence technology demands a separate acceptance proof, not an assumed new stack.

## 9. Review, evidence and compatibility protocol

Keep an ADR conformance index linking each normative statement to future source paths, unit/contract/negative tests, legal-review evidence, provider protocol version, failure/reconciliation runbook and known unsupported conditions. Any changed accepted contract or market assumption must be dated and explicitly reviewed; preserve historical financing submissions and accepted external agreements, not overwrite them. No direct writes to foreign authority databases and no unregistered finance/SCF events.

A documentation PR establishes **reviewable intentions**, not source implementation, authenticated provider availability, real-money approval, customer lending eligibility, CP registration or production readiness. Future implementation gates should each produce bounded PRs with reproducible CI and independent evidence.
