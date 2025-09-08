**Category:** #a7_ident_and_auth_failures  

---

## Finding  
The login endpoint does not implement sufficient protections against **brute-force** or **credential stuffing attacks**.  
Attackers can send unlimited login attempts without being blocked, delayed, or challenged.  

---

## Impact  
- **Account Takeover Risk:** Weak or reused credentials can be discovered via automated tools.  
- **Credential Stuffing Exposure:** Leaked credentials from other breaches can be tested at scale.  
- **High Attack Surface:** Since no rate-limiting, CAPTCHA, or IP-based restrictions exist, the login is a primary entry point for attackers.  

**Severity:** **High** — Direct compromise of user accounts, including administrative ones, is possible.  

---

## Proof of Concept (PoC)  
1. Capture a login request (e.g., `POST /rest/user/login`).  
2. Automate requests with a wordlist using a tool such as **Hydra**, **Burp Intruder**, or **ffuf**:  

```bash
ffuf -w /usr/share/wordlists/rockyou.txt \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"email":"admin@juice-sh.op","password":"FUZZ"}' \
  -u http://localhost:3000/rest/user/login \
  -fr "Invalid email or password" \
  -mc 200
```  

3. Observe multiple requests being processed without blocking or delays.  
4. Successful credentials are returned in responses, confirming brute-force feasibility.  
![Pasted image 20250903133910](Pasted%20image%2020250903133910.png)
---
## Recommendations  
1. **Rate Limiting / Lockout:**  
   - Limit failed login attempts per user/IP (e.g., 5 attempts, then temporary lockout).  

2. **Progressive Delays:**  
   - Add incremental delays (e.g., 1s, 5s, 10s) after repeated failed attempts.  

3. **CAPTCHA / Bot Detection:**  
   - Enforce CAPTCHA after multiple failed attempts.  
   - Integrate WAF/IDS with bot protection features.  

4. **Multi-Factor Authentication (MFA):**  
   - Require MFA for privileged or sensitive accounts.  

5. **Monitor & Alert:**  
   - Track failed login attempts in logs.  
   - Alert on suspicious brute-force patterns.  