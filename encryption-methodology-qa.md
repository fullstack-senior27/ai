# 🔐 QA: Encryption Methodology

This QA document provides an overview of encryption methodologies used in computer systems and communications. It covers key concepts, types of encryption, protocols, and best practices.

---

## ✅ General Concepts

**Q1: What is encryption?**  
**A:** Encryption is the process of converting plaintext data into an unreadable format (ciphertext) using an algorithm and a key, to protect the data from unauthorized access.

**Q2: What is the difference between encryption and hashing?**  
**A:**  
- Encryption is reversible (with a key) and used to protect data confidentiality.  
- Hashing is one-way and used for data integrity and verification.

---

## 🔑 Types of Encryption

**Q3: What are the two main types of encryption?**  
**A:**  
1. **Symmetric encryption**: The same key is used for both encryption and decryption.  
2. **Asymmetric encryption**: Uses a public key for encryption and a private key for decryption.

**Q4: Examples of symmetric encryption algorithms:**  
**A:** AES, DES, 3DES, Blowfish, ChaCha20

**Q5: Examples of asymmetric encryption algorithms:**  
**A:** RSA, ECC (Elliptic Curve Cryptography), ElGamal

---

## 🔒 Encryption Protocols

**Q6: What are common protocols that use encryption?**  
**A:**  
- TLS/SSL (for HTTPS)  
- IPsec (for VPN)  
- PGP/GPG (for email)  
- SSH (for secure shell access)  
- S/MIME (for email)

**Q7: What is TLS and how does it use encryption?**  
**A:** TLS (Transport Layer Security) is a protocol that uses asymmetric encryption to exchange a session key and then uses symmetric encryption to encrypt the session.

---

## 🔐 Key Management

**Q8: What is key exchange?**  
**A:** Key exchange is the process of securely sharing a key between parties, commonly using the Diffie-Hellman or ECDH algorithm.

**Q9: How should encryption keys be stored?**  
**A:**  
- Use secure key vaults (e.g., AWS KMS, HashiCorp Vault)  
- Never hard-code keys in code  
- Use hardware security modules (HSMs) for high security

---

## 🧪 Use Cases

**Q10: How is encryption used in databases?**  
**A:**  
- Transparent Data Encryption (TDE) encrypts data at rest.  
- Column-level encryption protects sensitive fields.  
- Encryption keys are managed via KMS or HSMs.

**Q11: How is encryption used in messaging apps?**  
**A:**  
- End-to-end encryption (E2EE) ensures only sender and receiver can read messages.  
- Uses asymmetric encryption to exchange session keys, then symmetric encryption for the session.

---

## 🛡️ Best Practices

**Q12: What are best practices for implementing encryption?**  
**A:**  
- Use strong, up-to-date algorithms (e.g., AES-256, RSA 2048+)  
- Rotate keys periodically  
- Avoid using outdated algorithms (e.g., MD5, SHA-1, DES)  
- Validate certificates in HTTPS  
- Use secure random number generators

**Q13: How to test encryption implementation?**  
**A:**  
- Use test vectors from algorithm specs  
- Perform security audits  
- Use penetration testing tools (e.g., SSL Labs, Wireshark)

---

## 🧠 Bonus Tips

- Use hybrid encryption (asymmetric for key exchange, symmetric for data)  
- For password storage, use salted hashing (e.g., bcrypt, Argon2)  
- Always enable encryption in transit (TLS) and at rest  
- Educate teams on key leakage risks

