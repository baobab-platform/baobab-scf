# ADR-SCF-0013 — Funding, Disbursement, Repayment and Cross-Border Payments Boundary

**Status:** Proposed — pending independent acceptance  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Scope:** Partner-led SCF evidence/orchestration; separate external finance, Payments, ERP and source-domain authority  
**Dependencies:** Proposed ADR-SCF-0001; relevant prior SCF decisions; accepted Shared/CP/IAM/Payments/ERP/Trade/Trade Docs/TMS and Regulations contracts where applicable  

> No executable SCF system, financial licence, bank account, qualified partner, legal credit decision, artificial intelligence product or production acceptance is demonstrated by this ADR.


## 1. Decision
SCF **observes** funder authorisation, disbursement, repayments, returns and reconciliation; it does not itself transfer funds, hold deposits, execute FX, authorise a beneficiary or keep a loan/GL ledger. Qualified funders own the financing action and financial rights; baobab-payments owns **only its actually implemented and entitled payment/payout operations**, while ERP owns journal posting, cash, receivable/payable accounting and bank matching. An external funder may pay outside Baobab Payments; SCF must support verified observational integration without assuming all banks use HyperSwitch.

## 2. Source-to-projection model
| Source fact / operation | Authority | SCF state |
|---|---|---|
| Funder authorises financing | authorised financing partner | funding-authorised observation |
| Payout instruction accepted | funder or permitted Payments API | instructed, not paid |
| PSP callback says processing | payment connector/provider | in-progress, not final |
| Payment succeeded | source provider assertion plus settlement caveat | source-reported success |
| Beneficiary credited / bank confirmation | actual bank/funder statements | stronger funds-received evidence |
| Payment returned/reversed | Payments/provider/funder | separate negative movement observation |
| Debt repayment/receivable collected | external funder/servicer and ERP | source-confirmed repayment |
| Cash and receivable reconciliation | ERP/bank | financial matched/unmatched state |
| FX conversion and costs | authorised PSP/provider | source rates/fees; not SCF fixing exchange rate |

~~~mermaid
sequenceDiagram
 participant F as Funder
 participant S as SCF
 participant P as Payments
 participant B as Bank / PSP
 participant E as ERP
 F->>S: Financing acceptance & source funding intent
 S->>P: Only if authorised payout API exists
 P->>B: Beneficiary-scoped payout
 B-->>P: Async success, pending, return or unknown
 P-->>S: PaymentRef + attributed status
 B-->>E: Financial statement
 E-->>S: Authoritative financial reconciliation
 S->>S: Rebuild case position (no loan ledger)
~~~

## 3. Currency roles and monetary integrity
Store source obligation currency, offer currency, disbursement funding currency, beneficiary received currency, repayment currency, service-fee currency and ERP functional/reporting currencies separately. Use decimal-safe amounts, independently sourced FX quote/contract, value dates, spreads, banking and provider fees. A cross-border payment may remain the same currency; multiple currencies may arise domestically. Reconcile exchange differences only when both source and actual amounts are supported.

## 4. Reversal and retry safety
A payout timeout is **UNKNOWN_OUTCOME**, not failed. Do not reissue with a different PSP or bank unless status inquiry/evidence establishes prior side effect is safe to retry. A beneficiary account amendment after signed acceptance requires authorised re-verification. Late payments, duplicated callback, returned payment, partial finance and returned collection must preserve historical observations; position may move from reconciled to exception, but past payment record is not deleted.

## 5. Privacy and financial licensing
Raw banking details, account numbers and card information should remain in approved Payments/PSP vaults. SCF holds only tokenised or appropriately classified refs. No assumption of direct PAPSS membership; eligible bank/PSP participants execute those payments. Collection, escrow or cross-border transfer permissions require legal review under ADR-0003.

## 6. Implementation gates
| Gate | Proof |
|---|---|
| SCF-PAY-01 | PAY-0016/PAY-0017 and ERP contract authority matrix |
| SCF-PAY-02 | Full payment/beneficiary/funding currency taxonomy and decimal amount tests |
| SCF-PAY-03 | Authorised observation, idempotent payout reference, bank/ERP reconciliation integration |
| SCF-PAY-04 | Unknown, duplicate, wrong beneficiary, reversed, partial and FX mismatch fixtures |
| SCF-PAY-05 | Signed provider contract and approved funds-flow diagram before any real-money operation |

## 7. Revision trigger and references
Reassess after accredited payment partner, bank rail, future licensed operating entity or new funding jurisdiction. [Payments Payout ADR-0016 and FX ADR-0017](https://github.com/baobab-platform/baobab-payments/tree/main/docs/adr); [PAPSS participant overview](https://papss.com/about-us/).

## 8. Cross-cutting safeguards, alternatives and review practice

Every consequential operation must bind IAM subject and CP trusted tenant, legal entity, market and relationship; never trust client-supplied actor or tenant labels. Protect commercial and personal finance data through scoped disclosures, encryption, classifications and retention review. Use precise source-owned CrossEngineObjectReference, immutable observations with original occurred_at/recorded_at, and auditable versioned rules. For external financial side effects, preserve uncertain outcomes and reconcile rather than retry blindly. Reject treating SCF as a loan ledger, credit committee, customs agency, payment processor, document issuer or hidden cross-subsidiary information channel.

**Review requirements:** record actual external legal/partner evidence with dated URL and contract revision; separate current approved reality from target architecture; preserve previously issued source facts; perform security and source-authority tests. Acceptance of this Proposed ADR does not register canonical capabilities/events or establish CP certification. Each gate must map to future code, tests, deployment and independent legal/business approval, with explicit unsupported operations and revisit triggers.
