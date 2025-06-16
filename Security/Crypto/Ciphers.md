# TLS Cipher Suites Explained

Example: `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`

A cipher suite defines the algorithms used in a TLS handshake. It typically includes four components:

1. **Key Exchange Algorithm**  
   `ECDHE` (Elliptic Curve Diffie-Hellman Ephemeral)  
   Determines how encryption keys are securely exchanged.  
   *Alternatives: RSA, DH*

2. **Authentication Algorithm**  
   `ECDSA` (Elliptic Curve Digital Signature Algorithm)  
   Verifies the server’s identity using its certificate.  
   *Alternatives: RSA, DSA*

3. **Bulk Data Encryption**  
   `AES_256_GCM`  
   Encrypts the actual data transmitted between client and server.  
   *Ensures confidentiality*

4. **Message Authentication Code (MAC) / Hash Function**  
   `SHA384`  
   Ensures message integrity and authenticity.  
   *Used for verifying data was not altered*

TLS 1.3 cipher suite: TLS_AES_256_GCM_SHA384
# TLS 1.3 Cipher Suites: Why Fewer Algorithms Are More Secure

In **TLS 1.3**, cipher suites only list **two algorithms**:

- **Bulk Data Encryption**
- **Message Authentication Code (MAC)**

Example:  
`TLS_AES_256_GCM_SHA384`

At first glance, it may seem less secure than TLS 1.2 (which lists four algorithms). However, TLS 1.3 is **actually more secure and efficient**. Here's why:

---

## 🔐 Key Exchange & Authentication

In TLS 1.3:

- **Ephemeral Diffie-Hellman (DHE/ECDHE)** is **always used** for key exchange  
  → Ensures **forward secrecy** by default
- **Authentication** is handled via the **certificate's digital signature algorithm** (e.g., RSA, ECDSA)  
  → Not specified in the cipher suite

These are no longer shown in the cipher suite name because they are standardized and enforced.

---

## ✅ Simplified & More Secure

TLS 1.3:
- **Removes** insecure/legacy options (e.g. static RSA or DH key exchange)
- **Reduces complexity**, lowering the risk of misconfiguration
- **Standardizes secure algorithms**, making negotiation faster and more predictable

---

## 🔒 Encryption & Integrity

The cipher suite still includes:
- **Encryption Algorithm**: e.g., `AES_256_GCM`
- **MAC / Hash Function**: e.g., `SHA384`  
  → Ensures confidentiality and integrity of data

---



