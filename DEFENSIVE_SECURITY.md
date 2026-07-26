# 🔵 Defensive Security Portfolio

[⬅ Back to main README](README.md)

**Project:** Digital Forensics & Incident Response (DFIR) - `dibimbingshop` Server Compromise  
**Target:** Linux E-Commerce Server | **Approach:** Log Correlation & DFIR  

---

## 🚨 Incident Summary
- **Severity:** `CRITICAL` (Full System Compromise)
- **Initial Access:** Web App Vulnerabilities → SSH Brute Force → Root Access
- **Key Evidence:** Wazuh SIEM alerts, `auth.log`, `.bash_history`, Apache logs, malware artifacts
- **Business Impact:** Data breach (public DB backup), RCE via webshell, root compromise, log tampering

## 📅 Attack Timeline Reconstruction
| Date | Phase | Event |
|------|-------|-------|
| `25 Sep 2025` | Recon | LFI probing (`/etc/passwd`) |
| `27 Sep 2025` | Exploit | Time-Based Blind SQLi (DB enumeration) |
| `Post-Sep` | Persistence | Unrestricted upload → `cmd.php`, `MARIJUANA.php` (Webshell) |
| `22 Feb 2026` | Compromise | SSH Brute Force Success → `Accepted password` (root) |
| `24 Feb 2026` | Evasion | `rm access.log.*` (Anti-forensics) |
| `27 Feb 2026` | Response | Containment initiated |

## 🧩 Key Findings & MITRE ATT&CK Mapping
| Finding | CVSS | MITRE ATT&CK Technique |
|---------|------|------------------------|
| Unrestricted File Upload → Webshell/RCE | 9.8 | `T1505.003` (Web Shell) |
| SSH Brute Force + `PermitRootLogin yes` | 9.8 | `T1110.001` (Password Guessing) |
| Blind SQL Injection | 8.6 | `T1190` (Exploit Public App) |
| Public DB Backup + Directory Listing | 7.5 | `T1083` (File Discovery) |
| Log Tampering | - | `T1070.004` (Indicator Removal) |

## 🕸️ Malware & Artifact Analysis
- **Simple Webshells:** `cmd.php`, `shell.php` → `system($_GET['cmd'])`
- **Obfuscated Webshell:** `MARIJUANA.php` → `eval(base64_decode(gzinflate(...)))`
- **PrivEsc Artifact:** `pwnkit.zip` (CVE-2021-4034) → Local escalation attempt
- **Persistence:** Webshells remain active as long as Apache runs & files aren't deleted

## 📊 Risk Assessment (CIA Triad)
- **Confidentiality:** 🔴 High (Public DB backup, SQLi extraction)
- **Integrity:** 🔴 High (Logs deleted, webshells planted, data mutable)
- **Availability:** 🟠 Medium-High (Root access enables shutdown/ransomware)

## 🛡️ Incident Response & Hardening
- **Immediate:** Block attacker IPs, isolate server, purge webshells/exploits, rotate all credentials, move DB backups outside web root
- **System Hardening:** `PermitRootLogin no`, SSH key auth, `Options -Indexes`, Fail2Ban + Strict WAF
- **Secure Coding:** MIME/whitelist upload validation, prepared statements, input sanitization
- **Process:** IR SOP (Identify → Contain → Eradicate → Recover), regular VA/PT, patch management (Polkit/PwnKit)

---

> *All sensitive data (IPs, passwords, session IDs, exact payloads) has been redacted for security. Full technical reports with PoC screenshots are available upon request under NDA.*

[⬅ Back to main README](README.md)
