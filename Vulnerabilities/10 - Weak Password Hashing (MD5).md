
**Category:** A02 – Cryptographic Failures  

---
## Overview
The application stores user passwords hashed with **MD5** without any salting or key stretching.  
- MD5 is a deprecated hashing algorithm, vulnerable to **fast brute-force attacks**.  
- The lack of salting means identical passwords result in identical hashes, enabling **rainbow table attacks**.  
- MD5 is also subject to **collision vulnerabilities**, which could allow an attacker to authenticate with a crafted input matching an existing hash.  

---
## Proof of Concept
During testing, leaked or captured password hashes (e.g., from the SQLi vulnerability in [1 - SQL Injection in Product Search](/Vulnerabilities/1%20-%20SQL%20Injection%20in%20Product%20Search.md)) were confirmed to be in **MD5 format**.  
- Example: `827ccb0eea8a706c4c34a16891f84e7b` → `"12345"`  

This demonstrates that common passwords can be cracked instantly using public wordlists like **rockyou.txt**.  

---
## Impact
- **Credential Compromise:** Any exposed password hash can be reversed quickly.  
- **Account Takeover:** Attackers can use cracked passwords to log in as legitimate users.  
- **Privilege Escalation:** If administrative accounts are cracked, attackers may gain full control.  

**Severity:** Critical  

---
## Recommendations
1. **Use Modern Hashing Functions:** Replace MD5 with strong algorithms such as **bcrypt**, **scrypt**, **PBKDF2**, or **Argon2**.  
2. **Add Salts and Peppers:** Ensure unique per-user salts are stored alongside hashes, and optionally use a server-side secret (pepper).  
3. **Rehash Existing Passwords:** Force a password reset for all users and rehash with the new algorithm.  
4. **Implement Strong Password Policy:** Require longer, complex passwords to slow brute-force attacks.  
