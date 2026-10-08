# ADR-SCF-0001 — Baobab Supply Chain Finance Mission, Authority, Financing Cases and Partner-Led Execution

**Status:** Proposed — foundational engine charter for review; **NOT** an approval to lend, fund, originate, settle or operate in production  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Engine:** Baobab Supply Chain Finance (SCF)  
**Decision scope:** engine mission, bounded context, operational and legal authority, case/evidence model, integrations, provider-neutral financing orchestration, financing models and distinct implementation gates  
**Initial consumers:** ZuriBeans and Thamani, then other independently entitled Baobab tenants  
**Current implementation evidence:** template scaffold only; no executable finance engine, authoritative financial core, provider declaration, registered financing capability, certified lending/servicing integration or authorised funder contract  
**Platform contract authority:** baobab-platform/shared  
**Capability resolution / tenant authority:** baobab-platform/baobab-cp  
**Identity authority:** baobab-platform/baobab-iam  
**Trade authority:** baobab-platform/baobab-trade  
**Financial accounting authority:** baobab-platform/baobab-erp (iDempiere)  
**Payment execution authority:** baobab-platform/baobab-payments, within its actual granted capability scope  
**Document/evidence authority:** baobab-platform/baobab-trade-docs  
**Transport execution authority:** baobab-platform/baobab-tms  
**Regulatory-policy authority:** baobab-platform/baobab-regulations within accepted contract scope  
**Funder/credit/legal authority:** applicable separately authorised finance providers, payment operators, obligated parties and competent legal/regulatory authorities

## 1. Executive decision proposed

Baobab SCF SHALL be a **reusable, headless, self-hosted, provider-neutral supply-chain-financing orchestration and evidence engine**. It coordinates **requests for financing**, transaction-evidence assembly, controlled submission to eligible funding providers, offer receipt, customer/funder consent and acceptance references, financing-position observations, exception handling, audit and reconciliation with authoritative facts in other engines.

**Default operating model: partner-led financing.** Baobab SCF is **NOT by default a lender, bank, deposit-taker, custodian, credit bureau, settlement system, payment processor or accounting ledger**. It must not approve credit, dictate regulated financing terms, advance/disburse money, assume legal ownership of receivables or advertise guaranteed funding solely by running application code.

A future internal or licensed financing/servicing model requires **a separately accepted legal/commercial authority decision, market-by-market compliance review, technology decision, credentials and deployment acceptance**. Such authority is not created by this ADR or by use of an OSS lending package.

## 2. Why this engine exists

ZuriBeans and Thamani can originate commercially valid opportunities needing financing across Uganda, South Africa and future markets without becoming finance companies:

- ZuriBeans: verified receivable financing, approved-payables/anchor buyer funding, pre-shipment supplier or purchase-order financing **where an authorised funder supports it**.
- Thamani: transport/carrier invoice advances, buyer-approved payable finance, logistics service payment acceleration **where a qualified external finance provider supports it**.
- Other tenants: eligible trade and supply-chain programmes independently provisioned by legal entity, market and entitlement.

Common value is orchestration of trade, financial, logistics and documentary evidence, including lifecycle observability, not speculative platform credit scoring.

**Autonomy:** Nabhold parent membership MUST NOT automatically share subsidiary financing authority, funder contracts, case records, liability, debt, guarantees, debtor credit limits or market entry. Each subsidiary/legal entity has distinct Control Plane identity, permissions, applicable contractual capacity and market participation. No `if tenant == ZURIBEANS/THAMANI` inside canonical domain.

## 3. Authoritative bounded context

| Concept / question | Authority | SCF permitted role |
|---|---|---|
| Is there a valid order, commercial agreement, RFQ or accepted trade transaction? | Trade / other commercial source | Pinned source reference and authorised factual projection |
| Does a posted invoice, payable or receivable exist; what is its amount, currency, due date or accounting status? | ERP | Pinned/observed obligation projection; no mutation of ledger |
| What are the documents, exact versions, issuer assertions, provenance, verification facts and content references? | Trade Docs | Evidence references or authorised fact bundles |
| Was cargo dispatched, delivered, accepted or held, and when? | TMS/Trade/recipient as appropriate | Historical operational reference; no replacement delivery authority |
| What regulatory requirement, permit or legal-policy assessment applies? | Regulations / competent authority, limited to contract coverage | Cited decision; do not treat absent coverage as approval |
| Who may submit a request and act for a legal entity? | IAM + CP entitlements, owner-specific business policy | Authorise in context; store actor authority evidence |
| Who offers the money, determines creditworthiness and financing terms? | Authorised funder | Preserve funder decision/offer/reference and authenticity; cannot self-approve |
| Is a receivable legally transferred, assigned, pledged or discharged? | Parties, enforceable legal instruments and competent jurisdictional authority | Record evidence of claimed/verified legal effect, not create it |
| Did money settle / repay? | Payments, payment operator, funder and ERP for their respective facts | Reconcile verified observations; no unilateral funded status |
| How does an SCF request progress? | **SCF** | Own financing case, offer lifecycle coordination, evidence snapshot, attempts, exceptions and audit |
| What is the exposure/credit facility ledger? | Authorised servicing/lending provider, or future separately governed core | Reference a position, never pretend to be a double-entry lending ledger |

**SCF owns FinancingCase workflow and evidence orchestration**, not the underlying receivable, credit legal instrument, sovereign decision, commercial transaction or actual funds.

## 4. Core terminology and invariants

| SCF term | Meaning and identity | Invariant |
|---|---|---|
| `FinancingCase` | Stable SCF-owned financing request/case scoped to one tenant/legal entity and programme | A case is **not** a loan or evidence of disbursement |
| `FinancingProgramme` | Funder-supported product/programme participation, tenant+market-specific eligibility rules and external agreement references | A programme is not an internal credit authorisation |
| `UnderlyingObligationReference` | Exact ERP or other authorised debtor/creditor obligation pointer, amount/version snapshot where supported | No new ERP invoice or receivable ID minted |
| `EvidenceManifest` | Pinned cross-engine versions, hashes, provenance, observation times and reason for inclusion | References, not copies or assertions of legal sufficiency |
| `FinancingSubmission` | Idempotent external funder request attempt with payload digest and correlation | "Sent" != "received" != "approved" |
| `FunderDecision` | Authenticated external decline/approve/needs-information observation and decision reference | Only source funder is credit decision authority |
| `FinancingOffer` | Verbatim/normalized external offer with terms provenance, rate disclosure, expiry, currency, acceptance requirements | Never locally fabricate lending terms |
| `AcceptanceRecord` | Actor(s), legal capacity, offer version, signature/consent evidence and related agreement reference | An in-app click is not automatically enforceable contract |
| `SecurityInterestOrAssignmentReference` | Claimed, pending or externally verified encumbrance/assignment evidence | Do not equate a record with legally effective priority or title |
| `FundingObservation` | Externally sourced payout, receipt, value date and currency, linked to payment/ERP truth | A provider promise is not settled funds |
| `FinancingPositionProjection` | Read-only view combining case, funder and reconciliation observations | Not a general ledger or loan-servicing book |
| `FinancingException` | Audit-preserved conflict, stale evidence, fraud indicator, disputed obligation, failed callback or irreconcilable state | No silent auto-close or approval |

SCF IDs are owner-engine domain identifiers. Use Shared `contracts/cross-engine-reference/v1` for portable owner-object references; distinguish external funder native identifiers from Baobab IDs using ExternalReference where governed. Do not collapse `CanonicalEntity`, `CrossEngineObjectReference`, `ExternalReference` and `engine_instance_id`.

## 5. Financing instruments and activation matrix

The model MAY eventually support the following, each requiring its own market+provider+product decision. None is active now.

| Candidate financing case | Expected source proof | Authoritative credit/settlement actors | Initial readiness |
|---|---|---|---|
| Receivables / invoice financing | ERP receivable and debtor acknowledgement/dispute/assignment checks | Funding institution, debtor as applicable, payments/ERP | **Candidate; partner-led** |
| Approved-payables / reverse factoring | Buyer-confirmed payable + funder-supported debtor programme | Buyer/obligor, funder, payment operator | **Candidate; partner-led** |
| Purchase-order / pre-shipment finance | Accepted commercial obligation, supplier qualification, delivery/performance and documentary controls | Funder, relevant obligors | **Candidate; higher risk** |
| Freight carrier / logistics invoice advance | Verified service/consignment, invoice, POD/acceptance and transport exceptions | Funder, carrier, obligor | **Candidate for Thamani** |
| Inventory-backed finance | Custody/title and control evidence, verified inventory and collateral rights | Funder + legal collateral/control provider | **Deferred** |
| Warehouse receipt / eBL-based finance | Legally valid transferable instrument and exclusive-control/possession evidence | Recognised scheme provider and funder | **Deferred** |
| Direct origination, deposit-taking, managed fund, internal lending book | Regulatory licence(s), product governance, financial core, control functions | Separately authorised financial entity | **Out of current scope** |

"Eligible for submission" is a procedural/documentary readiness state, NOT a credit score, creditworthiness determination, regulatory permission to lend or final investment recommendation. Pulse analysis may provide advisory insights, NEVER substitute for underwriting or legal authority.

## 6. Canonical workflow and guardrails

~~~text
Trade/ERP/TMS/Trade Docs
     -> exact authorised, tenant-scoped, historically pinned facts
     -> SCF FinancingCase + EvidenceManifest
     -> completeness, source authenticity, conflict/duplicate checks
     -> programme/market/actor entitlement and consent checks
     -> PARTNER SUBMISSION (authenticated, idempotent, logged)
     -> external authorised funder response
          -> reject / request-info / offer
     -> customer/obligor legally appropriate consent/acceptance
     -> authorised payment provider/funder executes disbursement
     -> SCF ingests independent payout and ERP/payment facts
     -> reconciliation, discrepancy handling, closure
~~~

The case state is separate from **external funder decision**, **contract/assignment verification**, **disbursement/settlement observation** and **accounting position**. Do not encode all as one "financed" boolean.

Suggested case transition candidates (to be formalised during implementation):

~~~text
DRAFT -> EVIDENCE_PENDING -> READY_FOR_PARTNER_SUBMISSION
      -> SUBMITTED -> PARTNER_RESPONSE_PENDING
      -> OFFER_RECEIVED -> CONSENT_PENDING -> OFFER_ACCEPTED
      -> FUNDING_VERIFICATION_PENDING -> FUNDING_OBSERVED
      -> RECONCILING -> CLOSED
Parallel outcomes: NEEDS_INFORMATION / REJECTED / EXPIRED / WITHDRAWN /
                  DISPUTED / SUSPENDED / RECONCILIATION_EXCEPTION
~~~

Not all products use all states. Transition guards MUST be product/actor/market aware but based on governed policy rather than hard-coded estate names. Never skip funder acknowledgement or legally required consent merely because application state advanced.

## 7. Anti-double-financing, stale evidence and security

### Exposure/duplicate prevention

- Correlate every FinancingCase to the exact obligation, obligor, beneficiary, currency, financed amount, evidence version and programme/funder.
- Local DB unique constraints plus serializable or protected reservation semantics must prevent double case submissions for the same conflicting obligation **within the visibility and authority of Baobab**.
- Refinance, part-finance, participations, multiple invoices and permitted tranches require explicit product rules, allocation and remaining-eligible-amount calculations.
- Check assignment/pledge indicators, funder registry claims, debtor confirmation, active SCF cases, cancellations, prior payout and disputes. Record missing information as uncertainty.
- **Do not claim global double-financing prevention** across third-party funders or jurisdictions without access to an authoritative external registry or reciprocal funder verification.

### Verification and revocation

Evidence is pinned at submission. New dispute, altered invoice, expired evidence, document revocation, shipment exceptions, unconfirmed delivery or changed policy flags a new evidence/reassessment action. No historical evidence rewriting. Withdrawal/cancellation before funding may be possible; once legal obligations or settlement exist, execute only legally authorised reversal/adjustment flows.

### Controls

- CP-redeemed, caller-bound tenant/legal-entity and market context; IAM actor and workload credentials.
- Maker/checker, limits, segregation of duties, funder eligibility and product-visibility rules.
- Encryption, explicit financial/personal data classifications, least-privilege views, privacy and retention controls; no raw sensitive documents copied when a pinned reference/projection suffices.
- Authenticated webhooks with signature/key rotation, replay protection, external event IDs, rate limits, idempotent inbox, outbox, audit/observability and manual dispute escalation.
- Accurate time-zone/currency/decimal-safe arithmetic, FX observation source and timestamp; SCF MUST NOT silently change contract amount or ERP books.
- Risk/fraud flags cannot be converted into unexplained automated rejection without appropriate product/legal/policy and human-oversight review.
- No self-funding, self-hosted credit scoring model or legal opinion automatically arising from code execution.

## 8. Financial/legal market constraints

Uganda and South Africa are initial possible markets, not blanket financial-services permissions. A **market-specific legal and compliance assessment** must determine whether the activities of each participant require licensing/registration, credit/financial-services disclosures, lending/servicing authorisation, assignment notice or consent, anti-money-laundering/KYC controls, privacy/data residency, collections rules, foreign-exchange approvals, tax and record retention.

Different lending and intermediary models may trigger different obligations. This ADR makes **no conclusion that Baobab, Nabhold, Thamani or ZuriBeans may legally lend or intermediate financing** in either jurisdiction. Use only qualified contractual partners until separately documented approvals and exact product scope exist.

Do not expose a customer-facing "instant credit approved" UX merely because an SCF internal case becomes technically complete.

## 9. Platform and software integration boundaries

| Edge | Direction | Integration rule |
|---|---|---|
| Trade -> SCF | Trade facts/refs to financing request | Trade remains commercial authority; no direct trade DB writes |
| ERP -> SCF | obligation/invoice/payment-accounting facts | One authoritative ERP accounting model; no duplicate GL |
| Trade Docs -> SCF | pinned DocumentVersion and evidence facts | Verification is documentary only; references/consent before artifact access |
| TMS -> SCF | shipment/milestone/POD facts | Delivery vs acceptance/receivable remains distinct |
| Regulations -> SCF | accepted policy/regulatory decision where covered | Cannot infer general credit legality from customs policy |
| IAM/CP -> SCF | authenticated identity, trusted context, grants, provider resolution | No cross-tenant portfolio visibility by holding a reference |
| Funder -> SCF | formal application/offer/decision/position callbacks | Strong authenticating adapter with versioned field mappings |
| SCF -> Payments/funder | only authorised instruction/observation handoff | No direct settlement or money custody by default |
| Pulse -> SCF | optional intelligence/risk indicators | Advice/evidence, not funder approval, regulator or lender mandate |

Integration is via Shared contracts, authorised APIs and separately approved events, not multi-repository database joins. Any new event contexts, capability keys or external-provider identities require Shared/CP governance. Existing `finance`, `payment` and `settlement` namespaces have other stewards; **do not reserve `scf.*` or register new keys in this engine ADR**. Propose semantic keys only via Shared review with precise collision/authority analysis.

## 10. Headless and self-hosted implementation direction

Initial reference implementation **MAY propose Python 3.14 + Django 6.0 + PostgreSQL 17** (all already represented in Baobab's language/database family) with a versioned, API-only engine and PostgreSQL-backed durable job/inbox/outbox. A Go implementation within existing platform conventions is also acceptable if an explicit SCF technical review chooses it. **No framework is made normative by this domain ADR.**

- Self-hosted SCF case orchestration is distinct from the authorised funder's external infrastructure. Partner endpoints necessarily remain external, governed integrations.
- Do **not** introduce Apache Fineract / JVM as a mandatory baseline. Apache Fineract is an optional future **provider/servicing adapter** only after an actual financial-core requirement, licence/dependency review, programme funding model and legal acceptance.
- There is no required SCF frontend. Thamani and ZuriBeans own differentiated, entitled user journeys, not duplicate financing case authorities.
- Use engine-owned PostgreSQL database, authenticated APIs, idempotency tokens, durable transaction outbox/inbox, audit and observability. Apply existing Baobab DevContainer, CI, image, secrets and staging conventions, but do not claim those are currently configured for SCF.
- Keep pricing calculation, interest schedules, loan servicing, collections, tax treatment and postings in the authorised financial product/ERP service rather than duplicating them without a formal boundary decision.

## 11. Canonical capability and event publication policy

No supply-chain-financing capability is currently claimed as registered, implemented or resolvable through this ADR. A future Shared proposal might describe semantics equivalent to `financing.request.manage`, `financing.evidence.resolve`, `financing.offer.query` or `financing.position.query`, **but these are illustrations, NOT approved capability keys**; the `financing` top-level namespace is not registered by this document. The accepted Shared namespace list/capability catalogue is authoritative.

Likewise, conceptual financing-case, submission, offer and funding-observation events remain candidate business facts with **no activated event type, claimed producer, deployed publisher or CP registration**. Event contract review must precede runtime publication; events must not masquerade as funder commands or lending decisions.

## 12. Distinct implementation and release gates

| Gate | Deliverables with objective evidence | Exit requirements and prohibited claims |
|---|---|---|
| **SCF-FND-00 — Authority confirmation** | Review this ADR, actor/RACI and legal role matrix per financing product/market, funder contractual model | No lending, brokerage, payment or product activation without legal authority |
| **SCF-TECH-01 — Runtime/OSS selection** | Same-stack technology spike, dependency and licence/SBOM review, headless API proof, DevContainer/security foundation | No Fineract or other new runtime without separate ADR |
| **SCF-DOM-01 — Canonical case/domain** | FinancingCase, manifest, immutable revisions, funder/offer provenance, separation of all status axes; unit/state tests | A case is not a loan or ledger balance |
| **SCF-CON-01 — Shared contracts** | Propose financing capability namespace/key stewardship, contracts, events and reference shapes; collision review with ERP, payment and settlement | **No C3 registration** until Shared accepts contracts; no self-activation |
| **SCF-API-01 — Context-bound execution** | IAM authenticated APIs, CP context and legal-entity/market controls, maker-checker, negative tenant/isolation tests | No broad Nabhold subsidiary privilege propagation |
| **SCF-EVD-01 — Evidence acquisition** | Pinned Trade/ERP/Docs/TMS refs, source/provenance/age verification, revocation and stale-data handling; exact history tests | "Documents present" is not credit eligibility |
| **SCF-ADP-01 — Simulated funder** | Provider-neutral adapter port, idempotent submit/acknowledge/offer/reject workflow, replay/sig-verification tests using synthetic partner | A simulator cannot be declared production permitted |
| **SCF-RSK-01 — Duplicate/encumbrance control** | Concurrency and local duplicate guard, partial-finance allocation, disputes and missing external registry tests | Never claim universal anti-fraud/global lien coverage |
| **SCF-INT-01 — Live partner proof** | Executable authorised pilot against a real qualified partner sandbox, signed callbacks, business/legal contracts, consent and reconciliation, exact product scope | No live disbursement through Baobab without payment/credit/legal approval |
| **SCF-OPS-01 — Durability/security** | Outbox/inbox and DLQ, encryption, privacy, incident drills, backup-restore, measured audit replay and independent security review | CI success is not operational resilience proof |
| **SCF-REL-01 — Conditional production** | Market-specific legal sign-off, licensed/qualified partner evidence, Shared conformance, provider implementation proof, EA-09 and CP certification/bindings/entitlements, staging and deployment acceptance | Production activation ONLY with all independent approvals and audited evidence |

Gate PRs are sequential at hard contract/authority dependencies; independent tests and internal models can proceed without external finance credentials. A new financing product must pass its own regulatory/contract/operational activation review.

## 13. First narrow, safe implementation proof

The first demonstrable scenario SHALL use **synthetic funds and a simulated funder**:

1. A ZuriBeans tenant authorised principal initiates a finance request for a referenced ERP obligation derived from a valid Trade transaction.
2. Trade Docs resolves immutable document/version evidence. TMS delivery facts may be absent but any required evidence is marked missing, not invented.
3. Case records obligation reference, money/currency, consent, market, funder-programme context and pinned evidence manifest.
4. Duplicate/assigned/disputed or stale obligation fails closed or moves to review, never automatically advances to funder approval.
5. Adapter sends an idempotent synthetic funding request, receives signed simulated offer/decline, and preserves external decision authority.
6. Acceptance requires the right actor and exact offer version; simulated payout creates **only a simulation-labelled observation**, never an ERP posting or real Payments transaction.
7. The same engine can serve an independently entitled Thamani financing case without implicit parent-company authority or cross-tenant visibility.
8. Restart, duplicate callback, forged event, changed document, stale decision and concurrent application tests preserve audit and source-of-truth invariants.

## 14. Rejected alternatives and consequences

**Rejected now:** adopting a full banking platform as default SCF domain; incorporating a JVM financial core simply to have an OSS product; SCF-issued "approved loan" without funder; unconditional automatic underwriting; custody or disbursement in SCF; copy of ERP/Trade Docs/TMS authoritative data; shared cross-engine finance DB; early tenant-facing guarantees of funding; unapproved new capability namespace.

**Consequences:** smaller first release, longer integration with qualified financial partners, clear legal accountability, transparent financial provenance and the ability to swap lending/servicing providers without changing canonical SCF case identity. It also avoids underwriting and regulated lending functionality that cannot be justified yet.

## 15. Decision status / non-claims

This is a **proposed architecture**. Neither creation nor merge of the ADR provides:
- SCF source code, tests, deployment or self-hosted financial core;
- a new canonical Shared capability or event;
- a funder partnership, credit authorisation, customer financing offer or enforceable assignment;
- evidence of market licensing or regulatory permission;
- legal proof of transferable document title, lien priority or universal duplicate prevention;
- a production-permitted, certified or active SCF CapabilityProvider.

Every substantive implementation and activation requires subsequent gated PRs and authority decisions.

## 16. References

- [Shared capability catalogue](https://github.com/baobab-platform/shared/blob/main/contracts/capability/v1/catalogue.yaml)
- [Shared capability namespace registry](https://github.com/baobab-platform/shared/blob/main/contracts/capability/v1/namespace-registry.yaml)
- [Shared cross-engine reference v1](https://github.com/baobab-platform/shared/tree/main/contracts/cross-engine-reference/v1)
- [Shared TradeDocument v2](https://github.com/baobab-platform/shared/tree/main/contracts/trade-document/v2)
- [Shared regulatory-document exchange](https://github.com/baobab-platform/shared/tree/main/contracts/regulatory-document-exchange/v1)
- [TMS ADR-TMS-0001/0002](https://github.com/baobab-platform/baobab-tms/tree/main/docs/adr)
- [Trade Docs ADR-TDOC-0001/0002](https://github.com/baobab-platform/baobab-trade-docs/tree/main/docs/adr)
