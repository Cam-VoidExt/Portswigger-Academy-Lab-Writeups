# Lab: Blind SQL injection with conditional responses

**Date:** February 4, 2026  
**Vulnerability Type:** Blind SQL Injection (Conditional Responses)  
**Difficulty:** Practitioner

## 1. Objective
The goal of this lab was to extract the `administrator` password by exploiting a blind SQL injection vulnerability in the `TrackingId` cookie. Since the application only provides a conditional "Welcome back!" message rather than direct data, I used boolean inference to guess the password character by character.

## 2. Discovery & Probing
### 2.1 Confirming the Boolean Trigger
I identified that the application returns a specific string ("Welcome back!") only when the SQL query is true.
- `' AND '1'='1` (Success: **"Welcome back!"** is visible)
- `' AND '1'='2` (False: **"Welcome back!"** is absent)

### 2.2 Validating Table and User
Before attempting extraction, I verified the target user existed in the `users` table:
- `' AND (SELECT 'a' FROM users WHERE username='administrator')='a` (Success: **200 OK / Welcome back!**)

## 3. Exploitation (The Automated Blind Attack)
I utilized Burp Suite Intruder to automate the brute-forcing of the 20-character password string.

### 3.1 The Pivot Logic
Using the `SUBSTRING()` function, I compared each character of the password against a set of alphanumeric possibilities:
```sql
' AND (SELECT SUBSTRING(password,§1§,1) FROM users WHERE username='administrator')='§a§'
