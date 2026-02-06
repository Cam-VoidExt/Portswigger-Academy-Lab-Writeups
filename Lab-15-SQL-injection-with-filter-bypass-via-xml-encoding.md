# Lab 15: SQL injection with filter bypass via XML encoding

**Date:** February 6, 2026
**Vulnerability Type:** SQL Injection (Bypassing WAF via XML Entities)
**Difficulty:** Practitioner
**Status:** SOLVED

## 1. Objective
Exfiltrate the administrator password from a stock check feature that uses XML and is protected by a Web Application Firewall (WAF) that blocks standard SQL keywords.

## 2. Methodology
The application uses a POST request with an XML body to check stock levels. Standard injection attempts (e.g., `1 UNION SELECT...`) were blocked by the WAF.

### Step 1: Interception
- **Target:** `POST /product/stock`
- **Payload Location:** `<storeId>` tag.

### Step 2: Obfuscation Strategy
To bypass the WAF, SQL keywords were encoded into **XML Hexadecimal Entities**. This allows the payload to pass through the filter as harmless text before being decoded by the server-side XML parser.

### Step 3: Final Payload
The following obfuscated payload was used to concatenate credentials into the single available output column:
`1 &#x55;&#x4e;&#x49;&#x4f;&#x4e;&#x20;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;username || '~' || password FROM users`

## 3. Results
The server decoded the entities and executed the query, returning:
- **Result:** `administrator:[PASSWORD]`

## 4. Remediation
- **Use Parameterized Queries:** Prevent SQL instructions from being interpreted regardless of encoding.
- **Input Validation:** Validate XML input against a strict XSD schema.
- **WAF Enhancement:** Configure the WAF to inspect decoded XML entities for malicious patterns.
