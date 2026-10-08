# Baobab Supply Chain Finance (SCF)

> **Status:** Foundation-0 scaffold + proposed architecture; no application runtime or production-ready capability.

Baobab SCF is the proposed **headless, self-hosted, provider-neutral supply-chain-financing orchestration and evidence engine** for independently entitled legal entities, including ZuriBeans and Thamani.

See [ADR-SCF-0001](docs/adr/ADR-SCF-0001%20%E2%80%94%20Supply%20Chain%20Finance%20Mission%20Authority%20and%20Partner-Led%20Financing.md) (**Proposed**, not activated) for the domain and full implementation gates.

## What SCF is expected to own

- FinancingCase identity, case progress, controlled workflow and audit.
- References to authorised, pinned trade, financial, logistics and documentary evidence.
- Funder programme and partner submission orchestration, offer response provenance and consent/acceptance coordination.
- Conflicts, duplicate-prevention within Baobab's observable scope, reconcilable funding observations and exception handling.

## What SCF does not own

- Orders and commercial truth (Trade), invoices/GL/receivables (ERP), authoritative documents (Trade Docs), transport facts (TMS), or legal policy decisions (Regulations).
- Authentication and trusted tenant/market/legal-entity context (IAM and Control Plane).
- Credit decisions, regulated lending, issuance of legal assignments, actual disbursement, funds custody, payment settlement or general loan ledgers without separately approved authority.

**Default model is partner-led financing:** qualified funders decide offers and authorised financial/payment systems record money movement. A financing case is not a loan or credit approval.

## Contract dependencies

Cross-engine contracts and capability semantics are governed by [baobab-platform/shared](https://github.com/baobab-platform/shared). Current architecture references include:

- `contracts/capability/v1/catalogue.yaml` and `namespace-registry.yaml`;
- `contracts/cross-engine-reference/v1`;
- `contracts/trade-document/v2`;
- `contracts/regulatory-document-exchange/v1`.

No SCF capability is claimed as catalogued, implemented, certified or resolvable. Canonical keys/events require a separate Shared review. A specific pinned Shared release/commit has not yet been selected for SCF application code.

## Implementation direction (not yet selected/deployed)

Reuse Baobab's existing runtime families. Python 3.14 / Django 6.0 / PostgreSQL 17 is a **candidate**, subject to SCF-TECH-01; a Go service is another same-stack option. API-first operation is required. Apache Fineract, a JVM-based financial core, is **not** a baseline dependency. No required SCF frontend.

The DevContainer/environment/repository YAML files remain examples until a real tech choice and Foundation-1 activation. No development or staging integration has been established by an ADR PR.

## Delivery and readiness

Start with the independent gates in ADR-SCF-0001: SCF-FND-00, SCF-TECH-01, SCF-DOM-01, SCF-CON-01, SCF-API-01, SCF-EVD-01, SCF-ADP-01, SCF-RSK-01, SCF-INT-01, SCF-OPS-01 and SCF-REL-01.

First acceptance uses synthetic funds and a simulated funder. Real financing cannot be represented as active without validated legal, partner, contract, capability and operations proof.

Security reports: [SECURITY.md](SECURITY.md). Contribution standards: [CONTRIBUTING.md](CONTRIBUTING.md).

## Complete SCF architecture programme — proposed

The [SCF ADR register](docs/adr/README.md) now lists **35 numbered decisions**: existing Proposed ADR-SCF-0001 and new Proposed ADR-SCF-0002 through ADR-SCF-0035. Every new ADR includes scoped architecture, authority, failure semantics, implementation gates and review/change triggers. ADR-SCF-0030..0035 preserve **future-conditional** opportunities (multi-funder distribution, agricultural/inclusive finance, sustainability, local-currency FX, digital collateral and optional separately licensed lending), not currently authorised product support.

[SCF-TECH-01](docs/architecture/SCF-TECH-01%20%E2%80%94%20Headless%20Self-Hosted%20Financing%20Orchestration%20Runtime%20and%20Build-Adopt%20Strategy.md) is a **separate Proposed** headless/self-hosted technology decision favouring an executable Python 3.14 / Django 6.0 / PostgreSQL 17 spike; Go/PostgreSQL remains a same-stack comparator. No new banking-core JVM stack, finance service, licensed lender, funder contract or real money processing is selected or implemented.

**Implementation order:** approve legal/partner roles → finalise canonical case/evidence/claim architecture → choose/prove minimal runtime → Shared capability census and contracts → simulated buyer-approved payable or factoring case → qualified partner sandbox → independent market, security, business and EA-09/CP admission. Keep provider, product, market and legal-entity support individually certified.
