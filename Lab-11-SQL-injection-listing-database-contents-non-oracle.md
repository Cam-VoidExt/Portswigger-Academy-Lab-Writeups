# Lab: SQL injection attack, listing the database contents on non-Oracle databases

**Date:** February 6, 2026
**Vulnerability Type:** SQL Injection (UNION-based)
**Difficulty:** Practitioner
**Status:** SOLVED

## 1. Objective
Exploit a UNION-based SQL injection to map the database schema and exfiltrate administrative credentials from a non-Oracle (PostgreSQL/MySQL) environment.

## 2. Discovery & Enumeration
- **Column Count:** Identified **2 columns** using `' ORDER BY 2--`.
- **Data Types:** Both columns confirmed as strings via `' UNION SELECT 'abc', 'def'--`.

## 3. Schema Mapping
1. **Locating Tables:** Queried the `information_schema.tables` to find the randomized users table. 
   - **Table Identified:** `users_edrura`
2. **Locating Columns:** Queried `information_schema.columns` where `table_name = 'users_edrura'`.
   - **Columns Identified:** `username_uqjoeu`, `password_opsbqo`

## 4. Final Data Exfiltration
Using the discovered identifiers, a final `UNION SELECT` was executed to dump the contents of the credential table.
- **Payload:** `' UNION SELECT username_uqjoeu, password_opsbqo FROM users_edrura--`

## 5. Remediation
- **Prepared Statements:** Implement parameterized queries to ensure user input is treated as data, not executable SQL.
- **Principle of Least Privilege:** Ensure the database user account has no access to the `information_schema` or system tables unless absolutely necessary.
