# Lab: Blind SQL injection with time delays and information retrieval

**Date:** February 5, 2026
**Vulnerability Type:** Time-Based Blind SQL Injection
**Difficulty:** Practitioner

## 1. Objective
The goal was to exfiltrate the administrative password from a PostgreSQL database that provided zero visual feedback (no changes in page content, status codes, or error messages). 

## 2. Discovery & Dialect Fingerprinting
To determine the database type, I tested common time-delay syntax for various SQL dialects:
- **Microsoft SQL Server Test:** I injected `'; IF (1=1) WAITFOR DELAY '0:0:10'--`. The server responded instantly, indicating the logic was not executed.
- **PostgreSQL Test:** I injected `' || pg_sleep(10)--`. This triggered a significant delay, confirming the backend was PostgreSQL.

## 3. Exploitation (Temporal Interrogation)
Since the database was "silent," I used a `CASE` statement to force a 3-second delay whenever a character guess was correct. 

### Final Payload:
`' || (SELECT CASE WHEN (username='administrator' AND SUBSTR(password,§1§,1)='§a§') THEN pg_sleep(3) ELSE pg_sleep(0) END FROM users)--`

### Intruder Configuration:
- **Attack Type:** Cluster Bomb (Position vs. Character set).
- **Resource Pool:** Limited to **1 concurrent request** to prevent network jitter and ensure "temporal clarity" for each guess.
- **The Signal:** I monitored the **"Response received"** column. Correct guesses resulted in a ~3,000ms+ response time (the delay executed twice due to application architecture), while incorrect guesses returned in <200ms.

## 4. Final Result
- **Data Recovered:** 20-character administrator password.
- **Methodology:** Successfully leveraged "Temporal Exfiltration" when all other data channels were "Void."
- **Status:** **LAB SOLVED**
