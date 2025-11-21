# Diffie–Hellman Key Exchange — Explained (and example code)

**Summary (TL;DR)**  
Diffie–Hellman (DH) lets two parties (client and server) establish a shared secret over an insecure channel. Each party has a *private key*, they exchange *public keys*, and each computes the same shared secret using modular exponentiation. That shared secret (or a derived value) becomes the *pre-master secret* used in TLS to derive symmetric keys.

---

## Correct notation and formulas

Common variables:

- `p` — a large prime modulus (public)
- `g` — a generator (also called base) modulo `p` (public)
- `a` — private key of party A (client), a random integer `1 < a < p-1`
- `b` — private key of party B (server), a random integer `1 < b < p-1`
- `A` — public key of A: `A = g^a mod p`
- `B` — public key of B: `B = g^b mod p`
- `s` — shared secret

Protocol steps:

1. Both agree on public parameters `p` and `g`.
2. A picks private `a`, computes `A = g^a mod p`, sends `A` to B.
3. B picks private `b`, computes `B = g^b mod p`, sends `B` to A.
4. A computes `s = B^a mod p`.
5. B computes `s = A^b mod p`.

Because modular exponentiation is commutative in this way:  
`B^a ≡ (g^b)^a ≡ g^(ab) ≡ (g^a)^b ≡ A^b (mod p)`.  
Both sides get the same `s = g^(ab) mod p`.

> Note: Your earlier formula `m^x mod n` is a single modular exponentiation. Diffie–Hellman uses that operation twice, with two private exponents and the public base `g` (and modulus `p`).

---

## How DH is used in TLS

- TLS (pre-TLS 1.3) uses **DHE** (ephemeral DH) or **ECDHE** (elliptic-curve) to provide key agreement with forward secrecy.
- The DH computed shared secret becomes the *pre-master secret*. TLS key schedule derives symmetric keys (MAC, encryption keys) from this pre-master secret (often combined with other nonces and PRF/HKDF).
- In practice, use well-tested library support (OpenSSL, BoringSSL, Java `javax.net.ssl`, etc.). Do **not** roll your own crypto for production.

---

## Security considerations

- **Use large primes**: `p` should be at least 2048 bits (better 3072+ or use elliptic-curve DH).
- **Safe primes / standardized groups**: Prefer standardized groups (RFC 7919 / RFC 7919 for finite-field groups) or use modern ECC groups (X25519).
- **Ephemeral keys**: Use ephemeral keys (generate fresh `a` or `b` per session) for forward secrecy.
- **Validate public keys**: Ensure received public values are in the correct range and not trivial (e.g., `1`, `0`, or `p-1`) to avoid small-subgroup attacks.
- **Use established libraries**: Use `cryptography` in Python, or Java TLS / BouncyCastle, except for learning/testing purposes.
- **Don't use small toy primes** in production — they are insecure.

---
