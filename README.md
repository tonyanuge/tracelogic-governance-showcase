# tracelogic-governance-showcase

> **A public showcase of TraceLogic — a deterministic decision governance engine for regulated mortgage decisioning.**
> This repository contains documentation, diagrams, synthetic case examples, and demo materials only. The product source code, real customer data, infrastructure details, and internal endpoints are not included.

---

## What this repository is

This repository is the **public-facing showcase** of TraceLogic. It is intended for:

- **Employers** evaluating capability across AI governance, regulated software architecture, and product thinking
- **Investors** assessing the product narrative and regulatory positioning at a high level
- **Pilot clients** exploring whether TraceLogic fits their decision-governance need
- **Collaborators** considering working on TraceLogic-adjacent problems

It contains documentation, architectural narratives, redacted screenshots, synthetic case examples, system-level diagrams, and a demo-script overview.

It does **not** contain the TraceLogic source code, internal endpoint specifications, infrastructure configuration, authentication logic, the deterministic policy engine's rule library, real customer data, or any secrets, credentials, or environment files. None of those will be added to this repository.

---

## Pilot-stage disclaimer

**TraceLogic is at the controlled mortgage pilot stage.** It is not certified, regulator-approved, audit-passed, or production-validated for general enterprise use.

Specifically:

- **No certification claim.** TraceLogic is designed against ISO/IEC 42001 themes and other governance frameworks. *Alignment with themes is not the same as certification under those standards.* No formal management-system audit has been undertaken.
- **No regulator approval.** No regulator has reviewed or approved TraceLogic. The Central Bank of Ireland does not endorse vendors; references to CPC 2025, CCMA, or the EU AI Act describe the regulatory context the product is designed to support, not approval status.
- **No legal-compliance claim.** TraceLogic supports evidence and process discipline that a regulated firm needs in order to meet its own obligations. The deploying firm — not TraceLogic — remains accountable for its compliance posture.
- **No production-maturity claim.** The current scope is a controlled mortgage pilot baseline.

This README, and the documents linked from it, are written to that boundary.

---

## What TraceLogic is

**TraceLogic is a deterministic decision governance engine** with retrieval-augmented intake and replayable evidence.

The product addresses a specific structural problem in regulated decision-making: regulated firms make thousands of high-impact decisions every quarter — credit decisions, restructure decisions, claims decisions, eligibility decisions — and many find it hard to reliably reconstruct, defend, or replay those decisions when a regulator, ombudsman, or court asks them to.

TraceLogic addresses this by treating the decision artifact itself as the system of record:

- The decision is produced by a **deterministic policy engine** — same inputs produce the same outputs, every time.
- The decision is captured as an **immutable artifact** carrying a tamper-evident integrity stamp.
- The decision moves through a controlled lifecycle: **submit → propose → review → attest → token mint → execute → replay**.
- A **manager review and attestation step** separates the operator who proposes a decision from the manager who approves it.
- **Execution requires a fresh, single-use, time-limited token** bound to the artifact's hash; if the artifact has been mutated since attestation, execution is refused.
- Decisions can be **replayed against frozen evidence** — the evidence captured at decision time, not the system as it stands today.

The pilot domain is **Irish mortgage forbearance and loan modification decisioning** — split mortgages, arrears capitalisation, term extension, interest-only relief, and Personal Insolvency Arrangement (PIA) routes.

---

## Problem statement

Three structural problems sit beneath the decision-governance gap in regulated firms.

**The reconstruction problem.** When a regulator, ombudsman, or court asks "show me how you made this decision in March 2024", many firms find it hard to answer with confidence. They can produce screenshots, emails, exports, and PDF copies of policies as they exist *today* — but reliably reproducing the exact decision logic, the exact policy version, the exact evidence set, and the exact human approvals in force at the moment the decision was made is consistently difficult.

**The evidence-burden problem.** Under the Mortgage Arrears Resolution Process (MARP) and the Consumer Protection Code 2025, a lender must obtain a Standard Financial Statement (SFS), assess affordability against defined inputs, explore alternative repayment arrangements, document why each option is or is not appropriate, and explain the proposed resolution to the borrower. Evidence is collected. It is rarely connected. The SFS lives in one system. The decision lives in another. Approval correspondence lives in email. The policy version that was in force lives in a content store. Reconstructing the full chain for one case can take hours; for a sample of 100 cases under thematic review, it can take weeks.

**The accountability-drift problem.** Decision-making in regulated environments has migrated, often quietly, into AI-assisted tools. Banks and credit servicers now use models to triage cases, score affordability, suggest restructures, and even draft borrower communications. From 2 August 2026, the EU AI Act's high-risk obligations apply to AI systems that evaluate the creditworthiness of natural persons. Producing the deployer-side evidence those obligations require, consistently, across high-volume decision processes, is a meaningful operational challenge.

TraceLogic is designed to make decision evidence **cheaper, more reliable, and more defensible — produced as a side-effect of the governed decision process** rather than as a separate, after-the-fact reconstruction exercise.

---

## Product positioning

| Question | TraceLogic answer |
|---|---|
| **What is it?** | A deterministic decision governance engine with RAG-assisted intake and replayable evidence. |
| **What is it not?** | It is not a workflow tool, a CRM, a dashboard product, or an autonomous AI decision-maker. |
| **What does it solve?** | The evidence burden and governance ambiguity around consequential mortgage decisions. |
| **Pilot domain.** | Irish mortgage forbearance and loan modification decisioning. |
| **Architectural commitment 1** | Determinism — same inputs produce the same outputs, every time. |
| **Architectural commitment 2** | Evidence integrity — every decision is captured as an immutable artifact with a tamper-evident integrity stamp. |
| **Architectural commitment 3** | Human accountability — a manager who is not the operator must attest the decision, and execution is hash-bound. |

A more detailed positioning document is in [`docs/tracelogic-overview.md`](docs/tracelogic-overview.md).

---

## TraceLogic's eight governance cores

The product's value is built on eight interlocking control points. Each one is a real backend control, not a marketing surface. Each is documented in [`docs/governance-architecture.md`](docs/governance-architecture.md).

| # | Governance core | What it does |
|---|---|---|
| 1 | **Authenticated tenant isolation** | Tenant identity is read from the user's authenticated session — never accepted from the client. Tenant isolation guards run at every governance route. |
| 2 | **Role-based access control** | Operator, manager, reviewer, and admin roles are enforced on every route. The user's role context determines what they can see and do. |
| 3 | **Deterministic policy evaluation** | The decision is produced by a versioned, transparent rule engine. Every numeric rule emits a boundary record showing actual value, threshold, distance to threshold, and proximity flag. |
| 4 | **Immutable decision artifact with integrity stamp** | The artifact is the system of record. It carries a canonical hash computed at creation time. Mutation invalidates downstream controls. |
| 5 | **Manager review and attestation (separation of duties)** | The user who creates a proposal cannot be the manager who attests it. Separation is enforced at the application layer, not by convention. |
| 6 | **Single-use, time-limited execution tokens (hash-bound)** | Execution requires a token minted at attestation, bound to the artifact's hash. Single-use, time-limited; rejected on hash mismatch. |
| 7 | **Proposal governance gate (execution readiness)** | A technically approved decision can still be blocked from execution if borrower acceptance, court/ISI confirmation, or other proposal evidence is missing. |
| 8 | **Replay from frozen evidence** | Decisions are replayed against the evidence captured at decision time, not against live systems. Drift between original and replay is measurable. |

---

## Governance architecture overview

TraceLogic is delivered as **two services with a strict trust boundary** between them.

```
                  ┌─────────────────────────────┐
                  │       Browser / Operator      │
                  │       Browser / Manager       │
                  └──────────────┬──────────────┘
                                 │  HTTPS, JWT
                                 ▼
                  ┌─────────────────────────────┐
                  │       Public gateway          │
                  │  (UI, intake, ML pipeline,    │
                  │   feedback, authenticated     │
                  │   proxy to internal engine)   │
                  └──────────────┬──────────────┘
                                 │  Internal Docker network only
                                 │  Authorization header forwarded
                                 │  X-Correlation-ID forwarded
                                 ▼
        ┌────────────────────────────────────────────────────┐
        │            Internal governance engine                │
        │                                                      │
        │   • Decision lifecycle state machine                  │
        │   • Deterministic policy evaluation                   │
        │   • Policy adapter (internal / external)              │
        │   • Immutable artifact store                          │
        │   • Manager review + attestation                      │
        │   • Hash-bound execution tokens                       │
        │   • Proposal governance gate                          │
        │   • Replay engine (frozen evidence only)              │
        │   • Regulatory timeline engine (CCMA deadlines)       │
        │   • Audit log                                         │
        │                                                      │
        │   No public ports. Browser cannot reach this service. │
        └────────────────────────────────────────────────────┘
```

The browser does not talk to the internal governance engine directly. All governance calls flow through the public gateway's authenticated proxy. The internal engine has no public ports.

A detailed architectural narrative is in [`docs/governance-architecture.md`](docs/governance-architecture.md).
A higher-resolution diagram is in [`diagrams/`](diagrams/).

---

## Demo screenshots

Redacted screenshots of the operator and manager experience are in [`screenshots/`](screenshots/). They are illustrative — synthetic data only, no real cases, no real customers, no real lender names.

| Screen | What it shows |
|---|---|
| `intake.png` | RAG-assisted intake screen with synthetic SFS upload and operator confirmation step |
| `manager-review.png` | Manager review queue with the governed artifact and approve / return controls |
| `execution-gate.png` | Execution-readiness check, including a proposal-governance-blocked scenario |
| `replay.png` | Replay output with original-vs-replay comparison and drift classification |
| `trust-dashboard.png` | Trust dashboard showing artifact status, evidence coverage, replay coverage, and policy provenance |

Real screenshots have not been embedded in this README to avoid them being indexed out of context. They are in the `screenshots/` directory with descriptive captions.

---

## Synthetic case examples

Four synthetic cases are included to illustrate the kind of decision the engine evaluates. They use fictional borrower names, fictional account numbers, fictional property details, and generic lender names. They are illustrative — they are not the policy library, they are not the rule engine, and they are not sufficient to reproduce TraceLogic's behaviour.

| File | Scenario |
|---|---|
| [`synthetic-cases/loan-modification-case.json`](synthetic-cases/loan-modification-case.json) | Standard rate reduction with affordability assessment |
| [`synthetic-cases/arrears-capitalisation-case.json`](synthetic-cases/arrears-capitalisation-case.json) | Arrears capitalisation with anti-stacking and warehouse-aware logic |
| [`synthetic-cases/term-extension-case.json`](synthetic-cases/term-extension-case.json) | Term extension within soft and hard limits |
| [`synthetic-cases/pia-case.json`](synthetic-cases/pia-case.json) | PIA-linked modification with court / ISI confirmation gate |

---

## High-level system architecture diagram

ASCII overview is above. Higher-resolution diagrams (PNG) are in [`diagrams/`](diagrams/):

- `governance-flow.png` — full decision lifecycle from submit to replay
- `evidence-replay-flow.png` — how replay reads from the frozen artifact, not from live systems
- `separation-of-duties.png` — operator / manager / reviewer / admin role separation across the lifecycle

---

## Short demo video link

A short walkthrough video showing the lifecycle end-to-end on a synthetic case is available here -> https://youtu.be/T2KwGhYSVwg

---

## Business use cases

The pilot domain is Irish mortgage forbearance and loan modification, but the underlying governance pattern applies to any high-impact decision in a regulated context. Adjacent use cases TraceLogic's architecture is designed to support:

- **Mortgage arrears and forbearance** — split mortgage, arrears capitalisation, term extension, interest-only relief, PIA routes (current pilot domain).
- **Buy-to-let mortgage decisioning** — same conduct framework, similar evidence pattern.
- **Retail credit and consumer lending decisions** — SFS-style affordability, MARP-style protections, same evidence shape.
- **Insurance claims pre-authorisation** — under enhanced consumer protection regimes, claims decisions need the same kind of evidence trail.
- **Pension and investment suitability decisions** — under MiFID II conduct rules.
- **AML and sanctions de-risking decisions** — wrongful exit decisions create conduct risk; the same artifact pattern applies.

A more detailed walkthrough of the pilot scope and business positioning is in [`docs/pilot-scope.md`](docs/pilot-scope.md).

---

## Regulatory alignment summary

TraceLogic is designed against the following frameworks. **Alignment with themes is not the same as certification under any of them.** Each framework's full mapping is in [`docs/iso-42001-alignment.md`](docs/iso-42001-alignment.md) and [`docs/regulatory-context-ireland.md`](docs/regulatory-context-ireland.md).

| Framework | Relationship |
|---|---|
| **ISO/IEC 42001:2023** (AI Management System) | Designed against; alignment, not certification. |
| **EU AI Act** (Annex III §5(b) high-risk creditworthiness systems) | Designed to support a deploying firm's evidence obligations under Articles 9, 10, 12, 13, 14, and 26. The deploying firm — not TraceLogic — remains the deployer. |
| **CBI Consumer Protection Code 2025** (in force 24 March 2026) | The pilot domain is configured against MARP / CCMA timeline expectations now embedded in CPC 2025 Part 3 Chapters 9–10. |
| **Code of Conduct on Mortgage Arrears (CCMA)** | Timeline engine carries the 5/21/28/5/40-day deadline windows as first-class objects. |
| **NIST AI Risk Management Framework** (Govern, Map, Measure, Manage) | Optional broader alignment frame; not a certification claim. |
| **IAPP AIGP Body of Knowledge** | Used to organise the governance narrative; not a certification of the product. |
| **GDPR (Articles 5, 13, 25, 30)** | Privacy-by-design implementation; tenant isolation, redaction-aware access, records-of-processing patterns. |
| **DORA** | Architecture is designed to fit cleanly into a deploying firm's DORA evidence pack. |

---

## Repository structure

```
tracelogic-governance-showcase/
│
├── README.md                          ← this file
├── LICENSE.md                         ← documentation licence (CC BY-NC 4.0)
│
├── docs/
│   ├── tracelogic-overview.md         ← product narrative for non-technical readers
│   ├── governance-architecture.md     ← the eight cores in detail
│   ├── pilot-scope.md                 ← controlled mortgage pilot scope
│   ├── regulatory-context-ireland.md  ← CPC 2025, CCMA, EU AI Act context
│   ├── iso-42001-alignment.md         ← thematic alignment, not certification
│   └── demo-script.md                 ← step-by-step walkthrough on a synthetic case
│
├── screenshots/
│   ├── README.md                      ← caption legend
│   ├── intake.png                     ← (placeholder — see notes)
│   ├── manager-review.png
│   ├── execution-gate.png
│   ├── replay.png
│   └── trust-dashboard.png
│
├── synthetic-cases/
│   ├── README.md                      ← what these cases are and are not
│   ├── loan-modification-case.json
│   ├── arrears-capitalisation-case.json
│   ├── term-extension-case.json
│   └── pia-case.json
│
└── diagrams/
    ├── README.md                      ← diagram captions
    ├── governance-flow.svg
    ├── evidence-replay-flow.svg
    └── separation-of-duties.svg
```

---

## What is **not** included in this repository

This repository is deliberately bounded. It does **not** include, and will never include:

- The TraceLogic source code (public gateway or internal governance engine)
- Infrastructure configuration, deployment topology, or hosting provider details
- Database schemas, connection strings, or storage details
- Internal endpoint specifications beyond the high-level lifecycle
- Authentication implementation, JWT signing details, or session handling
- The deterministic policy engine's full rule library or threshold values
- Any real customer data, account references, lender names, or loan details
- Any reference to a current or former employer
- Anything that exposes how to clone or rebuild the product
- Secrets, `.env` files, Docker Compose credentials, API keys, or database URLs

If something in this repository inadvertently approaches that line, please open an issue (or, for security-sensitive items, contact the maintainer privately) and it will be removed.

---

## Contact

For pilot conversations, employment evaluation, investor briefings, or collaborator discussions:

- **Open a GitHub Issue** for general questions about the public showcase
- **Reach out privately** for the demo video, the more detailed pilot scope memo, or a controlled walkthrough — contact details on the maintainer's GitHub profile

---

## Licence

Documentation and diagrams in this repository are licensed under **Creative Commons Attribution-NonCommercial 4.0 (CC BY-NC 4.0)**. See [`LICENSE.md`](LICENSE.md).

This licence covers the documentation and diagrams only. The TraceLogic source code, policy engine, and runtime artifacts are not included in this repository and are not licensed under this licence.

---

*Last updated: May 2026 — TraceLogic controlled mortgage pilot stage.*
