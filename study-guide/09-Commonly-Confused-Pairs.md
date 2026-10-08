# Commonly Confused Pairs — Side by Side

CompTIA builds distractors out of these. For each pair, the **"exam tell"** line is the phrase in the question stem that points to one side.

---

### IDS vs. IPS
| IDS (Intrusion Detection System) | IPS (Intrusion Prevention System) |
|---|---|
| Detects and **alerts** | Detects and **blocks** |
| Out-of-band (tap/SPAN (Switched Port Analyzer) port), copy of traffic | **Inline**, sees traffic live |
| Cannot stop an attack by itself | Can drop/reset the connection |

**Exam tell:** "monitor without affecting traffic" → IDS. "automatically block" / "prevent" → IPS.

---

### SIEM vs. SOAR vs. XDR
| SIEM (Security Information and Event Management) | SOAR (Security Orchestration, Automation, and Response) | XDR (Extended Detection and Response) |
|---|---|---|
| Aggregate + normalize + **correlate** logs; alert; dashboards; compliance | **Orchestrate + automate** the response via playbooks | Vendor-integrated **detection + response** across endpoint, network, cloud, identity |
| "Single pane of glass for many log sources" | "auto-contain + auto-ticket when an alert fires" | "cross-layer correlation from one vendor stack" |

---

### RTO vs. RPO
| RTO (Recovery Time Objective) | RPO (Recovery Point Objective) |
|---|---|
| Max time to **restore service** | Max **data loss**, measured **in time** |
| Drives DR (Disaster Recovery) staffing, hot/warm/cold site choice | Drives **backup / replication frequency** |

**Exam tell:** "how far back" / "since the last backup" / "data loss" → RPO. "how long until we're running again" → RTO.

---

### MTBF vs. MTTR
| MTBF (Mean Time Between Failures) | MTTR (Mean Time To Repair / Recover) |
|---|---|
| Mean Time **Between** Failures — reliability | Mean Time **To Repair/Recover** — fix speed |
| Higher is better | Lower is better |

---

### Vulnerability scan vs. Penetration test
| Vulnerability scan | Penetration test |
|---|---|
| **Identifies** and reports weaknesses | **Actively exploits** to prove real-world impact |
| Automated, frequent, low risk | Manual + tools, scoped, higher risk |
| Output: CVSS (Common Vulnerability Scoring System)-scored findings | Output: proof of compromise + narrative + remediation |

---

### Known / Partially known / Unknown environment (white / grey / black box)
| Known | Partially known | Unknown |
|---|---|---|
| Full info provided | Some info | No info |
| Simulates a **malicious insider** | Hybrid | Simulates an **external attacker** |
| Fastest, cheapest | — | Most recon, most resource-intensive |

---

### Active vs. Passive reconnaissance
| Active | Passive |
|---|---|
| Direct interaction: port scans, enumeration, OS (Operating System) fingerprinting | OSINT (Open-Source Intelligence), WHOIS (Who Is (domain registration lookup protocol)), public records, traffic observation |
| Generates traffic — **detectable** | No direct contact — **stealthy** |

---

### Red / Blue / Purple / White team
| Red | Blue | Purple | White |
|---|---|---|---|
| Offense — emulate the attacker | Defense — detect & respond | Red + Blue collaborating to improve both | Referees — set rules, adjudicate, ensure safety |

---

### Symmetric vs. Asymmetric encryption
| Symmetric | Asymmetric |
|---|---|
| One shared secret key | Public/private key pair |
| Fast — bulk data (AES) | Slow — key exchange, signatures, identity (RSA (Rivest-Shamir-Adleman), ECC (Elliptic Curve Cryptography)) |
| Problem: key distribution | Problem: performance; needs PKI (Public Key Infrastructure) |

---

### Hashing vs. Encryption vs. Encoding
| Hashing | Encryption | Encoding |
|---|---|---|
| **One-way**, fixed length; integrity | **Reversible with a key**; confidentiality | Reversible, **no key** (Base64) — *not* security |

---

### Tokenization vs. Masking vs. Hashing (for protecting a field)
| Tokenization | Masking | Hashing |
|---|---|---|
| Swap for a random token; real value in a **vault** | Show part (`****1234`), hide the rest | One-way digest, used for matching |
| Reversible via the vault | Not reversible for that view | Not reversible |
| **Removes data from PCI-DSS scope** | Display protection | Password storage, dedupe |

---

### TPM vs. HSM vs. Secure Enclave (TEE)
| TPM (Trusted Platform Module) | HSM (Hardware Security Module) | Secure Enclave / TEE (Trusted Execution Environment) |
|---|---|---|
| Chip on **one device's** motherboard | Dedicated appliance/card, **FIPS (Federal Information Processing Standards) 140-2/3** | Isolated region of the CPU (Central Processing Unit) |
| FDE (Full Disk Encryption) keys, measured/secure boot, attestation | Bulk key gen/storage/signing (CAs) | Protects **data in use** / biometric templates |

---

### DAC vs. MAC vs. RBAC vs. ABAC
| Model | Access decided by |
|---|---|
| **DAC (Discretionary Access Control)** | The resource **owner** |
| **MAC (Mandatory Access Control)** | System-enforced **labels/clearances** (user can't override) |
| **RBAC (Role-Based Access Control)** | The user's **job role** |
| **ABAC (Attribute-Based Access Control)** | **Attributes + context** (dept + device posture + time + location) — most granular |

---

### OAuth 2.0 vs. OIDC vs. SAML
| OAuth 2.0 | OIDC (OpenID Connect) | SAML (Security Assertion Markup Language) |
|---|---|---|
| **Authorization** — delegated, scoped access to resources | **Authentication** — identity layer on top of OAuth | **Authentication** — enterprise browser SSO (Single Sign-On) |
| "let this app post to my calendar" | "log in with Google" (modern web/mobile, JSON (JavaScript Object Notation)) | IdP↔SP (Service Provider) web SSO (XML) |

---

### AH vs. ESP (IPsec)
| AH (Authentication Header (IPsec — integrity, no encryption)) | ESP (Encapsulating Security Payload (IPsec — encryption + integrity)) |
|---|---|
| Integrity + authentication of the whole packet | **Encryption** + integrity + authentication |
| **No confidentiality** | Confidentiality — **use this** |

---

### Full tunnel vs. Split tunnel VPN
| Full tunnel | Split tunnel |
|---|---|
| **All** traffic routed through corporate | Only corporate-bound traffic through the VPN (Virtual Private Network) |
| More secure, more bandwidth/load, can inspect everything | Faster, less load, **personal traffic bypasses corporate controls** |

---

### MOU vs. MOA vs. MSA vs. SOW vs. SLA vs. BPA vs. NDA vs. ISA
| Doc | Binding? | Use |
|---|---|---|
| **MOU (Memorandum of Understanding)** | No | Intent / shared goals; preliminary |
| **MOA (Memorandum of Agreement)** | Yes | Formal roles, terms, responsibilities |
| **MSA (Master Service Agreement)** | Yes | Umbrella terms for an ongoing relationship |
| **SOW (Statement of Work) / WO** | Yes | Specific engagement: **scope, deliverables, cost, timeline** |
| **SLA (Service-Level Agreement)** | Yes | Measurable performance targets + penalties (uptime) |
| **BPA (Business Partnership Agreement)** | Yes | Long-term strategic partnership terms |
| **NDA (Non-Disclosure Agreement)** | Yes | Confidentiality of shared information |
| **ISA (Interconnection Security Agreement)** | Yes | Technical/security requirements to interconnect two networks |

---

### Risk appetite vs. Risk tolerance vs. Residual risk vs. Inherent risk
| Term | Meaning |
|---|---|
| **Inherent risk** | Level **before** any controls |
| **Residual risk** | Level **after** controls/transfer/acceptance |
| **Risk appetite** | Org-wide **strategic** willingness to take risk (expansionary / neutral / conservative) |
| **Risk tolerance** | Acceptable **variance** around the appetite for a specific risk |

---

### Accept vs. Avoid vs. Mitigate vs. Transfer (risk response)
| Response | Action | Example |
|---|---|---|
| **Mitigate** | Reduce likelihood/impact with controls | Patch, segment, MFA (Multifactor Authentication) |
| **Transfer** | Shift financial impact to a third party | Cyber insurance, outsourcing |
| **Accept** | Do nothing, within tolerance, documented | Sign-off; exception (temp) vs exemption (long-term) |
| **Avoid** | Stop the risky activity entirely | Discontinue the feature/vendor |

---

### Due diligence vs. Due care
| Due diligence | Due care |
|---|---|
| Investigating/assessing **before** acting (vetting a vendor) | Exercising **ongoing** reasonable protection ("prudent person") |

---

### Policy vs. Standard vs. Procedure vs. Guideline
| Policy | Standard | Procedure | Guideline |
|---|---|---|---|
| High-level intent, mandatory | Specific mandatory requirement | Step-by-step how-to, mandatory | Recommended, **optional** |
| "We protect customer data" | "AES (Advanced Encryption Standard)-256; 12-char minimum" | "Steps to onboard a user" | "Consider using a password manager" |

---

### Data controller vs. Data processor vs. Data owner vs. Custodian
| Role | Responsibility |
|---|---|
| **Owner** | Accountable; sets classification (business/leadership) |
| **Controller** (GDPR) | Decides **why and how** personal data is processed |
| **Processor** (GDPR) | Processes data **for** the controller |
| **Custodian / steward** | Day-to-day handling, protection, data quality |

---

### Whaling vs. BEC
| Whaling | Business Email Compromise |
|---|---|
| Spear phishing whose **target** is a senior executive | Fraud that **uses** a spoofed/compromised exec (or vendor) identity to request wire transfers / gift cards / data |

---

### Typosquatting vs. Pharming
| Typosquatting | Pharming |
|---|---|
| Look-alike domain; user **mistypes or clicks** it | User types the **correct** URL (Uniform Resource Locator); poisoned DNS (Domain Name System) / hosts file redirects them |

---

### Watering hole vs. Supply chain attack
| Watering hole | Supply chain |
|---|---|
| Compromise a **third-party website the target group visits** | Compromise a **vendor, component, or update** the target uses |

---

### Worm vs. Virus vs. Trojan
| Worm | Virus | Trojan |
|---|---|---|
| Self-propagates over the network, **no user action**, no host file | Attaches to a host file; needs **user action** | Disguised as legitimate software; opens the door |

---

### Password spraying vs. Credential stuffing vs. Brute force vs. Dictionary
| Spraying | Stuffing | Brute force | Dictionary |
|---|---|---|---|
| **Few** common passwords across **many** accounts (avoids lockout) | Reuse **breached** username/password pairs | Try **every** combination against one account | Try a **wordlist** of likely passwords |

---

### SPF vs. DKIM vs. DMARC
| SPF (Sender Policy Framework) | DKIM (DomainKeys Identified Mail) | DMARC (Domain-based Message Authentication, Reporting & Conformance) |
|---|---|---|
| Which servers may send for the domain | Cryptographic signature proving integrity + origin | Policy (none/quarantine/**reject**) + reporting, built on SPF/DKIM alignment |

**Exam tell:** "spoofed mail from our own domain still gets delivered" → set **DMARC to `p=reject`**.

---

### Fail-open vs. Fail-closed (fail-secure)
| Fail-open | Fail-closed |
|---|---|
| On failure, traffic/access **continues** — favors **availability** | On failure, traffic/access **stops** — favors **security** |
| e.g. a badge system that unlocks doors in a fire | e.g. a firewall that drops all traffic if it crashes |

---

### Detective vs. Deterrent vs. Preventive
| Detective | Deterrent | Preventive |
|---|---|---|
| Identifies/records during or after (logs, IDS (Intrusion Detection System), camera review) | **Discourages** the attempt psychologically (sign, visible camera, lighting) | **Stops** it before it happens (ACL (Access Control List), MFA (Multifactor Authentication), lock, mantrap) |

---

### Compensating vs. Corrective
| Compensating | Corrective |
|---|---|
| A **substitute** used because the primary control isn't feasible | Acts **after** an incident to fix/restore |
| "can't patch, so we segmented it" | "restored from backup", "applied the patch", "re-imaged" |
