**Category:** A04 – Insecure Design  

---
## Overview
The application uses **Base64-encoded coupon codes**.  
- Base64 encoding is **not encryption** and can be trivially decoded.  
- If coupon codes follow a predictable pattern (e.g., sequential IDs, known words), they can be **brute-forced or enumerated**.  
- This may allow attackers to generate **valid discount codes** without authorization.  

---
## Proof of Concept
Example observation:  
```json
{"couponData":"bnVsbA=="}
```

Decoded from Base64:  
```
null
```

If valid coupons follow a similar encoding structure, an attacker could attempt to brute force or guess valid coupons by:  
1. Decoding existing coupon codes.  
2. Identifying patterns (numeric sequences, words, UUIDs).  
3. Re-encoding guesses with Base64 and submitting them in the request.  

---
## Impact
- **Financial Loss:** Attackers may apply unauthorized discounts or free items.  
- **Abuse of Business Logic:** Legitimate coupons could be reverse-engineered and reused indefinitely.  
- **Reputation Damage:** Exploitation could undermine customer trust.  

**Severity:** Medium to High (depending on business impact).  

---
## Recommendations
1. **Use Secure Random Tokens:** Replace Base64-encoded values with **cryptographically secure random identifiers**.  
2. **Server-Side Validation:** Store coupon logic on the server; never rely on predictable or reversible tokens.  
3. **Limit Coupon Use:** Restrict coupons by user ID, expiration date, and number of uses.  
4. **Monitor for Abuse:** Implement logging and alerting for abnormal coupon redemption patterns.  
