# TraceLogic — Demo Script

> A step-by-step walkthrough of an end-to-end TraceLogic demonstration on a synthetic case.

---

## Pilot-stage disclaimer

This demo script presents TraceLogic as a **controlled pilot demonstration**. It is not a claim of ISO/IEC 42001 certification, legal compliance, audit readiness, regulator approval, or enterprise production readiness. Synthetic data only — no real borrowers, no real lenders, no real account references.

---

## Audience and outcomes

| Audience | What this demo should prove |
|---|---|
| **Hiring manager or recruiter** | Practical capability across AI governance, regulated software architecture, product thinking, and stakeholder communication. |
| **Technical reviewer** | Two-service architecture, internal-only governance engine, token-controlled execution, replay design, evidence flow. |
| **Risk, compliance, or governance stakeholder** | Separation of duties, manager review, artifact evidence, policy traceability, proposal governance, replay. |
| **Pilot client or advisor** | How TraceLogic supports controlled mortgage decision governance without overclaiming production readiness. |

---

## Pre-demo checklist

| Area | Item |
|---|---|
| **Environment** | Pilot environment is running. The base URL is known. The browser reaches only the public gateway. |
| **Services** | The internal governance engine is reachable only over the internal Docker network. |
| **Database** | The evidence store is available and can persist artifacts, documents, case state transitions, and audit data. |
| **Users** | At least one operator user and one separate manager / reviewer user are available. **Do not use the same user to demonstrate separation of duties.** |
| **Synthetic case** | Use synthetic data only. Fictional names, fictional account references, fictional property details. |
| **Browser** | Use separate browser profiles (or one regular and one incognito) for operator and manager to avoid token / session confusion. |
| **Evidence** | Screenshots may be captured for portfolio use. Avoid exposing secrets, environment variables, or any real data. |

---

## Suggested synthetic case

| Field | Suggested value |
|---|---|
| Case type | Mortgage forbearance / loan modification request |
| Borrower name | `Alex Morgan` (fictional) |
| Account reference | `SYN-MORT-0001` |
| Jurisdiction | Ireland |
| Scenario | Borrower in financial difficulty seeking an alternative repayment arrangement |
| Proposed treatment | Term extension *or* arrears capitalisation *or* split mortgage *or* PIA route, depending on the demo's focus |
| Governance focus | Show that the decision result alone is not sufficient — manager review, attestation, proposal evidence, and execution readiness are still controlled |

---

## End-to-end demo flow

The demo runs the lifecycle in order. Each step has a presenter cue noting the proof point.

### Step 1 — Operator login

1. Open the pilot URL.
2. Log in as an operator.
3. Confirm the operator sees the mortgage / case dashboard.

> **Proof point:** case creation starts from an authenticated role context, not anonymous access.

### Step 2 — Create a case (or RAG-assisted intake)

1. Open the mortgage intake screen.
2. Choose either manual entry or document-assisted ingestion.
3. If using RAG-assisted intake, upload a synthetic Standard Financial Statement and show the extracted candidate fields.
4. Use synthetic data only.

> **Proof point:** RAG reduces manual entry but keeps the operator responsible for input.

### Step 3 — Operator reviews and submits inputs

1. Check borrower details, proposed treatment, balance, repayment, arrears, rate, maturity, jurisdiction, vulnerability flag, and proposal-specific fields.
2. Correct any extracted or missing values before submission.
3. Submit the case for evaluation.

> **Proof point:** RAG supports intake; deterministic policy governance does not depend on it.

### Step 4 — Review the deterministic decision output

1. Open the decision result page.
2. Point out the decision outcome, reasons, constraints, required evidence, rule path, policy version, and governance fields.
3. Explain that the decision is generated through deterministic policy logic — not an uncontrolled generative model output.

> **Proof point:** repeatability, policy traceability, and business-readable explanation.

### Step 5 — Open the governance / artifact view

1. Show the artifact or governance report view.
2. Point out: stored evidence, policy mode, policy source, tenant context, integrity stamp, correlation identifier, and audit-trail fields.
3. Explain that this evidence supports later replay and review.

> **Proof point:** the system preserves a governed evidence package, not just a workflow status.

### Step 6 — Manager review

1. Switch to a separate browser session as a **different** user — the manager / reviewer.
2. Open the manager dashboard / review queue.
3. Open the submitted case.
4. Review the artifact and approve (or return) it.

> **Proof point:** human-in-the-loop governance and operator/manager separation.

### Step 7 — Manager attestation and token mint

1. From the approved case, complete attestation.
2. Mint the execution token through the manager / reviewer path.
3. Explain that token minting is a controlled handoff — not the same as execution.

> **Proof point:** controlled approval-to-execution separation; manager accountability preserved.

### Step 8 — Operator execution authorisation

1. Switch back to the operator session.
2. Open the execution screen.
3. Use the minted token / handoff reference.
4. Authorise execution where readiness checks pass.
5. Show the execution result and audit stamp.

> **Proof point:** execution is separately controlled; it is not automatically triggered by the decision alone.

### Step 9 — Demonstrate the proposal-governance block (optional)

Show a case where the decision is technically approved but execution is blocked because:

- Borrower acceptance is required after offer but has not been recorded, *or*
- A PIA route is in play but the court / Insolvency Service of Ireland / creditor confirmation has not been captured, *or*
- A "No ARA offered" state has been reached.

> **Proof point:** TraceLogic treats borrower, legal, and proposal evidence as execution-readiness controls — not cosmetic fields.

### Step 10 — Replay from frozen evidence

1. Open the replay screen for the artifact.
2. Show that replay reconstructs the decision explanation from stored artifact evidence.
3. Explain that replay does **not** call live policy systems, live RAG extraction, or external systems.

> **Proof point:** decision traceability; assurance review; closed-loop evidence.

### Step 11 — Trust dashboard review

1. Open the trust dashboard.
2. Show: artifact status, evidence coverage, policy provenance, execution state, replay coverage, regulatory mapping.
3. Confirm that values shown are backend-derived, not placeholders.

> **Proof point:** the supervisory surface sits on top of stored governance evidence.

---

## Suggested presenter framing

### Opening line

> *"TraceLogic is not a workflow tool. It is a deterministic decision governance engine. The user interface exists to demonstrate how governed intake, deterministic policy evaluation, manager review, attestation, controlled execution, replay, and trust evidence work together for a regulated mortgage decision."*

### When showing intake

> *"Retrieval-augmented intake helps pre-fill case data, but the operator remains responsible for reviewing extracted fields. The decision is not made by AI."*

### When showing the decision output

> *"This is a deterministic policy evaluation. Same inputs, same outputs — every time. Every numeric rule has a boundary record showing the actual value, the threshold, and the distance to the threshold."*

### When showing the artifact

> *"This is the system of record. The integrity stamp is computed at creation time. If the artifact is mutated, the execution token's hash check will refuse execution."*

### When showing manager attestation

> *"The user attesting this is not the user who created it. Separation of duties is enforced at the application layer — not by convention."*

### When showing execution

> *"The execution token is single-use, time-limited, and bound to the artifact's hash. We just recomputed the hash; if it had drifted, execution would have been refused."*

### When showing replay

> *"Replay reads from the evidence captured at decision time — not from live systems. This is what a regulator, ombudsman, or auditor needs to see when they ask 'how was this decision made'."*

### When asked: "Is TraceLogic ISO 42001 compliant?"

> *"TraceLogic is aligned to ISO/IEC 42001 themes. Alignment is not certification. No formal management-system assessment has been undertaken under the current controlled mortgage pilot."*

### When asked: "Is TraceLogic regulator-approved?"

> *"No regulator has approved TraceLogic. The Central Bank of Ireland does not endorse vendors. TraceLogic supports a deploying firm in producing decision evidence under CPC 2025 and the EU AI Act — the deploying firm remains accountable for its own compliance posture."*

---

## What not to demonstrate

- Do not demonstrate with real customer data, real account references, real lender names, or production credentials.
- Do not show the internal governance engine being reached directly from the browser. It is internal-only.
- Do not invent values on the trust dashboard. Trust values come from backend evidence.
- Do not claim that AI made a decision. AI assists intake; the decision is deterministic.
- Do not claim the system is certified, audit-ready, or regulator-approved.

---

## Demo timing

A full end-to-end demo covering steps 1–11 typically takes **20–25 minutes** at a comfortable pace, with time for stakeholder questions in between.

A condensed demo covering steps 1, 4, 6, 8, and 10 typically takes **8–10 minutes** and is suitable for a discovery call.

---

*See also:* [`governance-architecture.md`](governance-architecture.md), [`pilot-scope.md`](pilot-scope.md), and [`tracelogic-overview.md`](tracelogic-overview.md).
