# CyberBench

CyberBench is an **offline-first Android cybersecurity toolkit**, inspired by GCHQ’s CyberChef, designed for **data transformation, cryptography, and security analysis** directly on Android devices.

The project focuses on:
- Deterministic, auditable operations
- Recipe-based data processing pipelines
- Native Android performance
- Privacy-first, offline execution

---

## Features

### Core
- CyberChef-style **operation pipelines** with drag-to-reorder
- **Collapsible category menu** grouping operations by type (17 categories)
- **Recipe persistence** — save, load, search, and tag recipes (Room database)
- **Step Inspector** — debug pipelines with per-step intermediate output, timing, and hex/text views
- Recipe import/export (JSON)
- Offline execution (no network required)

### Available Operations (74)

| Category | Operations |
|----------|------------|
| **Encoding** | Base64, Base32, Hex, Binary, URL Encode, HTML Encode, Text Encoding |
| **Encryption** | AES (CBC/ECB/GCM), RC4, XOR, ROT13 |
| **Hashing** | Hash (SHA-256, SHA-1, SHA-512, MD5), HMAC, CRC32 |
| **Parsing** | JWT Decode, X.509 Parse, Certificate Chain, CSR Decode, PEM/DER Convert |
| **Text** | Find/Replace, String Reverse, Extract, Defang, Regex Workbench, IOC Extractor |
| **Compression** | Gzip (compress/decompress) |
| **Data Formats** | JSON Format (pretty-print/minify) |
| **Date / Time** | Unix Timestamp (decode/encode) |
| **Security** | Password Generate, Passphrase Generate, Password Strength, BCrypt, SCrypt, Argon2, PBKDF2, TOTP/HOTP, Hash Identify, Wordlist Mutator, Credential Format, JWT Forge, TOTP Brute |
| **Protocols** | DNS Decode, HTTP Decode, TLS Decode, TCP Flags, IP Header Parse, Packet Craft, DNS Rebind, Banner Grab |
| **Detection** | Magic Detect (encoding auto-detection with recursive decode) |
| **Steganography** | LSB Encode, LSB Decode, Steg Detect, EXIF Extract, Metadata Strip |
| **Comparison** | Hex Diff, Entropy Analysis, Structural Diff |
| **Batch** | Batch Process |
| **Web Exploit** | SQLi Payloads, XSS Payloads, SSRF URL, Path Traversal, HTTP Smuggling |
| **Offensive** | Cyclic Pattern, Format String, ELF Parser, Disassembler, ROP Gadgets |
| **Android Offense** | APK Manifest, DEX Parser, Intent Fuzzer, APK Signature, Smali Patch |

### Additional Tools
- **Web Tools** — HTTP form builder with request tooling
- **Network Scanner** — port scanning and network discovery
- **App Scanner** — Android app component analysis and risk assessment
- **Broadcast Monitor** — capture and inspect Android broadcasts

---

## 🧱 Architecture Overview

CyberBench follows **Clean Architecture** principles:

```
UI (Android Views)
├── CatalogAdapter (collapsible category groups)
├── PipelineAdapter (drag-to-reorder)
├── StepInspectorActivity (pipeline debugger)
├── RecipeLibraryActivity (save/load/search)
└── I/O panels
|
Domain (pure logic, no Android deps)
├── RecipeExecutor (execute + executeWithSteps)
├── Operation interface
├── OperationRegistry + OperationBootstrap
└── Operations (74, organized by category)
    ├── encoding/       ├── encryption/
    ├── hashing/        ├── parsing/
    ├── text/           ├── compression/
    ├── formats/        ├── datetime/
    ├── security/       ├── protocols/
    ├── detection/      ├── steganography/
    ├── comparison/     ├── batch/
    ├── web/            ├── offensive/
    └── android/
|
Data (serialization, persistence)
├── Room database (RecipeDao, RecipeEntity)
├── RecipeRepository
└── JSON serialization
```


### Key Principles
- **All operations are pure functions**
- **ByteArray in → ByteArray out**
- No dynamic code execution
- No inline scripts in recipes
- No external network calls during execution

---

## 📄 Recipe Format

Recipes are defined as **versioned JSON documents** describing a sequence of operations.

### Example
```json
{
  "schema_version": "1.0",
  "recipe_id": "7f42bcb2-18e1-45da-b2ff-12aa8d93c201",
  "name": "JWT Payload Hash",
  "input_type": "text",
  "operations": [
    {
      "id": "jwt-decode",
      "type": "jwt_decode",
      "params": {
        "verify_signature": false
      }
    },
    {
      "id": "hash",
      "type": "hash",
      "params": {
        "algorithm": "SHA-256",
        "output_format": "hex"
      }
    }
  ]
}
```

See docs/recipe_schema.json for the full schema.

## 🔐 Security Model

- Offline-first execution
- No telemetry by default
- No file system writes from operations
- Explicit error-handling rules per operation (HALT / SKIP / CONTINUE)
- Cryptographic primitives provided by well-known libraries (BouncyCastle, java.security)

This design allows recipes to be safely shared and audited.

## 🛠 Tech Stack

- **Language:** Kotlin
- **UI:** Jetpack Compose
- **Architecture:** MVVM + Clean Architecture
- **Crypto:** BouncyCastle, java.security
- **Serialization:** kotlinx.serialization
- **Persistence:** Room (SQLite)
- **Async:** Kotlin Coroutines
- **Min SDK:** 26 (Android 8.0)

## Development Status

- [x] Architecture design
- [x] Recipe JSON schema
- [x] Core execution engine
- [x] 74 operations across 17 categories
- [x] Unit tests (71 test files)
- [x] Android UI with three-panel layout and collapsible category catalog
- [x] Recipe persistence — Room database with save, load, search, tag, and delete
- [x] Step Inspector — per-step debug view with hex/text toggle and timing
- [x] Password & key derivation utilities (BCrypt, SCrypt, Argon2, PBKDF2)
- [x] TOTP/HOTP generator
- [x] Network protocol decoders (DNS, HTTP, TLS, TCP, IP)
- [x] Regex workbench with pattern library
- [x] IOC extractor (IPs, domains, hashes, CVEs, and more)
- [x] Encoding auto-detection ("Magic" mode)
- [x] TLS/Certificate toolkit (chain parsing, CSR decode, PEM/DER conversion)
- [x] Steganography tools (LSB encode/decode, steg detection, EXIF/metadata)
- [x] Data comparison tools (hex diff, entropy analysis, structural diff)
- [x] Batch processing mode
- [x] Web exploit payload generators (SQLi, XSS, SSRF, path traversal, HTTP smuggling)
- [x] Credential & auth attack tools (hash ID, wordlist mutation, JWT forge, TOTP brute)
- [x] Network offense tools (packet crafting, DNS rebinding, banner grabbing)
- [x] Binary & exploit dev tools (cyclic patterns, format strings, ELF parser, disassembler, ROP gadgets)
- [x] Android offense tools (APK manifest analysis, DEX parsing, intent fuzzing, APK signatures, smali patching)
- [ ] Play Store release

## 🤝 Contributing (Future)

Contributions will be welcome once the core engine stabilizes.

Planned contribution areas:
- New operations
- Test vectors
- Documentation
- Performance improvements

Contribution guidelines will be added later.

## 📜 License

CyberBench is released under a **Source-Available License**.  
You may view and inspect the source code and run it on your own device for personal, non-commercial evaluation.  
Redistribution, commercial use, and publishing compiled builds are not permitted without prior written permission.  
See the [LICENSE](LICENSE) file for full terms.

## Disclaimer

This application is intended for **authorized security testing, education, and defensive analysis** only.
Offensive operations (exploit payloads, fuzzing, bypass techniques) are provided for use in
**penetration testing engagements, CTF competitions, security research, and defensive validation**.

Use responsibly and only against systems you have explicit authorization to test.
