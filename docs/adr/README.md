# Baobab Supply Chain Finance — Architecture Decision Records

Engine-local decisions belong here. Shared remains the authority for cross-engine contracts, capability namespace registration, producer activation, cross-engine references and Control Plane provider resolution. A local SCF ADR does not grant financial-services permissions or funder authority.

| ADR | Status | Decision |
|---|---|---|
| [ADR-SCF-0001](ADR-SCF-0001%20%E2%80%94%20Supply%20Chain%20Finance%20Mission%20Authority%20and%20Partner-Led%20Financing.md) | **Proposed** | Supply Chain Finance mission, legal/financial authority, evidence, partner-led execution and implementation gates |

## Precedence and status

- [Shared](https://github.com/baobab-platform/shared) controls canonical capability names, registration, event schemas and reference semantics. No new `scf.*` domain is authorised by ADR-SCF-0001.
- [Control Plane](https://github.com/baobab-platform/baobab-cp) controls legal-entity/tenant context, provider certification/registration, binding, entitlement and activation.
- [IAM](https://github.com/baobab-platform/baobab-iam) authenticates people and service workloads.
- [ERP](https://github.com/baobab-platform/baobab-erp), [Trade](https://github.com/baobab-platform/baobab-trade), [Trade Docs](https://github.com/baobab-platform/baobab-trade-docs) and [TMS](https://github.com/baobab-platform/baobab-tms) retain their accepted business authority.
- Actual lending/servicing/credit decision/disbursement legality depends on relevant authorised external institutions, contractual arrangements and market-specific law.

This repository remains architecture/scaffold stage. No SCF runtime, financing provider support, credit authority or production acceptance has been demonstrated.
