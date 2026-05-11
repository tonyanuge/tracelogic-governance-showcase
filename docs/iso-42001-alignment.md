# TraceLogic — ISO/IEC 42001 Alignment

> Thematic alignment of TraceLogic's design with governance frameworks. **Alignment is not certification.**

---

## Pilot-stage disclaimer

This document maps TraceLogic's observed governance design to themes drawn from ISO/IEC 42001:2023 and other recognised governance frameworks. It is **not a certification statement**, **not a legal opinion**, and **not evidence of full compliance**. No formal management-system audit has been undertaken under the current controlled mortgage pilot.

This document is suitable for stakeholder explanation, technical evaluation, and portfolio discussion where claims must remain truthful and evidence-based.

---

## What "alignment" means in this document

In this document, **alignment** means: TraceLogic has been designed against themes drawn from a recognised governance framework, and the design discipline reflects those themes.

It does **not** mean:

- Certification under that framework
- Audit-readiness in absolute terms
- Compliance with the framework as a legal matter
- Endorsement by the framework's standards body or any regulator

The strongest position TraceLogic can honestly take is: *the system is aligned to ISO/IEC 42001 themes such as human oversight, separation of duties, immutable decision evidence, and replay from frozen artifacts. Alignment is meaningful design discipline, but it is not the same as certification, which would require a formal management-system audit.*

---

## Reference frames

| Reference frame | Relevant focus | How TraceLogic uses it |
|---|---|---|
| **ISO/IEC 42001:2023** | AI management system requirements covering establishment, implementation, maintenance, and continual improvement of an AI management system | Primary governance management-system alignment frame |
| **IAPP AIGP Body of Knowledge** | AI governance professional knowledge area covering AI foundations, laws and frameworks, governance and risk management practices | Used to explain the professional governance logic behind TraceLogic controls |
| **EU AI Act** | Risk-based AI regulation with themes including transparency, record-keeping, human oversight, accuracy, robustness, and cybersecurity for high-risk systems | Used as a thematic mapping; legal classification of any specific deployment requires formal review |
| **NIST AI Risk Management Framework** | Govern, Map, Measure, Manage functions for trustworthy AI risk management | Used as an optional broader AI risk frame, not as a certification claim |
| **CBI CCMA + CPC 2025** | Mortgage arrears conduct framework and borrower protection process for the Irish jurisdiction | Used as the domain context for the mortgage forbearance pilot |
| **EBA Loan Origination Guidelines** | Credit lifecycle governance, creditworthiness, internal governance, monitoring, model governance | Used as supporting financial services governance context |

---

## Control-to-framework mapping

The table below maps observed TraceLogic backend controls to themes drawn from the frameworks above. Each row reflects a control that is implemented at the pilot baseline; each is presented as evidence-based explanation, not certification.

### Tenant isolation and authenticated access

- **ISO 42001 themes:** organisational controls; responsibilities; operation
- **AIGP themes:** governance accountability and risk control
- **EU AI Act themes:** governance and accountability themes

The backend uses authenticated access and tenant-aware handling to prevent cross-tenant access and preserve accountability. Tenant identity is read from the user's authenticated session — never from the client request body.

### Deterministic policy evaluation

- **ISO 42001 themes:** AI system operation and control
- **AIGP themes:** AI lifecycle governance
- **EBA themes:** credit decision governance and monitoring

Policy outcomes, rule path, reasons, constraints, and required-evidence outputs support explainability and repeatability. The same inputs produce the same outputs every time.

### Immutable decision artifact

- **ISO 42001 themes:** operational evidence and performance evaluation
- **EU AI Act themes:** logging and record-keeping
- **NIST AI RMF:** Measure and Manage

The artifact preserves the evaluated decision state and supports replay and audit review. Critical fields are enforced at three independent points: at build time, at propose time, and at attest time.

### Artifact integrity stamp

- **ISO 42001 themes:** control of documented information
- **NIST AI RMF:** risk monitoring and integrity controls

The design supports tamper-evident evidence under server-side custody. Language discipline: this is described as a "tamper-evident integrity stamp" or "canonical hash verification" — not as "cryptographic proof", "publicly verifiable", or "non-repudiable".

### Manager review and attestation

- **ISO 42001 themes:** roles, responsibilities, human oversight
- **EU AI Act themes:** human oversight (Article 14)
- **AIGP themes:** human accountability

A human reviewer approves the governed artifact before execution authority is minted. The user who created the proposal cannot be the user who attests it.

### Separation of duties

- **ISO 42001 themes:** accountability and control
- **AIGP themes:** governance control design
- **Financial services themes:** internal control principles

Creator, reviewer, and executor responsibilities are separated to reduce self-approval risk. Separation is enforced at the application layer, not by convention.

### Execution token control

- **ISO 42001 themes:** operational control
- **NIST AI RMF:** Manage
- **EU AI Act themes:** robustness and traceability

Execution requires a token linked to the frozen artifact. Tokens are single-use, time-limited, and rejected on expiry, on prior use, or on artifact-hash mismatch. Application-layer reuse rejection is in place; database-level race-safety against simultaneous duplicate authorisation remains a verification item per the Documentation Pack Index.

### Proposal governance gate

- **CCMA / MARP themes:** domain-specific borrower-protection logic
- **EBA themes:** lifecycle governance
- **ISO 42001 themes:** operational controls

The gate can block execution where borrower acceptance, PIA confirmation, court / Insolvency Service of Ireland confirmation, or "No ARA offered" conditions mean execution should not proceed.

### Replay from frozen evidence

- **ISO 42001 themes:** monitoring, evaluation, records
- **EU AI Act themes:** logging and transparency
- **AIGP themes:** documentation and auditability

Replay reconstructs a decision from stored evidence rather than re-running live policy or external systems. This is locked in as a non-negotiable design principle: replay must not call live external systems.

### Retrieval-augmented intake (with operator review)

- **AIGP themes:** data governance and AI lifecycle risk
- **ISO 42001 themes:** data quality and AI system inputs
- **EU AI Act themes:** data governance

Document extraction reduces manual entry. Operator review and deterministic governance remain the controlling safeguards. RAG is not the decision-maker.

### Correlation identifier propagation

- **ISO 42001 themes:** traceability and incident handling support
- **NIST AI RMF:** monitoring
- **AIGP themes:** operational governance

Correlation identifiers make request chains easier to investigate across services and across the audit log.

---

## Eight-cores cross-reference to ISO/IEC 42001 themes

The eight governance cores documented in [`governance-architecture.md`](governance-architecture.md) are cross-referenced here to ISO/IEC 42001 themes — for stakeholder convenience, not as a certification claim.

| Core | Theme |
|---|---|
| 1. Authenticated tenant isolation | Organisational controls; responsibilities |
| 2. Role-based access control | Roles and responsibilities; human oversight |
| 3. Deterministic policy evaluation | AI system operation and control |
| 4. Immutable artifact + integrity stamp | Operational evidence; control of documented information |
| 5. Manager review and attestation (separation of duties) | Roles, responsibilities, human oversight; accountability |
| 6. Single-use, hash-bound execution tokens | Operational control; robustness |
| 7. Proposal governance gate | Operational control; domain compliance |
| 8. Replay from frozen evidence | Monitoring, evaluation, records |

---

## What we will not say

To preserve the integrity of the alignment claim, the following statements are **not** made about TraceLogic:

- *"TraceLogic is ISO/IEC 42001 compliant."* It is not. Alignment is not compliance.
- *"TraceLogic is ISO/IEC 42001 certified."* It is not. There is no certification.
- *"TraceLogic is certification-ready."* No formal readiness assessment has been completed.
- *"TraceLogic is audit-ready."* The design supports that goal but no readiness assessment has been completed.
- *"TraceLogic is regulator-approved."* No regulator has reviewed or approved TraceLogic.
- *"TraceLogic is legally compliant with the EU AI Act / CCMA / CPC 2025."* Legal classification of any specific deployment requires formal legal review.
- *"TraceLogic provides cryptographic proof / publicly verifiable evidence / non-repudiable records."* The integrity stamp is described as "tamper-evident under server-side custody" or "canonical hash verification" — and no further.

These language disciplines are not cosmetic. A careful interviewer, regulator, or auditor will challenge an overclaim. The strongest signal a product can give in a regulated context is the willingness to bound its claim to what is actually evidenced.

---

## Outstanding alignment work

A short list of items identified as next-priority for further alignment maturity, drawn from the Documentation Pack Index:

1. **Data retention and evidence-handling policy.** Required because the evidence store, document ingestion, audit logs, and user feedback records need clear retention rules. *(Drafted; should be reviewed against legal advice before production use.)*
2. **Information-security and secrets-handling policy.** Documents environment variables, token secrets, database credentials, key rotation, and operational controls.
3. **Model and policy change-control procedure.** Policy changes, adapter changes, and schema changes need to be reviewed, versioned, approved, and regression-tested.
4. **Incident and exception management procedure.** For execution failures, replay gaps, ingestion failures, data-quality failures, and suspected unauthorised access.
5. **User roles and access-control matrix.** Documents operator, manager, reviewer, admin, and tenant-scoped permissions.
6. **Privacy / data-protection impact assessment.** Required before handling real personal data in any pilot.
7. **Vendor and external-policy-adapter assurance note.** If external-decision tenants or third-party policy services remain in scope.
8. **Testing and regression evidence pack.** To prove key controls are tested, including tenant isolation, separation of duties, token reuse, artifact-hash mismatch, and replay integrity.

These are not blockers to a controlled pilot. They are the documents that need to exist before the alignment posture moves from "designed against the themes" to "ready for a formal management-system assessment".

---

## References

- **International Organization for Standardization (2023).** *ISO/IEC 42001:2023 Artificial Intelligence Management System.* Geneva: ISO.
- **International Association of Privacy Professionals (2025).** *AIGP Body of Knowledge and Exam Blueprint.* Portsmouth, NH: IAPP.
- **European Parliament and Council (2024).** *Regulation (EU) 2024/1689 laying down harmonised rules on artificial intelligence.* Official Journal of the European Union.
- **National Institute of Standards and Technology (2023).** *Artificial Intelligence Risk Management Framework 1.0.* Gaithersburg, MD: NIST.
- **European Banking Authority (2020).** *Guidelines on Loan Origination and Monitoring.*
- **Central Bank of Ireland (2025).** *Consumer Protection Code 2025.* Dublin: CBI.
- **OECD (2019).** *OECD Principles on Artificial Intelligence.*

---

*See also:* [`governance-architecture.md`](governance-architecture.md), [`tracelogic-overview.md`](tracelogic-overview.md), and [`regulatory-context-ireland.md`](regulatory-context-ireland.md).
