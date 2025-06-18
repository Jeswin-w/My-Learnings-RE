# EC, ECDH, ECDHE — A Concise Guide

## 🔷 1. What is EC (Elliptic Curve)?

- EC = **Mathematical foundation** used in cryptography.
- Defines an elliptic curve over a finite field `𝔽ₚ`.
- Enables **point addition** and **scalar multiplication** operations.

🔹 **Note:** EC **by itself** does nothing — it's a base for algorithms like ECDH and ECDSA.

---

## 🔐 2. ECDH (Elliptic Curve Diffie-Hellman)

- Used for **key exchange** between two parties.
- Relies on **scalar multiplication**: `PrivateKey * GeneratorPoint = PublicKey`.

### ECDH Process:

1. Agree on curve `(E)`, generator point `G`, and prime `p`.
2. Server:  
   - Private key: `a`  
   - Public key: `A = a * G`
3. Client:  
   - Private key: `b`  
   - Public key: `B = b * G`
4. Shared secret:  
   - Both compute `(a * b) * G`  
   - Hash of this shared point becomes symmetric key.

❌ **No forward secrecy**: If private keys leak later, past sessions can be decrypted.

---

## 🔐 3. ECDHE (Ephemeral ECDH)

- Same as ECDH but uses **temporary (ephemeral)** keys.
- New key pair generated for **each session**.

✅ **Provides Forward Secrecy**: Even if server key is compromised, past sessions remain safe.

---

## 🛡️ 4. ECDSA (Elliptic Curve Digital Signature Algorithm)

- Used for **signing** and **authentication**, not key exchange.
- Often used with TLS when server cert is based on EC key.

---

## 🔄 5. In TLS Cipher Suites

| Cipher Suite Example                              | Meaning                                               |
|---------------------------------------------------|-------------------------------------------------------|
| `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`           | ECDHE for key exchange, RSA for authentication       |
| `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`         | ECDHE for key exchange, ECDSA for authentication     |

---

## 📌 6. Notes on TLS 1.3

- TLS 1.3 **only uses ECDHE** (or its finite field cousin, DHE) for key exchange.
- **RSA key exchange and static ECDH are removed.**
- Authentication is separate and done via **signatures** (RSA or ECDSA).

---

## 🔚 Summary

| Term    | Stands For                      | Use Case           | Forward Secrecy? |
|---------|----------------------------------|---------------------|------------------|
| EC      | Elliptic Curve                  | Math foundation     | N/A              |
| ECDH    | Elliptic Curve Diffie-Hellman   | Key exchange        | ❌ No             |
| ECDHE   | Ephemeral ECDH                  | Key exchange        | ✅ Yes            |
| ECDSA   | EC Digital Signature Algorithm  | Identity/signature  | ✅ Yes (signatures) |

---
