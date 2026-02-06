# 🛡️ PortSwigger Academy Lab Writeups

This repository documents my progression through the PortSwigger Web Security Academy. It serves as a technical log of vulnerability discovery, exploitation, and defensive remediation strategies.

## 📊 SQL Injection Progress

| Lab | Vulnerability Description | Writeup Link |
| :--- | :--- | :--- |
| **01** | WHERE clause retrieval of hidden data | [View Report](./Lab-01-SQL-injection-vulnerability-in-WHERE-clause-allowing-retrieval-of-hidden-data.md) |
| **02** | Authentication bypass via SQLi | [View Report](./Lab-02-SQL-injection-vulnerability-allowing-login-bypass.md) |
| **03** | UNION attack: Determining column count | [View Report](./Lab-03-SQL-injection-UNION-attack-determining-the-number-of-columns-returned-by-the-query.md) |
| **04** | UNION attack: String concatenation | [View Report](./Lab-04-SQLi-Union-Concatenation.md) |
| **05** | Blind SQLi: Conditional responses | [View Report](./Lab-05-Blind-SQL-Injection-with-Conditional-Responses.md) |
| **06** | Blind SQLi: Conditional errors | [View Report](./Lab-06-Blind-SQL-Injection-with-Conditional-Errors.md) |
| **07** | Visible error-based SQLi | [View Report](./Lab-07-Visible-Error-Based-SQLi.md) |
| **08** | Blind SQLi: Time delays | [View Report](./Lab-08-Blind-SQL-Injection-with-Time-Delays.md) |
| **09** | Time delays with information retrieval | [View Report](./Lab-09-Blind-SQL-injection-with-time-delays-and-information-retrieval.md) |
| **10** | Out-of-band (OAST) interaction | [View Report](./Lab-10-Blind-SQL-injection-with-out-of-band-interaction.md) |
| **11** | Database schema mapping (Non-Oracle) | [View Report](./Lab-11-SQL-injection-listing-database-contents-non-oracle.md) |
| **12** | Database type and version (Oracle) | [View Report](./Lab-12-SQL-injection-querying-database-type-and-version-on-oracle.md) |
| **14** | Database schema mapping (Oracle) | [View Report](./Lab-14-SQL-injection-listing-database-contents-on-oracle.md) |
---

## 🛠️ Toolset
* **Burp Suite Community Edition** (Proxy, Repeater, Intruder)
* **Interact.sh** (Open-source OAST listener)
* **FoxyProxy** (Browser proxy management)

## 🎯 Project Goals
* Complete 100% of SQL Injection labs within a 14-day sprint.
* Document automated exfiltration techniques via Python.
* Implement professional-grade remediation advice for all identified vulnerabilities.

---
*Disclaimer: This repository is strictly for educational purposes.*
