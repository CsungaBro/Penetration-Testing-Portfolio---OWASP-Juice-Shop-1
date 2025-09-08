**Category:** A01 – Broken Access Control (IDOR)

---
## Overview
An **Insecure Direct Object Reference (IDOR)** vulnerability was discovered in the **product review functionality** of the OWASP Juice Shop application.  
By manipulating the `author` field in the review submission request, an attacker can post comments under another user’s identity.

---
## Proof of Concept

### Steps to Reproduce
1. Log in as a normal user.  
2. Intercept the product review request in Burp Suite.  
3. Modify the `author` parameter to another valid user’s email (e.g., `admin@juice-sh.op`).  
4. Submit the request.  
5. The review is successfully posted under the impersonated user’s identity.

### Example Request
```http
PUT /rest/products/1/reviews HTTP/1.1
Host: localhost:3000
X-User-Email: bender@juice-sh.op
Content-Type: application/json

{"message":"Not the admin","author":"admin@juice-sh.op"}
```

### Example Outcome
- The comment appears publicly as if written by **admin@juice-sh.op**, although it was submitted by another user.  
![Pasted image 20250811161447](Pasted%20image%2020250811161447.png)
---
## Impact
- **Identity spoofing:** Attackers can impersonate other users, including administrators.  
- **Reputation damage:** Malicious content could be posted under trusted identities.  
- **Trust exploitation:** Could be used in social engineering or phishing attacks within the platform.  

**Severity:** High

---
## Recommendations
1. **Enforce server-side ownership checks:** The `author` field should always be derived from the authenticated session or JWT, not user input.  
2. **Remove sensitive fields from client control:** Prevent users from supplying arbitrary email or identity values.  
3. **Log and monitor anomalous activity** such as mismatches between session identity and submitted data.  
4. **Regularly test for IDOR issues** in all input parameters referencing users or resources.  

