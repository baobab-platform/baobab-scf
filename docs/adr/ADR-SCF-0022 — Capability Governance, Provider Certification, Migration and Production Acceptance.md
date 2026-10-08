# ADR-SCF-0022 — Capability Governance, Provider Certification, Migration and Production Acceptance

**Status:** Proposed — no production or financial permission by documentation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Decision class:** Platform API/runtime, secure cross-engine orchestration and independent provider acceptance  
**Precedence:** Proposed ADR-SCF-0001 for default partner-led operating model; applicable accepted Shared, CP, IAM, ERP, Trade, Trade Docs, Payments and TMS boundary decisions  

> This repository is a Foundation-0 architecture scaffold. No application runtime, SCF capability declaration, licensed funder integration or market activation is established by these decisions.


## 1. Decision
SCF gets no implicit finance capability merely by adopting Proposed ADRs. **Shared** owns canonical capability-key vocabulary, namespace, events, producer stewardship and wire schemas; **Control Plane** owns Engine/CapabilityProvider definitions, certification status, deployment instances, tenant grants and capability bindings; **EA-09** decides independently evidenced provider support. SCF can be eligible for **one proven operation/product/market/partner** while every other product remains architecture-only.

~~~mermaid
flowchart TD
 A["Proposed ADR-SCF-0001..0035"] --> B["Real runnable code and contract tests"]
 B --> C["Shared approved capability census and versioned APIs/events"]
 C --> D["SCF support declaration with source/test paths"]
 D --> E["EA-09 independent certification review"]
 E --> F["CP provider registration and tenant/market grants"]
 F --> G["Staged deployment and real partner pilot"]
~~~

## 2. Capability governance sequence
1. Perform census over what SCF can actually do vs proposed case/domain vocabulary. Distinguish existing Shared **finance** domain for ERP financial facts and **payment** domain for Payments. Do not invent an **scf** domain or keys such as financing.case.create as canonical until a Shared ADR, namespace registry and catalogue definition are approved.
2. Propose truly canonical provider-neutral operations and their contracts with source authority, idempotency, audiences, errors, versions, event producers and applicable market/product scope.
3. Pin Shared revision and run contract fixture tests; record Shared consumers (Trade, Trade Docs, ERP, Payments, Pulse and digital estates) that require migrations.
4. Update provider-support declaration **only for demonstrated operations**; absence of a provider implementation is not a candidate-to-implemented promotion.
5. Follow independent EA-09 evidence and CP CapabilityBinding/entitlement; no default grant to all Nabhold subsidiaries.

## 3. Certification matrix
| Axis | Evidence before ACTIVE |
|---|---|
| Canonical meaning | approved Shared catalogue/schema and producers |
| Implementation | audited code, pinned deps, reproducible build, CI and no template files |
| Legal authority | product/market/legal entity, role, contracts, permitted intermediary activity |
| Partner scope | actual funder operation/sandbox/live proof, signing/credential and source response |
| Data | ERP/Trade Docs/Payments exact refs, tenant isolation and privacy assessment |
| Financial safety | no double local financing, unknown payout outcome, source funded vs settled separation |
| Resilience | outbox/inbox retry, reconciliation, restore, SLO/RPO/RTO and incident process |
| Business | consented real buyer/supplier, transparent fees, pilot acceptance and rollback |
| Activation | capability-specific CP provider registration, deployment, grant and controlled routing |

## 4. Migration / rollout
The first slice uses simulated funder and synthetic financial flows; then approved funder sandbox, staged consumer end-to-end integration, limited legally authorised pilot and specifically scoped production. Never retroactively upgrade old case evidence versions to newer contracts or disclose additional data without consent. Freeze risky finance writes on rollback, reconcile external submissions/agreements, and preserve already binding agreements. Add second funder/market only after differentiated conformance.

## 5. Prohibited support claims
"SCF is production-ready"; "lending available"; "Africa-wide supported"; "no duplicate invoices anywhere"; "instant payout"; "certified bank connection" and "underwriting AI" are prohibited without precise supporting scope, law, live partner and acceptance evidence. A successful code-only simulated pilot does not establish any such claim.

## 6. Implementation gates
| Gate | Proof |
|---|---|
| SCF-REL-01 | Foundation activation and accepted runtime technology decision |
| SCF-REL-02 | Canonical capability census/Shared contracts and event stewardship |
| SCF-REL-03 | Complete simulated buyer/supplier/funder case with two tenants |
| SCF-REL-04 | Adverse-case, security, privacy and accounting/Payments isolation proof |
| SCF-REL-05 | One qualified partner sandbox and approved legal role review |
| SCF-REL-06 | Measured recovery, reconciliation, operator runbook and compliance signoff |
| SCF-REL-07 | EA-09 certification, CP grant/binding, controlled pilot and audited rollback |

## 7. Revision triggers
New Shared EA-09 rules, supported first partner/product, changed market law or controlled internal finance programme requires a formally accepted staged amendment.

## 9. Review, evidence and compatibility protocol

Keep an ADR conformance index linking each normative statement to future source paths, unit/contract/negative tests, legal-review evidence, provider protocol version, failure/reconciliation runbook and known unsupported conditions. Any changed accepted contract or market assumption must be dated and explicitly reviewed; preserve historical financing submissions and accepted external agreements, not overwrite them. No direct writes to foreign authority databases and no unregistered finance/SCF events.

A documentation PR establishes **reviewable intentions**, not source implementation, authenticated provider availability, real-money approval, customer lending eligibility, CP registration or production readiness. Future implementation gates should each produce bounded PRs with reproducible CI and independent evidence.
