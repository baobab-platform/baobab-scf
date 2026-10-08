# ADR-SCF-0012 — Mandates, Consent, Contract Execution and Financing Acceptance

**Status:** Proposed — architecture review, not acceptance or implementation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Premise:** Partner-led, headless, self-hosted; existing ADR-SCF-0001 remains Proposed and controls the charter  
**Authority:** External qualified financiers own credit/contract decisions; ERP owns financial balances; Payments owns permitted payment execution; Shared owns canonical event/capability vocabulary, CP tenant/provider, IAM authentication  

> No regulator/licence, funder contract, provider certification, bank transfer or production capability is created by this ADR.


## 1. Decision
Separate a purpose-bound permission to process/share evidence, a corporate representative mandate, a user choice of offer, a **legally effective external financing agreement** and any assignment/control over receivables. An authenticated click does not prove a contract, nor does a signed PDF prove signatory power.

## 2. Distinct records
| Entity | Facts and legal role |
|---|---|
| ProcessingConsent | actor, purpose, data categories, lawful basis, time, withdrawal limits |
| DisclosureMandate | exact funder/recipient, manifest version, expiry, permitted data |
| ProgrammeMandate | corporate legal entity, product/market, source agreement, authorised representatives |
| RepresentativeAuthority | IAM principal, CP org, signatory role, delegation chain, effective validity |
| OfferSelectionIntent | selected offer revision and disclosed fee/recourse copy, time |
| AcceptanceEvidence | contract DocumentVersion, signer, funder receipt/ack, external legal determination |
| WithdrawalObservation | requested vs confirmed withdrawal and effect on existing obligations |

## 3. Acceptance workflow
~~~mermaid
flowchart TD
 A["IAM actor + CP legal entity"] --> B["Mandate and programme eligibility"]
 B --> C["Exact funder offer revision"]
 C --> D["Recipient cost/recourse/risks displayed"]
 D --> E["Actor selection and strong approval"]
 E --> F["External funder contract / qualified signer"]
 F --> G["Confirmed acceptance and legal evidence"]
 G --> H["SCF source-attributed status"]
~~~

Maker/checker independence when required by programme policy; cannot satisfy with two accounts controlled by same person. Check expiry and versions at submission, signature and external provider acknowledgement. Idempotent accept attempts use same digest; a changed offer after signature cannot rewrite contract history. Any legal assignment notice or security perfection is separately verified under ADR-0008.

## 4. Rights and privacy
Revocation of consent for future data processing is not equivalent to withdrawing an already binding financing agreement. Record disclosure recipient and artefacts already transmitted, lawful retention and continuing obligations without false claims of remote erasure. Cross-border sharing needs approved recipient, data purpose and appropriate agreements.

## 5. Adverse cases
Unsigned offer, forged signatory, expired proxy, consent for another partner, late funder callback, same-actor approval, materially changed terms and independent supplier/parent legal entity mismatch lead to DENY/UNKNOWN_REVIEW.

## 6. Alternatives rejected
Global consent=true, clickwrap legally valid everywhere, provider email regarded as bank signature, automatic legal assignment upon acceptance and parent-company signature rights over subsidiary.

## 7. Implementation gates
| Gate | Proof |
|---|---|
| SCF-CNS-01 | Separate consent, mandate, selection and acceptance domain snapshots |
| SCF-CNS-02 | CP/IAM representative, expiry, revocation and maker/checker negative tests |
| SCF-CNS-03 | Expired/changed offer, exact signed digest and foreign funder consent validation |
| SCF-CNS-04 | Trade Docs pinned agreement and provider acknowledgement conformance |
| SCF-CNS-05 | Qualified market counsel/partner assessment of actual acceptance procedure |

## 8. Revision trigger
New signatory regime, financing contracts, future electronic transferable instrument or digital signature standard.

## 8. Platform-wide implementation constraints

All case actions require IAM identity plus redeemed CP caller-bound tenant/legal entity and scope; no bare tenant/provider header authorises access. Keep separate immutable occurred/observed/recorded timestamps, source digests, cross-engine pinned references, audit and idempotent outbox/inbox when implemented. UNKNOWN external side effects must be reconciled, not retried blindly. Preserve maker/checker and funder independence where legally or contractually required. Data disclosure is purpose-minimised; provider callback identity must be verified, not trusted based on URL.

**Review/revision protocol:** on changed external law, market, source contract or product economics, record a dated amendment with evidence URL/version, effective period, changed assumptions, impact on accepted prior cases, Shared contracts and migration plan. Each implementation gate needs exact code/test references, negative tests, operational telemetry and relevant independent sign-offs. Never claim Shared finance namespace, a new SCF capability, or CP provider certification simply by merging a local ADR.
