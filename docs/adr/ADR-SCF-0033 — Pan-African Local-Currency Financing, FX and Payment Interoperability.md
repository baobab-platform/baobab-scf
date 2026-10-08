# ADR-SCF-0033 — Pan-African Local-Currency Financing, FX and Payment Interoperability

**Status:** Proposed — future-conditional and deferred activation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Prerequisites:** Proposed SCF charter/architecture, separate signed partner/legal market authority, existing Shared/CP/IAM/ERP/Payments/Trade Docs boundaries  

> Documentation does not grant lending, funder, payment, bank, transferable-record, collateral custody or market rights; no new mandatory stack is approved.


## 1. Strategic decision
Permit future **multi-currency, multi-market funder offers and financing observations** while leaving regulated FX dealing, cross-border payment initiation, bank settlement, beneficiary account verification and cash accounting to qualified banks/PSPs, Baobab Payments and ERP. PAPSS is a possible *participant-bank integration route*, not a default SCF payment connector or entitlement.

## 2. Currency, country and partner boundaries
| Role | Example | Source authority |
|---|---|---|
| Source obligation currency | USD-denominated export invoice | ERP |
| Offer / purchase currency | ZAR loan/receivables price | Funder contract |
| Funder debit currency | ZAR bank account | Funder/bank |
| Beneficiary credited currency | UGX supplier bank | Bank/Payments |
| Repayment currency | USD or agreed other | Lender contract |
| Fees and reserve currency | funding/contract terms | Funder/ERP |
| ERP reporting currency | local statutory accounts | ERP |
| FX quote and spread | effective rate, timestamps, fees | Approved bank/PSP |
| Transfer corridor | payer/payee, jurisdiction, bank, market | CP entity and licensed provider |
| Rail participation | eligible actual bank/PSP connector | Payments/participating provider |

Cross-border movement is not synonymous with currency conversion; a transfer in the same currency can cross a border, while domestic settlements can involve FX. Never hard-code country→currency relationships. FX quote is not necessarily executed conversion; rate source, validity, spread and who bears loss are source agreement terms.

~~~mermaid
flowchart LR
 A["ERP invoice currency and party refs"] --> B["SCF source-attributed funder offer"]
 B --> C["Licensed bank / Payments FX and payout"]
 C --> D["Provider payout and beneficiary amount"]
 D --> E["Bank statement + ERP reconciliation"]
 E --> F["SCF foreign-currency position projection"]
~~~

## 3. PAPSS and future partner interoperability
A participating bank/payment service provider may connect through PAPSS or another eligible network. Actual destination bank/currency, partner scheme version, cutoffs, refunds/returns, settlement periods, safeguarding and compliance screening must be validated per corridor. This ADR neither establishes a Baobab PAPSS membership nor guarantees instant funding or all African markets. Payments retains currency handling and settlement protocol ownership.

## 4. Risk and operating cases
Expired currency quote after financing acceptance, disbursement to different currency, payment-network fallback, beneficiary mismatch, sanctions delay, foreign exchange controls, weekend value date, different loan repayment FX, withholding/transaction tax and mismatch between provider "sent" and bank "credited" require separate source-specific exceptions. SCF must not issue a repayment instruction based on a stale exchange estimate or recognise realised FX accounting gains/losses.

## 5. Conditional triggers and alternatives
**Trigger:** qualified cross-border bank/PSP agreement, market-specific legality, signed borrower/beneficiary instructions, precise payouts/FX contract and measured demand. **Block:** missing lawful rail or FX rights, no reliable settlement observation, unsupported beneficiary residency, unaudited platform bank balances. Reject SCF-managed multi-currency wallet, hard-coded currency mapping, provider success=final settlement, and universal coverage claims.

## 6. Implementation gates (deferred)
| Gate | Objective proof |
|---|---|
| SCF-FX-01 | All currency roles modelled, approved cross-border activity matrix |
| SCF-FX-02 | Rates/spreads/rounding/quote expiry/FX differences with decimal accuracy tests |
| SCF-FX-03 | Payments PAY-0016/0017 payout/FX conformance and tenant entitlement |
| SCF-FX-04 | Timeout, return, beneficiary mismatch and source settlement reconciliation |
| SCF-FX-05 | Qualified corridor partner and bank sandbox, no direct participant assumptions |
| SCF-FX-06 | Legal/privacy/ERP accounting and independent release approval by market |

## 7. References and revision
[PAPSS participation model](https://papss.com/about-us/), [PAPSS settlement process](https://papss.com/how-it-works/), [Baobab Payments](https://github.com/baobab-platform/baobab-payments/tree/main/docs/adr). Review anew on real market availability, new rail, currency risk or law change.

## 8. Cross-cutting implementation and review protocol

IAM-authenticated principal and caller-bound CP tenant/legal entity/market context precede all cases, partner actions and data disclosures. Use source-owned cross-engine references, immutable external observations, source time/revision, purpose-scoped permissions, safe idempotency and reconciliation for UNKNOWN outcomes. Never treat funder approval, instrument issuance, received bank money, accounting balance and regulatory permission as the same status. Shared alone governs canonical capability/event semantics; CP/EA-09 registers and certifies only proven provider scopes.

**Revisit procedure:** date and cite changed law/partner/technical standards, classify the assertion as verified fact vs design inference, identify impacted old cases and contracts, map privacy/technical/financial risks, and file a superseding or amended ADR with evidence and rollback/grandfathering policy. Future gates require implementation tests and independent legal, business, security and partner approvals. No gate passes by merging this document.
