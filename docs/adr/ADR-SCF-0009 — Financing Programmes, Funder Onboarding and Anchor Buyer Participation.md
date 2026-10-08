# ADR-SCF-0009 — Financing Programmes, Funder Onboarding and Anchor Buyer Participation

**Status:** Proposed — architecture review, not acceptance or implementation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Premise:** Partner-led, headless, self-hosted; existing ADR-SCF-0001 remains Proposed and controls the charter  
**Authority:** External qualified financiers own credit/contract decisions; ERP owns financial balances; Payments owns permitted payment execution; Shared owns canonical event/capability vocabulary, CP tenant/provider, IAM authentication  

> No regulator/licence, funder contract, provider certification, bank transfer or production capability is created by this ADR.


## 1. Decision
A FinancingProgramme is a versioned arrangement linking approved product and operating model to sponsor/anchor buyer, external funder(s), enrolled suppliers, legal entity, market and permissible obligations. An approved funder business organisation is **not** automatically a Baobab CapabilityProvider, nor proof of funding availability.

## 2. Programme model
| Entity/field | Operational meaning |
|---|---|
| ProgrammeIdentity | immutable programme_id plus version, effective interval, configured owner legal entity |
| Sponsor/anchor references | CP parties and buyer approval workflow from Trade/ERP |
| FunderParticipation | verified contracts, legal roles, products, currencies, regions and source funding status |
| SupplierEnrollment | required KYB/contract, consent, source relationship and per-party approval |
| ProgrammeLimitObservation | source funder declared capacity, currency, freshness and concentration constraints |
| TermsProfile | recourse, maturity, reserve, discount, disclosure, fee responsibilities and version |
| ParticipationMandate | contract/issuer, permitted actions, review/expiry, representative and revocation |
| Compliance/RiskGateRef | external product approval, KYB/KYC/sanctions evidence with effective time |

Programme status: DRAFT -> LEGAL_REVIEW -> PARTNERS_CONTRACTED -> PILOT_APPROVED -> ACTIVE_FOR_SCOPE -> SUSPENDED/RETIRED. New funder requests must be blocked after expiry; historic legally accepted commitments remain auditable and may require servicer migration.

~~~mermaid
flowchart TD
 A["Programme proposal"] --> B["Market legal authority"]
 B --> C["Funder and anchor contracts/KYB"]
 C --> D["Participant eligibility and data sharing"]
 D --> E["Adapter sandbox and controls"]
 E --> F["Scoped programme activation"]
 F --> G["Continuous risk/limit/consent monitoring"]
~~~

## 3. Isolation and capacity
Limits are qualified external observations, not cash balances; absence of funder status must not imply unlimited facilities. ZuriBeans and Thamani participate through their own legal contracting identities; Nabhold is not their implicit guarantor. A programme sponsor may see authorised aggregate metrics but not competing funders' offers or unrelated suppliers' invoice-level data.

## 4. Commercial design
Measure real eligible obligations, funder quote response, take-up, source-confirmed financing, borrower cost and exceptions; differentiate programme gross flow from platform revenue. Fee model (subscription, programme setup, usage, referral/success fee) undergoes regulatory/activity classification before charging.

## 5. Failure cases and alternatives
Rejected: one global funder_active flag; funder eligibility inferred from logo, all-business market flag, cross-subsidiary credit facilities without assignment, concurrent programme offers overwriting each other. Cases: supplier mandate revoked, anchor insolvency, provider API retired, programme rate change, fee rule disputed.

## 6. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-PGM-01 | Programme terms, participant model and effective-dated scopes |
| SCF-PGM-02 | Contract/mandate, limit, KYB and revocation tests |
| SCF-PGM-03 | Two-funder mock with confidential rates and independent tenants |
| SCF-PGM-04 | Signed external sponsor/partner and legal fit evidence for pilot |
| SCF-PGM-05 | Partner sandbox and Control Plane binding only after code/proven support |

## 7. Review triggers
New anchor, jurisdiction, currency, multi-funder commitment or actual risk participation prompts explicit revision.

## 8. Platform-wide implementation constraints

All case actions require IAM identity plus redeemed CP caller-bound tenant/legal entity and scope; no bare tenant/provider header authorises access. Keep separate immutable occurred/observed/recorded timestamps, source digests, cross-engine pinned references, audit and idempotent outbox/inbox when implemented. UNKNOWN external side effects must be reconciled, not retried blindly. Preserve maker/checker and funder independence where legally or contractually required. Data disclosure is purpose-minimised; provider callback identity must be verified, not trusted based on URL.

**Review/revision protocol:** on changed external law, market, source contract or product economics, record a dated amendment with evidence URL/version, effective period, changed assumptions, impact on accepted prior cases, Shared contracts and migration plan. Each implementation gate needs exact code/test references, negative tests, operational telemetry and relevant independent sign-offs. Never claim Shared finance namespace, a new SCF capability, or CP provider certification simply by merging a local ADR.
