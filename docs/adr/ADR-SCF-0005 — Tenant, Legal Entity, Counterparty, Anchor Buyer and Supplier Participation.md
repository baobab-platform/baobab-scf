# ADR-SCF-0005 — Tenant, Legal Entity, Counterparty, Anchor Buyer and Supplier Participation

**Status:** Proposed — not automatically Accepted by PR merge  
**Date:** 2026-10-08  
**Repository:** `baobab-platform/baobab-scf`  
**Programme:** Foundation and product architecture; ADR-SCF-0001 remains the controlling **Proposed** foundational charter  
**Operating premise:** Partner-led, headless, self-hosted and provider-neutral; no lending, underwriting, funds custody, payment execution or financial ledger authority for SCF  
**Shared authority:** `baobab-platform/shared` controls wire contracts, canonical capability/event semantics; CP controls tenant/binding/certification; IAM controls identity.  

> **Decision status vs evidence:** this is an engine-local architecture *proposal*. No source runtime, licensed finance activity, external funding relationship, market permission, capability certification or production-readiness claim follows from this ADR.


## 1. Decision
Each SCF case belongs to an **independently entitled tenant and contracting legal entity**, with explicit participant roles and market/contract scope. CP creates tenant/legal-entity and canonical organisation references; IAM authenticates human/workload principals. SCF owns **programme participation and financing-role assignments**, not the central legal-entity master or platform-wide supplier qualification. Shared parentage never gives Nabhold Group inherited financing access to ZuriBeans or Thamani cases.

## 2. Participant model
| Role | Business capacity | Distinct permission |
|---|---|---|
| Supplier/Applicant | Receivable seller or business seeking early payment | Consents to evidence-sharing and offers |
| AnchorBuyer/Debtor | Approved payable party, optionally programme sponsor | Confirms obligation; cannot silently approve credit for funder |
| QualifiedFunder | Bank/factor/authorised financier | Returns binding offers/decisions subject to own authority |
| Servicer/CollectionAgent | Contracted post-funding administrator | Limited source read/write under mandate |
| ProgrammeSponsor | Commercial organisation establishing a scheme | Configuration approval, not automatic lending authority |
| Guarantor/Insurer | Credit/risk enhancement participant | Evidence and external coverage; not funder automatically |
| ReferringDigitalEstate | ZuriBeans/Thamani user experience | No direct core financing write without IAM/CP context |
| Borrower/Obligor | Liable party under exact legal agreement | Cannot infer from invoiced recipient or creator role |

## 3. Relationship and party identity
Introduce \`ProgrammeParticipation\` with scoped canonical organisation, role, business relationship, product, market/corridor, currency, limits, valid_from/to, consent/contract references, risk/qualification status and revocation history. Legal relationships can differ: invoice debtor vs purchaser vs parent; supplier vs assignee vs borrower. Entity may play multiple roles but the relationship must be explicit per case/contract.

External funder/bank native account numbers are ExternalReferences and sensitive payment destination references stay under Payments provider. No duplicate CP issuer master. Programme enrollment must not change CP global organisation status or take ownership of Trade's buyer/supplier master.

## 4. Trust and segregation
CP context is bound to IAM verified actor/audience and operation; an arbitrary \`tenant_id\` query parameter or website domain is not proof. Carrier, buyer, supplier and funder get **different** case projections. Funder access to one obligation does not reveal unrelated suppliers, other bids, platform fee structure or competing funder terms. The sponsor may see approved programme metrics without invoice PDFs or other legal entities' receivable details. A parent may receive approved consolidated reports only with documented legal grounds and data-minimising aggregation.

## 5. Onboarding journey
~~~mermaid
flowchart LR
 A["CP organisation/legal entity identity"] --> B["Programmatic role/market eligibility"]
 B --> C["Partner contract and mandates"]
 C --> D["KYC/KYB/screening result references"]
 D --> E["Independent participation approval"]
 E --> F["Case-specific identity, consent and grants"]
 F --> G["Audit and revocation"]
~~~

If a party exits a programme, stop future applications without deleting historic accepted financing agreements or legal evidence. Counterparty qualification must be time-scoped; a former eligible supplier is not automatically permitted tomorrow.

## 6. Implementation gates
| Gate | Proof |
|---|---|
| SCF-PART-01 | Party-role matrix and CP canonical org/relationship external-reference mapping |
| SCF-PART-02 | Buyer/supplier/funder/guarantor multi-role combinations and tenancy constraints |
| SCF-PART-03 | Parent-subsidiary access denial, competing funder confidentiality and customer projections |
| SCF-PART-04 | Market/contract/mandate expiry/revocation and stale context tests |
| SCF-PART-05 | UG/ZA independent onboarding simulation plus external contracted partner review |

## 7. Rejected alternatives and revision triggers
Reject one "customer_id" role for all parties, implicit parent permission, global finance provider approval, cross-tenant attachment access and treating a role displayed in a portal as an authorised corporate signatory. Revisit if CP organisation/counterparty semantics or externally governed cross-entity relationships change.

## 8. Non-negotiable cross-engine controls and acceptance

SCF must authenticate caller via IAM and redeem caller-bound CP context, authorise each source object/counterparty and store tenant/legal-entity scope in durable state and workers. Exact external references remain source-owned; no direct writes to ERP, Trade, Payments, Trade Docs, TMS, Regulations or partner databases. Sensitive evidence is purpose-minimised, encrypted, auditable and non-replicated across sibling tenants. Provider callbacks are independently authenticated, replay-safe and provenance-bearing; an HTTP acknowledgement is not funding. Canonical `financing.*` or `scf.*` keys and events are **illustrative until Shared-approved** and must never be declared as implemented or active by merging this ADR.

**Review protocol:** when evidence, market rules or provider terms change, add an explicit dated amendment/superseding ADR recording the triggering fact, impacted rules, consumer migration and backward-compatibility plan. Keep the original accepted source facts and issued financial documents immutable. Implementation gates require exact code, fixtures, failure tests, known deferrals and review sign-off, followed by independent EA-09/Control Plane activation only for proven product/market/provider operations.
