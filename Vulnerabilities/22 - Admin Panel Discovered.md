
**Category:** #a5_sec_misconfig #a1_broken_acces_control  

---
## Finding  
An **administration panel** was discovered at:  

👉 [http://localhost:3000/#/administration](http://localhost:3000/#/administration)  

Currently, the panel is only accessible when logged in with an **admin account**. However:  
- The location of the admin panel is **publicly exposed**, making it easy for attackers to find.  
- No obfuscation or secondary security measures are applied.  
- Combined with issues like [21 - Login is Brute-Forceable](#21---login-is-brute-forceable) and [19 - Weak Password Policy](#19---weak-password-policy), the risk of compromise increases significantly.  

---
## Impact  
- **Privilege Escalation Risk:** If an attacker obtains admin credentials (via brute force, weak password, or token leakage), they can directly access the administration interface.  
- **Sensitive Functionality Exposure:** The panel likely contains user management, system configurations, or business-critical operations.  
- **Reconnaissance Risk:** The panel’s location may help attackers prioritize attacks on admin accounts.  

**Severity:** **High** — Direct access to core administrative functionality.  

---
## Proof of Concept (PoC)  
1. Browse to `/administration` without login → redirected/blocked.  
2. Authenticate as admin → full panel is accessible.  

This confirms that the panel is reachable and functional.  

---
## Recommendations  
1. **Access Control Hardening:**  
   - Restrict admin panel access at the **network level** (e.g., only allow trusted IPs/VPN).  
   - Implement **role-based access control** strictly on backend endpoints (not only on frontend routing).  

2. **Obfuscation (Defense-in-Depth):**  
   - Move the admin panel to a less predictable path (e.g., `/internal-admin`).  
   - Do not reference the panel publicly in frontend code if unnecessary.  

3. **Authentication Enhancements:**  
   - Enforce **MFA** for all admin accounts.  
   - Implement **rate limiting** and **brute-force protection**.  

4. **Monitoring & Alerting:**  
   - Log all access attempts to the admin panel.  
   - Alert on unauthorized or suspicious access attempts.  
