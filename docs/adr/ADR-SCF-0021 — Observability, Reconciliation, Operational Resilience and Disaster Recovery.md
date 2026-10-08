# ADR-SCF-0021 — Observability, Reconciliation, Operational Resilience and Disaster Recovery

**Status:** Proposed — no production or financial permission by documentation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Decision class:** Platform API/runtime, secure cross-engine orchestration and independent provider acceptance  
**Precedence:** Proposed ADR-SCF-0001 for default partner-led operating model; applicable accepted Shared, CP, IAM, ERP, Trade, Trade Docs, Payments and TMS boundary decisions  

> This repository is a Foundation-0 architecture scaffold. No application runtime, SCF capability declaration, licensed funder integration or market activation is established by these decisions.


## 1. Decision
Finance workflow correctness depends on **source truth, durable side effects, reconciliation and recovery**, not just API uptime. Implement telemetry and runbooks for source-system data quality, provider submissions, offers, authorisations, funding/repayment observations and partner outages. No false "99.9%" SLO, confirmed payment or recoverability claim without measured and approved evidence.

## 2. Operating signals
| Signal | What to measure | Critical operator action |
|---|---|---|
| Case/API response | auth denials, error class, successful transactional commands | diagnose tenant/contract failure |
| Funder availability | per-partner latency, rate-limit, schema drift, credential age | suspend new requests and use reconciliation |
| Unknown outcome backlog | external submissions not verified, oldest age and by partner | never blindly resubmit |
| Outbox/inbox | lag, duplicated/conflicting messages, DLQ and expiry | safe replay with original ID |
| Evidence freshness | ERP balance/Trade Docs/Regulations source revision age | revalidate before money-action phase |
| Position mismatch | funder vs Payments vs ERP amount/time/currency | hold contradictory status and assign owner |
| Fraud/security | revoked context, callback forgery, anomalous disclosure | incident response and audit |
| Recovery evidence | consistent backup, WAL RPO and actual restore drill | rebuild and reconcile side effects |

## 3. Dependency failure policy
~~~mermaid
flowchart LR
 A["External source/funder fails"] --> B{"Side effect possibly occurred?"}
 B -->|Yes| C["UNKNOWN; query original provider; operator hold"]
 B -->|No proven| D["Safe bounded retry, same digest/ID"]
 C --> E["Source-confirmed outcome"]
 D --> E
 E --> F["Corrected append-only observation and position"]
~~~

If optional risk intelligence is unavailable, core evidence case may proceed with manual checks, provided mandatory KYB/AML, legal mandate and funder approval are not bypassed. If ERP financial source is stale or required funder result unknown, external finance submission is blocked/held under reviewed policy. Unavailable Payments does not imply external bank hasn't paid.

## 4. Disaster recovery order
Restore PostgreSQL with case history, outbox/inbox and replay markers; validate keys/secrets/context; rehydrate projections; reconcile funder applications/offer acceptances and unknown payout/repayment side effects **before** resuming write traffic; verify document version refs and ERP balances; conduct maker/checker sign-off. RPO/RTO targets require risk/contract/business approval and actual test. Restoring an SCF database cannot undo a legally signed finance agreement or reverse an external bank transfer.

## 5. Financial/operational incident drills
At minimum simulate funder timeout after accepted application, double funding offer, lost webhook, ERP credit note after funding, Payments return, identity compromise, cross-tenant evidence request, backup restore with stale externally completed repayment, data breach and funder insolvency. Incidents have declared owner, communication, affected product/programme/cases, next action and legal reporting assessment.

## 6. Alternatives rejected
Blind retry/requeue all jobs, treating green health check as funder-ready, automatic alternate provider submission after unknown outcome, deleting a corrupted audit record, and displaying stale estimated positions as settled.

## 7. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-OPS-01 | Logs/traces/metrics/redaction, runbooks and owner escalation routing |
| SCF-OPS-02 | Funder timeout/callback storm/schema drift and idempotent replay |
| SCF-OPS-03 | Document/ERP/Payments mismatch reconciliation with traceable operator repairs |
| SCF-OPS-04 | PostgreSQL PITR, outbox recovery and external side-effect reconciliation drill |
| SCF-OPS-05 | Measured service and business SLO, RPO/RTO proposal with signed governance |
| SCF-OPS-06 | Staging chaos test, incident review and independent operator acceptance |

## 8. Revision triggers
New SLA, provider operating geography, infrastructure failure domain, real contracted counterparties or security requirement.

## 9. Review, evidence and compatibility protocol

Keep an ADR conformance index linking each normative statement to future source paths, unit/contract/negative tests, legal-review evidence, provider protocol version, failure/reconciliation runbook and known unsupported conditions. Any changed accepted contract or market assumption must be dated and explicitly reviewed; preserve historical financing submissions and accepted external agreements, not overwrite them. No direct writes to foreign authority databases and no unregistered finance/SCF events.

A documentation PR establishes **reviewable intentions**, not source implementation, authenticated provider availability, real-money approval, customer lending eligibility, CP registration or production readiness. Future implementation gates should each produce bounded PRs with reproducible CI and independent evidence.
