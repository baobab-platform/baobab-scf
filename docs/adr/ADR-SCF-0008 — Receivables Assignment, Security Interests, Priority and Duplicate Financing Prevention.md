# ADR-SCF-0008 — Receivables Assignment, Security Interests, Priority and Duplicate Financing Prevention

**Status:** Proposed — architecture review, not acceptance or implementation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Premise:** Partner-led, headless, self-hosted; existing ADR-SCF-0001 remains Proposed and controls the charter  
**Authority:** External qualified financiers own credit/contract decisions; ERP owns financial balances; Payments owns permitted payment execution; Shared owns canonical event/capability vocabulary, CP tenant/provider, IAM authentication  

> No regulator/licence, funder contract, provider certification, bank transfer or production capability is created by this ADR.


## 1. Decision and legal distinction
SCF tracks **claims and evidence** concerning an obligation's financing and encumbrance. It is not a receivables registry, court, collateral agent or final legal title judge. A local uniqueness constraint can prevent repeated Baobab submissions but **cannot prove that another factor, bank or debtor has not financed the same receivable**. Perfection, debtor notices, priority and assignments depend on domestic law, instrument terms, obligor and security registries.

## 2. Domain contract
| Record | Identity / provenance | Cannot establish |
|---|---|---|
| FinanceableClaimReference | ERP obligation ID, creditor, debtor, currency, amount, source version | freely assignable right |
| LocalClaimReservation | allocated amount portion, case, tenant, funder, expiry, optimistic revision | external legal priority |
| AssignmentIntent | proposed parties, contract, effective time, notices and scope | transfer occurred |
| AssignmentEvidence | signed exact agreement DocumentVersion, external verification | universal enforceability |
| PerfectionObservation | registry/funder/holder assertion, jurisdiction, filing identifier, timestamps | no unregistered competing interests |
| CompetingInterestObservation | source-confirmed or alleged interest, confidence and discrepancy | conclusive fraud |
| ReleaseEvidence | creditor/holder notice, amount, scope and date | discharge of unrelated claims |
| ConflictCase | conflicting candidates, linked case references, reviewer, freeze reason | right to disclose competitor data |

## 3. Constraint and concurrency rules
Support partial tranches and currencies; source eligible balance, reservations and externally verified encumbrances must be separately recorded and timestamped. The same obligation may have competing offers but acceptance and actual assignment must not create overlapping allocations within the system. Different tenants may have legitimate claims to the same third-party invoice only under explicitly modelled permitted relationships; cross-tenant collision checks must never reveal private competitor financing details.

~~~mermaid
flowchart TD
 A["ERP receivable and Trade source"] --> B["Local amount reservation and duplicate check"]
 C["Funder/registry/debtor priority evidence"] --> D["Claim/assignment observations"]
 B --> E{"Eligibility and priority known?"}
 D --> E
 E -->|Yes within authorised scope| F["Qualified partner application"]
 E -->|Unknown or contested| G["Hold and legal/partner review"]
 F --> H["External finance/assignment confirmation"]
 H --> I["Reconcile source obligation and local reservation"]
~~~

## 4. Adverse cases and re-evaluation
Paid invoice after reservation; confidential factoring; partial credit note; debtor receives contradictory notices; assignment of proceeds rather than underlying receivable; lapsed registry entry; joint security interest; bank claims superior priority; cross-border conflicts of law. Status UNKNOWN must not be silently converted to UNENCUMBERED. Reconcile by append-only observation, and require authorised operator/counsel determination on high-risk ambiguity.

## 5. Rejected alternatives
Reject universal is_assigned boolean, invoice digest as global financing uniqueness proof, blockchain perfection without governing law, local database as collateral registry, and automatic finding of legal title from uploaded PDFs.

## 6. Implementation gates
| Gate | Proof |
|---|---|
| SCF-LIEN-01 | Legal assignment/notice/perfection/priority authority matrix by instrument and market |
| SCF-LIEN-02 | Partial receivable allocation, money precision and concurrent case race tests |
| SCF-LIEN-03 | Registry/assignee evidence port with UNKNOWN and disputed responses |
| SCF-LIEN-04 | Credit-note, double submission, contract cancellation and source mutation fixtures |
| SCF-LIEN-05 | External counsel/funder confirmation before any partner-facing priority assurance |

## 7. Revision triggers and sources
Reopen on new registry, collateral law, factoring agreement or market. [UNCITRAL secured-transactions materials](https://uncitral.un.org/en/texts/securityinterests) are a design reference, not automatically domestic law.

## 8. Platform-wide implementation constraints

All case actions require IAM identity plus redeemed CP caller-bound tenant/legal entity and scope; no bare tenant/provider header authorises access. Keep separate immutable occurred/observed/recorded timestamps, source digests, cross-engine pinned references, audit and idempotent outbox/inbox when implemented. UNKNOWN external side effects must be reconciled, not retried blindly. Preserve maker/checker and funder independence where legally or contractually required. Data disclosure is purpose-minimised; provider callback identity must be verified, not trusted based on URL.

**Review/revision protocol:** on changed external law, market, source contract or product economics, record a dated amendment with evidence URL/version, effective period, changed assumptions, impact on accepted prior cases, Shared contracts and migration plan. Each implementation gate needs exact code/test references, negative tests, operational telemetry and relevant independent sign-offs. Never claim Shared finance namespace, a new SCF capability, or CP provider certification simply by merging a local ADR.
