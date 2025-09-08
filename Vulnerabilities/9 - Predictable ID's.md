**Category:** A04 – Insecure Design  

---
## Overview
The application uses **predictable and sequential identifiers (IDs)** for key objects such as:  
- **Users**  
- **Baskets**  
- **Payment Cards**  

This design flaw allows attackers to enumerate IDs and gain unauthorized access to sensitive information or perform actions on other users' behalf.  

---
## Proof of Concept

### Steps to Reproduce
1. Intercept a request that contains a user, basket, or card ID (e.g., `basket/6`).  
2. Increment or decrement the ID value in the request.  
3. Observe that the application accepts the request and returns information belonging to another user.  

### Example Request
```http
GET /rest/basket/6 HTTP/1.1
Host: localhost:3000
Authorization: Bearer <valid_token>
```

By changing the `6` to another number (e.g., `5`, `7`), it is possible to access other users’ baskets.  

---
## Impact
- **Information Disclosure:** Attackers can enumerate user profiles, baskets, and payment card information.  
- **Privilege Escalation:** Attackers can act on behalf of other users by modifying predictable IDs in requests.  
- **Financial Abuse:** Predictable card IDs may be abused for unauthorized payments.  

**Severity:** High  

---
## Recommendations
1. **Use UUIDs:** Replace sequential IDs with non-predictable identifiers (UUID/GUID).  
2. **Authorization Checks:** Enforce strict server-side access controls to ensure users can only access their own resources.  
3. **Rate Limiting & Monitoring:** Detect and block suspicious enumeration attempts.  
