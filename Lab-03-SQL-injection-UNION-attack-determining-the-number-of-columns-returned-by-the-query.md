# Lab 03: SQL injection UNION attack, determining the number of columns returned by the query

**Date:** February 6, 2026
**Vulnerability Type:** UNION-Based SQL Injection
**Difficulty:** Practitioner
**Status:** SOLVED

## 1. Objective
Determine the number of columns being returned by the original query in the product category filter. This is a prerequisite for performing a successful UNION attack.

## 2. Methodology
I utilized two different techniques to identify the column count:
1. **ORDER BY:** Incrementing the index until a `500 Internal Server Error` occurred.
2. **UNION SELECT:** Adding `NULL` values until the query executed successfully.

### Final Payloads:
- `' ORDER BY 3--` (Success)
- `' ORDER BY 4--` (Error: Confirmed 3 columns)
- `' UNION SELECT NULL, NULL, NULL--` (Success)

## 3. Results
The application was confirmed to be returning **3 columns**. This allows for the construction of further UNION-based payloads to exfiltrate data.

## 4. Remediation
Implement strict input validation on the category filter and migrate to prepared statements to prevent query structure manipulation.
