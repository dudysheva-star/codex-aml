# AML Merchant Due Diligence Agent Playbook

## Purpose
This playbook defines the multi-agent collaboration required to deliver European AML merchant onboarding checks. It translates the provided business flow into concrete agent responsibilities, mandatory outputs, and supporting tools.

### Scope of a Case
* **Input dossier**: company legal name, website, UBOs, shareholders, directors.
* **Objective**: reach one of the following outcomes — Approve, Approve with Enhanced Due Diligence (EDD) monitoring, or Reject — and produce an audit-ready summary citing every check performed.
* **Geography**: European AML requirements, prioritising EU and UK sources.

## Agent Roster

### 1. Intake & Briefing Agent
* **Mission**: validate the dossier, prepare structured tasks for downstream agents.
* **Key actions**:
  - Confirm entity name spelling variants, registered jurisdiction, and primary industry keywords.
  - Expand personal names into search-ready variants (transliterations, maiden names if provided).
  - Identify regulatory triggers (high-risk industry, sanctions exposure, negative keywords) from the brief.
* **Tools**:
  - *Entity normaliser*: corporate registry or open data API for basic entity verification.
  - *Name variant generator*: library for transliteration/alias suggestions.
  - *Task tracker*: shared workspace (e.g., Notion, Jira, or YAML task file) for downstream instructions.
* **Deliverables**: structured case sheet with confirmed identifiers, risk flags, and task checklist. If dossier is incomplete, request clarification before work proceeds.

### 2. Open-Source Intelligence (OSINT) Agent
* **Mission**: collect publicly available information on the entity and principals.
* **Key actions**:
  - Perform broad web search queries combining entity name with risk keywords ("scam", "fraud", "regulator", etc.).
  - Visit the merchant website, capture business model description, payment methods, jurisdictions served, and compliance disclosures.
  - Review social media & professional networks (LinkedIn is mandatory) to validate leadership identities, employment history, and potential adverse signals.
  - Log each finding with URL, timestamp, and relevance tags.
* **Tools**:
  - *Web search connector*: Google/Bing Custom Search API with query templating.
  - *LinkedIn lookup*: automation respecting platform ToS (e.g., Sales Navigator API or manual workflow tracker).
  - *Web snapshotter*: saves page screenshots/PDFs for evidence.
* **Deliverables**: OSINT report summarising corporate profile, leadership bios, geographic footprint, and flagged items. Provide structured evidence links.

### 3. Sanctions & Compliance Screening Agent
* **Mission**: determine whether the entity or associated persons appear on sanctions, PEP, or watch lists.
* **Key actions**:
  - Screen entity and personal names against EU, UN, UK HMT, OFAC, and local sanctions databases.
  - Check for Politically Exposed Person (PEP) status using recognised datasets (World-Check, Dow Jones, or open PEP registries).
  - Verify if the merchant falls within regulated industries requiring licences (gambling, forex, crypto, etc.) and collect licence evidence when relevant.
* **Tools**:
  - *Sanctions screening engine*: supports fuzzy matching and generates hit/no-hit reports with scores.
  - *PEP database access*: commercial or open data interface.
  - *Licence registry connectors*: targeted scrapers/APIs per jurisdiction.
* **Deliverables**: screening summary detailing data sources, hits, resolution notes, and residual risk.

### 4. Adverse Media Analyst Agent
* **Mission**: deep-dive on potential negative news uncovered by OSINT or screening.
* **Trigger**: invoked when OSINT agent flags risk terms or screening produces near-hits.
* **Key actions**:
  - Conduct date-bounded media searches in multiple languages relevant to the entity's operations.
  - Classify media items by severity (e.g., criminal, regulatory, reputational) and evaluate credibility of sources.
  - Assess whether issues are resolved, ongoing, or allegations.
* **Tools**:
  - *Adverse media aggregator*: LexisNexis, Factiva, or open-source alternatives.
  - *Translation service*: for non-English content (DeepL, Google Translate) with retention of original text snippets.
* **Deliverables**: adverse media memo with severity ratings, mitigation evidence, and recommendation on whether to escalate to EDD.

### 5. Enhanced Due Diligence (EDD) Agent
* **Mission**: perform escalated checks when risk level is medium-high or unresolved issues remain.
* **Trigger conditions** (any):
  - High-risk jurisdiction or industry.
  - Unexplained corporate structure (complex shareholding, trusts, SPVs).
  - Negative media involving fraud, sanctions, or regulatory penalties.
  - Incomplete verification of UBO or key executive identity.
* **Key actions**:
  - Collect certified corporate documents (registry extracts, articles of association, financial statements).
  - Request source of funds/wealth declarations for UBOs.
  - Schedule interview or written questionnaire with merchant compliance contact.
  - Document remediation plan or outstanding requests.
* **Tools**:
  - *Secure document exchange*: encrypted portal or DMS.
  - *Video/meeting recorder*: for interviews, ensuring consent and compliance.
  - *EDD checklist manager*: tracks outstanding evidence and deadlines.
* **Deliverables**: EDD pack with document inventory, findings, and clear recommendation (approve with conditions, continue monitoring, or reject).

### 6. Risk Assessment & Decision Agent
* **Mission**: consolidate all findings, score risk, and issue final recommendation.
* **Inputs**: reports from Intake, OSINT, Screening, Adverse Media, and EDD agents.
* **Key actions**:
  - Apply risk-scoring matrix (jurisdiction, industry, ownership, negative news, compliance history).
  - Document rationale for each score component and reference supporting evidence.
  - Decide between Approve, Approve with EDD monitoring, or Reject, aligning with policy thresholds.
  - Compile final case report detailing checks performed, results, outstanding issues, and next review date.
* **Tools**:
  - *Risk scoring engine*: configurable calculator aligned with AML policy.
  - *Report generator*: templates in Markdown/Docx including evidence tables.
  - *Case management system*: archives all artefacts and approvals.
* **Deliverables**: signed-off decision memo ready for compliance oversight.

## Collaboration Protocol
1. **Task orchestration**: Cases live in a shared queue. Intake agent assigns statuses ("OSINT", "Screening", "EDD", "Decision"). Agents must update status upon completion.
2. **Evidence standards**: Every factual statement must reference a source (URL, document ID, interview note). Screenshots stored with hash + timestamp.
3. **Communication**: Use secure channel (e.g., Slack Enterprise Grid or MS Teams) with audit logging. Sensitive PII must stay inside approved systems.
4. **Escalation**: If any agent identifies potential criminal or sanctions exposure, pause the process and notify compliance officer before proceeding.
5. **Quality review**: Decision agent cannot be the same individual who performed Intake or OSINT for the same case. Peer review mandatory for high-risk outcomes.

## Tooling Requirements Summary
| Tool | Purpose | Owner | Notes |
| --- | --- | --- | --- |
| Entity normaliser | Validate legal entity details | Intake agent | Integrates with EU registries (VIES, Companies House, etc.) |
| Search connector | Automate risk keyword queries | OSINT agent | Support query templates in multiple languages |
| LinkedIn lookup | Confirm leadership identities | OSINT agent | Ensure compliance with LinkedIn usage policies |
| Web snapshotter | Capture site evidence | OSINT agent | Store in case management system |
| Sanctions screening engine | Screen entities/persons | Screening agent | Must support fuzzy logic & audit logs |
| PEP database | Identify politically exposed persons | Screening agent | Keep subscription updated |
| Media aggregator | Investigate adverse news | Adverse media agent | Should deduplicate articles |
| Translation service | Interpret non-English content | Adverse media agent | Log both original & translated excerpts |
| Secure document portal | Receive merchant documents | EDD agent | Encrypted storage & access controls |
| Risk scoring engine | Quantify residual risk | Decision agent | Configurable scoring thresholds |
| Report generator | Compile final report | Decision agent | Output includes checklist of performed checks |

## Output Template (Decision Agent)
```
Case ID: <auto-generated>
Merchant: <legal name>
Date: <YYYY-MM-DD>

Summary Recommendation: <Approve / Approve with Monitoring / Reject>
Risk Rating: <Low / Medium / High>

Checks Performed:
- Intake validation (by <agent>, date) – outcome
- OSINT review (by <agent>, date) – key findings
- Sanctions & PEP screening (by <agent>, date) – results
- Adverse media review (by <agent>, date) – highlights / N/A
- EDD actions (by <agent>, date) – documents obtained / outstanding

Key Findings & Evidence:
1. <Finding> — [Source]
2. ...

Unresolved Issues / Conditions:
- <Item>

Next Review Date: <if applicable>
Approvals: <names & roles>
```

## Governance & Continuous Improvement
* Conduct quarterly calibration sessions to update risk indicators, keyword lists, and scoring thresholds.
* Maintain version control for this playbook; material updates require compliance sign-off.
* Log tool outages or data quality issues in a central incident register.

