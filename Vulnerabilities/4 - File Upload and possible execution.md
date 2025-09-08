**Category:** A03 – Injection / A05 – Security Misconfiguration

---
## Overview
During testing of the OWASP Juice Shop file upload functionality, a **file upload vulnerability** was discovered.  
The application allows uploading of malicious files if the file extension or MIME type is modified to match an accepted type (e.g., `.pdf`).  
This opens the possibility of **Remote Code Execution (RCE)** if the uploaded file can be executed on the server or leveraged via SSRF.

---
## Proof of Concept

### Vulnerable Behavior
- The application validates file type based on file extension and superficial signature.  
- Malicious files (e.g., bash scripts) renamed and typechanged (e.g. hexeditor) are accepted.

**Example payload:** `shell.pdf` containing script code, with the filytype header of a pdf (`0x25 0x50 0x44 0x46`)

### Observed Risk
- File is successfully uploaded despite containing executable code.
- If server-side execution or SSRF is triggered, the attacker can gain full control over the server.

---

### Impact
- **Remote Code Execution (RCE):** Potential to execute arbitrary code on the server.  
- **Server compromise:** Full administrative access possible if RCE is exploited.  
- **Data exfiltration:** Sensitive data can be accessed or modified.  
- **Denial of Service:** Malicious files could crash services if executed.

**Severity:** Critical (CVSS high/critical range)

---

## Recommendations
1. **Restrict file types strictly**: Validate both MIME type and content, not just file extension.  
2. **Scan uploaded files** with antivirus or content inspection tools.  
3. **Store uploads outside of the web root** to prevent direct execution.  
4. **Implement access controls** to prevent unauthorized users from uploading files.  
5. **Disable execution permissions** for uploaded content.  
6. **Regularly test file upload endpoints** for bypasses and malicious payloads.  
