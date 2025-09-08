
**Category:** #a3_sensitive_data_exposure #a5_security_misconfiguration  

---
## Finding  
The FTP server contains an exposed KeePass database file:  

- **File:** `incident-support.kdbx`  

KeePass `.kdbx` files are commonly used to securely store sensitive credentials, API keys, and other secrets.  

---
## Impact  
- **Credential Exposure:** If the `.kdbx` file can be cracked (weak master password, dictionary/brute-force attack), it may reveal sensitive credentials such as:  
  - Administrator logins  
  - Database connection strings  
  - API keys and tokens  
- **Persistence Risk:** Once compromised, these credentials may provide long-term unauthorized access to the system.  

**Severity:** **Critical** — Because this file likely contains high-value secrets that can compromise the entire application and infrastructure.  

---
## Recommendations  
1. **Remove Sensitive Files from Public Access:**  
   - Immediately delete the `.kdbx` file from the public FTP directory.  
   - Store such files securely in internal storage, not accessible via the web server.  

2. **Rotate Credentials:**  
   - Assume credentials within the file are compromised.  
   - Rotate all potentially exposed credentials (databases, admin accounts, API keys).  

3. **Secure Backups:**  
   - Ensure no backup or archive files are exposed on public directories.  
   - Use proper access controls on internal storage.  

4. **Monitoring:**  
   - Monitor access logs for any downloads of this file.  
   - Investigate potential misuse of exposed credentials.  

---

## Next Steps for Testing  
- Attempt to download the `.kdbx` file.  
- Use tools like **KeeCracker**, **John the Ripper**, or **Hashcat** to test for weak passwords.  
- If cracked, enumerate stored credentials and assess their validity.  
