# Domain 5 — Security Program Management and Oversight (20%)

**Objectives:** 5.1 Summarize elements of effective security governance · 5.2 Explain elements of the risk management process · 5.3 Processes of third-party risk assessment and management · 5.4 Summarize elements of effective security compliance · 5.5 Explain types and purposes of audits and assessments · 5.6 Given a scenario, implement security awareness practices.

---

## 5.1 Security Governance

### Governance documents — know the hierarchy and which are mandatory

| Document | Mandatory? | What it is |
|---|---|---|
| **Policy** | Yes | High-level statement of management intent and direction ("we will protect customer data") |
| **Standard** | Yes | Specific mandatory requirement that supports a policy ("AES (Advanced Encryption Standard)-256 for data at rest", "8-char minimum") |
| **Procedure / SOP (Standard Operating Procedure)** | Yes (where it applies) | Step-by-step how-to for a task (onboarding, change, IR (Incident Response) playbook) |
| **Guideline** | No — recommended | Best-practice advice where flexibility is allowed |
| **Playbook** | Situational | A procedure for a specific scenario (phishing response, ransomware) |

Common **policies:** AUP (Acceptable Use Policy), information security policy, **BC/DR (Disaster Recovery)** plan, **incident response** plan, **SDLC (Software Development Life Cycle)**, change management, data governance/classification, remote access, password.

### Governance structures

- **Boards** and **committees** — oversight and accountability at leadership level.
- **Government entities / regulators** — set external requirements.
- **Centralized vs. decentralized** governance — one authority vs. business-unit autonomy.

### Roles & responsibilities

- **Owner** — accountable for an asset/risk; sets classification; usually leadership/business.
- **Controller** — determines **why and how** personal data is processed (GDPR (General Data Protection Regulation) term).
- **Processor** — processes data **on behalf of** the controller.
- **Custodian / steward** — day-to-day handling, protection, and data quality.
- **Data Protection Officer (DPO)** — oversees privacy compliance.

### External considerations that shape the program

**Regulatory**, **legal**, **industry-specific** (PCI-DSS, HIPAA (Health Insurance Portability and Accountability Act), SOX (Sarbanes-Oxley Act), GLBA (Gramm-Leach-Bliley Act), FERPA (Family Educational Rights and Privacy Act)), and **geographic** (local/regional/national/global — data sovereignty).

> [!TIP]
> **5.1 traps**
> - **Policy** (what/why, high level) vs **standard** (specific mandatory spec) vs **procedure** (how, step by step) vs **guideline** (optional). A "minimum password length of 12" is a **standard**, not a policy.
> - GDPR (General Data Protection Regulation): **controller decides**, **processor acts for the controller**.

---

## 5.2 Risk Management

### Assessment cadence

- **Ad hoc** — triggered by an event (new zero-day, acquisition).
- **One-time** — point-in-time (new system rollout).
- **Recurring** — scheduled (annual, quarterly).
- **Continuous** — real-time via agents/tools.

### Risk identification & analysis

- **Risk analysis** — examine causes, consequences, characteristics.
- **Risk assessment** — estimate risk **level = likelihood × impact**, then prioritize.
- **Qualitative** — words/ratings (Low/Med/High); fast, subjective, inconsistent between assessors.
- **Quantitative** — dollar figures (memorize and be able to calculate):
  - **SLE (Single Loss Expectancy) = Asset Value (AV) × Exposure Factor (EF)** — loss from **one** event.
  - **ARO (Annualized Rate of Occurrence)** — expected events per year.
  - **ALE (Annualized Loss Expectancy) = SLE × ARO** — expected annual loss.
  - A control is worth buying if its annual cost **< the ALE reduction** it delivers.

### Risk register & terminology

- **Risk register** — living record: each risk's description, **owner**, likelihood, impact, score, response, status; often a **heat map** (likelihood × impact) for stakeholders.
- **Key Risk Indicator (KRI)** — predictive early-warning metric.
- **Risk owner** — the accountable individual managing a specific risk end to end.
- **Inherent risk** — before controls. **Residual risk** — after controls. **Control risk** — risk that controls fail.
- **Risk appetite** — org-wide, strategic willingness to take risk: **expansionary** (aggressive), **conservative** (avoid), **neutral** (balanced).
- **Risk tolerance / threshold** — the acceptable variance/limit around the appetite; determines whether an item goes on the register or needs action.

### Risk management strategies (the 4 responses)

| Strategy | Meaning | Example |
|---|---|---|
| **Mitigate** (reduce) | Add controls to lower likelihood and/or impact | Patch, segment, MFA (Multifactor Authentication) |
| **Transfer** (share) | Shift financial impact to a third party | **Cyber insurance**, outsourcing (reputational risk can't be fully transferred) |
| **Accept** | Take no action; the risk is within tolerance and cost isn't justified | Documented sign-off; **risk exception** (temporary, reviewed) vs **risk exemption** (long-term strategic) |
| **Avoid** | Stop the activity that creates the risk | Discontinue the feature/product/vendor |

### Business Impact Analysis (BIA)

- Identifies **Mission Essential Functions (MEF)** and the impact of their disruption; justifies control spend.
- **Metrics (carry exact definitions):**
  - **RTO (Recovery Time Objective)** — target time to restore a system.
  - **RPO (Recovery Point Objective)** — max tolerable **data loss** (in time) → backup frequency.
  - **WRT (Work Recovery Time)** — extra time to test/reintegrate after restore.
  - **MTD (Maximum Tolerable Downtime)** — absolute downtime limit before irrecoverable harm. **RTO + WRT ≤ MTD.**
  - **MTBF (Mean Time Between Failures)** — reliability.
  - **MTTR (Mean Time To Repair / Recover)** — mean time to repair/recover.

> [!TIP]
> **5.2 exam angle**
> - Word problem: "AV (Asset Value) $200k, EF (Exposure Factor) 25%, happens twice a year — ALE (Annualized Loss Expectancy)?" → SLE (Single Loss Expectancy) = 200k × 0.25 = 50k; ALE = 50k × 2 = **$100k/yr**. A $60k/yr control is justified; a $120k/yr one is not.
> - "Bought cyber insurance" → **transfer**. "Signed off to do nothing" → **accept**. "Shut down the risky service" → **avoid**. "Added controls" → **mitigate**.
> - **RPO** always ties to *data loss / backup frequency*; **RTO** ties to *time to recover*.

---

## 5.3 Third-Party (Vendor) Risk

### Why it matters

Every vendor relationship **inherits risk into your organization** (SolarWinds, MOVEit, Target-via-HVAC (Heating, Ventilation, and Air Conditioning)-vendor). Supply-chain and MSP (Managed (Security) Service Provider) compromise are explicit exam themes.

### Due diligence & ongoing management

- **Vendor assessment:** penetration testing (or evidence of it), **right-to-audit clause**, evidence of **internal audits**, **independent assessments** (SOC 2, System and Organization Controls 2), **supply chain analysis** (your vendors' vendors), **vendor site visits**.
- **Vendor selection:** due diligence, **conflict-of-interest** checks (financial, personal, competitive, insider-information).
- **Questionnaires** — structured security/compliance data; validate the answers.
- **Rules of Engagement (RoE)** — access scope, security requirements, reporting/comms, change management, contractual provisions.
- **Ongoing monitoring** — reassess periodically; watch for the vendor's breaches and posture changes.

### Agreement types

| Agreement | Binding? | Purpose |
|---|---|---|
| **MOU** (Memorandum of Understanding) | Non-binding | Statement of intent / shared goals; preliminary |
| **MOA** (Memorandum of Agreement) | Binding | Formal roles, terms, responsibilities |
| **NDA** (Non-Disclosure Agreement) | Binding | Confidentiality of shared information |
| **BPA** (Business Partnership Agreement) | Binding | Terms of a long-term strategic partnership |
| **MSA** (Master Service Agreement) | Binding | Overarching terms; individual work hangs off it via SOWs |
| **SOW / WO** (Statement of Work / Work Order) | Binding | Scope, deliverables, **cost**, **timeline** for a specific engagement; can forbid subcontracting |
| **SLA** (Service-Level Agreement) | Binding | Measurable performance/quality targets + penalties (uptime, response time) |
| **ISA** (Interconnection Security Agreement) | Binding | Technical/security requirements to connect two networks |

> [!TIP]
> **5.3 traps**
> - "Document outlining **the project, the cost, and the completion time frame**" → **SOW**.
> - "Overall terms for an ongoing relationship, specifics later" → **MSA**.
> - "Guaranteed 99.9% uptime with penalties" → **SLA**.
> - "Non-binding statement of intent" → **MOU**; "binding formal responsibilities" → **MOA**.
> - "Contract right to audit the vendor's controls" → **right-to-audit clause**.
> - "Avoid a vendor secretly subcontracting the work" → put it in the **SOW**.

---

## 5.4 Security Compliance

### Consequences of non-compliance

**Fines**, **sanctions**, **reputational damage**, **loss of license**, **contractual impacts**, and legal liability.

### Compliance monitoring

- **Due diligence / due care** — investigate before acting; then act responsibly and continuously.
- **Attestation and acknowledgement** — staff sign that they've read policy; leaders attest to control effectiveness.
- **Internal and external** monitoring.
- **Automation** — continuous control checks (SCAP (Security Content Automation Protocol), CSPM (Cloud Security Posture Management)), evidence collection.

### Privacy

- **Legal implications:** local, regional (**GDPR (General Data Protection Regulation)** — EU (European Union)), national, and global scope.
- **Data subject** — the individual the personal data is about; GDPR rights include **right to be forgotten / erasure**, access, portability, rectification.
- **Controller vs. processor** (see 5.1).
- **Data inventory / mapping and retention** — know what personal data you hold, why, where, and for how long.
- **Right to be forgotten**, consent, breach-notification timelines (e.g. GDPR 72 hours).
- Key laws/standards to recognize: **GDPR**, **CCPA (California Consumer Privacy Act)/CPRA (California Privacy Rights Act)**, **HIPAA** (PHI), **PCI-DSS** (cardholder data — industry standard, not a law), **SOX (Sarbanes-Oxley Act)**, **GLBA (Gramm-Leach-Bliley Act)**, **FERPA (Family Educational Rights and Privacy Act)**, **COPPA (Children's Online Privacy Protection Act)**.

> [!TIP]
> **5.4 angle**
> - **PCI-DSS** is a **contractual industry standard**, not legislation — but non-compliance still means fines and losing the ability to process cards.
> - "EU residents' data, must support deletion requests" → **GDPR right to be forgotten**.
> - **Due diligence** = investigate/assess beforehand; **due care** = ongoing reasonable protection.

---

## 5.5 Audits and Assessments

### Attestation

Independent, objective confirmation that controls/processes are accurate and effective — provides stakeholder assurance (e.g. a SOC 2, System and Organization Controls 2, report).

### Internal

- **Compliance self-assessment** — internal check against laws/regs/policy.
- **Audit committee** — board-level independent oversight.
- **Self-assessment** — staff evaluate their own area against criteria.
- Required by frameworks like **NIST (National Institute of Standards and Technology) RMF (Risk Management Framework)** and by **PCI-DSS** for some merchant levels.

### External

- **Regulatory** — mandated by an authority.
- **Examination** — formal review of statements/processes/controls by an outside party.
- **Independent third-party audit** — unbiased evaluation (external auditors, CPAs, consultancies) — value = objectivity + specialist expertise internal teams can't fully replicate.

### Penetration testing

- **Perspectives / environment knowledge:**
  - **Known (white-box)** — full information; simulates a knowledgeable insider; efficient.
  - **Partially known (grey-box)** — limited information; some recon.
  - **Unknown (black-box)** — no prior information; simulates an external attacker; most recon, most resource-intensive.
- **Process:** recon → scanning/enumeration → gaining access/exploitation → **privilege escalation / lateral movement** → maintaining access → covering tracks → reporting. Key difference from a vuln scan: a pen test **actively exploits** to prove real risk.
- **Reconnaissance:** **active** (port scans, enumeration, OS (Operating System) fingerprinting — generates traffic, higher detection risk) vs **passive** (OSINT (Open-Source Intelligence), public records, traffic observation — no direct interaction).
- **Exercise types:** **red team** (offense), **blue team** (defense), **purple team** (collaborative), **white team** (referees/rules), **physical** pen test, **integrated** (offense + defense + physical).
- **Attestation of findings** — the formal report and, often, a retest.
- Notify the **ISP (Internet Service Provider)** and get written authorization / a get-out-of-jail letter before external testing.

> [!TIP]
> **5.5 traps**
> - "Test resilience against an **external** attacker with **no inside knowledge**" → **unknown / black-box** pen test.
> - "Simulate a **malicious insider**" → **known / white-box**.
> - "Only *identifies* weaknesses, doesn't exploit" → **vulnerability scan**; "*proves* exploitability" → **penetration test**.
> - "Unbiased assurance for customers/regulators" → **independent third-party audit / attestation** (e.g. SOC 2, System and Organization Controls 2).

---

## 5.6 Security Awareness Practices

### Building and running the program

- **Anomalous behavior recognition:** teach users to spot and report **risky** (unpatched personal device on VPN (Virtual Private Network)), **unexpected** (unfamiliar login prompt), and **unintentional** (misaddressed email) behavior.
- **User guidance and training topics:**
  - **Phishing / campaigns** — simulated phishing, recognizing and **reporting** (report button), reinforcing (not shaming) repeat clickers.
  - **Social engineering** generally — pretexting, tailgating, vishing.
  - **Anomalous behavior** and how/where to report it.
  - **Policy / handbooks** — acceptable use, data handling, acknowledgement.
  - **Situational awareness** — badges, tailgating, shoulder surfing, clean desk, travel risk.
  - **Insider threat** indicators and reporting channels.
  - **Password management** — managers, passphrases, no reuse, MFA (Multifactor Authentication).
  - **Removable media and cables** — no unknown USB (Universal Serial Bus), cable/juice-jacking.
  - **Hybrid / remote work** — home network hygiene, VPN use, physical document security.
- **Reporting and monitoring** — clear channels; track **initial** metrics and **recurring** trends (click rate, report rate, time-to-report).
- **Development and execution** — assign ownership, schedule recurring training, tailor to roles (developers, finance, executives, privileged users), measure and iterate.

> [!TIP]
> **5.6 angle**
> - Employees fell for a phish / tailgate → the answer is a **training / awareness** topic (phishing simulation, situational awareness), plus a technical backstop (MFA, DMARC (Domain-based Message Authentication, Reporting & Conformance)).
> - "Emphasize which topic" after badge/tailgating incidents → **situational awareness**.
> - Measure program effectiveness → **phishing-simulation click rate and report rate over time**.

---

## Domain 5 — 60-second self-check

1. Policy vs standard vs procedure vs guideline — which are mandatory, which is optional?
2. Calculate ALE (Annualized Loss Expectancy) from AV (Asset Value), EF (Exposure Factor), ARO (Annualized Rate of Occurrence). What makes a control financially justified?
3. The 4 risk responses, with one example each. Which one is cyber insurance?
4. MOU (Memorandum of Understanding) vs MOA (binding?), and which agreement states project scope/cost/timeline. *(MOU non-binding; MOA binding; SOW (Statement of Work))*
5. RTO (Recovery Time Objective) vs RPO (Recovery Point Objective) vs MTD (Maximum Tolerable Downtime); state the RTO + WRT (Work Recovery Time) ≤ MTD rule.
6. Known vs unknown environment pen test — which simulates an insider, which an outsider?
7. Controller vs processor under GDPR (General Data Protection Regulation); what is the "right to be forgotten"?
8. Two good metrics for a security-awareness program. *(phishing click rate, report rate / time-to-report)*
