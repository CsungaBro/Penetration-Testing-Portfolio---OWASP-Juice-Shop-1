**Category:** #a3_data_exposure #a1_broken_acces_control  

---
## Finding  
The application displays the **email addresses** of users who leave reviews or comments on products.  

- Example:  
  ```
  Author: bender@juice-sh.op
  Comment: "Not the admin"
  ```

This represents a **privacy issue** since emails are considered **personally identifiable information (PII)** under data protection regulations (e.g., GDPR).  

---
## Impact  
- **Privacy Violation:** Users’ personal data is publicly exposed.  
- **Spam/Phishing:** Attackers can scrape email addresses and use them for targeted phishing or spam campaigns.  
- **User Trust:** Visible email addresses may discourage users from leaving reviews.  
- **Regulatory Risk:** Non-compliance with GDPR/CCPA regarding the protection of user data.  

**Severity:** **Medium–High** (depending on regulatory scope and number of exposed users).  

---
## Proof of Concept (PoC)  
1. Navigate to any product page with reviews.  
2. Observe that the **author’s email address** is displayed in plain text.  

---
## Recommendations  
1. **Mask Email Addresses:**  
   - Show usernames or display a partially masked email (e.g., `b***r@juice-sh.op`).  
   - Provide users with the ability to set a **display name** instead of forcing email exposure.  

2. **Data Minimization:**  
   - Only expose data that is strictly necessary. Email addresses should not be shown in reviews.  

3. **Privacy by Design:**  
   - Review all user-facing features for unnecessary exposure of personal data.  
   - Update privacy policy and ensure compliance with GDPR/CCPA.  