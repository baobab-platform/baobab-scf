# ADR-SCF-0016 — Financial Crime, KYB/KYC, AML, Sanctions and Trade-Based Fraud Controls

**Status:** Proposed — pending independent acceptance  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Scope:** Partner-led SCF evidence/orchestration; separate external finance, Payments, ERP and source-domain authority  
**Dependencies:** Proposed ADR-SCF-0001; relevant prior SCF decisions; accepted Shared/CP/IAM/Payments/ERP/Trade/Trade Docs/TMS and Regulations contracts where applicable  

> No executable SCF system, financial licence, bank account, qualified partner, legal credit decision, artificial intelligence product or production acceptance is demonstrated by this ADR.


## 1. Decision
Build a **governed evidence and enforcement boundary** for financial-crime screening; do not claim SCF itself is a regulated AML authority or that every flagged case constitutes money laundering. Source identities, beneficial ownership and KYB/KYC verification come from authorised IAM/CP/counterparty providers and contracted regulated financial institutions. Regulations may supply a verified applicable policy/decision within its approved scope. Partners retain legal AML/sanctions reporting obligations and final risk decisions, as required.

## 2. Risk-source matrix
| Signal | Source / interpretation | Rule |
|---|---|---|
| Legal entity and UBO | Verified business registry/KYB provider, CP canonical org | preserve source confidence/age, not self-certify |
| Funder, buyer, seller restrictions | lawful sanctions/PEP screening provider under contract | false-positive review, not auto-public accusation |
| Document discrepancy | Trade Docs issuer vs extracted, changed invoice, invalid certificate | anomaly candidate only |
| Transport mismatch | TMS proof of route/delivery vs invoice/packing record | materiality/source trust required |
| Duplicate financing | local allocation collisions + externally confirmed assignment/registry | no universal cross-bank duplicate guarantee |
| Account redirection | Payments beneficiary change, actor mandate inconsistency | high-risk approval/step-up |
| Trade value/quantity anomaly | authoritative ERP/Trade source and optional Pulse analytics | risk indicator, not AML verdict |
| Identity takeover/collusion | IAM assurance, device/workload/auth events and approvals | privacy bounded and reviewed |

## 3. Screening and case treatment
~~~mermaid
flowchart LR
 A["Trusted identity and source transaction"] --> B["Applicable partner/legal risk control"]
 B --> C["Source screening and anomaly observations"]
 C --> D{"Verified actionable block?"}
 D -->|Yes| E["Fail closed on mandated operation"]
 D -->|Unknown| F["Human/partner investigation and hold"]
 D -->|No| G["Permit application to proceed, subject to funder decision"]
~~~

Compliance evidence includes source, list/version, screening timestamp, entity matching details, positive/negative reasons, licence/relationship, review policy and operator. A sanctions list match can be false; maintain review/appeal and privacy controls. Unknown required screening result blocks qualifying external submission, but does not erase the historical case.

## 4. Adverse trade-finance examples
Mispricing, misdeclared goods, multiple invoicing, unusual transshipment, mismatched consignee, suspicious same-bank-account reuse, inflated quantity, rapid repeated financing attempts and connected-party conflicts are **risk indicators**. A FATF/Egmont risk signal must be assessed in context and may not be a conclusive offence. If an obligated partner must submit a suspicious activity report, its legal owner manages confidential reporting; SCF should not disclose protected investigation content to the supplier or unrelated parties.

## 5. External legal obligations
Determine whether Baobab's **actual activity** creates accountable-institution, data-processor, screening, recordkeeping or reporting obligations separately in Uganda and South Africa. An enterprise fintech integration is not automatically a licensed financial institution, nor exempt from all rules. No provider screen can be labelled universal AML compliance.

## 6. Implementation gates
| Gate | Proof |
|---|---|
| SCF-FCR-01 | Market/activity-specific AML/KYB/sanctions/legal responsibility matrix |
| SCF-FCR-02 | Source-verified KYB/UBO, consent, list version and expiry schema |
| SCF-FCR-03 | False matches, illicit disclosure, beneficiary redirection and wrong-tenant tests |
| SCF-FCR-04 | Materiality and source discrepancy with human investigator protocol |
| SCF-FCR-05 | Contracted compliance partner integration, legal opinion and audit before pilot |

## 7. Review triggers and sources
New sanctions law, risk indicators, provider obligations or partner role requires prompt reassessment. [FATF/Egmont trade-based money laundering risk indicators](https://www.fatf-gafi.org/content/dam/fatf-gafi/reports/Trade-Based-Money-Laundering-Risk-Indicators.pdf).

## 8. Cross-cutting safeguards, alternatives and review practice

Every consequential operation must bind IAM subject and CP trusted tenant, legal entity, market and relationship; never trust client-supplied actor or tenant labels. Protect commercial and personal finance data through scoped disclosures, encryption, classifications and retention review. Use precise source-owned CrossEngineObjectReference, immutable observations with original occurred_at/recorded_at, and auditable versioned rules. For external financial side effects, preserve uncertain outcomes and reconcile rather than retry blindly. Reject treating SCF as a loan ledger, credit committee, customs agency, payment processor, document issuer or hidden cross-subsidiary information channel.

**Review requirements:** record actual external legal/partner evidence with dated URL and contract revision; separate current approved reality from target architecture; preserve previously issued source facts; perform security and source-authority tests. Acceptance of this Proposed ADR does not register canonical capabilities/events or establish CP certification. Each gate must map to future code, tests, deployment and independent legal/business approval, with explicit unsupported operations and revisit triggers.
