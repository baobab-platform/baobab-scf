# Baobab SCF — Architecture Decision Register and Review Masterplan

**Reviewed:** 2026-10-08  
**Repository:** `baobab-platform/baobab-scf`  
**Current maturity:** Foundation-0 / architecture scaffold — no runnable SCF financing engine, partner finance integration, registered canonical SCF capabilities, approved lending licence, real payouts or production certification.

## Authority and precedence

1. **ADR-SCF-0001** is the existing **Proposed** foundational charter for headless, self-hosted, provider-neutral and **partner-led** financing orchestration. It remains **Proposed**, not silently accepted by this programme.
2. **ADR-SCF-0002..0035** are new **Proposed** local architecture and product decisions, with market and finance-provider activation gates. **ADR-0030..0035 are expressly future-conditional** and must not be promoted into present-day financial support claims.
3. **[SCF-TECH-01](../architecture/SCF-TECH-01%20%E2%80%94%20Headless%20Self-Hosted%20Financing%20Orchestration%20Runtime%20and%20Build-Adopt%20Strategy.md)** is a separate **Proposed** technology decision, not an additional numbered ADR or production proof. Python 3.14/Django 6.0/PostgreSQL 17 is the preferred spike; existing-stack Go is a comparator.
4. **Shared** governs cross-engine references, event stewardship, canonical capability catalogue and namespace registration. The current `finance` domain in Shared is not a blanket SCF financing namespace. No `scf.*` or `financing.*` capability/event is registered by these local documents.
5. **CP** governs tenant/legal entity, market context, provider certification, deployments, bindings and entitlements; **IAM** owns identity; **Trade** commerce; **ERP/iDempiere** invoices/payables/receivables, financial GL and reporting; **Payments** approved payouts/FX/settlement; **Trade Docs** documentary version/evidence; **TMS** physical transport and delivery; **Regulations** contract-supported legal-policy decisions. Qualified funders, banks, credit/licensing authorities, courts, guarantors and counterparties retain external regulated, contractual and legal authority.
6. Nabhold subsidiaries remain autonomous. The parent company and sibling legal entities have no inherited right to funding facilities, offer information, guarantees, confidential bank data or production permissions.

**Important:** PR merge != ADR acceptance != code implementation != canonical capability registration != EA-09 certification != legal permission != operating production finance.

## ADR register — 35 numbered decisions

| ADR | Status | Full decision (click to review) |
|---|---|---|
| ADR-SCF-0001 | **Proposed — Charter** | [ADR-SCF-0001 — Supply Chain Finance Mission Authority and Partner-Led Financing](./ADR-SCF-0001%20%E2%80%94%20Supply%20Chain%20Finance%20Mission%20Authority%20and%20Partner-Led%20Financing.md) |
| ADR-SCF-0002 | **Proposed** | [ADR-SCF-0002 — SCF Product Taxonomy, Financing Techniques and Operating Models](./ADR-SCF-0002%20%E2%80%94%20SCF%20Product%20Taxonomy%2C%20Financing%20Techniques%20and%20Operating%20Models.md) |
| ADR-SCF-0003 | **Proposed** | [ADR-SCF-0003 — Financial Services Authority, Legal Capacity and Jurisdictional Participation](./ADR-SCF-0003%20%E2%80%94%20Financial%20Services%20Authority%2C%20Legal%20Capacity%20and%20Jurisdictional%20Participation.md) |
| ADR-SCF-0004 | **Proposed** | [ADR-SCF-0004 — Canonical FinancingCase, Programme, Offer and Position Domain Model](./ADR-SCF-0004%20%E2%80%94%20Canonical%20FinancingCase%2C%20Programme%2C%20Offer%20and%20Position%20Domain%20Model.md) |
| ADR-SCF-0005 | **Proposed** | [ADR-SCF-0005 — Tenant, Legal Entity, Counterparty, Anchor Buyer and Supplier Participation](./ADR-SCF-0005%20%E2%80%94%20Tenant%2C%20Legal%20Entity%2C%20Counterparty%2C%20Anchor%20Buyer%20and%20Supplier%20Participation.md) |
| ADR-SCF-0006 | **Proposed** | [ADR-SCF-0006 — Underlying Obligation, Buyer Approval and Source Financial Evidence](./ADR-SCF-0006%20%E2%80%94%20Underlying%20Obligation%2C%20Buyer%20Approval%20and%20Source%20Financial%20Evidence.md) |
| ADR-SCF-0007 | **Proposed** | [ADR-SCF-0007 — Documentary Evidence, Provenance, Verification and Financing Eligibility](./ADR-SCF-0007%20%E2%80%94%20Documentary%20Evidence%2C%20Provenance%2C%20Verification%20and%20Financing%20Eligibility.md) |
| ADR-SCF-0008 | **Proposed** | [ADR-SCF-0008 — Receivables Assignment, Security Interests, Priority and Duplicate Financing Prevention](./ADR-SCF-0008%20%E2%80%94%20Receivables%20Assignment%2C%20Security%20Interests%2C%20Priority%20and%20Duplicate%20Financing%20Prevention.md) |
| ADR-SCF-0009 | **Proposed** | [ADR-SCF-0009 — Financing Programmes, Funder Onboarding and Anchor Buyer Participation](./ADR-SCF-0009%20%E2%80%94%20Financing%20Programmes%2C%20Funder%20Onboarding%20and%20Anchor%20Buyer%20Participation.md) |
| ADR-SCF-0010 | **Proposed** | [ADR-SCF-0010 — Provider-Neutral Financing Adapters, Offer Discovery and Partner Routing](./ADR-SCF-0010%20%E2%80%94%20Provider-Neutral%20Financing%20Adapters%2C%20Offer%20Discovery%20and%20Partner%20Routing.md) |
| ADR-SCF-0011 | **Proposed** | [ADR-SCF-0011 — Financing Offers, Discounting, Pricing, Fees and Transparency](./ADR-SCF-0011%20%E2%80%94%20Financing%20Offers%2C%20Discounting%2C%20Pricing%2C%20Fees%20and%20Transparency.md) |
| ADR-SCF-0012 | **Proposed** | [ADR-SCF-0012 — Mandates, Consent, Contract Execution and Financing Acceptance](./ADR-SCF-0012%20%E2%80%94%20Mandates%2C%20Consent%2C%20Contract%20Execution%20and%20Financing%20Acceptance.md) |
| ADR-SCF-0013 | **Proposed** | [ADR-SCF-0013 — Funding, Disbursement, Repayment and Cross-Border Payments Boundary](./ADR-SCF-0013%20%E2%80%94%20Funding%2C%20Disbursement%2C%20Repayment%20and%20Cross-Border%20Payments%20Boundary.md) |
| ADR-SCF-0014 | **Proposed** | [ADR-SCF-0014 — Financing Positions, Exposure, Reconciliation and Financial Reporting](./ADR-SCF-0014%20%E2%80%94%20Financing%20Positions%2C%20Exposure%2C%20Reconciliation%20and%20Financial%20Reporting.md) |
| ADR-SCF-0015 | **Proposed** | [ADR-SCF-0015 — Disputes, Dilution, Recoveries, Collections and Exception Management](./ADR-SCF-0015%20%E2%80%94%20Disputes%2C%20Dilution%2C%20Recoveries%2C%20Collections%20and%20Exception%20Management.md) |
| ADR-SCF-0016 | **Proposed** | [KYC, AML, Sanctions and Trade-Based Fraud Controls](./KYC%2C%20AML%2C%20Sanctions%20and%20Trade-Based%20Fraud%20Controls.md) |
| ADR-SCF-0017 | **Proposed** | [ADR-SCF-0017 — AI-Assisted Financing Risk Intelligence, Model Governance and Human Oversight](./ADR-SCF-0017%20%E2%80%94%20AI-Assisted%20Financing%20Risk%20Intelligence%2C%20Model%20Governance%20and%20Human%20Oversight.md) |
| ADR-SCF-0018 | **Proposed** | [ADR-SCF-0018 — Headless APIs, Canonical Events, Idempotency and Durable Financing Workflows](./ADR-SCF-0018%20%E2%80%94%20Headless%20APIs%2C%20Canonical%20Events%2C%20Idempotency%20and%20Durable%20Financing%20Workflows.md) |
| ADR-SCF-0019 | **Proposed** | [ADR-SCF-0019 — PostgreSQL Persistence, Temporal History, Financing State and Projection Architecture](./ADR-SCF-0019%20%E2%80%94%20PostgreSQL%20Persistence%2C%20Temporal%20History%2C%20Financing%20State%20and%20Projection%20Architecture.md) |
| ADR-SCF-0020 | **Proposed** | [ADR-SCF-0020 — Security, Multi-Tenant Isolation, Consent, Privacy and Data Residency](./ADR-SCF-0020%20%E2%80%94%20Security%2C%20Multi-Tenant%20Isolation%2C%20Consent%2C%20Privacy%20and%20Data%20Residency.md) |
| ADR-SCF-0021 | **Proposed** | [ADR-SCF-0021 — Observability, Reconciliation, Operational Resilience and Disaster Recovery](./ADR-SCF-0021%20%E2%80%94%20Observability%2C%20Reconciliation%2C%20Operational%20Resilience%20and%20Disaster%20Recovery.md) |
| ADR-SCF-0022 | **Proposed** | [ADR-SCF-0022 — Capability Governance, Provider Certification, Migration and Production Acceptance](./ADR-SCF-0022%20%E2%80%94%20Capability%20Governance%2C%20Provider%20Certification%2C%20Migration%20and%20Production%20Acceptance.md) |
| ADR-SCF-0023 | **Proposed** | [ADR-SCF-0023 — Receivables Discounting, Factoring and Invoice Finance](./ADR-SCF-0023%20%E2%80%94%20Receivables%20Discounting%2C%20Factoring%20and%20Invoice%20Finance.md) |
| ADR-SCF-0024 | **Proposed** | [ADR-SCF-0024 — Approved Payables Finance and Buyer-Anchored Programmes](./ADR-SCF-0024%20%E2%80%94%20Approved%20Payables%20Finance%20and%20Buyer-Anchored%20Programmes.md) |
| ADR-SCF-0025 | **Proposed** | [ADR-SCF-0025 — Dynamic Discounting and Buyer-Funded Early Payment](./ADR-SCF-0025%20%E2%80%94%20Dynamic%20Discounting%20and%20Buyer-Funded%20Early%20Payment.md) |
| ADR-SCF-0026 | **Proposed** | [ADR-SCF-0026 — Purchase-Order, Pre-Shipment and Supplier Production Finance](./ADR-SCF-0026%20%E2%80%94%20Purchase-Order%2C%20Pre-Shipment%20and%20Supplier%20Production%20Finance.md) |
| ADR-SCF-0027 | **Proposed** | [ADR-SCF-0027 — Freight, Carrier, Logistics and Delivery-Backed Finance](./ADR-SCF-0027%20%E2%80%94%20Freight%2C%20Carrier%2C%20Logistics%20and%20Delivery-Backed%20Finance.md) |
| ADR-SCF-0028 | **Proposed** | [ADR-SCF-0028 — Inventory, Warehouse Receipt and Commodity-Backed Finance](./ADR-SCF-0028%20%E2%80%94%20Inventory%2C%20Warehouse%20Receipt%20and%20Commodity-Backed%20Finance.md) |
| ADR-SCF-0029 | **Proposed** | [ADR-SCF-0029 — Documentary Trade Finance, Bank Guarantees, Letters of Credit and Forfaiting](./ADR-SCF-0029%20%E2%80%94%20Documentary%20Trade%20Finance%2C%20Bank%20Guarantees%2C%20Letters%20of%20Credit%20and%20Forfaiting.md) |
| ADR-SCF-0030 | **Proposed — Future-conditional** | [ADR-SCF-0030 — Multi-Funder Marketplace, Risk Participation and Portfolio Financing](./ADR-SCF-0030%20%E2%80%94%20Multi-Funder%20Marketplace%2C%20Risk%20Participation%20and%20Portfolio%20Financing.md) |
| ADR-SCF-0031 | **Proposed — Future-conditional** | [ADR-SCF-0031 — Agricultural, Cooperative and Inclusive Supplier Finance](./ADR-SCF-0031%20%E2%80%94%20Agricultural%2C%20Cooperative%20and%20Inclusive%20Supplier%20Finance.md) |
| ADR-SCF-0032 | **Proposed — Future-conditional** | [ADR-SCF-0032 — Sustainability-Linked and Climate-Resilient Supply Chain Finance](./ADR-SCF-0032%20%E2%80%94%20Sustainability-Linked%20and%20Climate-Resilient%20Supply%20Chain%20Finance.md) |
| ADR-SCF-0033 | **Proposed — Future-conditional** | [ADR-SCF-0033 — Pan-African Local-Currency Financing, FX and Payment Interoperability](./ADR-SCF-0033%20%E2%80%94%20Pan-African%20Local-Currency%20Financing%2C%20FX%20and%20Payment%20Interoperability.md) |
| ADR-SCF-0034 | **Proposed — Future-conditional** | [ADR-SCF-0034 — Digital Collateral, Transferable Trade Records and Tokenisation Boundary](./ADR-SCF-0034%20%E2%80%94%20Digital%20Collateral%2C%20Transferable%20Trade%20Records%20and%20Tokenisation%20Boundary.md) |
| ADR-SCF-0035 | **Proposed — Future-conditional** | [ADR-SCF-0035 — Optional Licensed Lending, Loan Servicing and Financial-Core Evolution](./ADR-SCF-0035%20%E2%80%94%20Optional%20Licensed%20Lending%2C%20Loan%20Servicing%20and%20Financial-Core%20Evolution.md) |

## Programme groups and dependency choices

| Group | ADRs | Design and evidence milestone |
|---|---|---|
| A. Financial domain and legal foundations | 0002–0008 | Product/operating taxonomy, market law and acting role, canonical case and evidence, receivable assignment conflicts |
| B. Provider and funding execution | 0009–0015 | Programme/funder onboarding, source offers, signed consents, Payments/ERP observations, exposure and dispute handling |
| C. Platform controls and production gates | 0016–0022 + SCF-TECH-01 | AML/KYB, responsible AI, headless API/outbox, PostgreSQL history, security, DR, Shared/CP/EA-09 admission |
| D. Product-specific finance | 0023–0029 | Receivables factoring, approved payables, dynamic discount, pre-shipment, freight, inventory and bank documentary products |
| E. Future-conditional strategic options | 0030–0035 | Multi-funder distribution, inclusive agriculture, sustainability, local FX, ETR/collateral and separately licensed finance core |

This is an **ADR/research programme**, not an instruction to implement all 34 decisions at once. The early application should prove one small safe slice, and partner-led legal/operational readiness must lead the rollout.

## First narrow executable end-to-end proof

~~~mermaid
flowchart TD
  A["Buyer-approved ERP payable, exact source ref"] --> B["SCF FinancingCase + CP/IAM tenant"]
  B --> C["Trade Docs pinned EvidenceManifest"]
  C --> D["Check programme/consent and duplicate reservation"]
  D --> E["Simulated external qualified funder adapter"]
  E --> F["Versioned sample offer and explicit selection"]
  F --> G["Synthetic, SOURCE-LABELLED payout observation"]
  G --> H["ERP/Payments test reconciliation; no real funds"]
~~~

Start with approved-payables finance **only if a credible anchor and qualified funder agree**; otherwise try post-invoice receivables discounting. Use ZuriBeans as a potential first business consumer; Thamani freight financing is a separately gated later product. Prove wrong-tenant access denial, expired consent, duplicate submitted invoice, altered provider offer, mismatched buyer approval, credit note after submission, funder timeout/unknown outcome, reversed payout and case recovery.

## Decisions that must be independently resolved

| Dependency | Current status / unresolved decision |
|---|---|
| SCF legal entity and operating model | Default partner-led only; actual market/actor/fee permissions require qualified local counsel |
| Source truth and cross-engine contracts | ERP/Trade/Trade Docs/Payments/TMS maintain their sources; cross-engine contracts require Shared approval/pin |
| Canonical SCF capability census | Not yet defined/certified; do not register namespace/key without Shared |
| Real buyer/supplier/funder | None demonstrated in SCF implementation; programme contract, consent and underwriting source required |
| Financial and payment operation | SCF is not lender, PSP, settlement system or loan-accounting core |
| Framework and services | SCF-TECH-01 candidate only; DevContainer/Foundation CI/runtime remains to be implemented |
| Production security and reliability | No pen-test, running environment, bank sandbox, reconciliation drill or SLO/RPO/RTO proof |
| Future extensions | 0030–0035 only upon documented commercial, legal, partner and technology activation triggers |

## External research anchors — evidence, not incorporated law

- [GSCFF finance technique definitions](https://supplychainfinanceforum.org/glossary/) and [buyer-led payables finance](https://supplychainfinanceforum.org/techniques/payables-finance/): distinguish funding products, recourse and source commercial obligations.
- [IFRS IAS 7 and IFRS 7 supplier-finance reporting amendments](https://www.ifrs.org/news-and-events/news/2023/05/iasb-increases-transparency-of-companies-supplier-finance/): effective for annual periods beginning on or after 1 January 2024; financial-reporting entity/ERP owns final classification.
- [FATF/Egmont TBML indicators](https://www.fatf-gafi.org/content/dam/fatf-gafi/reports/Trade-Based-Money-Laundering-Risk-Indicators.pdf): context-specific risk indicators are **not conclusive fraud findings**.
- [UNCITRAL MLETR current enactment status](https://uncitral.un.org/en/texts/ecommerce/modellaw/electronic_transferable_records/status): domestic enactment and instrument-specific legal effect require independent verification.
- [Uganda UMRA licensing/regulation](https://umra.go.ug/service/regulation/) and [South Africa National Credit Act](https://www.justice.gov.za/mc/vnbp/act2005-034.pdf): research inputs only; each real proposed activity needs counsel and current permissions.
- [PAPSS network and participants](https://papss.com/about-us/): potential bank/PSP connectivity, not SCF or Baobab direct participation.
- [Python 3.14](https://docs.python.org/3.14/), [Django 6.0 async semantics](https://docs.djangoproject.com/en/6.0/topics/async/) and [PostgreSQL 17](https://www.postgresql.org/docs/17/): technology references for future SCF-TECH-01 executable test.

## Reviewer and amendment guide

For each ADR, reviewers should identify the exact **decision**; external source facts versus assumptions/inference; source-of-truth authority; class of protected data; legal/product/market scope; irreversible provider actions; failure/UNKNOWN semantics; alternatives; code/contract/negative-test evidence needed; provider funding and business feasibility; prerequisites and explicit triggers for updating the decision. When new information develops, use a dated addendum or superseding ADR: state source URL/date and change, old/new normative rule, consumer/contract impact, existing-case grandfathering, migration and rollback, and accountable approving authority.

**All 35 numbered ADRs and SCF-TECH-01 stay Proposed** until explicitly reviewed and accepted. No canonical SCF events, market licences, qualified lending provider, customer payouts or production support are asserted by this documentation package.
