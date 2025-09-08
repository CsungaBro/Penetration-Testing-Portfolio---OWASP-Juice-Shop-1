**Category:** A05 – Security Misconfiguration / Business Logic Flaw  

---
## Overview
A **business logic vulnerability** exists in the basket functionality of OWASP Juice Shop.  
It is possible to set a **negative quantity** for items in the basket. During checkout, this results in the system processing negative totals, effectively allowing an attacker to gain money from the store instead of paying.

---
## Proof of Concept

### Steps to Reproduce
1. Log in as a user and add an item to the basket.  
2. Intercept the request when changing the basket item quantity.  
3. Modify the `quantity` field to a negative value (e.g., `-5`).  
4. Complete the checkout process.  
5. The transaction is processed, and the attacker effectively earns money instead of being charged.

### Example Request
```http
PUT /api/BasketItems/17 HTTP/1.1
Host: localhost:3000
Authorization: Bearer <valid_token>
Content-Type: application/json

{"quantity":-5}
```

### Example Outcome
- The basket total becomes negative.  
- The checkout completes, simulating a **payout to the attacker**.
![Pasted image 20250817101524](Pasted%20image%2020250817101524.png)
---

## Impact
- **Financial loss:** Direct exploitation could drain the platform by generating negative transactions.  
- **Fraud:** Attackers could repeatedly exploit this to gain unlimited credits or money.  
- **Integrity issues:** Business rules are bypassed, making the platform untrustworthy.  

**Severity:** Critical  

---

## Recommendations
1. **Input validation:** Ensure that product quantities cannot be set below 1.  
2. **Server-side enforcement:** Reject any basket or checkout requests with invalid values.  
3. **Business logic checks:** Validate that the total price cannot be negative before finalizing payment.  
4. **Transaction monitoring:** Detect and block unusual negative-value transactions.  


