
**Category:** #a5_security_misconfiguration  

---
## Finding  
The application sets a **Content-Security-Policy (CSP) header** only on the `/profile` endpoint. Other endpoints across the application **do not enforce CSP**, leaving them unprotected against common client-side attacks.  

---
## Impact  
- **Increased Risk of XSS:** Without CSP, injected scripts (via input fields, parameters, or stored data) can execute in the user’s browser.  
- **Clickjacking & Data Exfiltration:** A missing or weak CSP makes it easier for attackers to embed malicious iframes, exfiltrate cookies or tokens, and steal sensitive data.  
- **Inconsistent Security Posture:** Attackers can deliberately target unprotected endpoints rather than the CSP-enforced `/profile` page.  

**Severity:** Medium  

---
## Recommendations  
1. **Apply CSP Globally:**  
   - Enforce a restrictive CSP on all endpoints, not just `/profile`.  
2. **Use a Secure Baseline Policy:**  
   - Example:  
```http
Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none'; frame-ancestors 'none'; base-uri 'self'
```
3. **Gradual Hardening:**  
   - Start with a **report-only** CSP to identify violations, then enforce progressively stricter rules.  
4. **Regular Audits:**  
   - Continuously monitor and adjust the CSP as the application evolves to ensure ongoing protection.  
