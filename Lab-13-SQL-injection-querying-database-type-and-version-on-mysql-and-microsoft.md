# Lab 13: Querying the database type and version on MySQL and Microsoft

**Date:** February 6, 2026
**Vulnerability Type:** SQL Injection (UNION-based)
**Difficulty:** Practitioner
**Status:** SOLVED

## 1. Objective
Identify the database version on a MySQL or Microsoft SQL Server backend.

## 2. Methodology & Troubleshooting
Initial attempts with standard `--` comments failed. The backend was identified as MySQL, which requires a space or a second character after the double-dash to validate the comment.

### Step 1: Column Enumeration
Using URL-encoded spaces (`+`), the column count was identified.
- **Payload:** `'+ORDER+BY+2--+` (Confirmed 2 columns).

### Step 2: Version Exfiltration
In MySQL and MSSQL, the `@@version` global variable is used to retrieve version metadata.
- **Payload:** `'+UNION+SELECT+@@version,NULL--+`

## 3. Results
The database returned the following version string:
- **Result:** `8.0.42-0ubuntu0.20.04.1`

## 4. Remediation
- Use **Parameterized Queries** to prevent structural manipulation of SQL statements.
- Disable detailed database error messages and system metadata access for web-facing accounts.
