# Domain 1 — General Security Concepts (12%)

**Objectives:** 1.1 Compare and contrast types of security controls · 1.2 Summarize fundamental security concepts · 1.3 Explain the importance of change management processes and their impact on security · 1.4 Explain the importance of using appropriate cryptographic solutions.

---

## 1.1 Security Controls

A **control** is a safeguard that reduces risk. Every control has **one category** (how it's implemented) and **one or more functional types** (what it does).

### Categories — *how* the control is implemented

| Category | Implemented by | Examples |
|---|---|---|
| **Managerial** (administrative) | Policy / oversight / process | Risk assessments, security policy, background checks, SDLC (Software Development Life Cycle), org charts |
| **Operational** | People carrying out procedures | Security-awareness training, guard patrols, incident response, config management, media handling |
| **Technical** (logical) | Systems / software | Firewall, ACL (Access Control List), IDS (Intrusion Detection System)/IPS (Intrusion Prevention System), encryption, MFA (Multifactor Authentication), antivirus, group policy |
| **Physical** | Real-world barriers | Locks, fences, bollards, lighting, badge readers, cameras, mantraps |

### Functional types — *what* the control does

| Type | Acts… | Purpose | Examples |
|---|---|---|---|
| **Preventive** | before | Stop an incident | ACL (Access Control List), firewall rule, MFA (Multifactor Authentication), disabled ports, hardening, AUP (Acceptable Use Policy), guard shack, mantrap |
| **Detective** | during/after | Identify & record it | IDS (Intrusion Detection System), SIEM (Security Information and Event Management) alert, log review, CCTV (Closed-Circuit Television) footage review, file-integrity monitoring, audit |
| **Corrective** | after | Fix / limit damage & restore | Backup restore, patch, re-image, IPS (Intrusion Prevention System) drop, quarantine, failover |
| **Deterrent** | before | Discourage the attempt (psychological) | Warning banner, "Beware of dog" / CCTV-in-use sign, visible guard, lighting |
| **Directive** | before/during | Tell people what to do | Policy, procedure, standard, acceptable use policy, signage, compliance mandate |
| **Compensating** | any | Stand-in when the primary control isn't feasible | Network segmentation instead of a patch; manual log review instead of separation of duties; application allow-list where a legacy app can't be updated |

> [!TIP]
> **Exam reflexes for 1.1**
> - A single control is described by **both** a category and a type at once (e.g. an AUP is *managerial + directive + preventive*; a camera is *physical + detective*, or *physical + deterrent* if the point is to scare people off).
> - **Camera** recording for later analysis = **detective**; camera visible to discourage = **deterrent**.
> - **Backup** = **corrective**. A *tested, offsite/immutable* backup also *compensates* for weak ransomware prevention.
> - "We can't patch this legacy system right now, so we isolated it" → **compensating** (and the segmentation itself is *technical + preventive*).
> - Warning banner / sign → **deterrent** *and* **directive**. Login banner that also establishes legal consent → still deterrent/directive.
> - If two answers both fit, pick **preventive over detective** and the control that addresses the **root cause over the symptom**.

---

## 1.2 Fundamental Security Concepts

### CIA triad (+ the fourth property)

- **Confidentiality** — only authorized subjects can read the data. Enforced by encryption, access control, MFA (Multifactor Authentication), steganography, data masking.
- **Integrity** — data is not modified without authorization, and any change is detectable. Enforced by hashing, digital signatures, HMAC (Hash-based Message Authentication Code), certificates, version control.
- **Availability** — authorized users get timely access. Enforced by redundancy, load balancing, backups, patching, DDoS mitigation, UPS (Uninterruptible Power Supply)/generator.
- **Non-repudiation** — a subject cannot credibly deny an action. Provided by **digital signatures**, robust **audit logging/accounting**, and video. *Not part of CIA (Confidentiality, Integrity, Availability), but always tested alongside it.*

> [!WARNING]
> **Confidentiality vs. integrity vs. authentication**
> Symmetric encryption alone gives **confidentiality only** — both parties share one key, so neither can prove which of them produced a ciphertext. Integrity needs a **hash**; authenticity + non-repudiation needs a **digital signature** (hash encrypted with the sender's **private** key).

### AAA and access-control vocabulary

- **Identification** — claiming an identity (username, account ID (Identifier), certificate CN (Common Name)).
- **Authentication (AuthN)** — proving it with one or more **factors**.
- **Authorization (AuthZ)** — granting the specific rights that identity should have (least privilege).
- **Accounting** — logging what was done; supports non-repudiation and forensics.
- **AAA (Authentication, Authorization, Accounting) servers / protocols:** **RADIUS** (network access, 802.1X, Wi-Fi; UDP (User Datagram Protocol); encrypts password only), **TACACS (Terminal Access Controller Access-Control System)+** (device administration; TCP (Transmission Control Protocol) 49; encrypts the entire payload; separates the three A's), **Diameter** (RADIUS successor).
- **Subject** = the active entity (user, process, device). **Object** = the resource being accessed.

### Authenticating people, systems, and services

- **People:** MFA (Multifactor Authentication) factors (see Domain 4.6) — know / have / are / somewhere you are / something you do.
- **Systems/devices:** certificates (802.1X EAP (Extensible Authentication Protocol)-TLS (Transport Layer Security)), TPM (Trusted Platform Module)-backed device identity, MAC (Media Access Control) allow-lists, IPsec with PSK (Pre-Shared Key) or certs.
- **Applications/services:** service accounts, API (Application Programming Interface) keys, OAuth 2.0 client credentials, mTLS between microservices, managed identities in cloud.

### Gap analysis

Structured comparison of the organization's **current** state against a **target** framework or baseline (NIST (National Institute of Standards and Technology) CSF (Cybersecurity Framework), ISO (International Organization for Standardization) 27001, CIS (Center for Internet Security (Benchmarks)) Controls). Output = a scored list of missing/misconfigured controls plus remediation recommendations. Done when adopting a framework, on a compliance change, and periodically. Often uses outside consultants for specialist expertise.

### Zero Trust

Principle: **never trust, always verify** — no implicit trust from network location; every request is authenticated, authorized, and encrypted.

- **Control plane** — makes access decisions:
  - **Policy Engine (PE)** — evaluates the policy and decides grant/deny.
  - **Policy Administrator (PA)** — executes the PE's decision, issues/revokes access tokens.
  - **Policy Decision Point (PDP)** = PE + PA together.
  - **Adaptive identity** — trust adjusts to context (device posture, geolocation, behavior, risk score).
  - **Threat scope reduction** — shrink the blast radius (least privilege, just-in-time).
  - **Policy-driven access control** — decisions come from written policy, not ad hoc.
- **Data plane** — enforces the decision:
  - **Policy Enforcement Point (PEP)** — the gateway/agent that allows or blocks the actual connection.
  - **Subject/system** — the requester; **implicit trust zones** are minimized.

### Physical security

- **Perimeter:** fencing, **bollards** (stop vehicles), gates, lighting, signage.
- **Surveillance:** cameras/CCTV (motion-activated, infrared/thermal for low light), security guards, robot sentries, **two-person integrity**.
- **Entry:** badge readers (proximity/smart card), biometric readers, **access control vestibule / mantrap** (one person at a time — defeats tailgating), locks (conventional, electronic, biometric, cipher).
- **Sensors:** infrared, pressure, microwave, ultrasonic, acoustic — detect presence/motion.
- **Attacks to know:** **tailgating/piggybacking**, **RFID (Radio-Frequency Identification)/prox-card cloning**, lock picking, **Faraday-cage** bypass, fence cutting.

### Deception and disruption technology

| Term | What it is |
|---|---|
| **Honeypot** | A single decoy system that looks vulnerable, to attract & study attackers |
| **Honeynet** | A whole decoy network of honeypots |
| **Honeyfile** | A bait file (e.g. `passwords.xlsx`) that alerts when opened/exfiltrated |
| **Honeytoken** | Any traceable fake data (fake API (Application Programming Interface) key, fake DB (Database) record, canary URL (Uniform Resource Locator)) whose use = intrusion |

Value: near-zero false positives (nobody has a legitimate reason to touch them), early warning, and forcing the attacker to waste time / reveal TTPs.

---

## 1.3 Change Management

Poorly managed change causes outages and security gaps; CompTIA treats change management as a **security** control.

### Business / governance process

- **Approval process** — formal request → review → authorization before work starts.
- **Change Advisory Board (CAB)** — cross-functional group that reviews and approves normal changes.
- **Ownership** — a named owner is accountable for the change end to end.
- **Stakeholders** — everyone affected (app teams, users, security, ops) is identified and consulted.
- **Impact analysis** — what could break, who is affected, severity if it fails.
- **Test / sandbox** — validate in a non-production environment first.
- **Backout / rollback plan** — a defined, tested way to return to the last known-good state.
- **Maintenance window** — scheduled low-impact time; **standard change** = pre-approved low-risk/repeatable; **emergency change** = expedited path with retroactive review.

### Technical implications

- **Allow lists / deny lists** must be updated when systems, ports, or apps change.
- **Restricted activities** — scope is bounded so the change can't drift.
- **Downtime** — planned service interruption; communicate it.
- **Service / application restart** — many changes only take effect after a restart; plan for it.
- **Legacy applications** — may block a change (can't be patched/updated); may need a compensating control.
- **Dependencies** — Service A can't change until Service B is ready; map them.

### Documentation & versioning

- Update **diagrams**, **policies/procedures**, and the **configuration management database (CMDB)** after every change.
- **Version control** — track revisions to configs, IaC, and code so you can diff and roll back.

> [!TIP]
> **1.3 exam angle**
> The correct answer to "a change caused an outage / an unauthorized change was made" is almost always a **change-management process gap**: no impact analysis, no backout plan, no CAB (Change Advisory Board) approval, no test environment, or documentation not updated. "Just make the change faster" is never right.

---

## 1.4 Cryptographic Solutions

### Symmetric vs. asymmetric

| | Symmetric | Asymmetric (public-key) |
|---|---|---|
| Keys | **One shared secret** key | **Key pair**: public + private, mathematically linked |
| Speed | Fast — bulk data | Slow — small data only |
| Main use | Encrypting data at rest / in transit (bulk) | Key exchange, digital signatures, identity |
| Problem it has | **Key distribution** — how do both sides get the key securely? | Performance; needs PKI (Public Key Infrastructure) to bind keys to identities |
| Algorithms | **AES** (128/192/256, modern standard), 3DES (legacy), ChaCha20, RC4 (broken), DES (broken), Blowfish/Twofish | **RSA** (2048+ bit), **ECC (Elliptic Curve Cryptography)/ECDSA** (smaller keys, same strength — good for mobile/IoT), **Diffie-Hellman** (key agreement, not encryption), ElGamal |

- **Key length** — longer = stronger but slower. AES-256, RSA-2048/3072, ECC-256 ≈ RSA-3072.
- **Confusion** (ciphertext ≠ obviously related to key) and **diffusion** (one plaintext bit change flips many ciphertext bits) are the design goals of a good cipher.

### Key exchange

The problem: use fast symmetric crypto without meeting in person to share the key.

- **Out-of-band** — deliver the key by a different channel (courier, phone).
- **In-band with asymmetric** — encrypt the symmetric ("session") key with the recipient's **public** key.
- **Diffie-Hellman (DH (Diffie-Hellman) / ECDH (Elliptic Curve Diffie-Hellman))** — both sides derive the same shared secret over a public channel without ever transmitting it.
- **Perfect Forward Secrecy (PFS)** — use **ephemeral** DH (**DHE (Diffie-Hellman Ephemeral) / ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)**) so each session gets a unique key; a future compromise of the server's long-term private key **cannot** decrypt previously recorded sessions.

### Encryption at different levels

- **Full-disk encryption (FDE)** — entire volume (BitLocker, FileVault, LUKS (Linux Unified Key Setup)); protects a lost/stolen laptop; typically anchored to a **TPM (Trusted Platform Module)**.
- **Partition / volume / file / folder** encryption — narrower scope; EFS (Encrypting File System), VeraCrypt.
- **Database** — column/field-level (e.g. encrypt only card numbers), transparent data encryption (TDE) for the whole DB (Database) at rest.
- **Record / application-level** — the app encrypts specific records before storing.
- **Transport / communication** — TLS (Transport Layer Security), IPsec, SSH (Secure Shell) protect data **in transit**.

### Cryptographic hardware & key management

- **TPM (Trusted Platform Module)** — crypto chip **on a device's motherboard**; stores keys, enables FDE (Full Disk Encryption), supports **measured/secure boot** and remote attestation. One device.
- **HSM (Hardware Security Module)** — dedicated appliance/card (often FIPS (Federal Information Processing Standards) 140-2/3 validated) for generating, storing, and using keys at scale; used by CAs and for high-volume signing.
- **Key Management System (KMS)** — centralized lifecycle: generate → distribute → rotate → revoke → archive → destroy. Cloud KMS + **envelope encryption** (a data key encrypted by a master key).
- **Secure enclave** — isolated processor region for keys/biometric data (Apple Secure Enclave, Intel SGX (Software Guard Extensions)).
- **Key escrow** — a third party holds a recoverable copy of a key. **M-of-N control** — recovery requires *M* of *N* authorized custodians, so no single person can misuse an escrowed key.

### Obfuscation (hiding, not always encrypting)

| Technique | What it does | Reversible? |
|---|---|---|
| **Steganography** | Hide data inside another file (image LSBs, audio, video, network packets) | Yes, if you know the method |
| **Tokenization** | Replace a value with a random token; real value kept in a separate **vault**. Removes data from PCI-DSS scope. | Only via the vault |
| **Data masking** | Show partial data (`****-****-****-1234`); the rest is gone from that view | No (for the masked view) |

### Hashing and digital signatures

- **Hash function** — one-way, fixed-length digest; proves **integrity**. **SHA (Secure Hash Algorithm)-2 (SHA-256/384/512)** and **SHA-3** are current; **MD5 (Message Digest 5)** and **SHA-1** are broken (collisions) — legacy only.
- **Collision** — two inputs, same hash → the function is broken.
- **Salt** — unique random value added per password before hashing; defeats **rainbow tables** and makes identical passwords hash differently. **Does not need to be secret**, just unique.
- **Key stretching** — deliberately slow hashing to resist brute force: **PBKDF2 (Password-Based Key Derivation Function 2), bcrypt, scrypt, Argon2**.
- **Pepper** — a secret value (stored separately from the DB (Database), e.g. in an HSM (Hardware Security Module)) added like a salt.
- **HMAC (Hash-based Message Authentication Code)** — hash + secret key → integrity **and** authenticity of a message.
- **Digital signature** — sender hashes the message, then encrypts the hash with their **private key**. Anyone with the sender's **public key** decrypts it, hashes the message themselves, and compares. Proves **integrity + authentication + non-repudiation** (but *not* confidentiality — the message itself isn't hidden). Algorithms: **DSA (Digital Signature Algorithm), RSA (Rivest-Shamir-Adleman), ECDSA (Elliptic Curve Digital Signature Algorithm)**.

### Blockchain

An append-only distributed ledger. Each block includes the **hash of the previous block**, so altering any historical block breaks the chain for every block after it → **tamper-evident**. Maintained by many nodes (open public ledger); no single point of trust.

### Public Key Infrastructure (PKI) & certificates

- **Certificate Authority (CA)** — issues and vouches for digital certificates.
- **Registration Authority (RA)** — verifies identity of requesters on the CA's behalf.
- **Root CA** — top of the chain; its certificate is **self-signed** (nothing above it). Kept offline.
- **Intermediate / subordinate CA** — issues end-entity certs so the root can stay offline; forms the **chain of trust**.
- **CSR (Certificate Signing Request)** — you generate a key pair, put the **public** key + identity info in a CSR, send it to the CA; the CA returns a signed certificate. Your **private key never leaves you**.
- **X.509** — the certificate standard. Key fields: subject, issuer, validity period, public key, **SAN (Subject Alternative Name)** for multiple hostnames.
- **Wildcard certificate** — `*.example.com` covers all first-level subdomains.
- **Revocation:**
  - **CRL (Certificate Revocation List)** — CA-published list of revoked serial numbers; downloaded periodically (can be stale).
  - **OCSP (Online Certificate Status Protocol)** — real-time query for one certificate's status.
  - **OCSP stapling** — the web server attaches a recent, CA-signed OCSP response to its TLS (Transport Layer Security) handshake, so the client doesn't have to contact the CA.
- **Key lifecycle:** generation → storage → distribution/installation → use → rotation → revocation → **destruction**.
- **Certificate pinning** — an app hard-codes/expects a specific cert or public key, rejecting anything else (defeats a rogue-CA on-path attack).
- **Trust models:** hierarchical (root → intermediates → leaf), **web of trust** (PGP (Pretty Good Privacy) — users sign each other's keys), bridge CA (connects separate PKIs).

> [!TIP]
> **1.4 scenario → answer**
> - "Protect data on stolen laptops" → **full-disk encryption** (+ TPM (Trusted Platform Module)).
> - "Prove a file wasn't altered" → **hash**; "prove *who* sent it and that they can't deny it" → **digital signature**.
> - "Recorded traffic must stay safe even if the server key leaks later" → **PFS (Perfect Forward Secrecy) via ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)**.
> - "Stop precomputed-hash attacks on the password DB (Database)" → **salt** (+ key stretching like bcrypt/Argon2).
> - "Let a value be used in a system without exposing the real data, and shrink compliance scope" → **tokenization**.
> - "Store the CA's signing key so it can't be extracted" → **HSM (Hardware Security Module)**.
> - "Recover an encrypted drive when the employee is gone" → **key escrow** with **M-of-N**.
> - "Certificate check without contacting the CA on every connection" → **OCSP stapling**.
> - If an option pairs the wrong key with the operation (e.g. "sign with the recipient's public key"), it's wrong: **sign with your private, encrypt with their public**.

---

## Domain 1 — 60-second self-check

1. Name the 4 control categories and 6 functional types. Which two properties can one control have simultaneously? *(category + type)*
2. What does symmetric encryption **not** provide, and why? *(integrity/authentication — shared key)*
3. Control plane vs. data plane in Zero Trust — which one is the PEP (Policy Enforcement Point)? *(data plane)*
4. Order the digital-signature steps and name the keys used. *(hash → encrypt hash with sender private → verify with sender public)*
5. CRL (Certificate Revocation List) vs. OCSP (Online Certificate Status Protocol) vs. OCSP stapling — which is real-time and which removes the client-to-CA (Certificate Authority) call? *(OCSP real-time; stapling removes the call)*
6. What must a change record always include so a failed change is survivable? *(a tested backout/rollback plan)*
7. TPM (Trusted Platform Module) vs. HSM (Hardware Security Module) in one line each.
8. Salt: secret or not? Purpose? *(not secret; unique per user; defeats rainbow tables)*
