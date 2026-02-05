# Lab: Blind SQL injection with conditional errors

**Date:** February 4, 2026
**Vulnerability Type:** Blind SQL Injection (Conditional Errors)
**Difficulty:** Practitioner

## 1. Objective
The goal of this lab was to extract the `administrator` password from an Oracle database. Unlike previous labs where the page content changed based on the query (Boolean-based), this application remains identical regardless of the query result. To exfiltrate data, I had to force the database to trigger an internal server error (HTTP 500) whenever a guess was correct.

## 2. Discovery & Probing
### 2.1 Identifying the Error Trigger
I tested the `TrackingId` cookie for error-based injection by breaking the SQL syntax:
- `TrackingId=ID'` -> **500 Internal Server Error** (Vulnerability confirmed)
- `TrackingId=ID''` -> **200 OK** (Syntax repaired)

### 2.2 Constructing the "Logic Bomb"
To ask the database "Yes/No" questions, I utilized an Oracle-specific `CASE` statement designed to trigger a divide-by-zero error only when a condition is met:
```sql
' || (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual) || '
