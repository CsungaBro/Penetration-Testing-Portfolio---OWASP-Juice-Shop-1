**Category:** #a7_identification_and_authentication_failures  

---
## Finding  
The application enforces a **minimal password policy**:  
- Password must only be **at least 5 characters long**.  
- No checks for:  
  - Uppercase / lowercase mix  
  - Numbers  
  - Special characters  
  - Common or dictionary words  

This allows users (and potentially attackers) to set weak and easily guessable passwords such as `12345`, `admin1`, or `qwert`.  

---
## Impact  
- **Brute Force & Credential Stuffing:** Short, weak passwords significantly reduce the time needed for brute-force or dictionary attacks.  
- **Account Takeover:** If an attacker gains access to one account, lateral movement may occur (especially if the compromised account is an administrator).  
- **Regulatory Non-Compliance:** Most security standards (NIST, OWASP, GDPR, ISO 27001) require strong password policies.  

**Severity:** **High** — Passwords are the first line of defense, and the lack of enforcement exposes all accounts to risk.  

---
## Proof of Concept (PoC)  
- Create a new account with the password:  
  ```
  12345
  ```  
- Account creation is accepted successfully, proving weak passwords are allowed.  

---
## Recommendations  
1. **Strengthen Password Policy:**  
   - Minimum length of **12+ characters**.  
   - Require a mix of **uppercase, lowercase, numbers, and special characters**.  
   - Disallow commonly used or breached passwords (e.g., use [HaveIBeenPwned password API](https://haveibeenpwned.com/Passwords)).  

2. **Implement Account Lockout & Rate Limiting:**  
   - Lock account or introduce CAPTCHA after several failed login attempts.  
   - Introduce exponenti
