# TraceLogic — Product Overview

> A non-technical introduction to TraceLogic, what it is, and what makes it different.

---

## Pilot-stage disclaimer

This document describes TraceLogic at its **controlled mortgage pilot stage**. It is not certified, not regulator-approved, and not production-validated for general enterprise use. References to ISO/IEC 42001, the EU AI Act, CCMA, or CPC 2025 describe the regulatory context the product is designed to support, not approval status. Alignment with framework themes is not the same as certification under those frameworks.

---

## In one paragraph

TraceLogic is a deterministic decision governance engine for regulated mortgage decisioning. It combines retrieval-augmented intake, versioned policy evaluation, immutable decision evidence, manager review, attestation, controlled execution, and replay from frozen evidence. The value proposition is not workflow automation. The value is governance assurance: every governed decision can be traced, reviewed, controlled, and replayed from stored evidence.

---

## What problem does TraceLogic solve

Three structural problems sit beneath the decision-governance gap in regulated firms.

### The reconstruction problem

When a regulator, ombudsman, or court asks "show me how you made this decision in March 2024", many regulated firms find it hard to answer with confidence. They can produce screenshots, emails, exports, and PDF copies of policies as they exist *today*. Reliably reproducing the exact decision logic, the exact policy version, the exact evidence set, and the exact human approvals in force at the moment the decision was made is consistently difficult.

### The evidence-burden problem

Under the Mortgage Arrears Resolution Process (MARP) and the Consumer Protection Code 2025, a lender must obtain a Standard Financial Statement, assess affordability against defined inputs, explore alternative repayment arrangements, document why each option is or is not appropriate, and explain the proposed resolution to the borrower. Evidence is collected. It is rarely connected. The Standard Financial Statement lives in one system. The decision lives in another. Approval correspondence lives in email. Reconstructing the full chain for one case can take hours; for a sample of 100 cases under thematic review, it can take weeks.

### The accountability-drift problem

Decision-making in regulated environments has migrated, often quietly, into AI-assisted tools. Regulated firms now use models to triage cases, score affordability, suggest restructures, and even draft borrower communications. From 2 August 2026, the EU AI Act's high-risk obligations apply to AI systems that evaluate the creditworthiness of natural persons. Producing the deployer-side evidence those obligations require, consistently, across high-volume decision processes, is a meaningful operational challenge.

---

## What TraceLogic is — and what it is not

| Question | TraceLogic answer |
|---|---|
| **What is it?** | A deterministic decision governance engine. The decision artifact is the system of record. |
| **What is it not?** | It is not a workflow tool, a CRM, a dashboard product, or an autonomous AI decision-maker. |
| **Does AI make the decision?** | No. Retrieval-augmented intake helps the operator extract data from documents. The deterministic policy engine produces the decision. |
| **Is the operator replaced?** | No. The operator remains responsible for case data, submission, and execution. |
| **Is the manager replaced?** | No. A manager who is not the operator must review and attest the decision before any execution can occur. |

---

## Three architectural commitments

### Commitment 1 — Determinism

The policy evaluation produces the same outputs from the same inputs every time. There is no probabilistic model in the decision path. Every numeric rule that fires emits a structured boundary record showing the actual value, the threshold value, the distance to the threshold, and a traffic-light proximity indicator. There are no silent fallbacks: if an external policy adapter fails, the result is an explicit reject with reasoning, not an implicit pass.

### Commitment 2 — Evidence integrity

Every decision produces an immutable decision artifact carrying a tamper-evident integrity stamp computed at creation time. The artifact is the system of record. Replay re-runs the original request against the frozen evidence captured at decision time — it does not call live external APIs.

### Commitment 3 — Human accountability

The governance lifecycle is **submit → propose → review → attest → token mint → execute → replay**. The user who creates a proposal cannot be the manager who attests it. Execution requires a single-use, time-limited token bound to the artifact's hash; if the artifact has been mutated since attestation, the hash check fails and execution is refused.

---

## Where TraceLogic sits

TraceLogic is **not** a policy engine replacement, an AI governance platform, a workflow tool, or a surveillance product. It sits adjacent to those.

- A workflow tool tracks **what happened operationally**. TraceLogic governs **what was decided and why**.
- A decision automation engine produces a policy outcome. TraceLogic governs the artifact, the human review, the controlled execution, and the replay around that policy outcome.
- An AI governance platform manages model documentation, bias testing, and lifecycle monitoring at the model level. TraceLogic governs at the *decision* level — the immutable artifact, separation of duties, controlled execution, and replay.

A firm needs both: model governance and decision governance.

---

## The pilot domain

The current scope is **Irish mortgage forbearance and loan modification decisioning**. Scenarios in scope include:

- Standard rate reduction
- Term extension (with soft and hard limits)
- Arrears capitalisation (with anti-stacking and warehouse-aware logic)
- Split mortgage (active and warehouse balance reconciliation)
- Interest-only relief
- Personal Insolvency Arrangement (PIA) routes (with court / Insolvency Service of Ireland confirmation gating)

The Irish mortgage domain was chosen because it has:

- A well-defined regulatory framework (CCMA, MARP, CPC 2025)
- High decision volumes per firm (thousands per year)
- High evidence burden per case
- Concentrated arrears volume in a specific operational sector

The architecture is domain-agnostic by design. The same governance engine, with a different policy module, can support insurance claims pre-authorisation, retail credit decisions, BTL mortgage decisions, or other high-impact regulated decisions.

---

## What "deterministic" means in this product

In TraceLogic, "deterministic" is a precise architectural property:

- **Same inputs, same outputs, every time.** The decision is reproducible.
- **No probabilistic model in the decision path.** The decision is not an opaque score.
- **Every rule and threshold is transparent.** The operator and manager can see every comparison, every threshold, and every value used.
- **Replay produces a comparable result.** Re-running the same request against the frozen evidence reproduces the original outcome (any drift is classified explicitly).

This is not the same as saying TraceLogic does not use AI at all. It uses retrieval-augmented techniques to assist the operator with intake. The operator confirms or corrects every extracted field. The decision itself is deterministic.

---

## Why this matters in 2026

Two regulatory landmarks define the operating environment.

**The Consumer Protection Code 2025** of the Central Bank of Ireland came into force on 24 March 2026. It folds the Code of Conduct on Mortgage Arrears into Part 3, Chapters 9 and 10, and raises the bar on "securing customers' interests", "vulnerable circumstances", and "effective informing". Each of those obligations raises the same regulatory question at the level of the individual case: *how do you evidence that you did the right thing?*

**The EU AI Act** entered into force on 1 August 2024. Its high-risk obligations apply from 2 August 2026. Annex III §5(b) explicitly classifies as high-risk any AI system intended to evaluate the creditworthiness of natural persons. Deployer obligations (Article 26) apply even when the AI system is supplied by a third party — a vendor's certification does not discharge the deploying firm's evidence duty.

TraceLogic is designed to support a regulated firm in producing the kind of decision evidence both regimes will require, on a per-decision basis, at the point of decision.

---

## What TraceLogic does not promise

The product positioning is bounded by what is honestly evidenced:

- TraceLogic does not promise compliance. The deploying firm remains responsible for its own compliance posture.
- TraceLogic does not promise to prevent regulatory fines. Whether a decision attracts a regulatory sanction depends on policy, conduct, and many factors outside any vendor's control.
- TraceLogic does not promise that AI output equals a human decision. It does not.
- TraceLogic does not promise certification. Alignment is not certification.

This restraint is itself part of the value. Buyers in this segment have heard a lot of overclaim from RegTech vendors. Honest scope-bounding is a competitive advantage.

---

*See also:* [`governance-architecture.md`](governance-architecture.md) for the eight governance cores in detail, [`pilot-scope.md`](pilot-scope.md) for the controlled pilot's boundary, and [`regulatory-context-ireland.md`](regulatory-context-ireland.md) for the regulatory backdrop.
