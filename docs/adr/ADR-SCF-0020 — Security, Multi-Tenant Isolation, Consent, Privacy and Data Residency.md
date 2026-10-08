# ADR-SCF-0020 — Security, Multi-Tenant Isolation, Consent, Privacy and Data Residency

**Status:** Proposed — no production or financial permission by documentation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Decision class:** Platform API/runtime, secure cross-engine orchestration and independent provider acceptance  
**Precedence:** Proposed ADR-SCF-0001 for default partner-led operating model; applicable accepted Shared, CP, IAM, ERP, Trade, Trade Docs, Payments and TMS boundary decisions  

> This repository is a Foundation-0 architecture scaffold. No application runtime, SCF capability declaration, licensed funder integration or market activation is established by these decisions.


## 1. Decision
SCF is a multi-tenant financial-evidence processor where commercial terms, bank/funder data and applicant identities may be highly sensitive. **IAM** authenticates principals/workloads; **CP** establishes trusted tenant/legal entity, relationships, market, entitlements and context; **SCF** enforces operation-specific authorisation and purpose-limited disclosures. A parent Nabhold identity confers **no automatic access** to Thamani or ZuriBeans finance cases, obligor names, funder rates or repayments.

## 2. Trust chain and audience
~~~mermaid
flowchart TD
 A["Human/workload IAM token"] --> B["CP caller-bound context redemption"]
 B --> C["SCF tenant / entity / case relationship policy"]
 C --> D["Document/obligation purpose entitlement"]
 D --> E["SCF API, SQL, worker, provider adapter"]
 E --> F["Audited, redacted recipient projection"]
~~~

| Actor | Permitted by explicit grant | Denied by default |
|---|---|---|
| Applicant supplier | own cases, applicable offers, disclosed costs, consent | other supplier/funder bids |
| Anchor buyer | obligations it has approved and programme-appropriate aggregates | supplier bank account/full history |
| Qualified funder | consented application/evidence and own offer/position | rival bids, sibling tenant |
| Partner servicer | cases specifically serviced, restricted evidence | global underwriting and new contract acceptance |
| Group executive | legally authorised consolidated aggregate report | direct subsidiaries' case details |
| Platform worker | bounded service context for exact operation and actor trace | silent all-tenant administrator |
| Auditor | approved purpose/time scope and immutable trace | unbounded bulk export |

## 3. Data classification and residency
Identify KYC/KYB/UBO, PII, commercial pricing, receivables, bank account tokens, partner risk reports, sanctions flags, contracts, signatures and documentary artifacts separately. Record data subject, controller/processor roles, legal basis, approved recipient, transfer geography, retention and encryption/key custody. Uganda and South Africa are initial scenarios; avoid assuming country-of-company-registration alone decides localisation or cross-border transfer legitimacy. No external AI processor on confidential finance data without approved processor agreement/consent, minimisation and security assessment.

## 4. Active controls
Attribute-based resource/role authorisation on commands, read-model queries, caching, webhooks, files, outbox/inbox, search and exports. Wrong tenant/actor/expired grant/revoked partner/funder should fail closed. Separate metadata permission from exact Trade Docs document content permission: owning a FinancingCase doesn't grant access to all source document URLs. Encrypt in transit and at rest, rotate partner credentials, avoid secrets in logs, use short-lived audience-bound access, prohibit open egress to unreviewed finance endpoints. Break-glass needs case, reason, dual control and audit.

## 5. Consent, retention and tenant exit
Distinguish future disclosure revocation from already made legally binding disclosures or necessary legal retention. Tenant exit preserves funder contracts, source evidence and historical dispute records within lawful retention, not access by parent or different legal entities. Backup copies and derived ML datasets must be accounted for. Credit/sanctions information is not automatically usable for supplier marketing or group benchmarking.

## 6. Failure threats
IDOR via invoice ID, guessed obligation hash, one-time offer token reused, callback spoof, overbroad CP context, shared key across partner tenants, compromised signature, credential redirect, data retention conflict and model training leakage. Protect at resource and relationship level, not front-end route hiding.

## 7. Implementation gates
| Gate | Proof |
|---|---|
| SCF-SEC-01 | Threat/data flow analysis and actor-resource-action matrix |
| SCF-SEC-02 | CP/IAM identity, actor binding, expired/revoked and different legal-entity tests |
| SCF-SEC-03 | SQL/RLS/cache/worker/document/provider wrong-tenant negative suite |
| SCF-SEC-04 | Secret rotation, issuer mandate, encryption, retention and consent/withdrawal tests |
| SCF-SEC-05 | Uganda/South Africa data protection/transfer review for actual operating arrangement |
| SCF-SEC-06 | Independent penetration/security audit and sensitive production data approval |

## 8. Revision triggers
Changed data protection law, new funder data recipient, cross-border analytics, lending permissions or tenant isolation topology.

## 9. Review, evidence and compatibility protocol

Keep an ADR conformance index linking each normative statement to future source paths, unit/contract/negative tests, legal-review evidence, provider protocol version, failure/reconciliation runbook and known unsupported conditions. Any changed accepted contract or market assumption must be dated and explicitly reviewed; preserve historical financing submissions and accepted external agreements, not overwrite them. No direct writes to foreign authority databases and no unregistered finance/SCF events.

A documentation PR establishes **reviewable intentions**, not source implementation, authenticated provider availability, real-money approval, customer lending eligibility, CP registration or production readiness. Future implementation gates should each produce bounded PRs with reproducible CI and independent evidence.
