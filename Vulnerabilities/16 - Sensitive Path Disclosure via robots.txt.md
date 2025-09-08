
**Category:** #a5_security_misconfiguration  

---

## Finding  
The application exposes a publicly accessible `robots.txt` file containing the following content:  

```
User-agent: *
Disallow: /ftp
```  

This reveals the existence of sensitive directory `/ftp` that is not be intended for public access.  

---
## Impact  
- **Information Disclosure:** Attackers can use this information to directly target the `/ftp` directory.  
- **Sensitive File Exposure:** If directory listing or weak authentication is in place, sensitive files (e.g., backups, configs, logs) may be accessible.  
- **Attack Surface Expansion:** Knowledge of hidden or private endpoints accelerates reconnaissance and exploitation attempts.  

**Severity:** Medium

---
## Recommendations  
1. **Avoid Sensitive Disclosures in robots.txt:**  
   - Do not list sensitive or internal paths in `robots.txt`.  
   - Use access control, not obscurity, to protect directories.  
2. **Harden the `/ftp` Path:**  
   - Ensure authentication and authorization are enforced.  
   - Disable directory listing.  
   - Remove or restrict access if the directory is not required.  
3. **Security through Design:**  
   - Treat all endpoints as potentially discoverable and secure them accordingly.  
