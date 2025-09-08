# 🛡️ Pentesting Portfolio – OWASP Juice Shop

This repository documents a penetration test performed against the intentionally vulnerable **OWASP Juice Shop** application.  
The goal of this project is to demonstrate practical penetration testing skills, structured reporting, and remediation planning according to industry standards (OWASP Top 10).  

---

## Project Structure
- **Executive Summary** – High-level overview of the engagement and key findings.  
- **OWASP Top 10 Mapping** – Grouping of discovered vulnerabilities by OWASP Top 10 categories.  
- **Findings Documentation** – Detailed, step-by-step descriptions of each vulnerability (PoC, screenshots, impact).  
- **Tooling & Techniques** – Tools and methodologies used during testing.  
- **Remediation Roadmap** – Prioritized plan for fixing the vulnerabilities.  

---

## Key Highlights
- Over **20 unique vulnerabilities** discovered and documented.  
- Issues cover a wide range of **OWASP Top 10 categories**: injection, broken access control, cryptographic flaws, XSS, insecure design, and more.  
- Detailed write-ups with **HTTP request/response samples, exploitation steps, and remediation advice**.  
- Findings supported with tooling outputs (Burp Suite, FFUF, Gobuster, custom scripts).  

---

## Tools Used
- [Burp Suite](https://portswigger.net/burp) – Web application proxy and exploitation.  
- [Gobuster](https://github.com/OJ/gobuster) – Directory brute-forcing.  
- [FFUF](https://github.com/ffuf/ffuf) – Fuzzing for hidden endpoints.  
- Python – Custom scripts for automation.  

---

## How to Navigate
Start with the main documentation:  

- [1. Executive Summary](1.%20Executive%20Summary.md)  
- [2. Scope](2.%20Scope.md)
- [3. Tooling & Techniques](3.%20Tooling%20&%20Techniques.md)
- [4. OWASP TOP 10 mapping](4.%20OWASP%20TOP%2010%20mapping.md)
- [5. Remediation Roadmap (Prioritized Fixes)](5.%20Remediation%20Roadmap%20(Prioritized%20Fixes).md)
