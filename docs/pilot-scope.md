# TraceLogic — Controlled Mortgage Pilot Scope

> What the pilot covers, what it does not cover, and how a pilot is run.

---

## Pilot-stage disclaimer

TraceLogic is at the **controlled mortgage pilot stage**. It is not certified, regulator-approved, audit-passed, or production-validated for general enterprise use. The boundary described in this document is an honest scope-of-work boundary, not a certification or commercial guarantee.

---

## Purpose of this document

This document explains what is in scope for a TraceLogic controlled mortgage pilot, what remains out of scope at this stage, and what the pilot lifecycle looks like. It is intended for prospective pilot clients, technical evaluators, and stakeholders deciding whether TraceLogic fits their decision-governance need.

---

## What is in scope at the pilot baseline

### Decision domain

The pilot covers **Irish mortgage forbearance and loan modification decisioning**. Specifically:

- Standard rate reduction
- Term extension (with soft and hard limits)
- Arrears capitalisation (with anti-stacking and warehouse-aware logic)
- Split mortgage (active and warehouse balance reconciliation)
- Interest-only relief
- Personal Insolvency Arrangement (PIA) routes (with court / Insolvency Service of Ireland confirmation gating)

### Governance lifecycle

The full lifecycle is exercised in a pilot:

1. **Submit.** Operator creates a case, optionally aided by retrieval-augmented intake from an uploaded synthetic document (Standard Financial Statement, valuation, proposed restructure template).
2. **Propose.** Operator submits the case for evaluation; the decision artifact is built; completeness checks run.
3. **Review.** A manager or reviewer (a different user from the operator) reviews the artifact and approves or returns.
4. **Attest.** The same manager or reviewer attests the approved artifact; the execution token is minted.
5. **Token retrieval.** The execution panel hydrates with the minted token's metadata.
6. **Execute.** An authorised user executes the decision; the artifact hash is rechecked; the token is consumed.
7. **Replay.** The decision is re-run against the frozen evidence; original-vs-replay drift is classified.

### Multi-tenant operation

Two tenant configurations are exercised at the pilot baseline:

- An **internal-policy tenant** using TraceLogic's deterministic policy engine
- An **external-decision tenant** using a tenant-configured external policy API via the adapter pattern, with fail-closed behaviour on external failure

Tenant isolation is enforced at every governance route. Tenant identity is always derived from the user's authenticated session.

### Trust evidence surfaces

The trust dashboard surfaces, for the supervisory user, decisions processed, approvals, rejections, hard stops, breaches, drift distribution, single-case replay search, and a thematic regulatory mapping. Values shown are sourced from the backend artifact store and audit log.

### Synthetic data only

A pilot at this stage uses synthetic data only. Fictional borrower names, fictional account references, fictional property details, and generic lender names. No real customer data, no real lender names, no real loan details.

---

## What is **not** in scope at the pilot stage

The boundary is bounded honestly. The following are out of scope at the controlled mortgage pilot:

### Certification claims

- No claim of ISO/IEC 42001 certification
- No claim of ISO/IEC 27001 certification
- No SOC 2 Type 1 or Type 2 attestation
- No claim of legal compliance with the EU AI Act, the Consumer Protection Code 2025, the Code of Conduct on Mortgage Arrears, or any other regulation
- No claim of regulator approval

The product is **designed against** the themes of these frameworks. Alignment is a meaningful design discipline. It is not the same as certification.

### Production maturity claims

- No claim of enterprise-production readiness across all markets and customer types
- No multi-region deployment posture promised at this stage
- No claim of universal multi-domain readiness — the pilot domain is Irish mortgage decisioning

### Real customer data

- The pilot does not handle real customer data without an explicit Data Processing Agreement, Data Protection Impact Assessment, and the relevant regulatory and contractual reviews. A privacy assessment is identified as a high-priority next document in the Documentation Pack Index before any real-data handling.

### Production integration claims

- The pilot does not include production-grade integration to a customer's core systems (origination, servicing, payments, ledger). That is a year-1 production engagement, not a pilot deliverable.
- The pilot does not include custom policy development beyond simple tenant configuration.

### Indemnities and legal-effectiveness claims

- The pilot does not indemnify the customer against their own regulatory obligations.
- The pilot does not provide legal effectiveness for the customer's own compliance position.

---

## Pilot lifecycle

A typical 90-day controlled pilot moves through five phases.

### Phase 1 — Setup (Days 1–14)

- Tenant provisioning on the pilot environment
- Policy adapter wiring (internal or external)
- JWT or single-sign-on configuration
- Synthetic data preparation
- Operator and manager training (separate sessions)

**Exit criterion:** an operator can run a synthetic case end-to-end through the full lifecycle.

### Phase 2 — Synthetic validation (Days 15–28)

- 25 to 50 synthetic cases run through the full lifecycle
- Replay tested on a known artifact set
- Trust dashboard reviewed

**Exit criterion:** zero hard-stop misses on synthetic cases; replay drift distribution is within expected bands.

### Phase 3 — Real-data validation (Days 29–60)

*(Subject to the customer's data-protection sign-off and any required Data Protection Impact Assessment.)*

- 50+ real customer cases (anonymised as required) run through the full lifecycle
- Side-by-side timing measurement against the customer's existing process

**Exit criterion:** at least 50 real cases successfully run end-to-end.

### Phase 4 — Parallel run (Days 61–82)

- Customer's existing process and TraceLogic operate in parallel
- Time, rework rate, and evidence completeness measured against the customer's baseline

**Exit criterion:** quantified time savings vs baseline.

### Phase 5 — Pilot report (Days 83–90)

- Pilot results write-up
- Conversion conversation with the economic buyer
- Decision on next steps (annual licence, extended pilot, or close-out)

---

## Pilot success metrics

Pilot performance is measured against the customer's actual baseline.

| Metric | Target movement |
|---|---|
| Time per case (operator + manager combined) | -40% to -60% |
| Rework rate | -50% to -70% |
| Audit pack preparation time per case | -90% or better |
| Replay drift on closed-case sample | 0% critical, ≤5% minor |
| Operator satisfaction (5-point scale) | ≥4.0 average |
| Manager confidence in evidence (5-point scale) | ≥4.5 average |

---

## Pilot risk and exit options

The pilot is structured so that either side can walk away cleanly:

- **TraceLogic can declare the pilot non-viable** at the end of Phase 1 (Day 14) if integration prerequisites are not met.
- **The customer can declare the pilot non-viable** at the end of Phase 2 (Day 28) if synthetic results do not match expectations — pro-rata refund of unused pilot fee.

This honest framing is intended to *increase* confidence. A pilot that allows either side to walk is a fairer commitment than one that locks in.

---

## Documentation that supports a pilot

A pilot conversation typically also references the following documents (not all of which are public):

- A pilot-readiness memo (consolidated charter)
- A pilot-operating manual (for operators and managers)
- A pilot-support runbook
- The data-retention and evidence-handling policy
- The governance and standards mapping
- A privacy assessment (Data Protection Impact Assessment) if real data is in scope

These documents extend beyond the public showcase. They are shared under appropriate confidentiality with active pilot clients.

---

## What a pilot is *not*

To be explicit:

- A pilot is not a sales pitch dressed as a pilot. The pilot has measurable success metrics and an honest exit option.
- A pilot is not a free engagement. It is a paid, scoped engagement with a defined deliverable.
- A pilot is not a commitment to multi-year enterprise contract. It is a 90-day window with a conversion conversation at the end.
- A pilot is not a regulatory blessing. It is a controlled exercise of the product on the customer's data.

---

*See also:* [`governance-architecture.md`](governance-architecture.md) for the technical foundations, [`regulatory-context-ireland.md`](regulatory-context-ireland.md) for the regulatory backdrop the pilot domain sits within, and [`demo-script.md`](demo-script.md) for what an end-to-end demo on a synthetic case looks like.
