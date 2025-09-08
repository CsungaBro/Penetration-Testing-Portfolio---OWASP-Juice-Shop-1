
**Category:** #a1_broken_access_control #a2_crypto_failures  

---
## Finding  
When a user **logs out**, their **JWT token remains valid** and can still be used to access protected endpoints.  

This indicates that the server does not implement **server-side token invalidation** (blacklisting, session revocation, or rotation). JWT tokens are therefore valid until their natural expiration time, regardless of user logout.  

---
## Impact  
- **Session Hijacking Risk:** If an attacker gains access to a JWT (e.g., via XSS, local storage compromise, or traffic interception in misconfigured setups), they can continue using it even after the user logs out.  
- **No Effective Logout:** Users expect logout to terminate their session, but in practice, the token remains active.  
- **Privilege Escalation Persistence:** If a privileged token is stolen, it remains usable until expiration, increasing the attack window.  

**Severity:** **High** — logout is a critical security control and its failure undermines session management.  

---
## Proof of Concept (PoC)  
1. Log in as a valid user and intercept the JWT token.  
2. Perform a logout request.  
3. Send an authenticated request using the **old JWT token** (e.g., `GET /api/Users/profile`).  
![Pasted image 20250903090400](Pasted%20image%2020250903090400.png)
4. Observe that the request still succeeds, proving logout does not invalidate the token.  
![Pasted image 20250903090423](Pasted%20image%2020250903090423.png)
![Pasted image 20250903090429](Pasted%20image%2020250903090429.png)
---
## Recommendations  
1. **Implement Token Blacklisting:**  
   - Maintain a server-side list of invalidated tokens until they expire.  
   - Check tokens against this list for every request.  

2. **Shorten JWT Expiry Times:**  
   - Use short-lived access tokens (e.g., 5–15 minutes).  
   - Pair with refresh tokens that can be invalidated server-side.  

3. **Rotate Tokens on Login/Logout:**  
   - Invalidate old tokens whenever a new login occurs.  
   - On logout, explicitly revoke the refresh token and associated access token.  

4. **Consider Stateful Sessions:**  
   - If strict logout is required, a traditional session mechanism may be more suitable than long-lived stateless JWTs.  

---
