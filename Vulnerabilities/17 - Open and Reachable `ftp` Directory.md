
**Category:** #a5_security_misconfiguration #a3_sensitive_data_exposure  

---

## Finding  
The `/ftp` directory is publicly accessible and lists multiple files. While only `.pdf` and `.md` files are directly viewable from the browser, several sensitive files are exposed, including:  
- **Backup files**: `coupons_2013.md.bak`, `package-lock.json.bak`, `package.json.bak`  
- **Encrypted/compiled files**: `incident-support.kdbx`, `encrypt.pyc`  
- **Potential secrets**: `suspicious_errors.yml`  
- **Hidden challenges / Easter eggs**: `eastere.gg`  
---
## Impact  
- **Information Disclosure:** Sensitive documents (`acquisitions.md`, `legal.md`) can reveal business or legal information.  
- **Source Code & Dependency Exposure:** Backup files (`package.json.bak`, `package-lock.json.bak`) may expose internal packages, dependencies, or even secrets.  
- **Password Database:** `.kdbx` files (KeePass database) may contain credential stores — if cracked, they provide full access.  
- **Weak Security by Obscurity:** Although `.pyc` files are not directly readable, they can be decompiled to recover source code.  
- **Data Exfiltration Risk:** PDF invoices/orders (`order_*.pdf`) expose potentially sensitive customer data.  

**Severity:** High — Due to the combination of sensitive business files, backups, and potential credentials.  

---
## Recommendations  
1. **Restrict Access:**  
   - Disable direct public access to `/ftp`.  
   - Serve only explicitly required files through controlled endpoints.  

2. **Remove Backup & Sensitive Files:**  
   - Delete `.bak`, `.json`, `.kdbx`, `.pyc`, and other non-public files from accessible directories.  
   - Store sensitive files outside the web root.  

3. **Enable Directory Listing Restrictions:**  
   - Disable directory indexing to prevent attackers from enumerating files.  

4. **File-Type Whitelisting:**  
   - If `/ftp` must remain accessible, configure the server to serve only whi
