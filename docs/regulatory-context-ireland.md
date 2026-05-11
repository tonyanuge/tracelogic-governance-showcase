# TraceLogic — Regulatory Context (Ireland)

> The Irish regulatory backdrop the TraceLogic pilot domain operates within.

---

## Pilot-stage disclaimer

This document describes the regulatory context the TraceLogic pilot domain is designed to operate within. **It is not a legal opinion**, it is not a claim of compliance with any regulation, and it is not a claim of regulator approval. Legal classification of a deploying firm's specific use case requires a formal legal review, which TraceLogic does not provide.

---

## Why this context matters

TraceLogic's pilot domain — Irish mortgage forbearance and loan modification decisioning — sits within a dense, evolving regulatory environment. Two regulatory landmarks frame the operating environment in 2026:

- The **Central Bank of Ireland's Consumer Protection Code 2025** came into force on 24 March 2026.
- The **EU AI Act's high-risk obligations** apply from 2 August 2026, and Annex III §5(b) explicitly classifies AI used to evaluate the creditworthiness of natural persons as high-risk.

Both regimes raise expectations for deterministic logic, human oversight, traceable evidence, and replayable decisions. TraceLogic is designed to support a deploying firm in producing the kind of decision evidence both regimes will require.

---

## Consumer Protection Code 2025 (CPC 2025)

### What it is

CPC 2025 is the Central Bank of Ireland's revised Consumer Protection Code. It replaces the 2012 Code and folds the previously separate Code of Conduct on Mortgage Arrears (CCMA) into Part 3, Chapters 9 and 10.

### Key dates

- Published: 2025
- In force: **24 March 2026**

### What changed

CPC 2025 retains the core Mortgage Arrears Resolution Process (MARP) architecture and introduces several enhancements:

- **Securing customers' interests.** Firms are expected to demonstrate they have considered customer outcomes proportionately, not just procedurally.
- **Vulnerable circumstances.** Explicit guidance on staff training and the identification of customers at risk of financial abuse.
- **Effective informing.** Firms are expected to provide clear, timely, comprehensible information at decision points.
- **Standards for Business.** Replace and strengthen the General Principles, with firms expected to demonstrate they act with integrity, exercise due care, and operate in customers' best interests.
- **Enhanced borrower-in-arrears disclosures.** References must now include the Insolvency Service of Ireland and warnings about non-cooperation impact on Personal Insolvency Arrangement eligibility.

Each obligation raises the same regulatory question at the level of the individual case: *how do you evidence that you did the right thing?*

### How TraceLogic supports the deploying firm

- The decision artifact is the per-case evidence record.
- Manager review and attestation provide the human-oversight trail.
- The proposal governance gate enforces that borrower acceptance, court / Insolvency Service of Ireland confirmation, and other proposal evidence are captured before execution.
- The replay engine reconstructs decision evidence on demand.

---

## Code of Conduct on Mortgage Arrears (CCMA) timelines

CCMA timelines are now embedded in CPC 2025 Part 3, Chapters 9 and 10. TraceLogic's regulatory timeline engine carries the CCMA deadline set as first-class objects.

| Deadline ID | Window (days) | Trigger → completion |
|---|---|---|
| CCMA-DL-001 | 5 | Borrower SFS notification → SFS return |
| CCMA-DL-002 | 21 | SFS received → SFS assessment complete |
| CCMA-DL-003 | 28 | SFS assessment complete → ARA offer issued |
| CCMA-DL-004 | 5 | Appeal lodged → appeal acknowledged |
| CCMA-DL-005 | 40 | Appeal lodged → Appeal Board decision |

Each deadline is reported as pending, met, breached, or not-applicable.

The deadline windows above reflect the CCMA framework in force at the time of writing. Windows can be subject to amendment under supervisory updates. Firms must verify the operative windows against current guidance from the Central Bank of Ireland.

---

## EU AI Act

### What it is

The EU AI Act (Regulation 2024/1689) is the European Union's risk-based framework for the regulation of artificial intelligence systems. It entered into force on 1 August 2024.

### Key dates

- Entered into force: **1 August 2024**
- General-purpose AI obligations applied: **2 August 2025**
- High-risk obligations apply: **2 August 2026**

### Annex III §5(b)

Annex III §5(b) explicitly classifies as high-risk any AI system intended to be used to evaluate the creditworthiness of natural persons or establish their credit score, with a narrow exception for AI systems used for the purpose of detecting financial fraud.

This applies to:

- Traditional credit scoring models that use machine learning
- Affordability assessment tools that score Standard Financial Statement data
- Restructure recommendation engines that suggest forbearance arrangements
- Triage tools that route borrower communications based on customer attributes
- General-purpose models repurposed for creditworthiness (Article 25(1)(b) reclassification)

### Deployer obligations

The deployer is the firm that puts a high-risk AI system into use. Deployer obligations are substantial and apply even when the AI system is supplied by a third party — a vendor's certification does not discharge the deployer's evidence duty.

| Article | Theme | What it means in practice |
|---|---|---|
| Art. 9 | Risk management | Documented, continuous lifecycle risk management |
| Art. 10 | Data governance | Training/validation/test data must be relevant and representative |
| Art. 12 | Logging | Automated record-keeping over the AI system's operation |
| Art. 13 | Transparency | Information enabling deployers to interpret outputs |
| Art. 14 | Human oversight | Effective human oversight during use |
| Art. 26 | Deployer obligations | Use per instructions; monitor performance; retain logs; FRIA where applicable |
| Art. 27 | Fundamental Rights Impact Assessment | Required for certain deployers under Annex III §5(b) and (c) |

### How TraceLogic supports the deploying firm

TraceLogic is a **deployer-support tool**, not a substitute for deployer obligations. The deploying firm remains the deployer.

- **Article 12 (logging).** The immutable artifact + audit log provide automated, structured records.
- **Article 13 (transparency).** Decision-level rule path, hard stops, and breaches are surfaced to the operator and manager.
- **Article 14 (human oversight).** The mandatory review-and-attest lifecycle, with separation of duties, provides the human oversight gate.
- **Article 26 (deployer).** Audit log, replay capability, and evidence pack export support a deployer's logging, monitoring, and record-retention obligations.

### A note on the Digital Omnibus

A Digital Omnibus proposal circulated in November 2025 may shift parts of the high-risk timetable to December 2027 by linking application to the availability of harmonised standards. As of the date of this document, that proposal is not law and prudent firms are planning against the 2 August 2026 date.

---

## Other relevant frames

### Digital Operational Resilience Act (DORA)

DORA has been fully applicable since 17 January 2025. It covers ICT risk management, third-party risk registers, incident reporting, and digital resilience testing for EU-regulated financial entities. TraceLogic's deterministic logging, immutable artifacts, and audit trail are designed to fit into a deploying firm's DORA evidence pack.

### European Banking Authority (EBA) Guidelines on Loan Origination and Monitoring

EBA Guidelines on Loan Origination and Monitoring (2020) cover credit lifecycle governance, creditworthiness assessment, internal governance, monitoring, and model governance themes. TraceLogic's deterministic policy evaluation, decision evidence, and human oversight controls map naturally to these themes — alignment, not certification.

### General Data Protection Regulation (GDPR)

TraceLogic's privacy posture supports a deploying firm in meeting GDPR obligations — particularly Articles 5 (data minimisation), 13 (right to information), 25 (privacy by design), and 30 (records of processing). Tenant isolation, redaction-aware access patterns, and the audit log support these obligations.

The deploying firm remains the data controller and remains responsible for its own GDPR compliance. A Data Protection Impact Assessment is identified as a prerequisite for any pilot involving real customer data.

---

## Sources

These are non-exhaustive. They are public sources cited for context. They are not certifications and they do not constitute legal advice.

- **Central Bank of Ireland — Consumer Protection Code 2025**
  https://www.centralbank.ie/regulation/consumer-protection/consumer-protection-code

- **Central Bank of Ireland — Mortgage Arrears statistics**
  https://www.centralbank.ie/statistics/data-and-analysis/credit-and-banking-statistics/mortgage-arrears

- **EU AI Act — official text and Annex III**
  https://artificialintelligenceact.eu/annex/3/
  https://ai-act-service-desk.ec.europa.eu/en/ai-act/annex-3

- **European Commission — Navigating the AI Act / Digital Strategy FAQ**
  https://digital-strategy.ec.europa.eu/en/faqs/navigating-ai-act

- **European Banking Authority — Guidelines on Loan Origination and Monitoring**
  https://www.eba.europa.eu/regulation-and-policy/credit-risk/guidelines-on-loan-origination-and-monitoring

- **DORA — Digital Operational Resilience Act**
  https://www.eiopa.europa.eu/digital-operational-resilience-act-dora_en

- **GDPR — General Data Protection Regulation**
  https://gdpr-info.eu/

- **MABS — Mortgage Arrears Resolution Process**
  https://mabs.ie/blogs/what-is-the-mortgage-arrears-resolution-process-marp/

---

*See also:* [`tracelogic-overview.md`](tracelogic-overview.md), [`governance-architecture.md`](governance-architecture.md), and [`iso-42001-alignment.md`](iso-42001-alignment.md).
