# Lab: SQL injection UNION attack, retrieving multiple values in a single column

**Date:** February 3, 2026
**Vulnerability Type:** SQL Injection (UNION-based)
**Difficulty:** Practitioner

## 1. Objective
The goal of this lab was to bypass a product category filter using a SQL injection attack to retrieve all usernames and passwords from the `users` table. 

## 2. Discovery & Probing
### 2.1 Determining Column Count
Using the `ORDER BY` technique, I determined the original query returns **2 columns**.
- `' ORDER BY 2--` (Success)
- `' ORDER BY 3--` (Error: Internal Server Error)

### 2.2 Finding String Compatibility
I tested each column for string data compatibility using `NULL` placeholders. Because a `UNION` attack requires matching data types, this was a necessary step.
- `' UNION SELECT 'abc', NULL--` (Failed: 500 Error)
- **`' UNION SELECT NULL, 'abc'--` (Success: 200 OK)**

**Result:** Column 2 is the "window" for exfiltrating string data.

## 3. Exploitation (The Pivot)
The lab required retrieving two values (username and password) but only provided one string-compatible column. I utilized the Oracle-specific concatenation operator (`||`) to join the values into a single string with a `~` separator for clarity.

### Final Payload:

' UNION SELECT NULL, username||'~'||password FROM users--

### 4. Analysis & Outcome
By injecting this payload into the `category` filter, the database merged the `username` and `password` fields into a single string. This bypassed the limitation of only having one string-compatible column. I identified the `administrator` credentials from the output and successfully authenticated to solve the lab.

## 5. Remediation
To prevent this vulnerability in a production environment:
1. **Use Parameterized Queries:** Implement prepared statements so that user input is never interpreted as a SQL command.
2. **Input Validation:** Use an "allow-list" for the `category` parameter to ensure only expected values are processed.
3. **Principle of Least Privilege:** Ensure the database user has the minimum permissions necessary, preventing access to sensitive tables like `users` from a public-facing product filter.
