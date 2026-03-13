# ECC Secure Communication System 🔐

> Python implementation of authenticated Elliptic Curve Cryptography — ECDH key exchange, ECDSA authentication, AES-256-GCM file encryption, and MITM attack prevention.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)
![Cryptography](https://img.shields.io/badge/Cryptography-ECC%20%7C%20AES--256--GCM-brightgreen)
![Tests](https://img.shields.io/badge/Tests-10%2F10%20Passing-success)

**Course**: Software Optimization — Final Exam Project (Winter Semester 2025)
**University**: University of Europe for Applied Sciences

---

## Overview

Standard ECDH key exchange is vulnerable to Man-in-the-Middle (MITM) attacks. This project solves that by integrating ECDSA digital signatures into the handshake — ensuring only authenticated parties can derive a shared session key.

The system simulates two communicating endpoints (Alice & Bob) performing a full authenticated cryptographic workflow over a network socket, with GUI-based interaction and file-level encryption/decryption.

**What it demonstrates:**
- ECC key pair generation using the SECP256R1 curve
- Authenticated ECDH key exchange using ECDSA signatures
- Shared session key derivation via HKDF-SHA256
- AES-256-GCM file encryption and decryption
- GUI-based simulation of real two-endpoint communication
- MITM attack detection and integrity protection validation

---

## Security Properties

| Threat | Mitigation |
|---|---|
| Man-in-the-Middle attack | ECDSA signatures authenticate all public keys |
| Key substitution | Handshake terminates on signature verification failure |
| Replay attacks | Ephemeral ECDH keys + random HKDF salts per session |
| File tampering | AES-GCM authentication tag detects any modification |
| Session key compromise | Forward secrecy via ephemeral key pairs |

---

## Test Results — 10/10 Passing ✅

| Test | Result |
|---|---|
| ECC key pair generation | ✅ Pass |
| ECDH shared secret equality (Alice = Bob) | ✅ Pass |
| HKDF 256-bit key derivation | ✅ Pass |
| AES-GCM encrypt/decrypt — text file | ✅ Pass |
| AES-GCM encrypt/decrypt — binary file | ✅ Pass |
| GUI handshake via localhost | ✅ Pass |
| File encryption after handshake | ✅ Pass |
| File decryption after handshake | ✅ Pass |
| Tamper detection (AES-GCM integrity check) | ✅ Pass |
| Missing trusted key → handshake blocked | ✅ Pass |

---

## Folder Structure

```
ECC-Secure-Communication/
│
├── Demo.ipynb              # Full implementation — Parts 1–4
├── Report.pdf              # Full project report with security analysis
├── test_document.txt       # Sample plaintext file for testing
├── Figures/                # Runtime and file-size benchmark graphs
└── README.md               # This file
```

**Generated automatically during execution:**
```
alice_id_priv.pem / alice_id_pub.pem
bob_id_priv.pem  / bob_id_pub.pem
ecc_shared_key.bin
*.enc   (encrypted output files)
*.dec   (decrypted output files)
```

---

## Requirements

- Python 3.9 or higher
- Jupyter Notebook
- Libraries: `cryptography`, `matplotlib`, `tkinter` (included with Python on Windows)

```bash
pip install cryptography matplotlib
```

---

## How to Run

### Part 1 — ECC Key Generation & ECDH Shared Secret

Run the first cell in `Demo.ipynb`.

**Expected:** ECC key pairs generated, both parties derive identical shared secrets.

---

### Part 2 — AES-256-GCM Encryption & Decryption

Run the second cell.

**Expected:** `test_document.txt.enc` created, `test_document.txt.dec` created, decrypted file matches original byte-for-byte.

---

### Part 3 — GUI Authenticated Key Exchange

Simulates secure communication between Alice and Bob over a localhost socket.

1. Open `Demo.ipynb` in **two separate Jupyter Notebook tabs**
2. **Tab A (Alice):**
   - Run Part 3 cell → select role = `Alice` → click **Start Server**
3. **Tab B (Bob):**
   - Run Part 3 cell → select role = `Bob` → set Alice IP = `127.0.0.1` → click **Connect + Handshake**

**Expected:** Handshake completes, `ecc_shared_key.bin` saved on both sides, GUI shows "Handshake OK".

---

### Part 4 — File Encryption & Decryption via GUI

After a successful handshake:

- **Encrypt File** → select any file → `.enc` file created
- **Decrypt File** → select `.enc` file → `.dec` file created, identical to original

---

## Security Tests

### MITM / Authentication Test
Remove `alice_id_pub.pem` or `bob_id_pub.pem` and attempt handshake.
**Expected:** Handshake blocked with clear error — signature verification fails.

### Tamper Detection Test
Modify bytes in any `.enc` file and attempt decryption.
**Expected:** Decryption fails — AES-GCM integrity check raises authentication error.

---

## Benchmark Graphs

The notebook includes a benchmarking cell that auto-generates:
- Runtime performance graph (ECC KeyGen, ECDH Derive, HKDF, AES-GCM Encrypt/Decrypt)
- File size comparison graph (original vs `.enc` vs `.dec`)

Saved to `Figures/`.

---

## Notes

- Alice and Bob GUIs must run in **separate Jupyter notebook kernels**
- All cryptographic keys and output files are generated automatically during execution
- See `Report.pdf` for full system design, security analysis, and validation evidence

---

## Academic Context

- **Degree**: MSc Software Engineering — University of Europe for Applied Sciences (Potsdam)
- **Module**: Software Optimization — Final Exam Project
- **Group**: No. 7 — Arpan Rai, **Ashwin Shrestha**, Ranjeet Yadav, Abhishek
- **Supervisor**: Prof. Dr. Rand Kouatly
- **Date**: January 2026
