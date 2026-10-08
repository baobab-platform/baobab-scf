# ADR-SCF-0010 — Provider-Neutral Financing Adapters, Offer Discovery and Partner Routing

**Status:** Proposed — architecture review, not acceptance or implementation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Premise:** Partner-led, headless, self-hosted; existing ADR-SCF-0001 remains Proposed and controls the charter  
**Authority:** External qualified financiers own credit/contract decisions; ERP owns financial balances; Payments owns permitted payment execution; Shared owns canonical event/capability vocabulary, CP tenant/provider, IAM authentication  

> No regulator/licence, funder contract, provider certification, bank transfer or production capability is created by this ADR.


## 1. Decision
Use versioned provider-neutral anti-corruption ports for eligibility checks, applications, status retrieval, offer read, acceptance/cancellation and **source-reported funding observations**. External lender decisions remain their own. The adapter's operation matrix is explicit by finance product, bank/factor, market, supported commands, contract, version, permission and evidence.

~~~mermaid
flowchart TD
 A["SCF FinancingCase + approved disclosure"] --> R["Product/market/contract routing"]
 R --> P["FunderAdapterPort"]
 P --> B["Bank adapter"]
 P --> F["Factor adapter"]
 P --> D["Development finance-supported partner"]
 B --> O["Signed source decision/offer"]
 F --> O
 D --> O
 O --> S["SCF attributed projection"]
~~~

## 2. Provider port taxonomy
| Port/operation | Request/proof | Critical distinction |
|---|---|---|
| check_programme_eligibility | tenant/party/programme/obligation snapshot | not underwriting |
| submit_application | pinned manifest digest, purpose, provider request_id | ack not approval |
| retrieve_application_status | source native reference, token and freshness | unknown is not decline |
| retrieve_offer | exact version, issuer, amount/currency/fees/recourse | quote not cash |
| confirm_acceptance | verified actor/mandate and precise signed offer reference | click not legal acceptance |
| cancel_application | provider-supported cancellation and confirmation | cancellation attempt not success |
| observe_disbursement | funder/payment ID, source amount/time, status | not ERP reconciliation |
| observe_financing_position | source outstanding, repayment, maturity | not SCF ledger |

## 3. Selection and failure rules
Route only among contracted, qualified, compatible and consented providers. **No automatic failover for side-effecting submissions when the result is unknown**. Confirm prior provider outcome before reissue or second-funder submission. Allow offer comparisons only when each funder has a scoped disclosure grant; no broadcast of confidential invoices by default. Retry bounded technical failures with same idempotency key and exact digest; conflicting provider response with same ID is quarantined.

## 4. Identity, callback, credential and security
External application, offer and loan IDs are ExternalReferences, never domain IDs. Credentials scoped by tenant legal entity, partner, operation, environment and term; stored in approved secret store. Validate callback signature, sender, audience, nonce, message ID and occurred time. Preserve native response hash, mapping version and receipt/recorded times; reject duplicate conflicting assertion and stale provider credentials.

## 5. Alternatives rejected
Single universal lender API, direct partner DB integration, automatic rerouting on timeout, provider-driven new canonical fields, multi-funder disclosure without legal permission and presenting mock partner availability as live.

## 6. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-ADP-01 | Typed adapters, provider support table, error taxonomy and version pins |
| SCF-ADP-02 | Two synthetic partners with denial, timeout, rate limit, stale offer and changed callback |
| SCF-ADP-03 | Secret rotation, forged callback, cross-tenant and improper sharing tests |
| SCF-ADP-04 | Unknown outcome replay and manual reconciliation without duplicate submission |
| SCF-ADP-05 | Real contracted partner sandbox and reviewed market/activity authority |

## 7. Revision triggers
Adapt on partner version change or product contract amendment, never imply new country coverage based solely on the existence of an HTTP API.

## 8. Platform-wide implementation constraints

All case actions require IAM identity plus redeemed CP caller-bound tenant/legal entity and scope; no bare tenant/provider header authorises access. Keep separate immutable occurred/observed/recorded timestamps, source digests, cross-engine pinned references, audit and idempotent outbox/inbox when implemented. UNKNOWN external side effects must be reconciled, not retried blindly. Preserve maker/checker and funder independence where legally or contractually required. Data disclosure is purpose-minimised; provider callback identity must be verified, not trusted based on URL.

**Review/revision protocol:** on changed external law, market, source contract or product economics, record a dated amendment with evidence URL/version, effective period, changed assumptions, impact on accepted prior cases, Shared contracts and migration plan. Each implementation gate needs exact code/test references, negative tests, operational telemetry and relevant independent sign-offs. Never claim Shared finance namespace, a new SCF capability, or CP provider certification simply by merging a local ADR.
