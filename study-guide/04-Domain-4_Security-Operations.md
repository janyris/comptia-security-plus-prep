# Domain 4 — Security Operations (28%)

The **highest-weighted domain**.

**Objectives:** 4.1 Apply common security techniques to computing resources · 4.2 Security implications of proper hardware/software/data asset management · 4.3 Vulnerability management activities · 4.4 Security alerting and monitoring concepts and tools · 4.5 Modify enterprise capabilities to enhance security · 4.6 Implement and maintain identity and access management · 4.7 Importance of automation and orchestration · 4.8 Appropriate incident response activities · 4.9 Use data sources to support an investigation.

---

## 4.1 Securing Computing Resources

### Secure baselines

- **Establish** — a documented, hardened standard configuration (CIS (Center for Internet Security (Benchmarks)) Benchmarks, DISA (Defense Information Systems Agency) STIGs, vendor guides).
- **Deploy** — push via imaging, Group Policy, MDM (Mobile Device Management), IaC (Infrastructure as Code).
- **Maintain** — monitor for drift; **configuration enforcement** re-applies the baseline.

### Hardening targets

- **Workstations / laptops** — EDR (Endpoint Detection and Response), host firewall, FDE (Full Disk Encryption), patching, least privilege (no local admin), disable USB (Universal Serial Bus) mass storage if required.
- **Servers** — least functionality, remove roles/features not used, separate admin plane, patch, host IPS (Intrusion Prevention System).
- **Mobile devices** — MDM (Mobile Device Management)/UEM (Unified Endpoint Management), screen lock + biometric, encryption, remote wipe, no jailbreak/root, app vetting.
- **Switches / routers** — change defaults, disable unused ports/services (Telnet, HTTP (Hypertext Transfer Protocol) mgmt), SSH (Secure Shell) only, ACLs (Access Control List), management VLAN (Virtual Local Area Network), secure SNMPv3 (Simple Network Management Protocol), control-plane policing.
- **Cloud infrastructure** — CSPM (Cloud Security Posture Management), least-privilege IAM (Identity and Access Management), disable public access by default, encrypt storage, log everything.
- **ICS (Industrial Control System)/SCADA (Supervisory Control and Data Acquisition) / RTOS (Real-Time Operating System) / embedded / IoT** — segment, change defaults, disable UPnP/unused services, vendor firmware, physical security; expect *can't patch* → compensating controls.

### Wireless & mobile

- **Deployment models:** **BYOD** (personal device, user-owned — hardest to control), **COPE** (corporate-owned, personally enabled), **CYOD** (choose from approved list), **corporate-owned business-only (COBO)**.
- **MDM (Mobile Device Management)/UEM (Unified Endpoint Management) controls:** containerization (separate work/personal), app allow/deny lists, conditional launch, remote lock/wipe, compliance policies, certificate push.
- **Connection methods:** cellular, **Wi-Fi**, **Bluetooth** (bluejacking = send, bluesnarfing = steal), NFC (Near Field Communication), infrared, USB (Universal Serial Bus).
- **Wireless security settings:**
  - **WPA3 (Wi-Fi Protected Access 3)** — current; **SAE** (Simultaneous Authentication of Equals / "Dragonfly") replaces WPA2 (Wi-Fi Protected Access 2)'s PSK (Pre-Shared Key) 4-way-handshake weakness; adds forward secrecy; **PMF** (protected management frames) mandatory.
  - **WPA2** — AES (Advanced Encryption Standard)-**CCMP (Counter Mode CBC-MAC Protocol (WPA2 encryption))**; avoid TKIP (Temporal Key Integrity Protocol)/WEP (Wired Equivalent Privacy).
  - **Personal (PSK)** vs **Enterprise (802.1X + EAP (Extensible Authentication Protocol) + RADIUS (Remote Authentication Dial-In User Service))** — enterprise = per-user credentials, no shared key. **EAP-TLS (Transport Layer Security)** strongest (mutual certs); PEAP (Protected Extensible Authentication Protocol)/EAP-TTLS (Tunneled Transport Layer Security) tunnel a weaker inner method.
  - **Site survey** + **heat map** — plan AP (Access Point) placement, channels, power to reduce dead zones and bleed.
  - Rogue AP / evil twin → **WIPS (Wireless Intrusion Prevention System)**, 802.1X, PMF.

### Application security

- **Input validation** (allow-list) and **output encoding**.
- **Secure cookies** (`Secure`, `HttpOnly`, `SameSite`).
- **Static code analysis (SAST)** — scan source, no execution. **Dynamic analysis (DAST)** — test the running app. **IAST (Interactive Application Security Testing)** — instrumented, both. **Fuzzing** — feed malformed input to find crashes.
- **Code signing** — cryptographically sign builds so their origin and integrity can be verified.
- **Secure SDLC (Software Development Life Cycle)**, dependency/**SCA (Software Composition Analysis)** scanning, secrets management (no hard-coded keys), sandboxing.

---

## 4.2 Asset Management

- **Acquisition / procurement** — security requirements in the purchase; vetted suppliers.
- **Assignment / accounting** — **ownership** assigned; **classification**; tracked in an inventory/CMDB (Configuration Management Database).
- **Monitoring / asset tracking** — inventory, **enumeration** (discover what's actually on the network — nothing is protected if it's unknown).
- **Disposal / decommissioning:**
  - **Sanitization** — **crypto-erase** (destroy the key), **secure erase (ATA)**, overwrite (multiple passes for HDD (Hard Disk Drive)).
  - **Destruction** — shred, pulverize, degauss (**magnetic media only** — useless on SSD (Solid State Drive)), incinerate.
  - **Certification** — documented proof of destruction (certificate of destruction).
  - **Data retention** — keep data as long as legally/operationally required, no longer.

> [!TIP]
> **4.2 reflexes**
> - **SSD** that will be **reused** → **crypto-erase** or **secure erase**; **degaussing does nothing to an SSD**.
> - Drive leaving the building for destruction → get a **certificate of destruction**.
> - "Unknown devices on the network" → **asset inventory / enumeration / NAC (Network Access Control)**.

---

## 4.3 Vulnerability Management

### Identification

- **Vulnerability scanning** — authenticated/credentialed (accurate, insider view, fewer false positives) vs unauthenticated (attacker view); agent-based (roaming hosts) vs network-based.
- **Application security testing** — SAST (Static Application Security Testing), DAST (Dynamic Application Security Testing), IAST (Interactive Application Security Testing), **package/dependency (SCA)** analysis.
- **Threat intelligence / feeds** — **OSINT (Open-Source Intelligence)**, **ISAC** (industry sharing), dark web monitoring, vendor advisories, **STIX (Structured Threat Information eXpression)/TAXII (Trusted Automated eXchange of Intelligence Information)** transport.
- **Penetration testing** — actively exploits to prove risk (vs. scanning which only identifies).
- **Responsible disclosure / bug bounty** — external researchers report flaws.
- **System/process audit**.

### Analysis & prioritization

- **CVSS (Common Vulnerability Scoring System)** — 0–10 severity (base + temporal + environmental metrics).
- **CVE (Common Vulnerabilities and Exposures)** — the individual vulnerability identifier; **CWE (Common Weakness Enumeration)** — weakness class; **CPE (Common Platform Enumeration)** — product naming; **NVD (National Vulnerability Database)** — the database.
- **EPSS (Exploit Prediction Scoring System)** — probability the vuln will be exploited in the next 30 days.
- **CISA (Cybersecurity and Infrastructure Security Agency) KEV (Known Exploited Vulnerabilities (CISA catalog)) catalog** — vulnerabilities **known to be exploited in the wild** → patch first, regardless of CVSS.
- Context: asset **exposure** (internet-facing?), asset **criticality**, compensating controls in place, **environmental variables**.
- **Validation:** confirm it's real — **false positive** (flagged but not real → verify before ticketing) vs **false negative** (missed real vuln → worse).

### Response & remediation

- **Patching** (primary), **insurance** (transfer), **segmentation/isolation**, **compensating controls** (WAF (Web Application Firewall)/IPS (Intrusion Prevention System) virtual patch, disable feature), **exception / exemption** (documented, approved, time-bound, reviewed), **decommission** if EOL (End of Life).
- **Rescan / re-test to verify** the fix worked — this last step is a frequent correct-answer trap. Then **report** (metrics, SLA (Service-Level Agreement) compliance, trend).

> [!TIP]
> **4.3 exam angle**
> - "A new exploit was announced — are we affected?" → **run a vulnerability scan** (not run the PoC on prod).
> - "Which vulns first?" → **KEV (Known Exploited Vulnerabilities (CISA catalog))-listed / actively exploited**, then high CVSS (Common Vulnerability Scoring System) **on exposed, critical assets**.
> - After remediation → **rescan to confirm**, always.
> - Scanner flagged something the team believes is patched → **validate / verify** before acting (possible false positive).

---

## 4.4 Alerting and Monitoring

### What to monitor

- **Systems** (host health, config), **applications** (errors, transactions), **infrastructure** (network devices, links).
- **Activities:** log aggregation, alerting, scanning, reporting, **archiving**, alert response/remediation, alert **tuning** (reduce false positives / alert fatigue).

### Tools & concepts

| Tool | Purpose |
|---|---|
| **SIEM (Security Information and Event Management)** | Aggregate + normalize + **correlate** logs from many sources → alerts, dashboards, reports, compliance. "Consolidated view of many devices" = SIEM |
| **SOAR (Security Orchestration, Automation, and Response)** | **Orchestrate + automate** the *response* (playbooks): enrich, ticket, contain, notify |
| **SNMP (Simple Network Management Protocol)** | Poll/trap device health and metrics (v3 = authenticated + encrypted) |
| **NetFlow / IPFIX (IP Flow Information Export) / sFlow** | Flow records — who talked to whom, when, how much (**no payload**) |
| **Packet capture (PCAP)** | Full content — when you need to see the actual traffic |
| **Syslog** | Standard log transport to a central collector (use TLS (Transport Layer Security) / 6514) |
| **Vulnerability scanners** | Identify weaknesses (4.3) |
| **Antivirus / EDR (Endpoint Detection and Response) / XDR (Extended Detection and Response)** | Endpoint detection & response |
| **DLP (Data Loss Prevention)** | Data-exfiltration monitoring |
| **Benchmarks / SCAP (Security Content Automation Protocol)** | Automated config compliance checks |
| **Log aggregation / data lake** | Central store for search, hunting, retention |
| **Alerting (NAC (Network Access Control), SNMP traps, agents)** | Real-time notification |

- **Log integrity** — forward logs to a **write-once / WORM** or append-only store, hash them, restrict access, so an attacker can't tamper or clear them locally.
- **Baselining** — establish "normal" so anomalies stand out; **UEBA (User and Entity Behavior Analytics)** does this for user/entity behavior (insider threat, compromised account, impossible travel).
- **Reporting** — trend analysis, KPIs/**KRIs**, compliance reports.

> [!TIP]
> **4.4 reflexes**
> - "Correlate events from firewall + AD (Active Directory) + endpoint into one alert" → **SIEM**.
> - "Automatically isolate the host and open a ticket when the SIEM fires" → **SOAR**.
> - "See traffic volume and relationships but not content" → **NetFlow**. "Need the actual packets" → **PCAP**.
> - "Detect a compromised account by unusual behavior" → **UEBA / anomaly detection**.
> - "Too many low-value alerts" → **alert tuning** (adjust thresholds, suppress known-good, write better correlation rules).

---

## 4.5 Modifying Enterprise Capabilities

### Firewall & filtering

- **Rules / ACLs** — order matters, first match, end with **implicit deny**; least privilege for traffic.
- **Ports/protocols** — open only what's needed; document.
- **Screened subnets** for exposed services.

### Web filtering

- **Secure web gateway (SWG)** — inspects and filters web traffic.
- **URL (Uniform Resource Locator) / content categorization**, **reputation** filtering, **block rules**.
- **Agent-based** (on the endpoint, works off-network) vs **centralized proxy** (on-net).
- **DNS (Domain Name System) filtering** — block resolution of known-bad/malware/phishing domains (fast, cheap layer).

### Operating-system security

- **Group Policy** (Windows) — push security settings at scale.
- **SELinux / AppArmor** (Linux) — mandatory access control confining processes.
- **Execution control** — application **allow-listing**, driver signing.

### Secure protocols (use the encrypted variant)

| Insecure | Secure replacement | Port |
|---|---|---|
| Telnet 23 | **SSH (Secure Shell)** | 22 |
| FTP (File Transfer Protocol) 20/21 | **SFTP** (over SSH) or **FTPS (File Transfer Protocol Secure)** | 22 / 989-990 |
| HTTP (Hypertext Transfer Protocol) 80 | **HTTPS (TLS)** | 443 |
| LDAP (Lightweight Directory Access Protocol (Secure)) 389 | **LDAPS (Lightweight Directory Access Protocol (Secure))** | 636 |
| SMTP (Simple Mail Transfer Protocol) 25 | **SMTPS (SMTP Secure)** / SMTP+STARTTLS (Start Transport Layer Security) | 465 / 587 |
| POP3 (Post Office Protocol version 3) 110 / IMAP (Internet Message Access Protocol) 143 | **POP3S (POP3 Secure) / IMAPS (IMAP Secure)** | 995 / 993 |
| SNMP (Simple Network Management Protocol) v1/v2 161 | **SNMPv3** | 161 |
| DNS (Domain Name System) 53 | **DNSSEC** (integrity), **DoH/DoT** (privacy) | 53 / 853 |
| Syslog 514/UDP (User Datagram Protocol) | **Syslog over TLS** | 6514 |
| Kerberos / NTP (Network Time Protocol) / RDP (Remote Desktop Protocol) | already encrypted (RDP is high-value — restrict) | 88 / 123 / 3389 |

- **Transport method** — pick the protocol that provides confidentiality + integrity for the data's sensitivity.

### Email security

- **SPF** (Sender Policy Framework) — DNS (Domain Name System) TXT record listing which mail servers may send for the domain (guards the envelope `MAIL FROM`).
- **DKIM** (DomainKeys Identified Mail) — sender signs the message with a private key; recipient verifies via the DNS-published public key (integrity + origin).
- **DMARC (Domain-based Message Authentication, Reporting & Conformance)** — policy (`none` / `quarantine` / `reject`) built on SPF + DKIM **alignment**, plus **aggregate/forensic reporting**. "Emails spoofing our domain are still delivered" → **DMARC with a `reject` policy**.
- **Gateway** — anti-spam, anti-malware, sandbox detonation of attachments/links, DLP (Data Loss Prevention).
- **S/MIME (Multipurpose Internet Mail Extensions)** or PGP (Pretty Good Privacy) for end-to-end email encryption/signing.

### Endpoint security

- **EDR (Endpoint Detection and Response) / XDR (Extended Detection and Response)** — behavioral detection, isolation, rollback, threat hunting; beats signature-only AV (Antivirus).
- **HIDS (Host-based Intrusion Detection System) / HIPS (Host-based Intrusion Prevention System)**, host firewall, **FIM (file integrity monitoring)**.
- **Application control**, USB (Universal Serial Bus)/device control.
- **Hardening** (4.1) and **configuration enforcement**.

> [!TIP]
> **4.5 scenario → answer**
> - "Spoofed emails from our domain still reach inboxes" → **DMARC (Domain-based Message Authentication, Reporting & Conformance) `p=reject`** (SPF (Sender Policy Framework)/DKIM (DomainKeys Identified Mail) alone don't enforce).
> - "Block malware call-outs cheaply, network-wide" → **DNS (Domain Name System) filtering**.
> - "Enforce settings on 5,000 Windows PCs" → **Group Policy**.
> - "Replace this cleartext protocol" → its **encrypted variant on the secure port** (memorize the table).
> - "Detect and *respond* to endpoint threats, not just alert" → **EDR/XDR**.

---

## 4.6 Identity and Access Management (IAM)

### Lifecycle

- **Provisioning** — create the account with least privilege on hire/role change; **SCIM (System for Cross-domain Identity Management)** automates.
- **Identity proofing** — verify the person before issuing credentials.
- **De-provisioning** — disable **immediately** on termination (biggest insider-risk gap); remove access, reclaim tokens/devices.
- **Access recertification / attestation** — periodic review that access is still appropriate.

### Authentication

- **Factors:** something you **know** (password, PIN (Personal Identification Number)), **have** (token, smart card, phone, security key), **are** (biometric), plus **somewhere you are** (geolocation) and **something you do** (behavioral). Two of the *same* type ≠ MFA (Multifactor Authentication).
- **MFA implementations:** authenticator app (**TOTP (Time-based One-Time Password)**), push, **FIDO2 (Fast IDentity Online 2) / passkeys / hardware security key (U2F)** — *phishing-resistant*; **SMS (Short Message Service)/email OTP (One-Time Password)** — weakest (SIM (Subscriber Identity Module) swap, interception).
- **Biometrics:** fingerprint, face, iris, retina, voice, gait. Metrics: **FAR** (false accept — security risk), **FRR** (false reject — usability), **CER (Crossover Error Rate)/EER** (crossover — lower is a better sensor).
- **Passwordless** — FIDO2 passkey + device biometric; no shared secret to phish or reuse.

### Authorization models

| Model | Basis for access |
|---|---|
| **DAC** (discretionary) | The resource **owner** grants permissions (NTFS (New Technology File System) ACLs) |
| **MAC** (mandatory) | System-enforced **labels/clearances** (government; user can't change) |
| **RBAC** (role-based) | Permissions attached to **job roles**; users get roles |
| **ABAC** (attribute-based) | Policy over **attributes/context** (user dept + device posture + time + location) — most granular |
| **Rule-based** | Global if/then rules (ACL (Access Control List) conditions, time-of-day) |
| **PBAC (Policy-Based Access Control)** | Policy-based; overlaps ABAC |

- **Least privilege**, **need to know**, **separation of duties**, **job rotation**, **mandatory vacation** — detect/deter fraud and collusion.

### Federation & SSO

- **SSO (Single Sign-On)** — authenticate once, access many services.
- **SAML (Security Assertion Markup Language)** — XML (Extensible Markup Language)-based browser SSO for enterprise web apps (IdP ↔ SP, Service Provider).
- **OAuth 2.0** — **authorization / delegated access** (grants an app scoped access to your resources) — *not authentication by itself*.
- **OpenID Connect (OIDC)** — an identity layer **on top of OAuth 2.0** — modern web/mobile authentication.
- **Kerberos** — on-prem AD (Active Directory); KDC (Key Distribution Center) issues tickets (TGT (Ticket Granting Ticket (Kerberos)) → service tickets); ports 88; watch for **golden/silver ticket**, **Kerberoasting**, **pass-the-ticket**.
- **LDAP (Lightweight Directory Access Protocol (Secure)) / LDAPS (Lightweight Directory Access Protocol (Secure))** — directory queries; **RADIUS (Remote Authentication Dial-In User Service) / TACACS (Terminal Access Controller Access-Control System)+** — AAA (see Domain 1.2).
- **IdP (Identity Provider)** — the authoritative source of identity in a federation.

### Privileged access management (PAM)

- **Just-in-time (JIT) access** — elevate only for a task, then auto-revoke.
- **Password vaulting** — check out credentials; auto-rotate on check-in; humans never know the password.
- **Ephemeral / temporal credentials**, **session recording & monitoring** for admin sessions.

### Password concepts

- **Policy:** length **> complexity** (long passphrases), screen against **breached-password lists**, no forced periodic expiration unless compromise is suspected (NIST (National Institute of Standards and Technology) SP, Special Publication, 800-63B), lockout/throttling on failed attempts.
- **Password managers** — unique strong passwords per site.
- **Age / history / reuse** settings; **complexity** where required by policy.

> [!TIP]
> **4.6 scenario → answer**
> - "Users manage a password manager *and* an authenticator app; want one secure, smooth login" → **FIDO2 (Fast IDentity Online 2) passkeys with device biometrics** (passwordless).
> - "Access based on department + device compliance + time of day" → **ABAC (Attribute-Based Access Control)**.
> - "Owner of the file decides who can read it" → **DAC (Discretionary Access Control)**. "Clearance labels, user can't override" → **MAC (Mandatory Access Control)**.
> - "Delegate an app limited access to your Google Drive" → **OAuth 2.0**. "Log into a site *with* your Google identity" → **OIDC (OpenID Connect)**.
> - "Admins should not know the root password and every session must be recorded" → **PAM** (vaulting + session recording + JIT (Just-in-Time (access))).
> - "Former employee still had access" → **de-provisioning gap** + **access recertification**.

---

## 4.7 Automation and Orchestration

### Use cases

User/resource **provisioning**, **guardrails** (auto-block non-compliant deploys), **security groups**, **ticket creation/escalation**, **CI (Continuous Integration)/CD (Continuous Deployment)** security gates, **service enablement**, **APIs**, integrations, **continuous integration/testing**, incident response playbooks (SOAR).

### Benefits

Efficiency / time savings, **consistency / enforced baselines**, reduced **human error**, scale, faster reaction time, **workforce multiplier**, standard secure configurations, employee retention (less drudgery).

### Considerations / risks

- **Complexity** and a learning curve.
- **Cost** to build/maintain.
- **Single point of failure** — the automation platform itself.
- **Technical debt** and brittle scripts.
- **Ongoing supportability** — who maintains it when the author leaves.
- A bad automation **scales the mistake** as fast as a good one → test, review, least privilege for automation accounts.

> [!TIP]
> **4.7 angle**
> "Why automate?" → **consistency + fewer errors + speed at scale**. "Risk of automation?" → **a flawed script propagates the flaw everywhere** and the platform becomes a SPOF (Single Point of Failure).

---

## 4.8 Incident Response

### Lifecycle (NIST SP 800-61) — recite in order

**Preparation → Detection → Analysis → Containment → Eradication → Recovery → Lessons Learned** (post-incident activity).

- **Preparation** — plan, tooling, training, comms tree, jump kit, hardened baselines.
- **Detection & Analysis** — identify and validate the incident; scope it; classify severity; declare.
- **Containment** — limit spread: isolate the host (keep it powered if you need memory), block IOCs, disable accounts. Short-term vs. long-term containment.
- **Eradication** — remove malware/artifacts, close the entry vector, reset credentials, rebuild.
- **Recovery** — restore from clean backups, validate, monitor closely for recurrence, phased return to production.
- **Lessons Learned** — after-action report (usually within ~2 weeks): what happened, timeline, what worked, gaps, action items.

### Supporting concepts

- **Training** and **testing:** **tabletop exercise** (discussion), **walkthrough**, **simulation** (e.g. simulated phishing), **penetration test**.
- **Root cause analysis (RCA)** — determine the underlying cause, not just the symptom.
- **Threat hunting** — proactively search for TTPs/IOCs *before* an alert exists (no signature yet).
- **Digital forensics:**
  - **Legal hold** — preserve potentially relevant data; suspend normal deletion.
  - **Chain of custody** — documented who handled evidence, when, and why — unbroken.
  - **Acquisition** — bit-for-bit **image**; hash before and after; work from the copy. **Order of volatility**: CPU (Central Processing Unit) registers/cache → RAM (Random Access Memory) → routing/ARP (Address Resolution Protocol)/process table/network connections → temp files/swap → **disk** → remote logs/monitoring → **archival media/backups**.
  - **Reporting**, **preservation**, **e-discovery**.
- **Communication / stakeholder management**, **regulatory reporting** timelines, out-of-band comms if email is compromised.
- **Playbooks** (SOAR) codify repeatable response.

> [!TIP]
> **4.8 traps**
> - Order the lifecycle exactly — a common question type. **Containment before eradication before recovery.**
> - "Preserve volatile evidence" → capture **RAM before disk** (order of volatility); don't power off yet.
> - "Proactively look for an attacker we have no alert for" → **threat hunting**.
> - "Make sure we don't destroy evidence that might be needed in court" → **legal hold** + **chain of custody**.
> - After recovery, the deliverable is the **lessons-learned / after-action report** with action items.

---

## 4.9 Data Sources for Investigations

### Log & artifact sources (map the question to the source)

| You need… | Source |
|---|---|
| What **executable/process** ran, when, parent process, hashes | **Endpoint / EDR (Endpoint Detection and Response) logs**, Sysmon |
| **Source IP (Internet Protocol)** of a scan, what was **allowed/denied**, port activity, C2 (Command and Control) **destination** | **Firewall logs** |
| Known-bad signature / exploit attempt on the wire | **IDS (Intrusion Detection System)/IPS (Intrusion Prevention System) logs** |
| Exact malicious **input** (SQLi string), app errors, transactions | **Application logs** |
| Logins, lockouts, privilege use, new accounts, Kerberos anomalies | **Authentication / OS (Operating System) security logs** |
| **DNS (Domain Name System)** tunneling, DGA (Domain Generation Algorithm) domains, resolution of known-bad names | **DNS logs** |
| Email routing, spoofed sender, attachment/link path | **Email / mail gateway logs** |
| Web requests, categories, blocked URLs, user-agent anomalies | **Web / proxy logs** |
| Traffic volume & relationships (no payload) | **NetFlow / flow data** |
| Full packet content | **Packet captures (PCAP)** |
| Consolidated, correlated, cross-source view + timeline | **SIEM (Security Information and Event Management)** |
| Device health, interface errors, up/down | **SNMP (Simple Network Management Protocol) / device metrics** |
| System/service events, crashes, driver loads | **OS / system logs** (Event Viewer, `journald`, `/var/log`) |
| Vulnerability posture at the time | **Vulnerability scan output** |
| File added/modified/deleted | **File integrity monitoring** |

### Other investigation data

- **Dashboards** — quick situational awareness, not raw evidence.
- **Automated reports** — trends, compliance.
- **Metadata** — email headers (`Received`, `Return-Path`, `Authentication-Results`), file metadata (EXIF (Exchangeable Image File Format), timestamps), document properties.

> [!TIP]
> **4.9 reflexes**
> - "Analyst needs more data about the **executable** on a laptop" → **endpoint log** (not application, not IPS (Intrusion Prevention System)).
> - "Find the **source IP (Internet Protocol)** of the port scan" / "where is the compromised host **calling out** to" → **firewall log** (DNS (Domain Name System) log also valid for the callout domain).
> - "Exact command used in the SQL (Structured Query Language) injection" → **application log**.
> - "Reconstruct the timeline across firewall + AD (Active Directory) + endpoint" → **SIEM** (correlation).
> - "See the actual data that was exfiltrated" → **packet capture**; "just confirm a large transfer happened" → **NetFlow**.

---

## Domain 4 — 90-second self-check

1. Recite the IR (Incident Response) lifecycle in order. Which two steps do people most often swap? *(containment/eradication)*
2. Order of volatility, top to bottom.
3. SIEM (Security Information and Event Management) vs SOAR (Security Orchestration, Automation, and Response) — one sentence each.
4. NetFlow vs PCAP (Packet Capture) — what does each give you?
5. Which log for: executable details / source IP (Internet Protocol) of a scan / exact SQLi string / login anomalies / DNS (Domain Name System) tunneling?
6. EDR (Endpoint Detection and Response) vs traditional AV (Antivirus) — the key difference. *(behavioral detection + response/isolation vs signature matching)*
7. DAC (Discretionary Access Control) / MAC (Mandatory Access Control) / RBAC (Role-Based Access Control) / ABAC (Attribute-Based Access Control) — basis for access in each.
8. OAuth 2.0 vs OIDC (OpenID Connect).
9. SPF (Sender Policy Framework) vs DKIM (DomainKeys Identified Mail) vs DMARC (Domain-based Message Authentication, Reporting & Conformance) — and which one you set to `reject` to stop domain spoofing.
10. After patching a vulnerability, what's the mandatory next step? *(rescan / re-test to verify)*
11. SSD (Solid State Drive) for reuse — sanitization method? *(crypto-erase / secure erase; not degauss)*
12. Passwordless login that's phishing-resistant → ? *(FIDO2 (Fast IDentity Online 2) passkey + biometric)*
