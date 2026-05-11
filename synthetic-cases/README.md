# Synthetic case examples

This directory contains four synthetic case examples that illustrate the kinds of decision the TraceLogic deterministic policy engine evaluates.

## What these are

Each case is a structured JSON document representing a hypothetical mortgage forbearance / loan modification scenario. Each case includes:

- A `_synthetic_data_disclaimer` field at the top
- Case metadata (case ID, jurisdiction, policy mode, timestamp)
- Borrower information (fictional names only)
- Loan state (balance, rate, term, arrears)
- Proposed treatment
- Standard Financial Statement summary
- An `expected_evaluation_summary` showing what the deterministic engine would produce
- Governance notes explaining what each case demonstrates

## What these are **not**

- **Not real cases.** All values are fictional. Borrower names, account references, property details, and lender names are invented.
- **Not the policy library.** The expected evaluation summaries are illustrative — they are not the actual rule definitions, threshold values, or computed outputs of the production engine.
- **Not the rule engine.** No code, threshold values, or engine internals are exposed.
- **Not sufficient to reproduce TraceLogic's behaviour.** They illustrate the *shape* of input and output, not the engine's logic.

## The four cases

| File | Scenario | What it demonstrates |
|---|---|---|
| `loan-modification-case.json` | Standard rate reduction | A clean APPROVE outcome with all proximity flags green |
| `arrears-capitalisation-case.json` | Arrears capitalisation | Anti-stacking and warehouse-aware logic; an AMBER proximity on the at-threshold rule |
| `term-extension-case.json` | Term extension | A NEEDS_OVERRIDE outcome triggered by a soft-threshold breach (borrower age at term end) — manager review is required |
| `pia-case.json` | PIA-linked modification | The proposal-governance gate blocking execution because borrower acceptance and Insolvency Service of Ireland confirmation have not been captured, even though the decision is technically approved |

## Synthetic data rule

These cases use:

- Fictional borrower names: `Alex Morgan`, `Pat Kennedy`, `Sam Walsh`, `Robin Doyle`
- Fictional account references in the format `SYN-MORT-NNNN`
- The generic lender name `Generic Lender plc`
- Realistic but fabricated balance, rate, term, and Standard Financial Statement values
- The Irish jurisdiction (`IE`)

No real customer data, no real lender names, no real account references appear in any synthetic case in this repository.
