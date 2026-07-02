# Security Policy

## ⚠️ Educational Purpose Disclaimer

**Thyreos-lite is an experimental educational tool.** It demonstrates password-based cryptographic primitives (PBKDF2, AES-256-GCM) in the browser and is **NOT intended for production use** or for protecting sensitive real-world data.

## Supported Versions

| Version | Supported |
|---------|-----------|
| 2.0.x (PBKDF2 + AES-256-GCM) | ✅ Educational support only |
| 1.0.x (RSA + ECDSA + AES-GCM) | ❌ Deprecated |

## Reporting Security Issues

If you discover a security vulnerability, please open an issue with the label `security`. However, please understand that:

- This project is for **demonstration and learning**
- No warranty or liability is provided
- Users assume all risk when using this tool

## Known Limitations

- Browser-based cryptography is subject to side-channel attacks
- No protection against compromised endpoints (keyloggers, malware)
- PBKDF2 with 200,000 iterations provides reasonable but not extreme resistance to brute-force
- No protection against weak passwords — users must choose strong passphrases
- Keys are derived in browser memory and exist only during the session
- No formal security audit has been performed
- AES-256-GCM auth-tag protects ciphertext integrity but cannot prevent brute-force password guessing

## Threat Model

### What the tool protects against

- ✅ Passive network eavesdropping (no data leaves the device)
- ✅ File tampering (GCM auth-tag detects modifications)
- ✅ Filename tampering (AAD binds filename to ciphertext)
- ✅ Accidental data exposure during transport (no server upload)

### What the tool does NOT protect against

- ❌ Compromised browser or operating system
- ❌ Keyloggers or screen capture malware
- ❌ Brute-force attacks against weak passwords
- ❌ Physical access to unlocked device during encryption/decryption
- ❌ Side-channel attacks (timing, cache, power analysis)

## Responsible Disclosure

We appreciate responsible disclosure of any issues that could help improve the educational value of this project.

## Password Recommendations

Since this tool uses password-based encryption, the security of your files depends entirely on password strength:

- **Minimum:** 12 characters (enforced by UI)
- **Recommended:** 16+ characters with mixed case, numbers, and symbols
- **Best practice:** Use a passphrase of 5+ random words (Diceware style) or a password manager
- **Never reuse** the same password across multiple files or services
