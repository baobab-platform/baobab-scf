# ADR-SCF-0017 — AI-Assisted Financing Risk Intelligence, Model Governance and Human Oversight

**Status:** Proposed — pending independent acceptance  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Scope:** Partner-led SCF evidence/orchestration; separate external finance, Payments, ERP and source-domain authority  
**Dependencies:** Proposed ADR-SCF-0001; relevant prior SCF decisions; accepted Shared/CP/IAM/Payments/ERP/Trade/Trade Docs/TMS and Regulations contracts where applicable  

> No executable SCF system, financial licence, bank account, qualified partner, legal credit decision, artificial intelligence product or production acceptance is demonstrated by this ADR.


## 1. Decision
Allow **optional** advisory models/rules for detecting evidence gaps, applicant service risks, buyer-payment patterns, invoice irregularities and operational delays through a **ModelAdvicePort**. SCF and Pulse **do not** independently approve, price or decline credit, create legally binding underwriting risk scores, decide sanctions or update verified documentary facts. Authorised funders remain responsible for model use in real lending decisions and for applicable fairness/consumer-law controls.

## 2. Model artefact and provenance
| Component | Required contents |
|---|---|
| AdviceRequest | exact case/evidence snapshot, purpose, permitted fields, tenant, actor and source times |
| ModelProfile | owner, algorithm/prompt version, expected inputs, licence, restrictions, evaluation dataset |
| AdvisoryObservation | suggestion/flag, attribution to underlying sources, confidence/uncertainty and generated time |
| ExplanationTrace | features, available reasons, detected gaps, inability to explain fields |
| HumanReview | reviewer, rationale, override, source comparison and decision authority |
| MonitoringRun | drift, calibration, false positives, subgroup impacts where lawful and meaningful |
| SuppressionPolicy | prohibited data, retention, allowed recipients, model shutdown triggers |

No real-person credit decisions should be made using a synthetic accuracy metric or unjustified proxy. Do not use protected personal characteristics as shortcuts for repayment assessment. Data for a Thamani supplier may not train a group-wide model without legal permission and appropriate privacy/corporate controls.

~~~mermaid
flowchart TD
 A["Version-pinned Trade/ERP/TMS/Docs facts"] --> B["Data minimisation and user purpose"]
 B --> C["Deterministic checks / optional Pulse or ML"]
 C --> D["Advice + explanation + confidence"]
 D --> E["Funder/human authorised review"]
 E --> F["Source-backed disposition"]
 F --> G["Monitoring, correction and audit"]
~~~

## 3. Runtime isolation
Use a bounded, replaceable adapter; no new mandatory ML platform or external LLM service at Foundation-1. If optional remote model is considered, review data residency, processor terms, purpose and retention, training opt-outs, log redaction, prompt injection via malicious documents, model inversion and access scope. Models can be nondeterministic; record dataset and model revision, generation parameters where applicable and repeatability limitations.

## 4. Explainability, fairness and uncertainty
Confidence is not a probability of repayment unless externally validated. An anomaly is not fraud; a source mismatch requires owner verification. Score drift, market shift, poor performance on smaller suppliers, sparse historical data, language bias and partner-policy overrides need review. Human can correct erroneous extraction/inference but cannot create legal issuer truth. Report model error rates by documented evaluation cohort; avoid invented accuracy.

## 5. Failure and fallback
Unreliable model, missing data, stale model, denied external processor, provider outage or unexplainable inference should return ADVICE_UNAVAILABLE; core verified evidence workflow can continue under mandatory manual/policy controls, without bypassing sanctions/eligibility holds. Funders may reject advice or use own models; portability must be maintained.

## 6. Implementation gates
| Gate | Evidence |
|---|---|
| SCF-AI-01 | Product-specific prohibited vs advisory model activities, threat/data policy |
| SCF-AI-02 | Typed advice/output provenance and explanation schema, model/version evidence |
| SCF-AI-03 | Calibration/false-positive and drift fixtures on synthetic/consented samples |
| SCF-AI-04 | Human override, prohibited decision automation and wrong-tenant training tests |
| SCF-AI-05 | Partner legal/AI policy, data-protection and measured evaluation prior to rollout |

## 7. Review triggers
New applicable automated-decision regulations, partner risk model, disparate error results or AI architecture change. Advisory insights never make Pulse a credit authority.

## 8. Cross-cutting safeguards, alternatives and review practice

Every consequential operation must bind IAM subject and CP trusted tenant, legal entity, market and relationship; never trust client-supplied actor or tenant labels. Protect commercial and personal finance data through scoped disclosures, encryption, classifications and retention review. Use precise source-owned CrossEngineObjectReference, immutable observations with original occurred_at/recorded_at, and auditable versioned rules. For external financial side effects, preserve uncertain outcomes and reconcile rather than retry blindly. Reject treating SCF as a loan ledger, credit committee, customs agency, payment processor, document issuer or hidden cross-subsidiary information channel.

**Review requirements:** record actual external legal/partner evidence with dated URL and contract revision; separate current approved reality from target architecture; preserve previously issued source facts; perform security and source-authority tests. Acceptance of this Proposed ADR does not register canonical capabilities/events or establish CP certification. Each gate must map to future code, tests, deployment and independent legal/business approval, with explicit unsupported operations and revisit triggers.
