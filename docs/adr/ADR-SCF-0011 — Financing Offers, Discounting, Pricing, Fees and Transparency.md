# ADR-SCF-0011 — Financing Offers, Discounting, Pricing, Fees and Transparency

**Status:** Proposed — architecture review, not acceptance or implementation  
**Date:** 2026-10-08  
**Repository:** baobab-platform/baobab-scf  
**Premise:** Partner-led, headless, self-hosted; existing ADR-SCF-0001 remains Proposed and controls the charter  
**Authority:** External qualified financiers own credit/contract decisions; ERP owns financial balances; Payments owns permitted payment execution; Shared owns canonical event/capability vocabulary, CP tenant/provider, IAM authentication  

> No regulator/licence, funder contract, provider certification, bank transfer or production capability is created by this ADR.


## 1. Decision
Persist **external funder-sourced offers** as immutable, precise, revisioned terms. SCF can calculate explainable comparisons from source formulas but does not set binding regulated rates. Discount rate, purchase price, loan interest, day-count, reserve, recourse, programme service fee, tax and FX are different concepts with distinct obligors and payees.

## 2. Offer terms
| Component | Required contract/projection data |
|---|---|
| Source face amount | ERP amount, currency, balance as_of and eligible funded portion |
| Purchase/advance amount | funder's original offer with settlement/currency basis |
| Discount or interest | rate/amount, pricing base, period/day-count, tier and calculation version |
| Reserve/retention | withheld amount/percent, release conditions, dispute treatment |
| Recourse | to whom, for which events, legal source and expiry |
| Fees | beneficiary and payer legal entities, time, tax treatment, source |
| Maturity | obligation date, funder date, calendars and effective zones |
| FX | obligation, offer, payout, beneficiary and repayment currencies; quote source/expiry |
| Disclosure/acceptance | exact offer revision, last valid timestamp, signed representations |

## 3. Example calculation rules
For a single discounting illustration, gross face minus funder discount minus authorised disclosed fees minus reserve may approximate initial proceeds. The actual contract defines whether fees are deducted, financed or charged separately; reserve is not automatically income. For loans, separate principal, time-based interest, repayment and compounding; **do not** use one universal percentage formula across receivable purchase and loan products. Monetary arithmetic must use decimals and declared ISO currency precision, never binary float.

~~~mermaid
flowchart LR
 A["ERP obligation snapshot"] --> B["Funder exact offer / disclosure"]
 B --> C["Deterministic cost comparison with assumptions"]
 C --> D["Informed selection, recourse and risk display"]
 D --> E["Signed/verified provider acceptance"]
 E --> F["Payment observation and ERP reconciliation"]
~~~

## 4. Fraud and consumer/business fairness
Hidden compensation, expired rate, fee double-count, irrelevant APR comparison, altered borrower name, currency conversion slipped after acceptance and funder terms changed at signature must block automatic selection. Present **effective amount recipient receives**, not only headline discount. Any jurisdiction-required APR, total-cost or credit disclosure requires product-specific counsel/partner review.

## 5. Alternatives rejected
One generic interest_rate; automatically choose cheapest nominal percentage despite recourse; platform-issued binding lender offer; silent FX rate of one; platform fee taken from payout without authority.

## 6. Implementation gates
| Gate | Proof |
|---|---|
| SCF-OFR-01 | Exact offer/fee and version schema with funder contract provenance |
| SCF-OFR-02 | Decimal, calendar, day-count, fee tier, reserve and FX unit tests |
| SCF-OFR-03 | Revised/expired/withdrawn offer and simultaneous accept conflict tests |
| SCF-OFR-04 | Entitled pricing confidentiality and bank/supplier disclosures |
| SCF-OFR-05 | Funder reviewed terms/legal transparency before customer use |

## 7. Revisit on change
Regulatory disclosure revisions, new contract formulas, funding currency or business model require amendment. [GSCFF glossary](https://supplychainfinanceforum.org/glossary/).

## 8. Platform-wide implementation constraints

All case actions require IAM identity plus redeemed CP caller-bound tenant/legal entity and scope; no bare tenant/provider header authorises access. Keep separate immutable occurred/observed/recorded timestamps, source digests, cross-engine pinned references, audit and idempotent outbox/inbox when implemented. UNKNOWN external side effects must be reconciled, not retried blindly. Preserve maker/checker and funder independence where legally or contractually required. Data disclosure is purpose-minimised; provider callback identity must be verified, not trusted based on URL.

**Review/revision protocol:** on changed external law, market, source contract or product economics, record a dated amendment with evidence URL/version, effective period, changed assumptions, impact on accepted prior cases, Shared contracts and migration plan. Each implementation gate needs exact code/test references, negative tests, operational telemetry and relevant independent sign-offs. Never claim Shared finance namespace, a new SCF capability, or CP provider certification simply by merging a local ADR.
