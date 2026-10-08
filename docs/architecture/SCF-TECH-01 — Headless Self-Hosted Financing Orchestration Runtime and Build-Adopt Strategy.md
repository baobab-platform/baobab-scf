# SCF-TECH-01 — Headless, Self-Hosted Financing Orchestration Runtime and Build–Adopt Strategy

**Status:** Proposed — separate engine-local technology decision; not accepted or implemented  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Primary architecture authority:** Proposed ADR-SCF-0001 and proposed ADR-SCF-0002..0035; accepted Shared/CP/IAM/Payments/ERP/Trade/Trade Docs/TMS boundaries  
**Stack preference:** **Python 3.14 + Django 6.0 + PostgreSQL 17** as a **candidate**, not an implementation claim or imposed cross-repository rule. Go/PostgreSQL is an existing-stack alternative to be tested.  
**Current repository evidence:** Foundation-0 template and Proposed charter; no real SCF server, financial processing runtime, migrations, certified provider or bank connectivity.

## 1. Question and proposed decision

Which minimal, durable, auditable technology should run Baobab SCF's canonical financing-case/evidence/partner orchestration while preserving provider neutrality and self-hosting? **Prefer an existing Baobab technology family and own the business-domain boundary.** Stage an executable proof using Python 3.14, Django 6.0 and PostgreSQL 17. A Go/PostgreSQL implementation remains a benchmarkable fallback if the workload or Python dependency constraints make it preferable. Neither option may bypass Shared contracts, trusted CP context, IAM identity, financial-event integrity, actor authority, legal permissions or funder contractual reality.

**Explicit non-selection:** Apache Fineract and other banking-core JVM platforms; mandatory Kubernetes-only components; proprietary core banking SaaS; mandatory blockchain; new event broker solely for SCF; custom payment/FX engine; integrated lender underwriting/servicing ledger; unreviewed OCR/LLM credit decision systems. These may be evaluated later with a separate business/technology ADR only when evidence demands them.

## 2. Intended topology

~~~mermaid
flowchart TD
  E["Thamani / ZuriBeans / external entitled estate"] --> G["IAM-authenticated and CP-context-bound SCF API"]
  G --> D["SCF Domain Kernel: Case / Programme / Offer / Evidence / Consent"]
  D --> P["SCF PostgreSQL 17 (isolated)"]
  P --> O["Durable outbox / inbox / workflow leases"]
  O --> A["Funder adapter ports / signed callbacks"]
  A --> F["Qualified external finance partners"]
  D --> R["Source-owned cross-engine read ports"]
  R --> T["Trade / ERP / Trade Docs / TMS / Regulations"]
  O --> Y["Authorised Payments/ERP reconciliation read models"]
  Y --> D
~~~

The system **may operate entirely as an API + workers**; no SCF-owned buyer/supplier frontend is required. Customer and partner interfaces belong to entitled digital estates or partner systems. Core case identity and stored decisions must survive funder/provider replacement.

## 3. Technology component responsibilities

| Layer | Candidate and responsibilities | Decision guard |
|---|---|---|
| Runtime | Python 3.14, Django 6.0, ASGI, standard tested Python packages | prove official compatibility and secure long-term support in CI |
| Domain | plain typed Python domain models / services with distinct application ports | no ORM model dictates canonical financial semantics |
| Database | isolated PostgreSQL 17, migration-managed schema, immutable observation tables, outbox/inbox | not ERP/funder loan ledger or Payments bank ledger |
| API | versioned HTTP/OpenAPI and exact validated Shared contracts | unknown/denied/exception outcomes explicit; no client-supplied tenancy authority |
| Authentication | IAM token/workload validation; CP redeemed caller-bound context and entitlements | identity alone does not confer finance/licence authority |
| Workers | bounded worker processes leasing durable Postgres workflow/outbox tasks | stable idempotency, source unknown-outcome reconciliation |
| External funder | typed provider-neutral adapter ports and tenant/legal-entity-scoped credentials | a bank API SDK never becomes domain authority |
| Source integrations | Trade/ERP/Payments/Trade Docs/TMS/Regulations exact versioned reference ports | no shared foreign database and no unsupported materialised truth |
| Secrets/crypto | Baobab approved secret manager/KMS and crypto/key rotation policy | no funds credentials in repo, logs or customer browser |
| Observability | existing logging/tracing/metrics and incident tooling used elsewhere in Baobab | health status != funder certification |
| Runtime packaging | standard DevContainer, Docker, pinned dependency locks and tag-driven staging when configured | no deployment/production promise without live evidence |

## 4. Specific Django 6.0 transaction constraint

Django 6.0 supports ASGI/async views and many asynchronous ORM operations, **but its official async documentation states transactions do not yet work in async mode**. Therefore every source-of-truth financial state mutation requiring atomic FinancingCase revision + outbox + idempotency response MUST execute inside a **synchronous transaction boundary**, optionally invoked from an async request using a safe bridge such as `sync_to_async`. Never interleave separate async ORM writes and assert they commit as one transaction. Prove duplicate request and worker-crash behaviour in PostgreSQL integration tests.

Use Django database migrations with controlled expand/migrate/contract deployment, not implicit schema drift. Require tests for query/row locking or equivalent concurrency mechanism for local receivable allocations. RLS is optional defence-in-depth, not a substitute for application-level CP context/relationship policy; Postgres table-owner bypass and session reuse must be exercised in negative tests.

## 5. Domain and event storage

A relational write model includes: FinancingCase, Participation, EvidenceManifest, ClaimAllocation, ApplicationAttempt, OfferSnapshot, Consent/AcceptanceEvidence, FundingObservation, Exception, Inbox, Outbox and ProjectionCheckpoint. **Immutable observations** keep original source, source ID, external/native reference, exact Shared object ref and pinning mode, provider-authenticated digest, occurred/received/recorded timestamps, effective version and source trust outcome. Current finance position is a **rebuildable projection**, not a loan ledger.

The outbox must be atomically inserted with consequential local state using one Postgres transaction. External provider submissions must use stable idempotency/correlation; timeout = UNKNOWN_OUTCOME until status lookup/reconciliation. Postgres is not a distributed transaction manager; irreversible funder actions cannot be undone by rolling back a local transaction.

## 6. Threat model / operational footprint

**Critical threats:** supplier/funder credential compromise; forged bank approvals and callbacks; double offer acceptance; repeated invoice purchase; cross-tenant financing leak; optimistic "funded" status; overbroad subsidiary parent permissions; malicious PDFs/prompt injection; unauthorized data distribution; inaccurate FX calculations; stale ERP receivable; actor impersonation; black-box model assigning creditworthiness without lender review.

Enforce secret/environment separation, immutable audit, maker/checker where required, recipient purpose and legal entity scope. Keep supplier/financial PII out of general logs and emitted events. Use dependency/SBOM/licence scanning, OS patches, DB backups/PITR, source reconciliation, monitoring and incident response. Do not deploy a new complete bank platform merely to supply PDF rendering, workflow timers or a payment provider adapter.

## 7. Technology alternatives and decision criteria

| Candidate | Main advantage | Principal risk | Decision |
|---|---|---|---|
| Python 3.14 / Django 6 / PostgreSQL 17 | aligns with Baobab Python/Regulations/Trade Docs direction and rich auditable domain workflows | ORM async transaction boundary, dependency compatibility, worker and transaction discipline | **Preferred proof-of-concept candidate** |
| Go / PostgreSQL 17 | aligns CP/IAM Go family, predictable resource use | finance-domain modelling/workflow libraries, extra engineering work | **Valid second candidate** |
| Dedicated banking/loan servicing core (e.g. Fineract) | rich lender accounting/servicing if required | adds mandatory JVM and licensed-core operational ownership despite default partner-led role | **Reject as baseline; revisit ADR-0035 only** |
| Financial SaaS provider SDK as SCF core | quicker demo | vendor-defined identity/workflows and hard-to-exit contract lock-in | **Reject canonical core** |
| Blockchain/tokenisation core | asset transfer possibility if qualified external law/scheme exists | no inherent legal title, custody, compliance or business case | **Reject baseline; ADR-0034 conditional** |

Selection must be evidence-backed: domain integration footprint, dependency support on Python 3.14, OpenAPI/Shared contracts, Postgres concurrency, secure IAM/CP binding, actual self-hosting, crash/replay and operational cost. No benchmark number or "faster" language claim is assumed without measurement.

## 8. No premature canonical capability or financial authority

At Foundation-0 the repository contains no SCF production-capable code or funder contract. Shared `finance` namespace is currently associated with financial accounting/receivables concepts, and any additional key/domain needs an explicit census and Shared authority decision. Do not create a new `scf` namespace by committing this technology document; do not mark proposed operations IMPLEMENTED or provider READY until source, tests, exact contracts and EA-09 evidence exist. CP bindings, actual partner permissions and business terms must be separately approved.

## 9. Implementation gates and measurable exit evidence

| Gate | Work package | Required proof |
|---|---|---|
| SCF-TECH-01A | Framework and same-stack selection | Python 3.14/Django 6/Postgres 17 pinned dependency and worker/ASGI compatibility spike, plus Go fit comparison |
| SCF-TECH-01B | Security/tenancy and financial transaction proof | CP caller-bound context, IAM verification, Postgres atomic case+outbox/idempotency and tenant denial tests |
| SCF-ENG-01 | Foundation activation | Replace template .example files, real DevContainer, workflows, reproducible Docker build, security scans and contracts.lock |
| SCF-ENG-02 | Domain/persistence slice | Source-bound FinancingCase, exact obligation/manifest reference, versioning and partial-amount concurrency tests |
| SCF-ENG-03 | APIs and event backbone | OpenAPI/Shared-approved contracts, outbox/inbox, duplicate and crash recovery, DLQ and projection replay |
| SCF-ENG-04 | Partner simulation | Two independent mock funders; offers, rejection, different fees, callbacks, uncertainty and reconciliation |
| SCF-ENG-05 | Source integration | Read-only ERP/Trade/Trade Docs/Payments/Regulations conformance; no cross-domain writes |
| SCF-ENG-06 | Tenant/security and operator controls | Revocation, wrong tenant, funder confidentiality, secrets, GDPR/POPIA-market privacy and incidents |
| SCF-ENG-07 | Constrained partner sandbox and readiness | Real funder agreement, legal scope, independent penetration review, backup/restore, monitored SLO and EA-09/CP proof |

**First executable journey:** synthetic qualified buyer-approved invoice -> canonical case -> pinned documentary evidence -> synthetic funder submission -> binding mock offer -> explicit acceptance intent -> simulated payout/ERP funding observation -> reconciled projection. Test a second tenant and negative flows; label synthetic funds. It is **not** a legal lending pilot or live payment.

## 10. Standards and authoritative documentation consulted

- [Python 3.14 documentation](https://docs.python.org/3.14/).
- [Django 6.0 async documentation and transaction limitation](https://docs.djangoproject.com/en/6.0/topics/async/).
- [Django 6.0 migrations](https://docs.djangoproject.com/en/6.0/topics/migrations/).
- [PostgreSQL 17 row security](https://www.postgresql.org/docs/17/ddl-rowsecurity.html) and [row locks](https://www.postgresql.org/docs/17/explicit-locking.html).
- [Shared capability catalogue](https://github.com/baobab-platform/shared/tree/main/contracts/capability/v1) and [CrossEngineObjectReference](https://github.com/baobab-platform/shared/tree/main/contracts/cross-engine-reference/v1).
- [Global Supply Chain Finance Forum product definitions](https://supplychainfinanceforum.org/glossary/).

## 11. Review/change control

Proposed technology status must remain until ADR acceptance and specific compatibility, security, performance and operating evidence are reviewed. If a funder, licensing authority, market or runtime framework changes, record a dated evidence-based decision delta, migrations, affected API contracts and safer alternative. **Do not mark this ADR as accepted or deployed merely because its GitHub PR merges.**