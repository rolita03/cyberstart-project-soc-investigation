# cyberstart-project-soc-investigation
SOC investigation: AiTM phishing, OAuth account takeover, BEC attempt &amp; data exfiltration — M365 | Entra ID | EDR | Graph API
# 🔐 Home Lab 3 — SOC & Incident Response Investigation

> **Simulated Scenario** | AiTM Phishing · Account Takeover · Business Email Compromise · Data Exfiltration  
> **Duration:** 23 Days | **Grade:** 100 Points | **Status:** ✅ Completed

---

## 📌 Overview

A targeted phishing campaign hit a simulated organisation using Microsoft 365.  
In **38 minutes**, an attacker bypassed MFA, exfiltrated full payroll data, and attempted financial fraud — all without ever stealing a password.

This lab required me to act as a SOC analyst: correlate 5 simultaneous alerts across multiple log sources, separate two overlapping attack vectors, and produce a professional SOC/IR ticket.

---

## ⚡ The Attack — What Happened in 38 Minutes

```
09:02  Phishing email delivered to 6 users (lookalike SharePoint domain)
09:11  Victim clicks link → AiTM proxy captures credentials + MFA token
09:22  Attacker logs in from Amsterdam (impossible travel — 1,700 km in 6 min)
09:23  Rogue OAuth app consent granted → persistent token created
09:26  Hidden inbox rule created to suppress Finance replies
09:29  Graph API used to read 148 emails + 12 attachments
09:37  Payroll file exfiltrated (names, PIDs, salaries, bank accounts)
09:40  Fraudulent bank details email sent to Finance (BEC attempt)
10:05  SOC receives 5 correlated alerts → investigation begins
```

> 🔑 **Key insight:** A password reset would NOT have contained this incident.  
> The attacker held an `offline_access` OAuth token that survives credential changes.  
> Token revocation + app consent removal was required.

---

## 🧩 Two Attack Vectors — One Campaign

| Vector | Target | Technique | Outcome |
|--------|--------|-----------|---------|
| **AiTM Phishing** | Nigar (cloud account) | Fake SharePoint login → session token capture → OAuth consent | ❌ Compromised |
| **Macro Document** | Emil (endpoint) | `.docm` file → PowerShell via WINWORD.EXE | ✅ Blocked by EDR |

---

## 🚨 Alert Correlation

| Alert | Description | Verdict |
|-------|-------------|---------|
| A1 | Suspicious M365 login from unusual location | ✅ Part of main incident |
| A2 | New mailbox rule on finance-linked account | ✅ Part of main incident |
| A3 | Bulk email access via Microsoft Graph API | ✅ Part of main incident |
| A4 | EDR blocked PowerShell on laptop | ⚠️ Related — separate vector |
| A5 | Internal email with updated bank details | ✅ Part of main incident |

---

## 📋 Investigation Deliverables

| # | Deliverable | Description |
|---|-------------|-------------|
| 1 | **Timeline** | 12-event correlated timeline across 5 log sources |
| 2 | **Incident Decision** | Confirmed Incident — HIGH / P1 |
| 3 | **Attack Path** | 5-phase kill chain + parallel endpoint vector |
| 4 | **Affected Assets** | Users, devices, data — compromise status per asset |
| 5 | **Containment Actions** | 10 prioritised immediate steps |
| 6 | **Evidence Assessment** | Strong / moderate / weak / missing classification |
| 7 | **SOC/IR Ticket** | Full structured ticket with regulatory notes |

---

## 🛡️ Key Takeaways

**1. MFA ≠ phishing-resistant**  
AiTM attacks bypass standard TOTP and push-based MFA by proxying the session token in real time. Only FIDO2/hardware keys or Entra ID Conditional Access (compliant device required) would have stopped this.

**2. OAuth persistence survives password resets**  
The attacker's `offline_access` token kept working after any credential change. Token revocation and app consent removal must be part of every M365 incident response playbook.

**3. Correlating overlapping vectors is the real skill**  
Alerts 1–3 and 5 were one incident. Alert 4 was a related but separate attack vector. Separating them correctly without missing either is the kind of judgment that matters in a real SOC.

**4. GDPR clock starts ticking immediately**  
Payroll data containing bank accounts and personal IDs was exfiltrated. Under GDPR, the 72-hour breach notification window begins from confirmed awareness — not from full investigation completion.

---

## 🧰 Tools & Platforms Covered

`Microsoft 365` · `Entra ID / Azure AD` · `Microsoft Graph API` · `EDR` · `Exchange Online` · `OneDrive` · `SIEM` · `Web Proxy Logs` · `VPN Logs` · `Email Security Gateway`

---

## 📁 Files in This Repo

| File | Description |
|------|-------------|
| `HomeLab3_SOC investigation.docx` | Full investigation report (short & precise format) |
| `README.md` | This file |

---

## 🔗 Part of My Cybersecurity Portfolio

This is **Lab 3** in a series of hands-on cybersecurity home labs I'm building as part of my transition into security operations and incident response.

| Lab | Topic | Status |
|-----|-------|--------|
| Lab 1 | IAM & Access Control | ✅ Completed |
| Lab 2 | Backup & Business Continuity | ✅ Completed |
| **Lab 3** | **SOC & Incident Response (this lab)** | ✅ Completed |
| Lab 4 | Vendor & Third-Party Risk | 🔄 In Progress |

---

## 👤 About Me

**Rolita Shrine (Raja)** — AWS Certified QA Automation Engineer transitioning into Cybersecurity  
Focus areas: SOC / Incident Response · IAM · Security Automation  
📍 Helsinki, Finland


---

*All scenarios in this repository are simulated for educational purposes.*
