# Domain 3 — Security Architecture (18%)

**Objectives:** 3.1 Compare and contrast security implications of different architecture models · 3.2 Given a scenario, apply security principles to secure enterprise infrastructure · 3.3 Compare and contrast concepts and strategies to protect data · 3.4 Explain the importance of resilience and recovery in security architecture.

---

## 3.1 Architecture Models & Their Security Implications

### Cloud

- **Service models & shared responsibility:**
  - **IaaS** — provider secures the physical host, storage, network. **Customer** secures OS (Operating System), runtime, apps, **data**, IAM (Identity and Access Management), network config. *(In an IaaS question, "who secures the database?" → the customer/client.)*
  - **PaaS** — provider also manages OS/runtime; customer owns apps, data, access, config.
  - **SaaS** — provider manages almost everything; customer owns **data, user access, and configuration** only.
  - The customer's security burden **shrinks** IaaS → PaaS → SaaS; the provider's grows. Provider *always* owns physical infrastructure.
- **Deployment models:** public, **private**, **hybrid**, **community**, multi-cloud.
- **Responsibility matrix** — a written table of who secures each layer; part of the contract.
- **Third-party vendors** in the chain add risk (see Domain 5.3).

### On-prem, hybrid, and specialized

- **On-premises** — full control, full responsibility, high capex.
- **Centralized** (easy to manage/monitor, single point of failure) vs. **decentralized** (resilient, harder to govern consistently).
- **Virtualization** — hypervisor **Type 1** (bare-metal, e.g. ESXi) vs **Type 2** (hosted, e.g. VirtualBox). Risks: VM (Virtual Machine) escape, VM sprawl, host compromise = all guests.
- **Containerization** — shares the host kernel; lighter than VMs, weaker isolation; secure the images and the orchestration (Kubernetes RBAC (Role-Based Access Control), admission control).
- **Serverless / Function-as-a-Service** — no servers to patch; you still own function code and its IAM (Identity and Access Management) permissions; cold-start and event-injection concerns.
- **Microservices** — small independent services; large network attack surface; use **mTLS**, API (Application Programming Interface) gateways.
- **IaC (Infrastructure as Code)** — provision from version-controlled templates; consistent and auditable; a bad template scales a misconfig everywhere → scan IaC.
- **IoT** — cheap, long-lived, weak defaults, rarely patched → segment, change defaults, disable unused services.
- **ICS (Industrial Control System) / SCADA (Supervisory Control and Data Acquisition) / OT (Operational Technology)** — controls physical processes; often native **unencrypted protocols** (Modbus, DNP3 (Distributed Network Protocol 3), S7 (Siemens S7 (industrial control protocol))), legacy/unpatchable, **availability and safety outrank confidentiality** → network segmentation, unidirectional gateways (data diodes), strict change control.
- **Embedded systems / RTOS (Real-Time Operating System)** — fixed-function, constrained, hard to update.
- **High availability** designs (see 3.4).

### Considerations when choosing/assessing an architecture

Availability, resilience, **cost**, **responsiveness**, **scalability**, ease of deployment, **risk transference**, ease of **recovery**, **patch availability** (and the risk when a component *cannot* be patched), power, and compute requirements.

### Zero-trust architecture (recap from 1.2)

Control plane (Policy Engine + Policy Administrator = PDP (Policy Decision Point); adaptive identity; threat-scope reduction; policy-driven access) vs. data plane (**Policy Enforcement Point**; subject/system; implicit trust zones minimized).

> [!TIP]
> **3.1 reflexes**
> - "Secure the DB (Database) / OS (Operating System) / app in **IaaS**" → customer. "Secure the hypervisor / physical DC (Domain Controller)" → provider (any model).
> - OT (Operational Technology)/ICS (Industrial Control System) question → the answer usually involves **segmentation/isolation** and prioritizing **availability**, not "encrypt everything" (often can't).
> - "Consistent, repeatable, auditable provisioning" → **IaC**.
> - "One template mistake exposed 400 hosts" → still IaC — the fix is **scanning IaC in the pipeline**, not abandoning it.

---

## 3.2 Securing Enterprise Infrastructure

### Device placement & security zones

- **Security zones** — group assets by trust/sensitivity; control traffic between them.
- **Screened subnet (DMZ)** — for internet-facing services (web, reverse proxy, mail relay); inbound internet never touches the internal LAN (Local Area Network).
- **Intranet / trusted zone** — internal users and systems.
- **Extranet** — partner access, tightly scoped.
- **Guest network** — isolated from corporate.
- **Management network** — **out-of-band** admin access, separate from production traffic.
- **East-west** (server-to-server) vs **north-south** (client-to-server / internet) traffic — microsegmentation controls east-west.

### Attack surface & connectivity

- Minimize the **attack surface**: fewer open ports, fewer services, fewer entry points.
- **Device attribute placement:** an IPS (Intrusion Prevention System)/firewall goes **inline** (can block); an IDS (Intrusion Detection System)/tap/SPAN (Switched Port Analyzer) port goes **out-of-band** (monitor only, can't block).
- **Fail-open vs fail-closed:** a security device that **fails closed** stops all traffic on failure (favor security); **fail-open** keeps traffic flowing (favor availability). Choose per asset criticality.
- **Active vs passive** appliances.

### Network appliances

| Appliance | Role |
|---|---|
| **Firewall** | Filter by rules; see types below |
| **NGFW (next-gen firewall)** | Layer 3–7, app-aware, user-aware, integrated IPS (Intrusion Prevention System) + deep packet inspection |
| **UTM (Unified Threat Management)** | Many functions in one box (FW (Firewall) + IPS + AV (Antivirus) + web filter + VPN (Virtual Private Network)) |
| **WAF (web application firewall)** | Layer 7, protects web apps: SQLi, XSS (Cross-Site Scripting), CSRF (Cross-Site Request Forgery), directory traversal, HTTP (Hypertext Transfer Protocol) floods |
| **IDS (Intrusion Detection System) / IPS** | Detect (IDS) or detect-and-block (IPS); signature-, anomaly-, behavior-based |
| **Proxy (forward)** | Sits between users and the internet; filtering, caching, DLP (Data Loss Prevention), anonymity |
| **Reverse proxy** | Sits in front of servers; TLS (Transport Layer Security) offload, load distribution, hides backend |
| **Load balancer** | Distributes traffic across servers (round-robin, least-connections); **health checks**; improves availability *and* performance; can do TLS termination and session persistence/affinity |
| **NAC (network access control)** | Checks identity + **device posture** (patch level, AV (Antivirus), config) before granting access; 802.1X; can quarantine |
| **Jump server / bastion host** | Single hardened entry point for administering a protected zone |
| **VPN concentrator** | Terminates many VPN tunnels |

### Firewall types

- **Packet-filtering** — stateless, by IP (Internet Protocol)/port/protocol; fast, no context.
- **Stateful inspection** — tracks connection state; allows return traffic for established sessions.
- **Proxy / application-layer gateway** — terminates and inspects at layer 7.
- **NGFW (Next-Generation Firewall)** — as above.
- **Host-based firewall** — on the endpoint itself.
- **Implicit deny** — anything not explicitly permitted is blocked (always the last rule).

### Port security & Layer-2 protections

- **802.1X** — port-based network access control; supplicant → authenticator (switch/AP (Access Point)) → authentication server (RADIUS). Often with **EAP (Extensible Authentication Protocol)** methods (**EAP-TLS (Transport Layer Security)** = certs both sides, strongest; PEAP (Protected Extensible Authentication Protocol), EAP-TTLS (Tunneled Transport Layer Security), EAP-FAST).
- **Port security** — limit MAC (Media Access Control) addresses per switch port; stop rogue devices/hubs.
- **DHCP (Dynamic Host Configuration Protocol) snooping** — drop rogue DHCP offers.
- **Dynamic ARP (Address Resolution Protocol) Inspection (DAI)** — block ARP poisoning.
- **BPDU (Bridge Protocol Data Unit) guard / root guard / loop protection** — protect spanning tree.
- **MAC (Media Access Control) filtering** — allow-list of hardware addresses (weak alone — spoofable).

### Secure communication / remote access

- **VPN (Virtual Private Network) types:** **site-to-site** (gateway-to-gateway, always-on link between offices), **remote-access / client** (user device to corp), **clientless SSL (Secure Sockets Layer)/TLS (Transport Layer Security) VPN** (browser-based), **full tunnel** (all traffic through the VPN — more secure, more load) vs **split tunnel** (only corp traffic through — faster, riskier).
- **IPsec:** **AH** (authentication/integrity only — no encryption) vs **ESP** (encryption + integrity — use this). **Transport mode** (host-to-host, payload only) vs **tunnel mode** (gateway-to-gateway, whole packet). **IKE (Internet Key Exchange)** phase 1 (build the secure channel: main/aggressive mode, DH (Diffie-Hellman) group, PSK (Pre-Shared Key) or certs) and phase 2 (negotiate the IPsec SA (Security Association)).
- **TLS** for application traffic; **SSH (Secure Shell)** for secure admin and tunneling; **SD-WAN** (policy-based multi-link WAN) and **SASE** (SD-WAN + cloud-delivered SWG (Secure Web Gateway)/CASB (Cloud Access Security Broker)/ZTNA (Zero Trust Network Access)/FWaaS).
- **Selection of effective controls** — match the control to the data sensitivity and threat, not "most expensive."

> [!TIP]
> **3.2 scenario → answer**
> - "Admins need to reach servers in a secure zone" → **jump server / bastion host** in a management zone.
> - "Only allow authorized, healthy devices onto the LAN (Local Area Network)" → **NAC (Network Access Control)** with posture assessment + 802.1X.
> - "Distribute load and survive a server failure" → **load balancer** with health checks.
> - "Protect the public web app from SQLi/XSS (Cross-Site Scripting)" → **WAF (Web Application Firewall)**.
> - "Connect two branch offices permanently and securely" → **site-to-site VPN**, **IPsec ESP**, tunnel mode, AES (Advanced Encryption Standard)-256/SHA (Secure Hash Algorithm)-256, DH group 14+.
> - "Monitor traffic without the ability to affect it" → **IDS (Intrusion Detection System) on a SPAN (Switched Port Analyzer)/tap** (out-of-band).
> - "Roaming laptops must not send personal traffic through corp, but corp apps must be protected" → **split tunnel** (know the trade-off).

---

## 3.3 Protecting Data

### Data classifications

- **Sensitive**, **confidential**, **public**, **restricted**, **private**, **critical**. Government scheme: **unclassified / confidential / secret / top secret**.
- **Regulated data** — governed by law/standard: **PII** (personally identifiable), **PHI** (health — HIPAA (Health Insurance Portability and Accountability Act)), **PIFI (Payment Industry Financial Information) / cardholder data** (PCI-DSS), **financial**, **intellectual property**, **trade secrets**, **legal information**, **human- and non-human-readable** data.
- **Data owner** sets classification; classification drives **labeling/tagging**, handling, retention, and destruction rules.

### States of data (match the control to the state)

| State | Where | Controls |
|---|---|---|
| **At rest** | Disk, DB (Database), backup, archive | FDE (Full Disk Encryption), file/DB encryption, access control, DLP (Data Loss Prevention)-at-rest |
| **In transit / in motion** | Network | **TLS (Transport Layer Security)**, **IPsec**, SSH (Secure Shell), VPN (Virtual Private Network) |
| **In use / in processing** | RAM (Random Access Memory), CPU (Central Processing Unit) | Application access control, **secure enclave**, confidential computing / homomorphic encryption (emerging), screen-lock, clipboard controls |

### Methods to secure data

- **Geographic restrictions / geofencing** — where data may be stored/accessed (data **sovereignty / residency**).
- **Encryption** — see Domain 1.4.
- **Hashing** — integrity / non-reversible comparison (e.g. storing password hashes).
- **Masking** — hide part of a displayed value.
- **Tokenization** — swap for a token; real value in a vault; shrinks compliance scope.
- **Obfuscation** — make data hard to interpret.
- **Segmentation** — keep regulated data on isolated systems (PCI cardholder data environment).
- **Permission restrictions** — least privilege, need-to-know, RBAC (Role-Based Access Control).
- **Data Loss Prevention (DLP)** — inspects data in email, web uploads, USB (Universal Serial Bus), cloud, endpoints; blocks/alerts on classified content leaving. Detection by pattern, fingerprint, exact match, or classification label.

> [!TIP]
> **3.3 reflexes**
> - "Card numbers visible in logs / to support staff" → **masking** (display) or **tokenization** (storage & scope reduction).
> - "Prevent data leaving via email/USB/cloud" → **DLP**.
> - "Data must physically stay in-country" → **geographic restriction / data sovereignty**.
> - "Protect data while it's being processed in memory" → **data in use** → secure enclave / confidential computing.
> - Encryption vs. tokenization for compliance: **tokenization** removes the field from scope; encryption does not (still "the same data").

---

## 3.4 Resilience and Recovery

### High availability & redundancy

- **Redundancy** — no single point of failure: dual NICs/power supplies/ISPs/switches, RAID (Redundant Array of Independent Disks), clustered databases.
- **Load balancing** (active-active — all nodes work, spread load) vs **clustering / failover** (active-passive — standby takes over on failure).
- **Geographic dispersion** — put replicas far enough apart to survive a regional disaster.
- **Platform diversity / multi-cloud** — avoid a shared flaw or a single vendor outage taking everything down.

### Site resiliency

| Site | State | Recovery time | Cost |
|---|---|---|---|
| **Hot** | Fully running, data replicated | Minutes | Highest |
| **Warm** | Hardware + connectivity ready, data restored from backup | Hours | Medium |
| **Cold** | Space, power, cooling only | Days–weeks | Lowest |
| **Mobile / cloud** | Trailer or cloud region spun up on demand | Varies | Varies |

### Capacity planning

Provision enough **people**, **technology** (compute, storage, bandwidth), and **infrastructure** for normal load *and* spikes/growth. Under-provisioning → availability failure; over-provisioning → wasted cost.

### Backups

- **Types:** **full** (everything; slow backup, fast restore), **incremental** (changes since the last backup of any type; fast backup, slow multi-step restore; clears archive bit), **differential** (changes since the last **full**; grows over time; restore = full + one differential), **snapshot** (point-in-time image), **replication** (continuous copy).
- **3-2-1 rule:** **3** copies, on **2** different media, with **1** offsite.
- **Offsite / off-network / immutable / air-gapped** copy is the defense against ransomware encrypting your backups.
- **Journaling** — log-based recovery to a precise point.
- **Encryption** of backups; secure the backup catalog and credentials.
- **Frequency** is driven by the **RPO** (how much data loss is tolerable).

### Recovery testing

- **Test restores** regularly — an untested backup is not a backup.
- **Tabletop** (discussion) → **walkthrough** → **simulation** → **parallel processing** (DR (Disaster Recovery) site runs alongside prod, no cutover) → **failover / full interruption** (actually cut to DR).
- Always test **fail back** to primary, not just fail over.

### Power resiliency

- **UPS (Uninterruptible Power Supply)** — battery; covers **brownouts/sags** and short outages; gives time for graceful shutdown or generator start (minutes).
- **Generator** — sustained outages (hours to days); needs fuel and periodic load testing.
- **PDU** (power distribution unit), dual power feeds, line conditioners for surges/spikes.

### Recovery metrics (carry these exact definitions)

- **RTO (Recovery Time Objective)** — target time to restore a system after disruption.
- **RPO (Recovery Point Objective)** — maximum tolerable **data loss**, measured in time → drives backup frequency.
- **WRT (Work Recovery Time)** — extra time after restore to test/reintegrate/brief before going live.
- **MTD (Maximum Tolerable Downtime)** — absolute outage limit before irrecoverable business harm. **Rule: RTO + WRT ≤ MTD.**
- **MTBF (Mean Time Between Failures)** = total operational time ÷ number of failures — reliability.
- **MTTR (Mean Time To Repair/Recover)** = total repair time ÷ number of failures — how fast you fix things.

> [!TIP]
> **3.4 traps**
> - **RTO** = time to be back up. **RPO** = how much data you can lose (look for "measured in time" / "since the last backup").
> - Ransomware encrypted the backups too → the missing control is an **immutable / offline / air-gapped** backup copy.
> - "99.999% uptime" (~5 min/year) → full redundancy at **every** layer + geographic dispersion.
> - **UPS (Uninterruptible Power Supply)** ≠ **generator**: UPS = seconds-to-minutes bridge + power conditioning; generator = long-haul.
> - Differential restore = last full + **one** differential. Incremental restore = last full + **every** incremental since.

---

## Domain 3 — 60-second self-check

1. In IaaS, who secures the OS (Operating System) and database? In SaaS, what three things does the customer still own?
2. Screened subnet — what goes in it and why does inbound internet never reach the LAN (Local Area Network)?
3. IDS (Intrusion Detection System) vs IPS (Intrusion Prevention System) placement (inline vs out-of-band) and capability.
4. AH (Authentication Header (IPsec — integrity, no encryption)) vs ESP (Encapsulating Security Payload (IPsec — encryption + integrity)); transport vs tunnel mode.
5. Hot vs warm vs cold site — recovery time and cost for each.
6. State the RTO (Recovery Time Objective) + WRT (Work Recovery Time) ≤ MTD (Maximum Tolerable Downtime) rule and define each term.
7. Full / incremental / differential — which is fastest to back up, which is fastest to restore?
8. What single backup property defeats ransomware that targets backups? *(immutable / offline / air-gapped offsite copy)*
