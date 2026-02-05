# Lab: Visible error-based SQL injection

**Date:** February 5, 2026
**Vulnerability Type:** Error-Based SQL Injection
**Difficulty:** Practitioner

## 1. Objective
The goal was to exfiltrate the `administrator` password by forcing the database to display sensitive data within its own error messages. This is a highly efficient "In-band" technique that bypasses the need for blind brute-forcing.

## 2. Discovery & Probing
### 2.1 Identifying the Leak
Injecting a single quote (`'`) into the `TrackingId` cookie confirmed that the server returns detailed database error messages.

### 2.2 Exploiting Type Conversion
I used the `CAST()` function to force a data type conflict. By asking the database to convert the administrator's password (a string) into an integer, the database threw an error containing the actual password.

## 3. Exploitation
I removed the original `TrackingId` value to stay within character limits and injected the following:
`' AND 1=CAST((SELECT password FROM users WHERE username='administrator') AS int)--`

## 4. Final Result
- **Data Recovered:** Administrative password leaked directly in the error response.
- **Status:** **LAB SOLVED**
