# Contributing to Thyreos-lite

Thank you for your interest in contributing! This is an **educational project** designed to demonstrate browser-based cryptography concepts using PBKDF2 and AES-256-GCM.

## ⚠️ Important Notice

This project is intended for **educational and research purposes only**. It is not a replacement for production-grade encryption tools. If you find a security issue, please see [SECURITY.md](SECURITY.md).

## How to Contribute

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-idea`)
3. Commit your changes (`git commit -m 'Add amazing idea'`)
4. Push to the branch (`git push origin feature/amazing-idea`)
5. Open a Pull Request

## Guidelines

- Keep the code **client-side only** (no server components)
- Maintain compatibility with modern browsers supporting Web Crypto API
- All cryptographic operations must use the native Web Crypto API — no external crypto libraries
- Document any cryptographic changes thoroughly (update README.md and this file)
- Add comments explaining the educational purpose of features
- Ensure the UI remains accessible and responsive
- Do not introduce external dependencies (the project must remain a single HTML file)

## Areas for Contribution

- **UI/UX improvements** — Better accessibility, mobile responsiveness, dark/light themes
- **Educational content** — Tooltips, explanations, visual diagrams of the crypto flow
- **Security hardening** — Within the educational scope (e.g., password strength indicators, iteration count tuning)
- **Browser compatibility** — Testing and fixes for older browser versions
- **Localization** — Translations to other languages (currently Russian UI with English README)
- **Performance** — Optimizing large file handling (up to 100 MB limit)

## Code of Conduct

Be respectful. This is a learning space for cryptography enthusiasts. See [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for details.

## Development Setup

Since this is a pure HTML/CSS/JS project, no build step is required:

```bash
git clone https://github.com/mironov-bmt/thyreos-lite.git
cd thyreos-lite
# Simply open index.html in your browser
# Or use a local server for development:
python3 -m http.server 8000
```

## Testing Checklist

Before submitting a PR, please verify:

- [ ] Encryption works in Chrome, Firefox, Edge, Safari
- [ ] Decryption correctly restores the original file
- [ ] Wrong password produces a clear error (no crash)
- [ ] Tampered `.thy` file is rejected (GCM auth-tag failure)
- [ ] Filename tampering is detected (AAD mismatch)
- [ ] Files up to 100 MB process without UI freezing
- [ ] Password strength meter works correctly
- [ ] All UI text is in Russian (as per current design)
- [ ] No external network requests are made during encryption/decryption
