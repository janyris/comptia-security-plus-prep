# Reference — Sequences You Must Recite & Formulas You Must Calculate

Ordering questions and risk word-problems become easy points if you drill these. Practice reciting each list forward with no gaps.

---

## 1. Incident Response Lifecycle — NIST SP 800-61

**Preparation → Detection → Analysis → Containment → Eradication → Recovery → Lessons Learned (post-incident activity)**

- Mnemonic: **"Please Don't Always Clean Every Room Late"** → *P*repare, *D*etect, *A*nalyze, *C*ontain, *E*radicate, *R*ecover, *L*essons learned.
- The two most-swapped steps on the exam: **Containment comes before Eradication comes before Recovery.**
- "Preserve the machine for analysis" happens in **Containment** (isolate, don't necessarily power off).
- The final deliverable is the **lessons-learned / after-action report** with assigned action items.

---

## 2. Order of Volatility (digital forensics — collect most volatile first)

1. **CPU (Central Processing Unit) registers, CPU cache**
2. **RAM** (memory) — routing table, ARP (Address Resolution Protocol) cache, process table, kernel stats, running network connections
3. **Temporary file systems / swap / pagefile**
4. **Disk** (drives, SSDs)
5. **Remote logging & monitoring data** (SIEM (Security Information and Event Management), syslog server)
6. **Physical configuration & network topology**
7. **Archival media / backups**

- Take a **bit-for-bit image**, **hash before and after**, work only from the copy, maintain **chain of custody**.

---

## 3. Change Management Process (governance side)

1. **Request** submitted (with reason, scope, risk)
2. **Impact analysis** (what breaks, who's affected)
3. **Approval** by the **Change Advisory Board (CAB)** / owner
4. **Test** in a sandbox / non-production environment
5. **Schedule** a maintenance window; notify stakeholders (plan for downtime / service restart)
6. **Implement** the change (within the restricted scope)
7. **Verify / validate**
8. **Document** — update diagrams, procedures, CMDB (Configuration Management Database), version control
9. **Backout / rollback** plan is ready the whole time; execute it if verification fails

- **Standard change** = pre-approved, low-risk, repeatable (skips full CAB). **Emergency change** = expedited, reviewed retroactively.

---

## 4. Cyber Kill Chain (Lockheed Martin — attacker's phases)

**Reconnaissance → Weaponization → Delivery → Exploitation → Installation → Command & Control (C2) → Actions on Objectives**

- Defensive value: breaking any single link stops the attack. Compare with **MITRE (MITRE Corporation) ATT&CK** (a matrix of real-world tactics & techniques, not a linear chain) and the **Diamond Model** (adversary–capability–infrastructure–victim).

---

## 5. Penetration Test Phases

**Planning & scoping / Rules of Engagement → Reconnaissance (passive then active) → Scanning & Enumeration → Gaining Access (exploitation) → Privilege Escalation → Lateral Movement / Pivoting → Maintaining Access (persistence) → Covering Tracks → Analysis & Reporting → (often) Retest**

- Get **written authorization**; notify the **ISP (Internet Service Provider)** before external testing.
- A pen test **actively exploits**; a vulnerability scan only **identifies**.

---

## 6. Vulnerability Management Cycle

**Identify (scan / intel / pen test) → Analyze & validate (rule out false positives) → Prioritize (CVSS (Common Vulnerability Scoring System) + KEV (Known Exploited Vulnerabilities (CISA catalog)) + EPSS (Exploit Prediction Scoring System) + exposure + criticality) → Remediate (patch / mitigate / compensating / accept / decommission) → Rescan & verify → Report**

- **Never skip the rescan.** "We patched it" is not "we confirmed it's fixed."

---

## 7. PKI Certificate Lifecycle

**Key generation → CSR (Certificate Signing Request) to the CA (Certificate Authority) → identity validation (RA) → issuance → distribution / installation → active use → renewal / rotation → revocation (CRL (Certificate Revocation List) / OCSP (Online Certificate Status Protocol)) → expiration → key destruction / archival**

---

## 8. Digital Signature — create & verify

**Create:** hash the message → encrypt the hash with the **sender's private key** → attach.
**Verify:** decrypt the signature with the **sender's public key** to recover the hash → independently hash the received message → **compare**. Match = integrity + authentication + non-repudiation. (The message body is *not* encrypted by this process.)

---

## 9. TLS Handshake (conceptual)

1. **ClientHello** (supported cipher suites, random)
2. **ServerHello** (chosen cipher suite, random) + **certificate** (+ optional stapled OCSP (Online Certificate Status Protocol))
3. Client validates the cert chain to a trusted root; checks revocation
4. **Key exchange** — **ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)** for Perfect Forward Secrecy → both derive the same session key
5. **Finished** — switch to symmetric encryption (**AES (Advanced Encryption Standard)-GCM (Galois/Counter Mode)**) for the session

---

## 10. Zero Trust — plane split

- **Control plane:** Policy Engine (decides) + Policy Administrator (executes) = **Policy Decision Point**; adaptive identity; threat-scope reduction; policy-driven access control.
- **Data plane:** **Policy Enforcement Point** (allows/blocks the actual connection); subject/system; implicit trust zones minimized.

---

## 11. OSI Model (Layer 7 → 1) — "All People Seem To Need Data Processing"

7 **Application** · 6 **Presentation** · 5 **Session** · 4 **Transport** · 3 **Network** · 2 **Data Link** · 1 **Physical**

- Common exam hooks: firewalls/ACLs at L3 (Layer 3)/L4 (Layer 4); **NGFW (Next-Generation Firewall)/WAF (Web Application Firewall)/proxy** at L7 (Layer 7); switches/MAC (Media Access Control)/ARP (Address Resolution Protocol) at L2 (Layer 2); hubs/cabling at L1 (Layer 1); TLS (Transport Layer Security) commonly described as L5 (Layer 5)–L6 (Layer 6).

---

## 12. Recovery-Test Escalation (least to most disruptive)

**Tabletop (discussion) → Walkthrough → Simulation → Parallel processing (DR (Disaster Recovery) runs alongside prod) → Failover / Full interruption (actually cut to DR)** — always also test **fail back**.

---

## 13. Data Lifecycle

**Create/collect → Store → Use → Share/transmit → Archive → Destroy** — classification and retention rules apply at every stage; destruction is verified with a **certificate of destruction**.

---

# Formulas — be able to calculate under 30 seconds

## Risk (quantitative)

| Formula | Meaning |
|---|---|
| **SLE (Single Loss Expectancy) = AV (Asset Value) × EF (Exposure Factor)** | Single Loss Expectancy = Asset Value × Exposure Factor (% lost per event) |
| **ALE (Annualized Loss Expectancy) = SLE × ARO (Annualized Rate of Occurrence)** | Annualized Loss Expectancy = SLE × Annualized Rate of Occurrence |
| **ARO** | Expected number of occurrences per year (once in 4 years = 0.25) |
| **ROSI (Return on Security Investment) = (ALE_before − ALE_after − control cost) ÷ control cost** | Return on Security Investment |
| **Control is worth it if:** annual cost < (ALE_before − ALE_after) | i.e. it saves more than it costs |

### Worked example

> A server is worth **$40,000**. A flood destroys **50%** of it. Floods occur **once every 4 years**.
> - SLE (Single Loss Expectancy) = 40,000 × 0.50 = **$20,000**
> - ARO (Annualized Rate of Occurrence) = 1 ÷ 4 = **0.25**
> - ALE (Annualized Loss Expectancy) = 20,000 × 0.25 = **$5,000 / year**
> - A **$3,000/yr** flood control is justified (saves $5,000). A **$9,000/yr** control is not.

## Recovery / reliability

| Metric | Definition |
|---|---|
| **RTO (Recovery Time Objective)** | Max acceptable **time to restore** a service |
| **RPO (Recovery Point Objective)** | Max acceptable **data loss, measured in time** → sets backup frequency |
| **WRT (Work Recovery Time)** | Test/reintegrate after restore |
| **MTD (Maximum Tolerable Downtime)** | **RTO + WRT ≤ MTD** |
| **MTBF (Mean Time Between Failures)** | Total operational time ÷ number of failures (reliability) |
| **MTTF (Mean Time To Failure)** | For non-repairable items |
| **MTTR (Mean Time To Repair / Recover)** | Total repair time ÷ number of failures (average fix time) |

## Availability

| Term | Value |
|---|---|
| "Five nines" = 99.999% | ≈ **5.26 minutes** downtime per year |
| 99.99% | ≈ 52.6 minutes/year |
| 99.9% | ≈ 8.77 hours/year |
| 99% | ≈ 3.65 days/year |

## Biometrics

- **FAR** (False Acceptance Rate) — impostor accepted → **security** failure.
- **FRR** (False Rejection Rate) — legitimate user rejected → **usability** failure.
- **CER (Crossover Error Rate) / EER** (Crossover / Equal Error Rate) — the point where FAR = FRR; **lower CER = better sensor**.
