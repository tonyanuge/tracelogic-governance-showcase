# TraceLogic — Governance Architecture

> The eight governance cores, the two-service architecture, and the decision lifecycle.

---

## Pilot-stage disclaimer

This document describes TraceLogic's design intent and architectural commitments at its **controlled mortgage pilot stage**. It does not describe production-validated certification status under any standard. Alignment with framework themes is not the same as certification. Implementation references describe controls present at the pilot baseline; race-safety, edge-case behaviour, and full audit-grade evidence remain subjects for ongoing verification.

---

## Two-service architecture

TraceLogic is delivered as **two services with a strict trust boundary** between them.

### Public gateway (browser-facing)

The public gateway hosts the operator and manager user interfaces, exposes the document intake and machine-learning ingestion endpoints, exposes a feedback endpoint for support, and proxies authenticated requests forward to the internal governance engine. It is the only service the browser ever talks to. The public gateway forwards the user's authentication context (the JWT in the `Authorization` header) and a correlation identifier (`X-Correlation-ID`) to the internal engine on every governance call.

### Internal governance engine (no public ports)

The internal governance engine owns the decision lifecycle state machine, the deterministic policy evaluation, the policy adapter resolution, the immutable artifact store, the replay engine, the regulatory timeline engine carrying the CCMA deadlines, and the audit log. **It has no public ports.** The browser cannot reach this service. The host network cannot reach this service. Only the public gateway can reach it, and only over the internal Docker network.

This isolation pattern keeps the governance core small, auditable, and free of public attack surface. It is documented in the source repository's `docker-compose.yml`, where the public service exposes a port to the host and the internal service does not.

---

## The decision lifecycle

The full lifecycle moves through seven stages:

```
submit → propose → review → attest → token mint → execute → replay
```

| Stage | Owner role | Effect |
|---|---|---|
| **Submit** | operator | Runs the policy engine, creates an immutable artifact, captures the user as the originator. |
| **Propose** | operator | Validates artifact completeness; transitions the artifact to a pending-review state. |
| **Review** | manager / reviewer | Approves or returns the proposal; stamps the reviewer identity, notes, and time. |
| **Attest** | manager / reviewer (≠ creator) | Enforces separation of duties; recomputes the immutable execution snapshot; mints a single-use, time-limited token bound to the artifact's hash. |
| **Token retrieval** | any authenticated role | Hydrates the token panel; checks tenant scope, expiry, and used-flag. |
| **Execute** | operator (or other authorised role) | Re-validates the token, recomputes the artifact hash, refuses execution on drift, marks the token used, builds the execution record. |
| **Replay** | authenticated user (per role) | Re-runs the original request against the frozen evidence captured at decision time; classifies any drift between original and replay outcomes. |

Every stage writes to the audit log with the actor's identity, the action, the target tenant, and metadata.

---

## The eight governance cores

The product's governance value is built on eight interlocking control points. Each is a real backend control, not a marketing surface. They are described below in the order they are exercised in a typical case.

### Core 1 — Authenticated tenant isolation

**What it is.** Tenant identity is always read from the user's authenticated session — never accepted from the client request body. Every governance route runs a tenant consistency check before reading or writing artifacts.

**Why it matters.** Most data-isolation failures in multi-tenant SaaS happen because tenant identity was provided by the caller and trusted. TraceLogic does not trust caller-supplied tenant identity.

**Where it shows up.** At every governance route entry; in every artifact read and write; in every audit log entry.

**Pilot maturity.** Implemented as a backend control at the pilot baseline. Independent isolation testing remains a pilot-progression item.

### Core 2 — Role-based access control

**What it is.** Operator, manager, reviewer, and admin roles are derived from the user's authentication context. Routes enforce the role context required for that action — for example, the attestation route requires `reviewer` or `manager` and the execution route requires the appropriate authorised role.

**Why it matters.** Without role enforcement, every governance gate becomes an honour system. With role enforcement, the lifecycle's separation properties are mechanically defended.

**Where it shows up.** On every protected route through the standard FastAPI `Depends(require_role(...))` pattern.

**Pilot maturity.** Implemented at the pilot baseline. The full role-and-permission matrix is recommended next-step documentation, per the Documentation Pack Index.

### Core 3 — Deterministic policy evaluation

**What it is.** The decision is produced by a versioned, transparent rule engine. The engine takes a normalised decision request plus a tenant-aware policy dictionary and returns a structured result containing:

- An overall status (approve, needs-override, or reject)
- Hard stops (disqualifying breaches)
- Breaches (soft warnings)
- A rule path (an ordered list of every rule evaluated)
- Boundary analysis for every numeric rule (actual value, threshold value, distance, proximity flag)
- Computed values used in the evaluation

**Why it matters.** Determinism is the foundation. Without it, replay is not meaningful, evidence is not stable, and human oversight has no anchor.

**What it is not.** It is not a black-box score. It is not a probabilistic model with explainability layered on top. It is not "AI deciding the case".

**Pilot scope.** Coverage at the pilot baseline includes standard rate reduction, term extension, arrears capitalisation (with anti-stacking and warehouse-aware logic), split mortgage (active + warehouse balance reconciliation), interest-only relief, and PIA routes.

### Core 4 — Immutable decision artifact with integrity stamp

**What it is.** Every governed decision produces a Decision Artifact carrying:

- Identity (artifact, case, tenant, request, timestamp)
- Provenance (policy version, policy mode, policy source, policy timestamp, correlation identifier)
- Inputs (the full normalised payload at decision time)
- Retrieval evidence (RAG-assisted intake outputs as captured at decision time)
- Decision output (status, hard stops, breaches, rule path, boundary analysis, governance block)
- Lifecycle stamps (proposal status, governance state, reviewer, executor)
- A canonical hash computed over the canonical hash payload at build time

**Why it matters.** The artifact is the system of record. The hash is a tamper-evident integrity stamp under server-side custody — if the artifact is mutated, downstream controls (especially execution) detect it.

**Language discipline.** The integrity stamp is described as "tamper-evident under server-side custody" or "canonical hash verification". It is *not* described as "cryptographic proof", "publicly verifiable", or "non-repudiable" — those words imply a stronger property than is currently evidenced.

### Core 5 — Manager review and attestation (separation of duties)

**What it is.** A manager or reviewer reviews the governed artifact and either approves or returns it. If approved, the same role then *attests* the artifact — a separate, explicit step that is the gate to execution-token minting.

**The separation-of-duties property.** The user who created the proposal cannot be the user who attests it. The attestation route checks the attester's identity against the artifact's `created_by` field and refuses if they match.

**Why it matters.** Self-approval is the most common conduct-risk surface in regulated decisioning. Mechanical separation removes the failure mode at the application layer.

**Pilot maturity.** The application-layer enforcement is in place at the pilot baseline.

### Core 6 — Single-use, time-limited execution tokens (hash-bound)

**What it is.** Attestation mints a token. The token carries:

- The artifact's hash at attestation time
- The tenant identifier
- The case identifier
- The policy version
- The runtime version
- An expiry timestamp
- A used-flag (false at mint time)

**At execution.** The execution route validates the token, recomputes the artifact's hash from current state, and refuses execution if:

- The token is expired
- The token is already used
- The recomputed hash does not match the hash on the token (artifact drift)

If execution proceeds, the token's used-flag is set.

**Why it matters.** This pattern prevents the most common decision-drift failure mode: someone edits the case after approval and then executes the old approval. The hash check catches it. The single-use property prevents replay attacks at the execution layer.

**Pilot maturity.** Implemented at the pilot baseline. Database-level race-safety against simultaneous duplicate authorisation is identified in the Documentation Pack Index as a verification item before final assurance claims.

### Core 7 — Proposal governance gate (execution readiness)

**What it is.** A separate execution-readiness gate, distinct from the decision approval, that can block execution where post-decision evidence is incomplete. Examples of conditions that block execution:

- Borrower acceptance is required after offer but has not been recorded
- A PIA route is in play but the court / Insolvency Service of Ireland / creditor confirmation has not been captured
- A "No ARA offered" state has been reached and execution would be inappropriate

**Why it matters.** A decision being technically approved is not the same as a decision being ready to execute. The proposal governance gate enforces this distinction. It treats borrower, legal, and proposal evidence as execution-readiness controls — not cosmetic fields.

**Where it shows up.** As an authoritative module in the internal engine, consulted at the execution route.

### Core 8 — Replay from frozen evidence

**What it is.** Any past decision can be re-run against the evidence captured at decision time. The replay engine reads from the artifact store and the case state store. **It does not call live external APIs, live policy systems, or live retrieval-augmented services.**

**What it produces.** A replay bundle containing the state history, the rules evaluated, the rule engine result, the policy result, the evidence snapshot, the decision status, the artifact hash, and the regulatory timeline state at original decision time.

**Drift classification.** The replay output is compared to the artifact's original output. Drift is classified into severity bands: none, minor, moderate, critical.

**Why it matters.** Replay is the most powerful trust-establishment exercise in the product. A regulator, ombudsman, or auditor asking "show me how this decision was made" can be answered with a deterministic re-run from the evidence the decision was made on. Live re-runs against current systems would not answer that question — the systems and the evidence have moved on.

**Pilot maturity.** The frozen-evidence-only replay principle is locked in at the pilot baseline. Verification that every Trust dashboard surface is fed exclusively from backend evidence (no placeholders) remains a pilot-progression item per the Documentation Pack Index.

---

## Adjacent supporting controls

These are not "the eight cores" but they support the cores in important ways.

### Retrieval-augmented intake

The intake pipeline accepts uploaded documents (PDF, DOCX, TXT) and uses retrieval-augmented techniques to surface candidate field values. **The operator confirms or corrects every value before submission.** No field is auto-populated without explicit operator confirmation. Retrieval-augmented techniques are an aid to intake — not a decision-maker.

### Policy adapter pattern

For tenants that prefer their own policy engine, an external policy adapter calls a tenant-configured policy API and normalises the response into the same shape used internally. Two important properties:

- **Fail-closed.** If the external policy call fails (timeout, server error, schema mismatch), the result is a reject with explicit reasoning, not a silent fallback to the internal engine.
- **Adapter-resolved.** The adapter is selected from tenant configuration; business logic does not branch on tenant identity.

### Regulatory timeline engine

The timeline engine evaluates events against deadline definitions. The Code of Conduct on Mortgage Arrears (CCMA) deadline set is configured for the Irish jurisdiction. Each deadline has a trigger event, a completion event, and a window in days. The engine reports each deadline as pending, met, breached, or not-applicable.

### Correlation identifiers and audit log

Every request carries a correlation identifier propagated across services. Every governance action — submit, propose, review, attest, token mint, execute, replay, tenant isolation incident, adapter failure, completeness failure — writes to the audit log with actor, action, tenant, target, and metadata.

### Trust dashboard

The trust dashboard surfaces, for an authenticated supervisory user, system status, governance controls, decisions processed, approvals, rejections, hard stops, breaches, drift distribution, single-case replay search, and regulatory mapping (EU AI Act, GDPR, ISO/IEC 42001 themes). Trust dashboard values must come from backend evidence — no placeholders, no invented values.

---

## What this architecture is *not*

The architecture is described above. To bound the claim explicitly, this architecture is not:

- **A guarantee of regulatory compliance.** The deploying firm remains the deployer and remains accountable for its own compliance.
- **An autonomous decision-making system.** Every decision moves through human review and attestation before execution.
- **A replacement for the firm's policy.** The firm's policy is encoded, versioned, and applied; the engine governs how it is applied, not what it is.
- **A black-box score.** Every numeric comparison is visible in the artifact and the operator UI.

---

*See also:* [`tracelogic-overview.md`](tracelogic-overview.md) for the non-technical introduction, [`pilot-scope.md`](pilot-scope.md) for the controlled pilot's boundary, and [`iso-42001-alignment.md`](iso-42001-alignment.md) for thematic mapping to governance frameworks.
