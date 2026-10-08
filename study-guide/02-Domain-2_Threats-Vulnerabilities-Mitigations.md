# Domain 2 — Threats, Vulnerabilities, and Mitigations (22%)

**Objectives:** 2.1 Compare and contrast common threat actors and motivations · 2.2 Explain common threat vectors and attack surfaces · 2.3 Explain various types of vulnerabilities · 2.4 Given a scenario, analyze indicators of malicious activity · 2.5 Explain the purpose of mitigation techniques used to secure the enterprise.

---

## 2.1 Threat Actors and Motivations

### Actors — know *resources*, *sophistication*, *intent*

| Actor | Resources / skill | Typical motivation | Internal/External |
|---|---|---|---|
| **Nation-state / APT (Advanced Persistent Threat)** | Very high; funded, patient, custom tools, zero-days | Espionage, strategic advantage, disruption, war | External |
| **Organized crime** | High; professional, buys tools/access | **Financial gain** (ransomware, fraud, data theft) | External |
| **Hacktivist** | Low–moderate | **Ideology / political** message; disruption, doxxing, defacement | External |
| **Insider threat** | Low–moderate, but **already has access & knowledge** | Revenge, financial, espionage; also *unintentional* | Internal |
| **Unskilled attacker** ("script kiddie") | Very low; runs existing tools/scripts | Thrill, notoriety, chaos | External |
| **Shadow IT** | Varies; employees standing up unsanctioned tech | Not malicious — convenience/speed; creates unmanaged risk | Internal |
| **Competitor** | Varies | Corporate espionage, market advantage, sabotage | External |

### Attributes & motivations vocabulary

- **Attributes:** internal vs. external; resources/funding; sophistication/capability.
- **Motivations:** data exfiltration, espionage, service disruption, **financial gain**, blackmail/extortion, ideology/**philosophical or political beliefs**, ethical (authorized testing), revenge, war, disruption/chaos.

> [!TIP]
> **2.1 tells**
> Match one unique detail: "sophisticated, long dwell time, targeted our R&D" → nation-state/APT (Advanced Persistent Threat). "Encrypted files + ransom note" → organized crime. "Website defaced with a political statement" → hacktivist. "Former admin still had VPN (Virtual Private Network) creds" → insider. "Employee signed up for a SaaS with a credit card" → shadow IT.

---

## 2.2 Threat Vectors and Attack Surfaces

### Vectors (how the attack gets in)

- **Message-based:** email (phishing/malware/links), SMS (smishing), instant messaging.
- **Image / file-based:** malicious macros in Office docs, PDFs, embedded objects, malformed images.
- **Voice call:** vishing, pretext calls.
- **Removable device:** malicious USB (HID (Human Interface Device)/"Rubber Ducky", infected drive left in a parking lot).
- **Vulnerable software:** client-based (installed agent has a flaw) vs. **agentless** (browser, web portal).
- **Unsupported systems / applications:** EOL (End of Life), no patches.
- **Unsecure networks:** open/rogue **wireless**, **wired** with no port security, **Bluetooth** (bluejacking/bluesnarfing).
- **Open service ports:** unnecessary listening services.
- **Default credentials:** unchanged admin/admin.
- **Supply chain:** compromised **MSP (Managed (Security) Service Provider)**, **vendor**, or **supplier**/hardware.
- **Human / social engineering** (below).

### Social engineering — get the exact term

| Attack | Precise definition |
|---|---|
| **Phishing** | Fraudulent email to many recipients |
| **Spear phishing** | Phishing targeted at a specific person/group using tailored detail |
| **Whaling** | Spear phishing aimed at senior executives |
| **Vishing** | Voice/phone phishing |
| **Smishing** | SMS (Short Message Service)/text phishing |
| **Business Email Compromise (BEC)** | Compromised or spoofed *legitimate* business account used to request wire transfers / gift cards / data; urgency + authority + secrecy |
| **Pretexting** | Inventing a believable scenario to justify the request |
| **Impersonation** | Pretending to be a specific trusted person/role |
| **Watering hole** | Compromise a third-party site the target group is known to visit |
| **Typosquatting / URL (Uniform Resource Locator) hijacking** | Register a look-alike domain (`rnicrosoft.com`, `MotorCoood` vs `MotorCood`) |
| **Pharming** | Redirect users to a malicious site via poisoned DNS (Domain Name System) / hosts file — *correct URL, wrong destination* |
| **Brand impersonation** | Fake site/email/app using a real company's branding |
| **Disinformation / misinformation** | Planting false narratives (influence campaigns) |
| **Pretexting via "help desk"** | Attacker calls IT pretending to be a user (or vice-versa) to reset a credential |
| Physical: **shoulder surfing** | Reading a screen/keypad over someone's shoulder → privacy screens |
| Physical: **dumpster diving** | Recovering info from trash → shredding, clean-desk policy |
| Physical: **tailgating / piggybacking** | Following someone through a secure door (unknowing / knowing) |

**Principles of influence (be able to name the *principle*, not just the attack):** **authority**, **intimidation**, **consensus/social proof**, **scarcity**, **urgency**, **familiarity/liking**, **trust**.

> [!TIP]
> **2.2 traps**
> - Voice = vishing; text = smishing; QR (Quick Response) code = *quishing* (a form of phishing). Don't mix them up.
> - **Typosquatting** = you mistyped/clicked a look-alike URL. **Pharming** = you typed it correctly but DNS sent you elsewhere.
> - **Watering hole** = they hacked a *site you visit*. **Supply chain** = they hacked a *vendor/component you use*.
> - "CEO (Chief Executive Officer) emails accounting for an urgent wire transfer" → **BEC** (whaling is targeting the *exec*; BEC is the *fraud using* an exec identity).

---

## 2.3 Types of Vulnerabilities

### Application / code

- **Memory injection** — attacker inserts code/data into a running process's memory.
- **Buffer overflow** — writing past a buffer's bounds overwrites adjacent memory (can hijack execution). Mitigate: bounds checking, safe languages, **ASLR (Address Space Layout Randomization)**, **DEP (Data Execution Prevention)/NX (No-eXecute)**, stack canaries.
- **Race condition / TOCTOU (time-of-check to time-of-use)** — state changes between the check and the use. Mitigate: locking, atomic operations.
- **Injection** — **SQLi** (database), command injection, LDAP (Lightweight Directory Access Protocol (Secure)) injection, XML (Extensible Markup Language) injection. Mitigate: **input validation (allow-list)** + **parameterized queries / prepared statements**.
- **Cross-Site Scripting (XSS)** — reflected, stored/persistent, DOM-based; runs attacker script in a victim's browser. Mitigate: **output encoding**, input validation, **Content Security Policy (CSP)**.
- **Cross-Site Request Forgery (CSRF/XSRF)** — tricks an authenticated browser into sending a state-changing request. Mitigate: anti-CSRF tokens, SameSite cookies.
- **SSRF (Server-Side Request Forgery)** — the server is tricked into making requests to internal resources.
- **Directory traversal** — `../../etc/passwd` to reach files outside the web root.
- **Privilege escalation** — vertical (user → admin) or horizontal (user → another user).
- **Replay** — captured valid data (session token, auth) is resent. Mitigate: nonces, timestamps, session expiration.

### Operating system / hardware / firmware

- **OS (Operating System) vulnerabilities** — unpatched kernel/service flaws; mitigate with patch management + hardening.
- **Firmware** — BIOS (Basic Input/Output System)/UEFI (Unified Extensible Firmware Interface), device firmware; **end-of-life (EOL)** and **legacy** hardware get no updates.
- **Side-loading / jailbreaking / rooting** — bypassing OS controls to install unsigned code.

### Virtualization & cloud

- **VM (Virtual Machine) escape** — breaking out of a guest VM to the hypervisor/host.
- **VM sprawl** — unmanaged, unpatched VMs proliferate.
- **Resource reuse** — memory/storage reassigned to a new tenant still holds old data.
- **Cloud-specific:** misconfigured storage buckets/security groups, over-permissive IAM (Identity and Access Management), exposed management consoles/keys, insecure APIs. **Misconfiguration is the #1 cloud breach cause.**

### Supply chain

- Compromised **hardware** (implants, counterfeit parts), **software** (poisoned update — SolarWinds pattern), or **service provider / MSP (Managed (Security) Service Provider)**.
- Mitigate: vendor due diligence, **SBOM (Software Bill of Materials)**, code signing verification, **SLSA**/provenance, least-privilege vendor access.

### Misc

- **Zero-day** — no patch exists yet; the vendor may not know. Detection relies on behavior/anomaly, not signatures.
- **Misconfiguration** — open ports, default creds, weak permissions, verbose errors, disabled logging.
- **Mobile:** side-loading, jailbreak/root, insecure app data storage, excessive permissions.

---

## 2.4 Analyzing Indicators of Malicious Activity

### Malware types

| Type | Defining behavior |
|---|---|
| **Virus** | Attaches to a host file; needs **user action** to execute/spread (boot, macro, script, polymorphic, metamorphic) |
| **Worm** | **Self-propagates** over the network with **no user action** and no host file (classic: TCP (Transmission Control Protocol) 445/SMB (Server Message Block) spikes, fast spread) |
| **Trojan** | Disguised as legitimate software; opens the door for other payloads. **RAT** = Remote Access Trojan (full remote control) |
| **Ransomware** | Encrypts data (or whole disk) and demands payment; often deletes **shadow copies**/backups first; double-extortion = also steals data |
| **Rootkit** | Hides itself and other malware at OS (Operating System)/kernel/**boot** level; very hard to detect from the running OS → detect via **measured/secure boot**, offline scan |
| **Bootkit** | Rootkit in the boot process/UEFI (Unified Extensible Firmware Interface) |
| **Keylogger** | Captures keystrokes (software or hardware inline device) |
| **Spyware** | Covertly collects browsing/usage/credential data |
| **Bloatware** | Unwanted pre-installed software — attack surface, not malware per se |
| **Logic bomb** | Dormant code that triggers on a condition/date/event (often insider-planted) |
| **Botnet / C2 (Command and Control)** | Compromised hosts ("bots/zombies") controlled from **command-and-control**; used for DDoS, spam, mining. Look for **beaconing** (regular callback intervals) |
| **Fileless malware** | Lives in memory / uses PowerShell, WMI (Windows Management Instrumentation), `mshta`, `regsvr32` (LOLBins); no executable on disk |
| **Cryptomalware / cryptojacking** | Uses victim resources to mine cryptocurrency (high CPU (Central Processing Unit)/GPU (Graphics Processing Unit), thermal) |

### Attack indicators by category

- **Physical:** brute-forcing a door, RFID (Radio-Frequency Identification) cloning, environmental tampering, malicious USB (Universal Serial Bus).
- **Network:** **DDoS** (volumetric, protocol, application-layer / amplification & reflection), **DNS (Domain Name System) attacks** (poisoning/spoofing, domain hijacking, DNS tunneling for C2 (Command and Control)/exfil, DGA (Domain Generation Algorithm) domains), **on-path** (formerly MITM (Man-in-the-Middle) — ARP (Address Resolution Protocol) poisoning, rogue AP (Access Point), SSL (Secure Sockets Layer) strip), **replay**, **credential relay**, **rogue services** (rogue DHCP (Dynamic Host Configuration Protocol)/AP).
- **Wireless:** evil twin, rogue AP, deauth/**disassociation** flood, jamming, **KRACK (Key Reinstallation Attack)**, WPS (Wi-Fi Protected Setup) attacks.
- **Application:** injection, XSS (Cross-Site Scripting)/CSRF (Cross-Site Request Forgery), directory traversal, buffer overflow, privilege escalation, **API (Application Programming Interface) abuse**, session hijacking, request forgery, **resource exhaustion**.
- **Cryptographic:** **downgrade** (force weak TLS (Transport Layer Security)/cipher), **collision** (break a hash), **birthday** attack, **key-stretching absent** → fast brute force, harvest-now-decrypt-later (quantum concern).
- **Password:** **brute force**, **dictionary**, **password spraying** (few common passwords across many accounts — avoids lockout), **credential stuffing** (reusing breach dumps), **rainbow table** (defeated by salting), offline hash cracking.

### Reading the indicators (IoCs)

- Account lockouts, **impossible-travel** / concurrent-session logins, out-of-hours logins.
- Blocked/allowed content anomalies, **impossible bandwidth** or data volume, traffic to known-bad IPs/domains.
- **Resource consumption** spikes (CPU (Central Processing Unit)/RAM (Random Access Memory)/disk), **resource inaccessibility** (ransomware).
- **Out-of-cycle logging**, **missing logs** (attacker cleared them), logging suddenly disabled.
- **Published/leaked** company data appearing externally.
- New/unknown accounts, new admin group members, new scheduled tasks/services, new registry Run keys.
- Unusual **beaconing** intervals, DNS (Domain Name System) TXT-record floods, long random subdomains.

> [!TIP]
> **2.4 scenario → answer**
> - No user interaction + rapid spread + port 445 → **worm**.
> - High CPU, fans loud, unknown process phoning a mining pool → **cryptojacking**.
> - Files renamed with a new extension + `README_DECRYPT.txt` + Volume Shadow Copies gone → **ransomware**.
> - Many failed logins for *one* account → brute force/dictionary. **One or two attempts across hundreds of accounts** → **password spraying**. Valid credentials from another breach → **credential stuffing**.
> - Regular 60-second callbacks to one external IP (Internet Protocol) → **C2 (Command and Control) beaconing** (botnet).
> - Security tooling can't see it, only PowerShell/WMI (Windows Management Instrumentation) artifacts remain → **fileless**.
> - Attacker forced the session to TLS (Transport Layer Security) 1.0 / RC4 (Rivest Cipher 4) → **downgrade attack**.

---

## 2.5 Mitigation Techniques

### Segmentation & access control

- **Segmentation** (VLANs, subnets, **microsegmentation**, air-gap) — contain lateral movement; isolate legacy/OT (Operational Technology)/IoT and high-value assets.
- **Access control lists (ACLs)** — permit/deny by IP (Internet Protocol), port, protocol; top-down, first match.
- **Least privilege** — grant only what the role needs; review regularly.
- **Isolation / containment** — quarantine a host; sinkhole a domain.

### Mitigation actions

| Technique | Addresses |
|---|---|
| **Application allow-listing** | Unauthorized/unknown software, ransomware, LOLBin abuse (deny-list is weaker) |
| **Patching / patch management** | Known CVEs, EOL (End of Life) risk (with upgrade path) |
| **Input validation + parameterized queries** | Injection (SQLi, command), some XSS (Cross-Site Scripting) |
| **Output encoding + CSP (Content Security Policy)** | XSS |
| **Encryption** | Data at rest/in transit exposure, eavesdropping, lost devices |
| **Monitoring / EDR (Endpoint Detection and Response) / SIEM (Security Information and Event Management)** | Detection of what prevention missed; fileless, lateral movement |
| **Least privilege + PAM (Privileged Access Management) + JIT (Just-in-Time (access)) access** | Privilege escalation, insider misuse, stolen-credential blast radius |
| **Configuration enforcement** | Config drift, weak baselines |
| **Decommissioning** | EOL systems that can't be secured |
| **MFA (phishing-resistant)** | Credential theft, phishing, password attacks |
| **Network appliances** (NGFW (Next-Generation Firewall), WAF (Web Application Firewall), IPS (Intrusion Prevention System), NAC (Network Access Control), proxy) | Perimeter/app-layer attacks, unmanaged devices |

### Hardening techniques (memorize the list)

- **Encryption** (FDE (Full Disk Encryption), data-at-rest, transport).
- **Installation of endpoint protection** (EDR (Endpoint Detection and Response)/XDR (Extended Detection and Response), host firewall, HIPS (Host-based Intrusion Prevention System)).
- **Host-based firewall** and **host-based IPS (HIPS)**.
- **Disabling ports/protocols** not in use; **changing default passwords**.
- **Removal of unnecessary software** (reduce attack surface).
- **Secure baseline** — a hardened, documented standard config; **configuration enforcement** keeps systems on it.
- **Least functionality** — a system does only its intended job.
- **Patch management** and firmware updates.
- **Group Policy / MDM (Mobile Device Management)** to push and enforce settings.
- **SELinux / AppArmor** — mandatory access control on Linux.
- Device-specific hardening: mobile (MDM, no jailbreak), IoT (segment, change defaults, disable UPnP), ICS (Industrial Control System)/SCADA (segment, unidirectional gateways), cloud (CSPM (Cloud Security Posture Management), IaC scanning, guardrails), servers/workstations/switches/routers (see Domain 4.1).

> [!TIP]
> **2.5 "best mitigation" logic**
> Map the mitigation to the *stated* vulnerability, and pick the most **specific, preventive, root-cause** option:
> - Unauthorized software running → **application allow-listing** (not "user training").
> - Injection found in logs → **input validation** (not "WAF (Web Application Firewall)" if a code fix is offered — though a WAF is the right *compensating* control when you can't touch the code).
> - Lateral movement after one host was popped → **segmentation** + least privilege.
> - Stolen password led to account takeover → **MFA (Multifactor Authentication)**.
> - Config drift across the fleet → **secure baseline + configuration enforcement**.
> - Legacy app can't be patched → **compensating control**: isolate/segment + monitor, virtual patch via IPS/WAF.

---

## Domain 2 — 60-second self-check

1. Nation-state vs. organized crime vs. hacktivist — one motivation each.
2. Typosquatting vs. pharming — what's different about how the user ends up on the bad site?
3. Worm vs. virus vs. trojan in one clause each.
4. Password spraying vs. credential stuffing vs. brute force.
5. Which mitigation for: injection / unauthorized software / lateral movement / stolen credentials / config drift?
6. Name 6 items from the hardening list without looking.
7. VM (Virtual Machine) escape, resource reuse, VM sprawl — define each.
8. What does a WAF (Web Application Firewall) do that an input-validation code fix also does, and when do you pick the WAF? *(both stop injection/XSS (Cross-Site Scripting) reaching the app; pick the WAF as a compensating control when you can't modify the code)*
