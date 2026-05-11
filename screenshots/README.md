# Screenshots

This directory is intended to hold **redacted screenshots** of the TraceLogic operator and manager experience, taken from the pilot environment with synthetic data only.

> **This directory contains a curated set of redacted screenshots that illustrate the TraceLogic controlled mortgage pilot lifecycle using synthetic or approved test data only.** This README documents what should be captured, the redaction rules that apply, and the captions to use when the screenshots are added. This avoids accidentally publishing real or partially-real UI captures into a public repository before they are properly reviewed.

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

## Files included in this directory

| File | What it shows | Caption |
|---|---|---|
| `01-case-intake.png` | RAG-assisted intake and structured case capture using synthetic mortgage data | "Operator reviews extracted case fields from synthetic mortgage evidence. Retrieval-augmented intake assists the process, while the operator remains responsible for confirming the data." |
| `02-decision-review.png` | Decision review, decision outcome, and before-and-after comparison | "Decision review presents the governed outcome in business language, including the proposed treatment impact before the case proceeds to governance review." |
| `03-policy-check-trail.png` | Readable policy check trail, decision considerations, breaches, hard stops, and next step | "Policy checks are displayed as a readable trail so reviewers can see which conditions passed, which issues were recorded, and what action is required next." |
| `04-manager-review.png` | Manager governance review with approval controls and Separation of Duties message | "Manager review of the governed artifact, with Separation of Duties enforced so the approving user cannot be the same user who submitted the decision." |
| `05-execution-gate.png` | Execution gate with governance integrity and token handoff shown in redacted form | "Execution is controlled through a governed gate. Approval, attestation, token handoff, and integrity checks are required before operational execution." |
| `06-evidence-replay.png` | Evidence replay showing frozen decision evidence and before-and-after comparison | "Replay reconstructs the decision from stored evidence, showing the decision state, policy context, execution status, and before-and-after comparison." |
| `07-trust-dashboard.png` | Trust Dashboard showing governance posture and audit coverage | "Trust Dashboard provides a business-readable view of governance posture, audit coverage, replay status, tenant isolation, and policy version visibility." |

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
