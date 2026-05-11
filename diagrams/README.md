# Diagrams

Three SVG diagrams illustrating TraceLogic's high-level architecture and governance flow. SVG was chosen so they render at any resolution and can be embedded cleanly in documentation, slide decks, and the README itself.

## What's here

### `governance-flow.svg`

The full decision lifecycle, from `submit → propose → review → attest → token mint → execute → replay`. Each stage is annotated with the role that owns it and the effect on the artifact. The two stages most distinctive to TraceLogic relative to typical workflow tools — **attest** and **replay** — are highlighted. The eight governance cores are listed in the footer of the diagram for cross-reference.

### `evidence-replay-flow.svg`

The single most important assurance property of the replay engine: **replay reads from frozen evidence, never from live systems**. The diagram shows the flow from original decision (T0) to the frozen artifact, to the allowed replay path (reading from artifact and case state stores), to the explicitly disallowed path (live systems). The replay output — the replay bundle — is shown in detail.

### `separation-of-duties.svg`

A swimlane view of the four roles (Operator, Manager, Reviewer, Admin) across the seven lifecycle stages. Each cell shows whether that role is permitted to take that action at that stage. The separation-of-duties property at the **attest** step — that the user who attests cannot be the user who created the proposal — is highlighted in a callout box.

## How to use these

- Embed in slide decks (SVG renders cleanly at any resolution)
- Reference from the README and the docs in this repository
- Use as background reference during a stakeholder conversation
- Print as A3 / poster reference for an internal session

## What these are *not*

- These are **architectural** diagrams, not implementation diagrams. They show the design intent at the pilot baseline.
- They do not show internal data structures, schemas, or code.
- They do not show infrastructure topology (clouds, regions, VPCs, subnets) — that information is private to the deployment.
- They are not certified or audit-stamped artifacts. They illustrate the design discipline; they do not evidence compliance.

## Pilot-stage disclaimer

These diagrams describe the design at the **controlled mortgage pilot stage**. They are not certifications, not legal opinions, and not regulator-approved artifacts.
