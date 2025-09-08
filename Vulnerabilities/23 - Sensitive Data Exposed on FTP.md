**Category:** #a3_data_exposure #a5_sec_misconfig  

---
## Finding  
Sensitive corporate information was discovered in a file hosted on the publicly accessible FTP server:  

**File:** `acquisitions.md`  
**Location:** [http://localhost:3000/ftp/acquisitions.md](http://localhost:3000/ftp/acquisitions.md)  

### Extracted Content (snippet):
```
# Planned Acquisitions

> This document is confidential! Do not distribute!

Our company plans to acquire several competitors within the next year.
This will have a significant stock market impact...
```

The document explicitly states it is **confidential** and contains **non-public business strategy** related to planned acquisitions, which could have a **direct impact on stock markets** and corporate reputation.  

---

## Impact  
- **Information Disclosure:** Competitors or malicious actors could gain insight into upcoming acquisitions.  
- **Insider Trading Risk:** The disclosed information could be abused for financial gain, leading to **legal and compliance violations**.  
- **Reputation Damage:** Loss of trust from shareholders, partners, and customers if it becomes public.  
- **Attack Surface Expansion:** Sensitive documents may provide hints about internal operations, employees, or partners that attackers can leverage.  

**Severity:** **Critical** — Exposed business-critical and confidential information.  

---
## Proof of Concept (PoC)  
1. Navigate to `/ftp/acquisitions.md`  
2. Download and open the file → **confidential content accessible without authentication**  

---
## Recommendations  
1. **Restrict Access:**  
   - Immediately remove confidential documents from the publicly accessible FTP server.  
   - Restrict FTP access using authentication and network segmentation.  

2. **Data Handling Policy:**  
   - Ensure sensitive business information is stored securely (encrypted at rest, access-controlled).  
   - Prohibit storage of confidential files in web-exposed directories.  

3. **Monitoring & Alerting:**  
   - Monitor FTP access logs for unauthorized downloads.  
   - Implement DLP (Data Loss Prevention) to detect accidental leaks.  

4. **Legal & Compliance:**  
   - Notify legal/compliance teams of potential data exposure.  
   - Assess regulatory impact (e.g., insider trading laws, GDPR if personal data involved).  