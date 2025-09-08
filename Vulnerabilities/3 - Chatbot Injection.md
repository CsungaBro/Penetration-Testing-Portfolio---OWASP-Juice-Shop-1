**Category:** A03 – Injection

---
## Overview
During testing of the OWASP Juice Shop chatbot, an **injection vulnerability** was discovered in the first chatbot prompt, which asks for the user's name.  
Supplying special characters such as `"`, `'`, `%`, or `$` causes the server to return a `500 Internal Server Error`.  
This behavior indicates improper input handling and a potential injection point.

---
## Proof of Concept

### Vulnerable Input
```text
'"%$
```

![Pasted image 20250815051613](Pasted%20image%2020250815051613.png)
### Observed Behavior
- The server returns a **500 Internal Server Error**.
- This indicates that user input is not properly sanitized before being processed by the server.
![Pasted image 20250815051736](Pasted%20image%2020250815051736.png)

---

### Impact
- **Potential code injection:** Unsanitized input may allow execution of arbitrary code or queries.  
- **Denial of Service (DoS):** Server crashes when certain characters are provided.  
- **Data exposure:** If exploited, could lead to access to sensitive information or backend resources.

**Severity:** Medium to High (depending on exploitability)

---
## Recommendations
1. **Sanitize and validate input** for all chatbot fields.  
2. **Escape special characters** before processing input on the server.  
3. **Implement proper error handling** to prevent server crashes from malformed input.  
4. **Regular security testing** for injection vulnerabilities in user-interactive components.  
