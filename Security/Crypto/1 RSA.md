# RSA Encryption — Complete Overview

## 1. What is RSA?

**RSA** is an **asymmetric (public-key) cryptographic algorithm** used for:

- Secure key exchange
- Digital signatures
- Authentication
- Encrypting small pieces of data (like symmetric keys)

RSA is named after its inventors:
**Rivest, Shamir, and Adleman (1977)**.

---

## 2. Why RSA Is Asymmetric

RSA uses **two different keys**:

- **Public Key** → Used for encryption or signature verification  
- **Private Key** → Used for decryption or signing  

This allows secure communication **without sharing a secret key in advance**.

---

## 3. Mathematical Foundation

RSA security is based on the **difficulty of factoring large composite numbers**.

> It is easy to multiply two large prime numbers,  
> but extremely hard to factor their product back into the original primes.

---

## 4. Key Generation (Step-by-Step)

### Step 1: Choose Two Large Primes

```
p, q
```

Example (small for demonstration):
```
p = 61
q = 53
```

---

### Step 2: Compute Modulus `n`

```
n = p × q
n = 3233
```

---

### Step 3: Compute Euler’s Totient Function

```
φ(n) = (p − 1)(q − 1)
φ(n) = 3120
```

---

### Step 4: Choose Public Exponent `e`

Constraints:
- 1 < e < φ(n)
- gcd(e, φ(n)) = 1

Common value:
```
e = 65537
```

---

### Step 5: Compute Private Exponent `d`

```
d ≡ e⁻¹ mod φ(n)
(e × d) mod φ(n) = 1
```

---

### Resulting Keys

**Public Key**
```
(e, n)
```

**Private Key**
```
(d, n)
```

---

## 5. Encryption and Decryption

### Encryption

```
c = m^e mod n
```

### Decryption

```
m = c^d mod n
```

---

## 6. Why Modulo Makes RSA Secure

Modulo arithmetic:
- Limits number size
- Creates one-way functions
- Prevents reversing without private key

---

## 7. Why Raw RSA Is Insecure

❌ Deterministic  
❌ Malleable  
❌ Vulnerable to chosen-plaintext attacks  

**Never use RSA without padding**.

---

## 8. RSA Padding (Critical)

### PKCS#1 v1.5 (Legacy)

```
EM = 0x00 || 0x02 || PS || 0x00 || M
```

❌ Vulnerable to padding oracle attacks.

---

### OAEP (Recommended)

- Uses hash functions
- Uses randomness
- Provides semantic security

```
c = RSA(OAEP(m))
```

Common hash: **SHA-256**

---

## 9. RSA Digital Signatures

### Signing

```
signature = hash(m)^d mod n
```

### Verification

```
hash = signature^e mod n
```

Recommended padding:
- **RSA-PSS**

---

## 10. Performance Characteristics

| Aspect | RSA |
|------|----|
| Key Size | 2048–4096 bits |
| Speed | Slow |
| Use Case | Keys & signatures |

---

## 11. Hybrid Encryption Model

1. Generate AES key
2. Encrypt data with AES
3. Encrypt AES key using RSA

Used in:
- TLS
- Secure agents
- Messaging systems

---

## 12. Recommended Parameters (2025)

| Parameter | Value |
|--------|------|
| Key Size | ≥ 2048 bits |
| Padding | OAEP (SHA-256) |
| Signatures | RSA-PSS |
| Public Exponent | 65537 |

---

## 13. Summary

- RSA is asymmetric
- Based on prime factorization
- Uses modular arithmetic
- Padding is mandatory
- Best for key exchange & signatures

---

**RSA without padding is broken.  
RSA with OAEP/PSS is secure.**
