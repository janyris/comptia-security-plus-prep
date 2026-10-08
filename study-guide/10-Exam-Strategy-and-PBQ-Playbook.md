# Exam Strategy & PBQ Playbook

SY0-701 is a **reading-comprehension-under-time-pressure** test as much as a knowledge test. Most people who fail knew the content and misread the question or took a distractor. This file converts knowledge into a score. Read it once now, again the night before, again the morning of.

---

## Exam mechanics (confirm on your CompTIA account before test day — these can change)

- **Up to 90 questions**, **90 minutes**.
- Scaled score **100–900**; **passing = 750**. Not a raw percentage — questions aren't equally weighted, so don't panic-average.
- **MCQ (Multiple-Choice Question) + PBQ** mixed; **PBQs are front-loaded** (usually most in the first third) and each is worth more than a single MCQ.
- You can **flag and revisit** within the session; once you submit or leave, you can't return.
- Domain weights: **D4 28% · D2 22% · D5 20% · D3 18% · D1 12%.** D4 + D2 = half the exam.

---

## The one habit that wins: find the qualifier first

Before reading any option, underline the qualifier in the stem:

| Qualifier                           | What it means                                                           |
| ----------------------------------- | ----------------------------------------------------------------------- |
| **BEST / MOST appropriate**         | Several options are valid; pick the most complete/direct fit            |
| **FIRST / NEXT**                    | Ordering question — other options may all be things you *eventually* do |
| **MOST LIKELY**                     | Probability judgment — drop the technically-possible-but-improbable     |
| **MOST SECURE**                     | Pick the strongest option even if others "work"                         |
| **MOST EFFICIENT / COST-EFFECTIVE** | There's a "works but wasteful" trap — pick the lean one                 |
| **PRIMARY / MAIN purpose**          | Core function, not a side benefit                                       |
| **LEAST / NOT / EXCEPT**            | The odd one out — logic is reversed; circle it, slow down               |

> Most wrong answers are **true statements that don't answer the specific qualifier asked.** When two answers both look right, the qualifier breaks the tie.

---

## Read method (~45–60 sec per MCQ)

1. Read the **last sentence first** — the actual question + qualifier usually lives there.
2. Read the scenario, noting **constraints** ("without disrupting availability", "for a legacy system that can't be patched", "the CISO (Chief Information Security Officer) wants…", a port number, a protocol, a caller-ID (Identifier) detail).
3. **Answer it in your head before looking at options.**
4. **Eliminate the two clearly-wrong options.** Now reason between two, not guess between four.
5. Between the final two: *which one directly serves the qualifier and respects the constraint?*
6. Stuck > 90 seconds → **flag, pick your best guess, move on.** One question must never cost three questions' time.

---

## Distractor patterns CompTIA loves

| Trap                                    | Looks like                                                     | Defense                                                             |
| --------------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------- |
| **True-but-irrelevant**                 | A real security fact that doesn't address *this* scenario      | Re-match to the qualifier                                           |
| **"More secure = always right" reflex** | Most locked-down option when the ask is *efficient* or *first* | Serve the qualifier, not your gut                                   |
| **Almost-right term**                   | Whaling vs BEC (Business Email Compromise), RTO (Recovery Time Objective) vs RPO (Recovery Point Objective), IDS (Intrusion Detection System) vs IPS (Intrusion Prevention System), MOU (Memorandum of Understanding) vs MOA (Memorandum of Agreement), DAC (Discretionary Access Control) vs MAC (Mandatory Access Control) | Know the exact distinctions ([09-Commonly-Confused-Pairs](09-Commonly-Confused-Pairs.md))        |
| **Later valid step offered as "first"** | An action you *do* take, just not now                          | Recall the exact sequence ([08-Reference_Orderings-and-Formulas](08-Reference_Orderings-and-Formulas.md)) |
| **Two synonym options**                 | If two choices mean the same thing, **neither** is the answer  | Eliminate both                                                      |
| **Absolutes**                           | "always", "never", "all", "guarantees", "100%"                 | Almost always wrong in security                                     |
| **Vendor/product name as answer**       | A brand where a *concept* is asked                             | Prefer the concept unless the stem is product-specific              |

---

## Scenario → answer reflexes (train until instant)

**Crypto & PKI (Public Key Infrastructure)**
- "Confirm a file wasn't altered" → **hashing / integrity**
- "Prove who sent it, can't be denied" → **digital signature / non-repudiation**
- "Anyone can encrypt to us, only we can read" → recipient's **public key** (asymmetric)
- "Recorded traffic safe even if the server key leaks later" → **PFS (Perfect Forward Secrecy) / ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)**
- "Stop rainbow-table attacks on the password DB (Database)" → **salting** (+ bcrypt/Argon2)
- "Protect lost laptops" → **full-disk encryption + TPM (Trusted Platform Module)**
- "Use a value in a system without exposing real data + shrink PCI scope" → **tokenization**
- "Store the CA (Certificate Authority) signing key so it can't be extracted" → **HSM (Hardware Security Module)**
- "Cert check without contacting the CA each connection" → **OCSP (Online Certificate Status Protocol) stapling**

**Ops & IR (Incident Response)**
- "Which log?" → executable/process = **endpoint/EDR (Endpoint Detection and Response)**; source IP (Internet Protocol) of a scan / allowed-denied = **firewall**; exact SQLi string = **application**; logins/lockouts = **auth**; DNS (Domain Name System) tunneling = **DNS**; correlate across sources = **SIEM (Security Information and Event Management)**
- "FIRST action in an incident" → follow **Prepare→Detect→Analyze→Contain→Eradicate→Recover→Lessons**; if already detected, usually **contain**
- "Collect evidence in what order?" → **order of volatility** (RAM (Random Access Memory) before disk)
- "Proactively hunt an attacker with no alert/signature" → **threat hunting**
- "Preserve for possible litigation" → **legal hold + chain of custody**
- "Verify the patch worked" → **rescan**
- "Automatically contain + ticket when SIEM fires" → **SOAR (Security Orchestration, Automation, and Response)**
- "Detect a compromised account by behavior" → **UEBA (User and Entity Behavior Analytics) / anomaly detection**

**Architecture & data**
- "Limit lateral movement / blast radius" → **segmentation / microsegmentation**
- "Admins reach a secure zone through one hardened box" → **jump server / bastion**
- "Only healthy, authorized devices on the LAN (Local Area Network)" → **NAC (Network Access Control) + 802.1X**
- "Protect the public web app from SQLi/XSS (Cross-Site Scripting)" → **WAF (Web Application Firewall)**
- "Two offices linked permanently and securely" → **site-to-site IPsec VPN (ESP (Encapsulating Security Payload (IPsec — encryption + integrity)), tunnel, AES (Advanced Encryption Standard)-256/SHA (Secure Hash Algorithm)-256, DH (Diffie-Hellman) 14+)**
- "Data must stay in-country" → **geographic restriction / data sovereignty**
- "Protect data while being processed in memory" → **data in use → secure enclave / TEE (Trusted Execution Environment)**
- "Ransomware also encrypted the backups" → **immutable / offline / air-gapped backup**
- "OT (Operational Technology)/ICS (Industrial Control System)/SCADA (Supervisory Control and Data Acquisition) priority" → **availability** > confidentiality; **segment/isolate**

**IAM (Identity and Access Management)**
- "Access by job function" → **RBAC (Role-Based Access Control)**; "by dept + device + time + location" → **ABAC (Attribute-Based Access Control)**
- "Owner sets file permissions" → **DAC (Discretionary Access Control)**; "labels the user can't change" → **MAC (Mandatory Access Control)**
- "Grant elevated rights only for the task, then revoke" → **JIT (Just-in-Time (access)) access (PAM)**
- "One smooth, phishing-resistant, passwordless login" → **FIDO2 (Fast IDentity Online 2) passkey + biometric**
- "Former employee still had access" → **de-provisioning gap + access recertification**
- "Delegate an app scoped access to my data" → **OAuth 2.0**; "log in *with* my Google identity" → **OIDC (OpenID Connect)**

**Threats & governance**
- "Correct URL (Uniform Resource Locator) but redirected" → **pharming**; "mistyped / look-alike URL" → **typosquatting**
- "Email to the CFO (Chief Financial Officer) for an urgent wire transfer" → **BEC** (whaling if the point is *targeting the exec*)
- "Insurance" → **risk transference**; "documented do-nothing" → **acceptance**; "stop the activity" → **avoidance**
- "Risk left after controls" → **residual risk**
- "Alternative control because the primary isn't feasible" → **compensating control**
- "Document with project scope, cost, and timeline" → **SOW (Statement of Work)**
- "Simulate an external attacker, no inside info" → **unknown / black-box** pen test
- "Spoofed mail from our domain still delivered" → **DMARC (Domain-based Message Authentication, Reporting & Conformance) `p=reject`**
- "Employees keep tailgating / clicking phish" → **security-awareness training** (situational awareness / phishing simulation) + technical backstop (MFA (Multifactor Authentication), DMARC)

---

## PBQs — the front-loaded time trap

- Appear **first** (typically 3–5), interactive, worth more than one MCQ (Multiple-Choice Question).
- **Strategy:** do the quick ones immediately. If one is dense and you're stuck, **flag it and jump to the MCQs** — bank the easy points, come back with leftover time. Never let one PBQ eat 15 minutes up front; that's how people rush the last 40 MCQs.

### The 7 PBQ formats

| # | Format | What to have automatic |
|---|---|---|
| 1 | **Match / drag-and-drop** — terms↔definitions, attacks↔indicators, controls↔category/type, data classification↔disposal method | The control matrix; attack→mitigation pairs; magnetic media (degauss/wipe) vs. paper/flash (physical destruction) |
| 2 | **Firewall / ACL (Access Control List) rule table** — fill source, dest, port, protocol, action | Top-down first-match; specific *permit* rules **above** the broad *deny*; `/32` = one host; end with implicit deny |
| 3 | **Network diagram placement** — where to put FW (Firewall) / IDS (Intrusion Detection System) / IPS (Intrusion Prevention System) / WAF (Web Application Firewall) / jump server / DMZ (Demilitarized Zone (screened subnet)) / load balancer | IPS inline; IDS out-of-band; public services in screened subnet; DB (Database) never directly internet-facing; redundant tiers across zones |
| 4 | **Log / command-output analysis** — read a log/`nmap`/`nslookup`/PCAP (Packet Capture) summary and name the attack or root cause | Port-scan pattern, brute force vs spraying, C2 (Command and Control) beaconing interval, SQLi string, DNS (Domain Name System) tunneling |
| 5 | **Ordering / sequencing** — put steps in order | IR (Incident Response) lifecycle; order of volatility; change-management steps; PKI (Public Key Infrastructure) cert request; TLS (Transport Layer Security) handshake |
| 6 | **Categorization / sorting** — items into buckets | Managerial/Operational/Technical/Physical; preventive/detective/…; data classification; CIA (Confidentiality, Integrity, Availability) impact |
| 7 | **Risk / BIA (Business Impact Analysis) calculation** — fill SLE (Single Loss Expectancy) / ARO (Annualized Rate of Occurrence) / ALE (Annualized Loss Expectancy) or RTO (Recovery Time Objective) / RPO (Recovery Point Objective) / MTD (Maximum Tolerable Downtime) | `SLE = AV (Asset Value) × EF`; `ALE = SLE × ARO`; `RTO + WRT ≤ MTD` |

---

## Time budget

- 90 min / 90 questions ≈ **1 min average**. PBQs need 3–8 min each; make it back on quick-recall MCQs.
- Checkpoint at **45 minutes** — you should be near question 45. If not, speed up and flag more.
- Leave **10 minutes** for flagged questions.
- Answer **every** question — no penalty for guessing. An eliminated-to-two guess is ~50%.

---

## Final 7 days

- [ ] Daily 10-min drill: [06-Reference_Ports-Protocols-Crypto](06-Reference_Ports-Protocols-Crypto.md) + [08-Reference_Orderings-and-Formulas](08-Reference_Orderings-and-Formulas.md) until every table is automatic.
- [ ] One full **timed** 90-question practice exam from a reputable source. Score per domain; restudy the two lowest.
- [ ] Practice each of the 7 PBQ formats above at least once.
- [ ] Read [09-Commonly-Confused-Pairs](09-Commonly-Confused-Pairs.md) end to end — this is where distractors live.
- [ ] Skim each domain file's **60-second self-check** and answer aloud.
- [ ] Retake the mock; target **80+/90** cold.

## Final 48 hours

- [ ] [11-Final-Review_High-Yield-Sheet](11-Final-Review_High-Yield-Sheet.md) only. No new material.
- [ ] Recite: IR (Incident Response) lifecycle, order of volatility, the cleartext→secure port swaps, `SLE/ALE`, `RTO+WRT≤MTD`.
- [ ] Confirm exam logistics: ID(s), test-center address or **OnVUE** system check + room requirements, start time, what's allowed.
- [ ] Light day before. Stop studying by early evening. Sleep 8 hours.

## Exam day

- [ ] Arrive / log in **30 minutes early**.
- [ ] Do the tutorial slowly to settle nerves.
- [ ] PBQs: quick wins now, flag-and-skip the dense ones, return after MCQs.
- [ ] Every question: **qualifier first**, eliminate two, serve the qualifier.
- [ ] Watch **NOT / EXCEPT / LEAST** — reversed logic.
- [ ] Trust your first instinct; only change an answer if you find a concrete misread.
- [ ] Answer everything. Submit with confidence.
