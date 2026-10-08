# Reference — Ports, Protocols & Cryptography

Pure rote-recall. Cover the right side, recite the left. Ports and the cleartext→secure swaps are the two highest-ROI (Return on Investment) tables on the exam. 

---

## 1. Ports & Protocols (memorize cold)

| Port(s) | Protocol | Purpose / notes | Secure? |
|---|---|---|---|
| 20/21 TCP (Transmission Control Protocol) | FTP (File Transfer Protocol) | File transfer (21 control, 20 data) | ❌ cleartext |
| 22 TCP | SSH (Secure Shell) / SCP (Secure Copy Protocol) / SFTP (SSH File Transfer Protocol) | Secure remote admin & file transfer | ✅ |
| 23 TCP | Telnet | Remote admin — cleartext, **replace with SSH** | ❌ |
| 25 TCP | SMTP (Simple Mail Transfer Protocol) | Email sending (server-to-server) | ❌ |
| 49 TCP | TACACS (Terminal Access Controller Access-Control System)+ | Cisco AAA (Authentication, Authorization, Accounting) — encrypts the **entire** packet | ✅ |
| 53 TCP/UDP (User Datagram Protocol) | DNS (Domain Name System) | Name resolution (UDP queries, TCP zone transfers) | ❌ (DNSSEC (DNS Security Extensions)/DoH/DoT add security) |
| 67/68 UDP | DHCP (Dynamic Host Configuration Protocol) | Dynamic IP (Internet Protocol) assignment | ❌ |
| 69 UDP | TFTP (Trivial File Transfer Protocol) | Trivial FTP (no auth) | ❌ |
| 80 TCP | HTTP (Hypertext Transfer Protocol) | Web, cleartext | ❌ |
| 88 TCP/UDP | Kerberos | Authentication (tickets) | ✅ |
| 110 TCP | POP3 (Post Office Protocol version 3) | Email retrieval (download) | ❌ |
| 123 UDP | NTP (Network Time Protocol) | Time sync | ❌ |
| 137–139 TCP/UDP | NetBIOS | Legacy Windows name/session | ❌ |
| 143 TCP | IMAP (Internet Message Access Protocol) | Email retrieval (server-side folders) | ❌ |
| 161/162 UDP | SNMP (Simple Network Management Protocol) / SNMP trap | Network device monitoring | ❌ (v1/v2), ✅ (v3) |
| 389 TCP/UDP | LDAP (Lightweight Directory Access Protocol (Secure)) | Directory services | ❌ |
| 443 TCP | HTTPS (HTTP over TLS (Transport Layer Security)) | Secure web | ✅ |
| 445 TCP | SMB (Server Message Block) | Windows file sharing (worm vector) | ❌ (harden/segment) |
| 465 / 587 TCP | SMTPS (SMTP Secure) / SMTP + STARTTLS (Start Transport Layer Security) | Secure email sending | ✅ |
| 500 UDP | ISAKMP (Internet Security Association and Key Management Protocol) / IKE (Internet Key Exchange) | IPsec VPN (Virtual Private Network) key exchange | ✅ |
| 514 UDP | Syslog | Log collection | ❌ (use TLS variant) |
| 636 TCP | LDAPS (Lightweight Directory Access Protocol (Secure)) | Secure directory services | ✅ |
| 853 TCP | DoT (DNS over TLS) | Encrypted DNS | ✅ |
| 989/990 TCP | FTPS (File Transfer Protocol Secure) | FTP over TLS | ✅ |
| 993 TCP | IMAPS (IMAP Secure) | Secure IMAP | ✅ |
| 995 TCP | POP3S (POP3 Secure) | Secure POP3 | ✅ |
| 1433 TCP | Microsoft SQL (Structured Query Language) Server | Database engine (1434/UDP = SQL Server Browser) | ❌ |
| 1521 TCP | Oracle DB (Database) | Database | ❌ |
| 1645/1646 or 1812/1813 UDP | RADIUS (Remote Authentication Dial-In User Service) | AAA (encrypts **password only**) | partial |
| 3306 TCP | MySQL | Database | ❌ |
| 3389 TCP | RDP (Remote Desktop Protocol) | Remote Desktop (Windows GUI (Graphical User Interface)) | ⚠️ encrypted but high-value target — restrict |
| 5060/5061 | SIP (Session Initiation Protocol) / SIP-TLS | VoIP signaling (5061 secure) | 5061 ✅ |
| 6514 TCP | Syslog over TLS | Secure logging | ✅ |

### Cleartext → secure pairings (the single most common ports question)

| Insecure | Secure replacement |
|---|---|
| Telnet (23) | **SSH (22)** |
| FTP (20/21) | **SFTP (22)** or **FTPS (989/990)** |
| HTTP (80) | **HTTPS (443)** |
| LDAP (389) | **LDAPS (636)** |
| SMTP (25) | **SMTPS (465)** / STARTTLS (587) |
| POP3 (110) | **POP3S (995)** |
| IMAP (143) | **IMAPS (993)** |
| SNMP (Simple Network Management Protocol) v1/v2 (161) | **SNMPv3** |
| DNS (53) | **DNSSEC** (integrity) / **DoH** (443) / **DoT** (853) |
| Syslog (514/UDP (User Datagram Protocol)) | **Syslog over TLS (6514/TCP (Transmission Control Protocol))** |

> A port number never guarantees security by itself — confirm encryption is actually active.

### RADIUS vs. TACACS+

| | RADIUS (Remote Authentication Dial-In User Service) | TACACS (Terminal Access Controller Access-Control System)+ |
|---|---|---|
| Transport | UDP (mostly) | TCP (49) |
| Encrypts | Password only | **Entire payload** |
| AAA (Authentication, Authorization, Accounting) | Combines authN + authZ | **Separates** all three A's |
| Origin | Open standard | Cisco proprietary |
| Best for | Network access (802.1X, Wi-Fi) | Device administration (router/switch mgmt) |

---

## 2. Cryptography Quick-Reference

### Symmetric algorithms

| Algorithm | Status | Notes |
|---|---|---|
| **AES** (128/192/256) | ✅ current standard | Block cipher; modes GCM (auth), CBC (Cipher Block Chaining), CTR (Counter (block cipher mode)) |
| **ChaCha20 / ChaCha20-Poly1305** | ✅ | Stream cipher; fast on devices without AES hardware |
| 3DES | ⚠️ legacy | Being retired |
| DES (Data Encryption Standard) | ❌ broken | 56-bit key |
| RC4 (Rivest Cipher 4) | ❌ broken | Stream cipher, biases |
| Blowfish / Twofish | ok but uncommon | Twofish = AES finalist |

### Asymmetric algorithms

| Algorithm | Use | Notes |
|---|---|---|
| **RSA (Rivest-Shamir-Adleman)** | Encryption, signatures, key transport | 2048-bit minimum, 3072+ preferred |
| **ECC (Elliptic Curve Cryptography) / ECDSA (Elliptic Curve Digital Signature Algorithm) / EdDSA** | Signatures, key agreement | Small keys, same strength — mobile/IoT |
| **Diffie-Hellman (DH (Diffie-Hellman) / ECDH (Elliptic Curve Diffie-Hellman))** | Key **agreement** only | Not encryption; **DHE (Diffie-Hellman Ephemeral)/ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)** = ephemeral → PFS (Perfect Forward Secrecy) |
| ElGamal / DSA (Digital Signature Algorithm) | Signatures (legacy) | DSA largely superseded by ECDSA |

### Hashing

| Hash | Status | Output |
|---|---|---|
| **SHA (Secure Hash Algorithm)-2** (SHA-256/384/512) | ✅ current | 256/384/512-bit |
| **SHA-3** | ✅ current | Sponge construction |
| SHA-1 | ❌ broken (collisions) | 160-bit — legacy only |
| MD5 (Message Digest 5) | ❌ broken (collisions) | 128-bit — integrity checks on non-adversarial data only |
| **HMAC (Hash-based Message Authentication Code)-SHA256 (Secure Hash Algorithm, 256-bit)** | ✅ | Hash + secret key → integrity **and** authenticity |

### Password storage (key stretching)

**PBKDF2 (Password-Based Key Derivation Function 2)**, **bcrypt**, **scrypt**, **Argon2** — deliberately slow; always with a **unique salt** (defeats rainbow tables); optionally a secret **pepper** stored separately.

### PKI objects

| Term | Meaning |
|---|---|
| **CA (Certificate Authority)** | Issues and vouches for certificates |
| **RA (Registration Authority)** | Verifies requester identity for the CA |
| **Root CA** | Top of the chain; **self-signed**; kept offline |
| **Intermediate CA** | Issues leaf certs so the root stays offline |
| **CSR (Certificate Signing Request)** | Public key + identity sent to the CA; **private key never leaves you** |
| **X.509** | Certificate format; **SAN (Subject Alternative Name)** field lists extra hostnames |
| **Wildcard** | `*.domain.com` — all first-level subdomains |
| **CRL (Certificate Revocation List)** | Downloadable list of revoked serials (can be stale) |
| **OCSP (Online Certificate Status Protocol)** | Real-time single-cert status query |
| **OCSP stapling** | Server attaches a signed OCSP response to the TLS (Transport Layer Security) handshake |
| **Pinning** | Client hard-expects a specific cert/key |
| **Key escrow + M-of-N** | Third party holds a recoverable key; M of N custodians needed to use it |

### TPM vs HSM vs Secure Enclave

- **TPM (Trusted Platform Module)** — chip on one device's motherboard; FDE (Full Disk Encryption) keys, measured/secure boot, attestation.
- **HSM (Hardware Security Module)** — dedicated appliance/card (FIPS (Federal Information Processing Standards) 140-2/3); bulk key generation/storage/signing (CAs).
- **Secure enclave / TEE (Trusted Execution Environment)** — isolated CPU (Central Processing Unit) region protecting keys and **data in use**.

### Obfuscation methods

| Method | Reversible? | Purpose |
|---|---|---|
| **Steganography** | Yes, with the method | Hide the *existence* of data inside another file |
| **Tokenization** | Only via the vault | Replace value with a token; removes data from PCI scope |
| **Masking** | No (for that view) | Show partial value (`****1234`) |

---

## 3. Email Authentication (always tested together)

| Record | What it does | Answers |
|---|---|---|
| **SPF (Sender Policy Framework)** | DNS (Domain Name System) TXT listing servers authorized to send for the domain | "Is this sending server allowed?" |
| **DKIM (DomainKeys Identified Mail)** | Sender signs the message; recipient verifies via DNS public key | "Was the message altered / really from the domain?" |
| **DMARC (Domain-based Message Authentication, Reporting & Conformance)** | Policy (`none` / `quarantine` / `reject`) over SPF+DKIM **alignment** + reporting | "What do I DO when SPF/DKIM fail?" |

DMARC **requires SPF and/or DKIM** first. To stop spoofed mail from your domain being delivered: **DMARC `p=reject`**. End-to-end message encryption/signing: **S/MIME (Multipurpose Internet Mail Extensions)** or PGP (Pretty Good Privacy).

---

## 4. Wireless & Authentication

| Item | Key fact |
|---|---|
| **WPA3 (Wi-Fi Protected Access 3)** | Current; **SAE** ("Dragonfly") replaces the WPA2 (Wi-Fi Protected Access 2) PSK (Pre-Shared Key) 4-way-handshake weakness; forward secrecy; **PMF (Protected Management Frames (802.11w))** mandatory |
| **WPA2** | AES (Advanced Encryption Standard)-**CCMP (Counter Mode CBC-MAC Protocol (WPA2 encryption))**; vulnerable to KRACK (Key Reinstallation Attack); avoid TKIP (Temporal Key Integrity Protocol) |
| WEP (Wired Equivalent Privacy) / WPS (Wi-Fi Protected Setup) | ❌ broken / brute-forceable — disable |
| **802.1X** | Port-based NAC (Network Access Control); uses **EAP (Extensible Authentication Protocol)**; needs a **RADIUS (Remote Authentication Dial-In User Service)** server (Enterprise mode) |
| **EAP-TLS (Transport Layer Security)** | Strongest — mutual certificate auth (client + server certs) |
| PEAP (Protected Extensible Authentication Protocol) / EAP-TTLS (Tunneled Transport Layer Security) | Server cert + tunneled inner credentials |
| PSK (Personal) | Shared passphrase — home/small office |
| Site survey + heat map | Plan AP (Access Point) placement, channel, power |

**Authentication factors:** know (password/PIN (Personal Identification Number)) · have (token/smart card/security key) · are (biometric) · somewhere you are (geolocation) · something you do (behavioral).
**MFA (Multifactor Authentication) = factors from ≥ 2 different categories.** Password + PIN = *not* MFA (both "know").

**Biometric error rates:** **FAR** (false accept — security risk) · **FRR** (false reject — usability) · **CER (Crossover Error Rate)/EER** (crossover — lower = better sensor).
