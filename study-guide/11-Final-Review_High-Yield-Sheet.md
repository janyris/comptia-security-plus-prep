# Final Review — High-Yield Sheet

The one file to read in the last 48 hours. Everything here is "know it cold." Deeper explanations are in the domain files; the exact tables are in [06-Reference_Ports-Protocols-Crypto](06-Reference_Ports-Protocols-Crypto.md) and [08-Reference_Orderings-and-Formulas](08-Reference_Orderings-and-Formulas.md).

---

## Must-recite sequences

- **IR (Incident Response) lifecycle:** Preparation → Detection → Analysis → **Containment → Eradication → Recovery** → Lessons Learned.
- **Order of volatility:** CPU (Central Processing Unit) registers/cache → RAM (Random Access Memory) → swap/temp → disk → remote logs → archives/backups.
- **Change management:** request → impact analysis → CAB (Change Advisory Board) approval → test → schedule → implement → verify → document → (backout ready throughout).
- **Vuln management:** identify → validate → prioritize (KEV (Known Exploited Vulnerabilities (CISA catalog)) > CVSS (Common Vulnerability Scoring System) on exposed/critical) → remediate → **rescan** → report.
- **Cert lifecycle:** keygen → CSR (Certificate Signing Request) → validation → issue → install → use → renew → **revoke (CRL (Certificate Revocation List)/OCSP (Online Certificate Status Protocol))** → destroy.
- **Cyber kill chain:** Recon → Weaponize → Deliver → Exploit → Install → C2 (Command and Control) → Actions on objectives.

## Must-calculate

- **SLE (Single Loss Expectancy) = AV (Asset Value) × EF (Exposure Factor)** · **ALE (Annualized Loss Expectancy) = SLE × ARO (Annualized Rate of Occurrence)** · buy a control if annual cost < ALE reduction.
- **RTO (Recovery Time Objective) + WRT (Work Recovery Time) ≤ MTD (Maximum Tolerable Downtime).** RPO (Recovery Point Objective) = tolerable data loss in time → backup frequency.
- Five nines (99.999%) ≈ 5 min/year downtime.

---

## Control types

- **Categories:** Managerial · Operational · Technical · Physical.
- **Functional:** Preventive · Detective · Corrective · Deterrent · Directive · Compensating.
- Compensating = substitute *because the primary isn't feasible*. Deterrent = discourages only. AUP (Acceptable Use Policy) = managerial + directive + preventive.

## Crypto one-liners

- Symmetric = one key, fast, bulk (AES (Advanced Encryption Standard)-256). Asymmetric = key pair, slow, exchange/signatures (RSA (Rivest-Shamir-Adleman), ECC (Elliptic Curve Cryptography)).
- **Sign** with sender's **private**; **verify** with sender's **public**. **Encrypt** with recipient's **public**; **decrypt** with recipient's **private**.
- Hash = one-way integrity (SHA (Secure Hash Algorithm)-256; MD5 (Message Digest 5)/SHA-1 broken). **Salt** = unique, not secret, defeats rainbow tables. Key stretching = bcrypt/scrypt/PBKDF2 (Password-Based Key Derivation Function 2)/Argon2.
- **PFS (Perfect Forward Secrecy)** = ephemeral DH (ECDHE) → past sessions safe if the long-term key leaks.
- **TPM (Trusted Platform Module)** = device motherboard chip (FDE (Full Disk Encryption), boot integrity). **HSM (Hardware Security Module)** = appliance for bulk keys (CAs). **TEE (Trusted Execution Environment)/enclave** = data in use.
- **CRL (Certificate Revocation List)** = downloadable revoked list (stale-able). **OCSP (Online Certificate Status Protocol)** = real-time. **OCSP stapling** = server attaches the response.
- **Digital signature** ≠ confidentiality (message not hidden).
- Root CA (Certificate Authority) = self-signed, offline. CSR (Certificate Signing Request) carries the **public** key; private key never leaves you.

## Data protection

- States: **at rest** (FDE (Full Disk Encryption)/DB (Database) encryption) · **in transit** (TLS (Transport Layer Security)/IPsec/VPN (Virtual Private Network)) · **in use** (secure enclave/TEE (Trusted Execution Environment)).
- **Tokenization** (vault, reversible, removes PCI scope) vs **masking** (partial display) vs **hashing** (one-way).
- **DLP (Data Loss Prevention)** = stop exfiltration via email/USB (Universal Serial Bus)/cloud/endpoint. **Data sovereignty** = local law applies where data sits.
- Owner sets classification; controller decides why/how (GDPR); processor acts for the controller; custodian protects day-to-day.

---

## Threats

- Actors: **nation-state** (espionage, resources), **organized crime** (money), **hacktivist** (ideology), **insider** (has access), **unskilled** (uses tools), **shadow IT** (unsanctioned, not malicious).
- Social engineering: phishing (email) · vishing (voice) · smishing (SMS) · whaling (exec target) · **BEC** (uses exec identity for fraud) · pretexting (fake scenario) · **typosquatting** (look-alike URL (Uniform Resource Locator)) · **pharming** (poisoned DNS (Domain Name System), correct URL) · watering hole (site you visit) · supply chain (vendor you use).
- Influence principles: authority, urgency, scarcity, consensus/social proof, familiarity/liking, intimidation, trust.
- Malware: **worm** (self-spread, no user action) · **virus** (host file + user action) · **trojan/RAT (Remote Access Trojan)** · **ransomware** (encrypt + note + kills shadow copies) · **rootkit/bootkit** (hides at kernel/boot) · **logic bomb** (condition trigger) · **keylogger** · **botnet/C2** (beaconing) · **fileless** (memory/PowerShell/LOLBins).
- Password attacks: **spraying** (few passwords × many accounts) · **stuffing** (reused breach creds) · **brute force** (all combos, one account) · **dictionary** · **rainbow table** (beaten by salt).
- Vuln classes: zero-day, misconfiguration, injection (SQLi/command), XSS (Cross-Site Scripting)/CSRF (Cross-Site Request Forgery)/SSRF (Server-Side Request Forgery), buffer overflow, **race condition/TOCTOU (Time-of-Check to Time-of-Use)**, VM (Virtual Machine) escape, resource reuse, EOL (End of Life)/legacy, supply chain, weak/deprecated crypto, side-loading/jailbreak.

## Mitigations / hardening (say 8)

Encryption · endpoint protection (EDR) · host firewall · host IPS (Intrusion Prevention System) · disable unused ports/protocols · change default passwords · remove unnecessary software · secure baseline + configuration enforcement · least functionality · patch management · application allow-listing · segmentation · least privilege · MFA (Multifactor Authentication) · input validation + parameterized queries · Group Policy / MDM (Mobile Device Management) · SELinux/AppArmor · decommission EOL (End of Life).

Map: injection → input validation · unauthorized software → allow-listing · lateral movement → segmentation · stolen credentials → MFA · config drift → baseline + enforcement · can't patch → compensating control (segment + IPS/WAF (Web Application Firewall) virtual patch).

---

## Architecture

- **Shared responsibility:** IaaS customer owns OS (Operating System)/app/**data**/IAM (Identity and Access Management); SaaS customer owns data/access/config only; provider always owns physical.
- **Zero Trust:** control plane = Policy Engine + Administrator (= PDP (Policy Decision Point)), adaptive identity, threat-scope reduction; data plane = **Policy Enforcement Point**.
- Zones: **screened subnet (DMZ)** for internet-facing services; management network out-of-band; microsegmentation controls east-west.
- Appliances: **IDS (Intrusion Detection System)** out-of-band/alert · **IPS (Intrusion Prevention System)** inline/block · **WAF (Web Application Firewall)** L7 (Layer 7) web attacks · **NAC (Network Access Control)** posture + 802.1X · **jump server** hardened entry · **load balancer** availability + health checks · **proxy** outbound filter · **reverse proxy** in front of servers.
- **Fail-closed** = security; **fail-open** = availability.
- IPsec: **ESP** (encrypt) not AH (Authentication Header (IPsec — integrity, no encryption)); **tunnel** mode site-to-site. **Split tunnel** = only corp traffic (fast, riskier) vs **full tunnel** (all traffic, secure).
- Sites: **hot** (minutes) · **warm** (hours) · **cold** (days). Backups: full (fast restore) / incremental (fast backup, slow restore) / differential (full + one). **3-2-1**; immutable/offline copy beats ransomware.
- **UPS (Uninterruptible Power Supply)** = brownout + minutes bridge; **generator** = hours-days.
- OT (Operational Technology)/ICS (Industrial Control System)/SCADA (Supervisory Control and Data Acquisition): unencrypted native protocols, legacy, **availability first** → segment + data diodes.

---

## Operations

- **SIEM (Security Information and Event Management)** aggregate/correlate/alert; **SOAR (Security Orchestration, Automation, and Response)** automate response; **XDR (Extended Detection and Response)** cross-layer detect+respond; **EDR (Endpoint Detection and Response)** behavioral endpoint detect+respond (beats signature AV (Antivirus)).
- **NetFlow** = who/when/how-much (no payload); **PCAP (Packet Capture)** = full content.
- **Logs → source:** executable = endpoint; scan source IP (Internet Protocol) / allow-deny = firewall; exploit signature = IDS (Intrusion Detection System)/IPS (Intrusion Prevention System); SQLi string / app error = application; logins/lockouts = auth/OS (Operating System) security; DNS (Domain Name System) tunneling = DNS; email spoof path = mail gateway; timeline across all = SIEM.
- **Asset disposal:** SSD (Solid State Drive) reuse → **crypto-erase / secure erase** (degauss does nothing to SSD); off-site destruction → **certificate of destruction**.
- **IAM (Identity and Access Management):** provision least-privilege → **de-provision immediately on termination** → recertify periodically. **SCIM (System for Cross-domain Identity Management)** automates provisioning.
- **MFA (Multifactor Authentication) factors:** know / have / are / somewhere you are / something you do — two *different* categories. Phishing-resistant = **FIDO2 (Fast IDentity Online 2) / passkey / security key**; weakest = **SMS (Short Message Service) OTP (One-Time Password)**.
- **Access models:** DAC (owner) · MAC (labels) · RBAC (role) · ABAC (attributes/context).
- **Federation:** SAML (enterprise web SSO (Single Sign-On), XML (Extensible Markup Language)) · **OAuth 2.0** (delegated authorization) · **OIDC** (authentication on OAuth) · Kerberos (on-prem AD (Active Directory) tickets).
- **PAM (Privileged Access Management):** vaulting + auto-rotation + **JIT (Just-in-Time (access))** + session recording.
- **Password policy (NIST):** length > complexity, screen against breach lists, no forced periodic expiry unless compromised.
- **Automation:** benefits = consistency, fewer errors, speed at scale; risks = a bad script scales the mistake, platform is a SPOF (Single Point of Failure).
- **Email:** SPF (allowed senders) · DKIM (signature/integrity) · **DMARC** (`none`/`quarantine`/**`reject`** + reporting) — set **`reject`** to stop domain spoofing.
- **Secure protocol swaps:** Telnet→SSH (Secure Shell) 22 · FTP (File Transfer Protocol)→SFTP (SSH File Transfer Protocol) 22 / FTPS (File Transfer Protocol Secure) 990 · HTTP (Hypertext Transfer Protocol)→HTTPS (HTTP over TLS) 443 · LDAP (Lightweight Directory Access Protocol (Secure))→LDAPS (Lightweight Directory Access Protocol (Secure)) 636 · SMTP (Simple Mail Transfer Protocol)→SMTPS (SMTP Secure) 465/587 · POP3 (Post Office Protocol version 3)→995 · IMAP (Internet Message Access Protocol)→993 · SNMPv1/2→SNMPv3 · DNS→DNSSEC (DNS Security Extensions)/DoH 443/DoT 853 · Syslog→6514.
- **Forensics:** legal hold + chain of custody; bit-for-bit image, hash before/after; order of volatility; RCA (Root Cause Analysis) finds the underlying cause; **threat hunting** = look before an alert exists.

---

## Program management

- Docs: **policy** (intent, mandatory) > **standard** (specific, mandatory) > **procedure** (steps, mandatory) > **guideline** (optional).
- Risk responses: **mitigate / transfer (insurance) / accept (exception=temp, exemption=long-term) / avoid**.
- Risk terms: inherent (before) · residual (after) · **appetite** (strategic willingness: expansionary/neutral/conservative) · **tolerance** (variance around appetite). **Risk register** = owner + likelihood + impact + response + status; **KRI (Key Risk Indicator)** = early-warning metric.
- BIA (Business Impact Analysis): **MEF (Mission Essential Function)** restored first; RTO (Recovery Time Objective)/RPO (Recovery Point Objective)/WRT (Work Recovery Time)/MTD (Maximum Tolerable Downtime)/MTBF (Mean Time Between Failures)/MTTR (Mean Time To Repair / Recover).
- Agreements: **SOW** (scope/cost/timeline) · **MSA** (umbrella) · **SLA** (measurable targets + penalties) · **MOU** (non-binding intent) · **MOA** (binding responsibilities) · **NDA (Non-Disclosure Agreement)** · **BPA (Business Partnership Agreement)** · **ISA** (interconnect two networks) · **right-to-audit clause**.
- Vendor: due diligence before, ongoing monitoring after; conflict of interest (financial/personal/competitive/insider); RoE defines access & reporting.
- Compliance: non-compliance → fines, sanctions, reputational damage, license loss, contract impact. **Due diligence** (assess before) vs **due care** (protect continuously).
- Privacy: GDPR (General Data Protection Regulation) — **controller** decides why/how, **processor** acts for it; **right to be forgotten**; 72-hour breach notice; data sovereignty. PCI-DSS = contractual industry standard, not law.
- Audits: **attestation** (independent assurance, e.g. SOC 2, System and Organization Controls 2); internal (self-assessment, audit committee) vs external (regulatory, independent third-party audit).
- Pen test env: **known** = insider sim; **unknown** = external attacker sim; pen test *exploits*, scan only *identifies*. Active recon = detectable; passive = OSINT (Open-Source Intelligence).
- Awareness: after tailgating/phishing incidents → **situational awareness** / phishing simulation; measure by **click rate + report rate over time**.

---

## The 15 distinctions most likely to decide a question

IDS (Intrusion Detection System)/IPS (Intrusion Prevention System) · SIEM (Security Information and Event Management)/SOAR (Security Orchestration, Automation, and Response) · RTO (Recovery Time Objective)/RPO (Recovery Point Objective) · MTD (Maximum Tolerable Downtime) vs both · symmetric/asymmetric · hashing/encryption · tokenization/masking/hashing · TPM (Trusted Platform Module)/HSM (Hardware Security Module)/TEE (Trusted Execution Environment) · DAC (Discretionary Access Control)/MAC (Mandatory Access Control)/RBAC (Role-Based Access Control)/ABAC (Attribute-Based Access Control) · OAuth/OIDC (OpenID Connect) · AH (Authentication Header (IPsec — integrity, no encryption))/ESP (Encapsulating Security Payload (IPsec — encryption + integrity)) · MOU (Memorandum of Understanding)/MOA (Memorandum of Agreement) · SOW (Statement of Work) vs MSA (Master Service Agreement) vs SLA (Service-Level Agreement) · vuln scan / pen test · known vs unknown environment · preventive/detective/deterrent/compensating · whaling/BEC (Business Email Compromise) · typosquatting/pharming · worm/virus/trojan · spraying/stuffing/brute force · SPF (Sender Policy Framework)/DKIM (DomainKeys Identified Mail)/DMARC (Domain-based Message Authentication, Reporting & Conformance) · full/split tunnel · fail-open/fail-closed · due diligence/due care · risk appetite/tolerance.
