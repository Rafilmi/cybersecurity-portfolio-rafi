# 🔴 Offensive Security Portfolio

[⬅ Back to main README](README.md)

**Project:** Web Application Penetration Testing - `DibimbingShop` (E-Commerce Platform)  
**Target:** `http://dibishop.duckdns.org` | **Approach:** Black-Box Testing  

---

## 🎯 Executive Summary
- **Risk Level:** `CRITICAL`
- **Findings:** 11 vulnerabilities (3 Critical, 5 High, 2 Medium, 1 Low)
- **Business Impact:** Full data breach, revenue leakage via price manipulation, privilege escalation to admin, persistent backdoor deployment.
- **Status:** `Not Production Ready`

## 🗺️ Scope & Methodology
| Aspect | Details |
|--------|---------|
| **Approach** | Black-box Penetration Testing |
| **Phases** | Recon → Mapping → Exploitation → Post-Exploitation → Reporting |
| **Tools** | Nmap, Dirb/Gobuster, Burp Suite, GitTools, Browser DevTools, Webhook.site |
| **Standards** | OWASP Top 10 2021, PTES, CVSS v3.1 |

## 🔍 Attack Surface & Recon
- **Stack:** LAMP (Linux, Apache, MySQL, PHP Native)
- **Open Ports:** `80/HTTP` (443, 3306, 22 filtered/closed)
- **Key Discoveries:** 
  - `/.git/` exposed publicly (Source Code Leak)
  - `/admin_area/` accessible without initial auth
- **Insight:** Missing basic controls enabled rapid mapping to advanced attack vectors.

## 🚨 Critical Findings
| Vulnerability | CVSS | Impact |
|---------------|------|--------|
| SQL Injection (Union & Error-Based) | 9.8 | Full DB dump (admins/customers), auth bypass |
| Hardcoded DB Credentials | 9.8 | Plaintext DB access (`password`), lateral movement risk |
| Git Repository Exposed | 7.5 | White-box analysis, config/secret leakage |

## ⛓️ Chained Attack & Privilege Escalation
- **Vector:** Stored XSS + Missing CSRF Token (`CVSS 8.8`)
- **Mechanism:** Payload injected via registration → executed on admin panel → auto-creates rogue admin (`bimbingrafi`)
- **Result:** Vertical privilege escalation, session hijacking, persistent backdoor.

## 💼 Business Logic & High-Risk Flaws
- `Price Manipulation` (CVSS 7.5): Client-side price override → purchase expensive items for Rp1
- `IDOR` (CVSS 7.1): `?order_id=` manipulation → access other users' orders
- `Improper Date Validation` (CVSS 4.3): Future dates accepted → auditing disruption
- `Unrestricted File Upload` (CVSS 3.3): Code vulnerable, but mitigated by server permissions (`Permission Denied`)

## 🛡️ Remediation Strategy
- **Immediate:** Remove `/.git/`, disable `display_errors`, rotate credentials, purge backdoor accounts
- **Code Fixes:** Prepared statements, CSRF tokens, `htmlspecialchars()`, server-side price/date validation
- **Architecture:** `.env` for secrets, ownership checks for IDOR, Secure SDLC integration

---

> *All sensitive data (IPs, passwords, session IDs, exact payloads) has been redacted for security. Full technical reports with PoC screenshots are available upon request under NDA.*

[⬅ Back to main README](README.md)
