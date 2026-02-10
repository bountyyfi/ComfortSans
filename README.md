# ComfortSans™

**"The font that cares about your eyes"** — or does it?

> **Security Research PoC** — This is a proof-of-concept demonstrating font-based phishing via OpenType glyph substitution. **Do not use this for malicious purposes.**

![Format: TTF + WOFF2](https://img.shields.io/badge/Format-TTF%20%2B%20WOFF2-green.svg)
![Type: Security PoC](https://img.shields.io/badge/Type-Security%20PoC-red.svg)

---

## What Is This?

ComfortSans is a weaponized font that abuses the OpenType `calt` (Contextual Alternates) feature to **visually replace text on screen** while leaving the underlying DOM, clipboard, and source code completely untouched.

When this font renders specific strings — like `paypal.com`, `0x742d35Cc`, or `bountyy.fi` — the GSUB table silently swaps each glyph with an attacker-controlled alternate. Your eyes see one thing; every inspection tool sees another.

### What it bypasses

- View Source
- DOM Inspector
- Copy-Paste
- Ctrl+F Search
- Screen Readers
- CSP Headers
- SAST / DAST scanners
- Code Review
- WAF Rules
- SRI Hashes (on HTML)

The only thing it **doesn't** bypass: human eyes that know what to look for.

---

## How It Works

1. The font includes a GSUB table with **Chaining Context Substitution** rules
2. The `calt` feature is **enabled by default** in every browser, every OS, and most applications
3. When the text shaping engine encounters a target string, it substitutes glyph IDs during rendering
4. Unicode codepoints in the text buffer are **never modified** — only glyph IDs change
5. The HTML is 100% clean. The font binary is the payload.

Font size: ~1.5KB WOFF2. Looks like any other web font in a PR or dependency.

---

## Demo

Open `demo.html` in a browser to see the PoC in action. It demonstrates:

- **Domain phishing** — `paypal.com` renders as a different domain
- **Crypto address swap** — `0x742d35Cc...` renders as a different wallet
- **Custom substitution** — `bountyy.fi` renders as something else entirely
- **Interactive test box** — type target strings and watch the swap live

---

## Files

| File | Format | Description |
|------|--------|-------------|
| `ComfortSans-Regular.ttf` | TrueType | Font binary with malicious GSUB table |
| `ComfortSans-Regular.woff2` | WOFF2 | Web-optimized version (~1.5KB) |
| `demo.html` | HTML | Interactive PoC demonstration page |

---

## About

Created by [@bountyyfi](https://github.com/bountyyfi) — security researcher and bug bounty hunter at **Bountyy Oy**.

This PoC demonstrates that **no existing tool scans font ligature/substitution tables for malicious glyph swaps**. It's a blind spot in the entire security toolchain — from static analysis to WAFs to code review.

---

## Responsible Use

This project is published for **security research and awareness only**. It is intended to:

- Demonstrate a novel phishing vector to the security community
- Encourage browser vendors and security tools to inspect font substitution tables
- Raise awareness among developers who blindly trust custom fonts

**Do not use this to deceive, defraud, or phish real users.**

---

## License

ComfortSans is released for security research purposes under the [SIL Open Font License, Version 1.1](https://scripts.sil.org/OFL).
