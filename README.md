# Cyber Security Portfolio - Rafi Ilmi Putra Nurwahyudi

> **Role:** Offensive & Defensive Security Analyst  
> **Focus:** Web Application Penetration Testing | Digital Forensics & Incident Response (DFIR)  

---

## 📌 Table of Contents
- [🧑‍🔬 About Me](#-about-me)
- [🎓 Education](#-education)
- [🏢 Work Experience](#-work-experience)
- [🔍 Skills](-skills)
- [🔴 Offensive Security Portfolio](#-offensive-security-portfolio)
- [🔵 Defensive Security Portfolio](#-defensive-security-portfolio)
- [🛠️ Tools & Frameworks](#-tools--frameworks)
- [📊 Competencies Demonstrated](#-competencies-demonstrated)
- [📖 Projects](#-projects)
- [📄 Certificates](#-certificates)
- [🏅 Achievements](#-achievements)
- [📬 Contact](#-contact)

---
## 🧑‍🔬 About Me
I'm currently a computer science graduate, researching smart contract vulnerabilities using deep learning. I'm starting my career in cybersecurity with a focus on penetration testing. Previously, I attended a six-month bootcamp and completed two final projects that trained me in offensive and defensive security. In the future, I'd like to pursue a career as a penetration tester, particularly in the field of web penetration testing, as I'm interested in continuously developing my technical skills and contributing to system security.

## 🎓 Education
- **University of Electronic Science and Technology of China**, Master of Computer Science (Sep 2024 - Jul 2026)
- **Shandong University of Science and Technology**, Bachelor of Telecommunication Engineering (Sep 2022 - Jul 2024)
- **State Polytechnic of Malang**, Bachelor of Applied Studies (Sep 2020 - Jul 2022)

## 🏢 Work Experience
PT Dwi Tunggal Putra, Student Intern (2019 Jul - 2019 Sep)
- Learning how to use dedicated applications for antenna maintenance, thereby understanding how to maintain antennas regularly
- Simulate the Linux operating system using VirtualBox, so that we can use Linux-based applications within the VirtualBox application

## 🔍 Skills
- **Hard Skills:** Penetration Testing, Risk Assessment, Threat Detection, Incident Response
- **Soft Skills:** Communication, Continuous Learning, Time Management, Dedication
- **Tools:** Burp Suite, Wireshark, Metasploit, Nmap
- **Languages:** Indonesian, English

---

## 🔴 Offensive Security Portfolio
**Project:** Web Application Penetration Testing - `DibimbingShop` (E-Commerce Platform)  
**Target:** `http://dibishop.duckdns.org` | **Approach:** Black-Box Testing  

### 🎯 Executive Summary
- **Risk Level:** `CRITICAL`
- **Findings:** 11 vulnerabilities (3 Critical, 5 High, 2 Medium, 1 Low)
- **Business Impact:** Full data breach, revenue leakage via price manipulation, privilege escalation to admin, persistent backdoor deployment.
- **Status:** `Not Production Ready`

### 🗺️ Scope & Methodology
| Aspect | Details |
|--------|---------|
| **Approach** | Black-box Penetration Testing |
| **Phases** | Recon → Mapping → Exploitation → Post-Exploitation → Reporting |
| **Tools** | Nmap, Dirb/Gobuster, Burp Suite, GitTools, Browser DevTools, Webhook.site |
| **Standards** | OWASP Top 10 2021, PTES, CVSS v3.1 |

### 🔍 Attack Surface & Recon
- **Stack:** LAMP (Linux, Apache, MySQL, PHP Native)
- **Open Ports:** `80/HTTP` (443, 3306, 22 filtered/closed)
- **Key Discoveries:** 
  - `/.git/` exposed publicly (Source Code Leak)
  - `/admin_area/` accessible without initial auth
- **Insight:** Missing basic controls enabled rapid mapping to advanced attack vectors.

### 🚨 Critical Findings
| Vulnerability | CVSS | Impact |
|---------------|------|--------|
| SQL Injection (Union & Error-Based) | 9.8 | Full DB dump (admins/customers), auth bypass |
| Hardcoded DB Credentials | 9.8 | Plaintext DB access (`password`), lateral movement risk |
| Git Repository Exposed | 7.5 | White-box analysis, config/secret leakage |

### ⛓️ Chained Attack & Privilege Escalation
- **Vector:** Stored XSS + Missing CSRF Token (`CVSS 8.8`)
- **Mechanism:** Payload injected via registration → executed on admin panel → auto-creates rogue admin (`bimbingrafi`)
- **Result:** Vertical privilege escalation, session hijacking, persistent backdoor.

### 💼 Business Logic & High-Risk Flaws
- `Price Manipulation` (CVSS 7.5): Client-side price override → purchase expensive items for Rp1
- `IDOR` (CVSS 7.1): `?order_id=` manipulation → access other users' orders
- `Improper Date Validation` (CVSS 4.3): Future dates accepted → auditing disruption
- `Unrestricted File Upload` (CVSS 3.3): Code vulnerable, but mitigated by server permissions (`Permission Denied`)

### 🛡️ Remediation Strategy
- **Immediate:** Remove `/.git/`, disable `display_errors`, rotate credentials, purge backdoor accounts
- **Code Fixes:** Prepared statements, CSRF tokens, `htmlspecialchars()`, server-side price/date validation
- **Architecture:** `.env` for secrets, ownership checks for IDOR, Secure SDLC integration

---

## 🔵 Defensive Security Portfolio
**Project:** Digital Forensics & Incident Response (DFIR) - `dibimbingshop` Server Compromise  
**Target:** Linux E-Commerce Server | **Approach:** Log Correlation & DFIR  

### 🚨 Incident Summary
- **Severity:** `CRITICAL` (Full System Compromise)
- **Initial Access:** Web App Vulnerabilities → SSH Brute Force → Root Access
- **Key Evidence:** Wazuh SIEM alerts, `auth.log`, `.bash_history`, Apache logs, malware artifacts
- **Business Impact:** Data breach (public DB backup), RCE via webshell, root compromise, log tampering

### 📅 Attack Timeline Reconstruction
| Date | Phase | Event |
|------|-------|-------|
| `25 Sep 2025` | Recon | LFI probing (`/etc/passwd`) |
| `27 Sep 2025` | Exploit | Time-Based Blind SQLi (DB enumeration) |
| `Post-Sep` | Persistence | Unrestricted upload → `cmd.php`, `MARIJUANA.php` (Webshell) |
| `22 Feb 2026` | Compromise | SSH Brute Force Success → `Accepted password` (root) |
| `24 Feb 2026` | Evasion | `rm access.log.*` (Anti-forensics) |
| `27 Feb 2026` | Response | Containment initiated |

### 🧩 Key Findings & MITRE ATT&CK Mapping
| Finding | CVSS | MITRE ATT&CK Technique |
|---------|------|------------------------|
| Unrestricted File Upload → Webshell/RCE | 9.8 | `T1505.003` (Web Shell) |
| SSH Brute Force + `PermitRootLogin yes` | 9.8 | `T1110.001` (Password Guessing) |
| Blind SQL Injection | 8.6 | `T1190` (Exploit Public App) |
| Public DB Backup + Directory Listing | 7.5 | `T1083` (File Discovery) |
| Log Tampering | - | `T1070.004` (Indicator Removal) |

### 🕸️ Malware & Artifact Analysis
- **Simple Webshells:** `cmd.php`, `shell.php` → `system($_GET['cmd'])`
- **Obfuscated Webshell:** `MARIJUANA.php` → `eval(base64_decode(gzinflate(...)))`
- **PrivEsc Artifact:** `pwnkit.zip` (CVE-2021-4034) → Local escalation attempt
- **Persistence:** Webshells remain active as long as Apache runs & files aren't deleted

### 📊 Risk Assessment (CIA Triad)
- 🔒 **Confidentiality:** 🔴 High (Public DB backup, SQLi extraction)
- ✅ **Integrity:** 🔴 High (Logs deleted, webshells planted, data mutable)
- 📡 **Availability:** 🟠 Medium-High (Root access enables shutdown/ransomware)

### 🛡️ Incident Response & Hardening
- **Immediate:** Block attacker IPs, isolate server, purge webshells/exploits, rotate all credentials, move DB backups outside web root
- **System Hardening:** `PermitRootLogin no`, SSH key auth, `Options -Indexes`, Fail2Ban + Strict WAF
- **Secure Coding:** MIME/whitelist upload validation, prepared statements, input sanitization
- **Process:** IR SOP (Identify → Contain → Eradicate → Recover), regular VA/PT, patch management (Polkit/PwnKit)

---

## 🛠️ Tools & Frameworks
| Category | Tools |
|----------|-------|
| **Recon & Scanning** | Nmap, Dirb, Gobuster, GitTools |
| **Web Exploitation** | Burp Suite, Browser DevTools, Webhook.site |
| **DFIR & Monitoring** | Wazuh SIEM, Linux CLI (`grep`, `awk`, `find`), Log Analysis |
| **Frameworks** | OWASP Top 10 2021, MITRE ATT&CK, CVSS v3.1, PTES, NIST SP 800-61 |

## 📊 Competencies Demonstrated
✅ Vulnerability Assessment & Penetration Testing (VAPT)  
✅ Chained Exploitation & Business Logic Testing  
✅ Log Correlation & Digital Forensics (DFIR)  
✅ Malware & Webshell Analysis  
✅ MITRE ATT&CK Mapping & Risk Assessment  
✅ Secure Coding Recommendations & Hardening Strategy  

---

## 📖 Projects
- **Intuitive and Natural Control through Hand Gestures for Effortless Interactions in Portable Computer Devices.** An algorithm was created using the Python programming language to control the mouse cursor using hand gestures.
- **Image Classification.** A rudimentary machine learning model was developed in Python to classify hand gestures in image.
- **Sentiment Analysis.** A basic machine learning model utilizing TF-IDF to evaluate reviews to gain an understanding of whether the sentences tend to be positive, negative, or neutral.
- **Hierarchical Graph Transformer with Adaptive Community Integration for Smart Contracts Vulnerability Detection.** A deep learning model implementing Hierarchical Graph Transformer to detect vulnerabilities in smart contract for DevSecOps purpose.

## 📄 Certificates
- **Learn Machine Learning for Beginners** by Dicoding (2024 July)
- **Learn Fundamentals of AI** by Dicoding (2024 September)
- **Learn Fundamentals of Deep Learning** by Dicoding (2025 November)
- **Networking Basics** by Cisco (2026 April)
- **Networking Devices and Initial Configuration** by Cisco (2026 April)
- **Cybersecurity Essentials** by Cisco (in progress)

## 🏅 Achievements
- Best Final Project Phase Offensive in Cyber Security Bootcamp Batch 4

---

## 📬 Contact
📧 `[rafilmiputra@gmail.com]` | 💼 `[www.linkedin.com/in/rafi-ilmi-putra-nurwahyudi-4348a6212]` | 📱 `[+62 82246894989]`  
*Open for Security Analyst / Pentester / DFIR Roles*

> 🔐 *All sensitive data (IPs, passwords, session IDs, exact payloads) has been redacted for security. Full technical reports with PoC screenshots are available upon request under NDA.*
