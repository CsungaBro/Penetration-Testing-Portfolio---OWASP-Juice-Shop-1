26 - JWT Token Contains the Password  
#a2_crypto #a3_injection #a1_broken_access_control  

## Finding  
The **JWT token** issued after authentication contains sensitive information, including the **user’s password (hashed with MD5)**. This severely increases the impact of other vulnerabilities such as XSS, since leaking a JWT token also leaks the user’s password hash.  

### Example JWT  
``eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MSwidXNlcm5hbWUiOiIiLCJlbWFpbCI6ImFkbWluQGp1aWNlLXNoLm9wIiwicGFzc3dvcmQiOiIwMTkyMDIzYTdiYmQ3MzI1MDUxNmYwNjlkZjE4YjUwMCIsInJvbGUiOiJhZG1pbiIsImRlbHV4ZVRva2VuIjoiIiwibGFzdExvZ2luSXAiOiIiLCJwcm9maWxlSW1hZ2UiOiJhc3NldHMvcHVibGljL2ltYWdlcy91cGxvYWRzL2RlZmF1bHRBZG1pbi5wbmciLCJ0b3RwU2VjcmV0IjoiIiwiaXNBY3RpdmUiOnRydWUsImNyZWF0ZWRBdCI6IjIwMjUtMDktMDggMTE6MTE6MDYuNjkxICswMDowMCIsInVwZGF0ZWRBdCI6IjIwMjUtMDktMDggMTE6MTE6MDYuNjkxICswMDowMCIsImRlbGV0ZWRBdCI6bnVsbH0sImlhdCI6MTc1NzMzMDM3M30.o5tG72YqNfJUYTer2BVq5Mp4QXJSJDzv1brrsHLUIwJNKFTelFD1JD8V7DXAJW7tnnHnPMQ4KGx00D1Qo47qQFbDB4HI-rxnG9SEgzzSW43mWOP4bPpRcCP3vPutoe0Jmaq1-l_sHLtSmu1ueDoWpECO3dteIEaw4bgsGQd4y6g``  

### Decoded JWT Payload  
``{
  "status": "success",
  "data": {
    "id": 1,
    "username": "",
    "email": "admin@juice-sh.op",
    "password": "0192023a7bbd73250516f069df18b500",
    "role": "admin",
    "profileImage": "assets/public//images/uploads/defaultAdmin.png",
    "isActive": true,
    "createdAt": "2025-09-08 11:11:06.691 +00:00",
    "updatedAt": "2025-09-08 11:11:06.691 +00:00"
  },
  "iat": 1757330373
}``  

The `password` field contains an **MD5 hash** of the admin user’s password.  

## Impact  
- **Credential exposure**: Password hashes are leaked directly through JWTs. With weak hashing (MD5, no salt), they can be cracked with tools like `hashcat` or `john the ripper`.  
- **Chaining with XSS**: If an attacker can exfiltrate the JWT (e.g., via XSS), they automatically obtain the user’s password hash.  
- **Privilege escalation**: If an admin JWT is stolen, the attacker gains both the active session and the admin’s hashed password.  
- **Replay attacks**: JWTs remain valid even after logout, making this exposure more dangerous (see [20 - JWT token is still valid after loging out](20%20-%20JWT%20token%20is%20still%20valid%20after%20loging%20out)).  
**Severity:** **High**

## Recommendations  
- **Remove sensitive data**: Never store or transmit user passwords (even hashed) in JWTs or client-side storage.  
- **Use strong hashing algorithms**: Replace MD5 with a modern KDF such as `bcrypt`, `scrypt`, or `Argon2`, with unique salts.  
- **Short-lived tokens**: Reduce token lifetime and require reauthentication after logout.  
- **JWT claims principle**: Only include minimal required information (e.g., `sub`, `exp`, `role`).  

