# Screenshots

This directory is intended to hold **redacted screenshots** of the TraceLogic operator and manager experience, taken from the pilot environment with synthetic data only.

> **The screenshot files themselves are not included in this commit.** This README documents what should be captured, the redaction rules that apply, and the captions to use when the screenshots are added. This avoids accidentally publishing real or partially-real UI captures into a public repository before they are properly reviewed.

## How to capture a screenshot for this repository

Every screenshot in this directory must satisfy **all** of the following rules:

1. **Synthetic data only.** Use the synthetic borrowers from `/synthetic-cases/` (`Alex Morgan`, `Pat Kennedy`, `Sam Walsh`, `Robin Doyle`) or comparable fictional names. Use account references in the form `SYN-MORT-NNNN`. Use the generic lender name `Generic Lender plc`.
2. **No real customer data.** Even if a real case is closed, paid, and well in the past, do not screenshot it.
3. **No real lender names.** Even tier-1 banks and named non-bank servicers must not appear.
4. **No employer references.** Do not capture screens that show an employer's branding, environment names, or system identifiers.
5. **No secrets, no credentials.** No environment variables, no tokens, no database URLs, no API keys, no AWS / cloud account identifiers, no internal hostnames, no internal IPs.
6. **No internal URLs beyond high-level demonstration.** A screenshot showing `mortgage-intake` in the URL is fine; a screenshot showing the internal governance engine's hostname or internal route is not.
7. **Browser chrome cleanup.** Close password managers, hide bookmarks bar, close other tabs, hide auto-fill suggestions.
8. **Profile cleanup.** The logged-in user's name and email in the header should be a synthetic operator / manager identity (`operator@example.invalid`, `manager@example.invalid`), not a real identity.

## Files this directory should eventually contain

| File | What it should show | Caption |
|---|---|---|
| `intake.png` | The RAG-assisted intake screen with a synthetic SFS uploaded and the operator confirming the extracted fields | "Operator confirms extracted fields from a synthetic Standard Financial Statement. Retrieval-augmented intake assists; the operator remains responsible for the data." |
| `manager-review.png` | The manager review queue with the governed artifact open and approve / return controls visible | "Manager review of the governed artifact, with the rule path, boundary analysis, and audit fields visible. The user reviewing is different from the user who created the proposal." |
| `execution-gate.png` | The execution screen on a case where execution is blocked because borrower acceptance / Insolvency Service of Ireland confirmation is missing | "The proposal-governance gate blocks execution because borrower acceptance and ISI confirmation have not been captured. The decision is technically approved but execution is held until the proposal evidence is complete." |
| `replay.png` | The replay output for a closed synthetic case, showing original-vs-replay comparison and a 'no drift' classification | "Replay against the artifact's frozen evidence. The original decision and the replay re-run produced the same outcome. Drift classification: none." |
| `trust-dashboard.png` | The trust dashboard showing artifact status, evidence coverage, replay coverage, and policy provenance | "Trust dashboard surfaces artifact status, evidence coverage, replay coverage, and policy provenance. Values shown are sourced from backend evidence." |

## Redaction guidance

If a screenshot shows anything that should not be published, redact it before commit:

- **Black bars over user identities** (real names, real emails, real role IDs)
- **Black bars over URLs** that show internal hostnames or paths
- **Black bars over timestamps** that could correlate to real events
- **Black bars over correlation identifiers** that match real production traces
- **Replace any account / case / borrower reference** that does not start with `SYN-` or another clearly synthetic prefix

If in doubt, do not commit the screenshot.

## What this directory is *not*

- It is not a marketing gallery. The screenshots illustrate the governance flow; they are not visual sales material.
- It is not a complete UI documentation set. The full UI is documented separately under controlled access.
- It is not a substitute for a live demo. A live demo is more informative for a serious evaluator. (See [`docs/demo-script.md`](../docs/demo-script.md).)

## Pilot-stage disclaimer

Any screenshot in this directory illustrates TraceLogic at the **controlled mortgage pilot stage** with synthetic data. No screenshot is to be presented as evidence of certification, legal compliance, regulator approval, or production-grade enterprise readiness.
