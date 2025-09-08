
**Category:** #a1_broken_access_control #a5_security_misconfiguration  

---
## Overview
The **Recycle API endpoint** allows users to submit recycle requests. However, the API does not properly validate ownership of the **UserId** and **AddressId** fields.  
This means an attacker can manipulate these fields to **submit recycle requests on behalf of other users**.  

---
## Proof of Concept
### Request
```http
POST /api/Recycles/ HTTP/1.1
Host: localhost:3000
Authorization: Bearer <valid_token>
Content-Type: application/json

{"UserId":7,"AddressId":7,"quantity":112}
```

- `UserId` and `AddressId` were manually set to another user’s values.  
- The request is **accepted by the server**, proving missing authorization checks.  

---
## Impact
- **Unauthorized actions:** An attacker can abuse the recycle function in another user’s name.  
- **Data integrity issues:** Malicious users can pollute or overwrite legitimate recycle records.  
- **Business impact:** Fraudulent recycle claims could lead to abuse of any rewards, credits, or statistics associated with recycling.  

**Severity:** High  

---
## Recommendations
1. **Implement Ownership Validation:** Ensure that the `UserId` and `AddressId` always match the authenticated user’s identity.  
2. **Enforce Server-Side Security:** Derive `UserId` from the **JWT/session token** instead of trusting client input.  
3. **Access Control Checks:** Apply proper **authorization checks** for sensitive operations.  
4. **Audit Logs:** Track unusual or suspicious recycle requests for further investigation.  
