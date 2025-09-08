**Category:** A01 – Broken Access Control

---
## Overview
A broken access control vulnerability was identified in the **basket endpoint** of the OWASP Juice Shop application.  
By manipulating the **basket ID** parameter in the request, an authenticated user can access the shopping carts of other users.

---
## Proof of Concept

### Steps to Reproduce
1. Log in as a regular user.  
2. Intercept the basket request using Burp Suite.  
3. Modify the basket ID in the request URL to another user’s ID.  
4. Observe that the application returns the contents of another user's basket.

### Example Request
```http
GET /rest/basket/6 HTTP/1.1
Host: localhost:3000
Authorization: Bearer <valid_user_token>
User-Agent: Mozilla/5.0
Accept: application/json, text/plain, */*
```

### Example Response
- The server responds with basket data belonging to another user.  
- No authorization checks are in place to verify ownership of the basket.
![Pasted image 20250906124949](Pasted%20image%2020250906124949.png)
---
## Impact
- **Confidentiality breach:** Attackers can view the shopping cart contents of any user.  
- **Business impact:** Could expose sensitive order details, product preferences, and partial payment flow.  
- **Privilege escalation risk:** With further chaining, attackers may modify or purchase items on behalf of others.  

**Severity:** High

---
## Recommendations
1. **Implement ownership checks**: Ensure that users can only access baskets linked to their own account.  
2. **Use session-bound identifiers** instead of exposing sequential basket IDs.  
3. **Apply server-side authorization controls** for every request to sensitive endpoints.  
4. **Perform regular access control testing** to detect IDOR (Insecure Direct Object Reference) vulnerabilities.  
