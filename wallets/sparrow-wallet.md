---
tags: [wallet, bitcoin, desktop, psbt, multisig, coinjoin, taproot, privacy]
ecosystem: bitcoin
platforms: [windows, macos, linux]
license: Apache-2.0
language: Java / JavaFX
status: active
version: 2.5.3
access_date: 2026-08-12
related: [[psbt-hardware-signing]], [[spv-electrum-server]], [[full-node-wallet]], [[coinjoin-privacy]]
---

# Sparrow Wallet

## Overview

Sparrow is a desktop Bitcoin wallet for Windows, macOS, and Linux, built by Craig Raw (GitHub: @craigraw) and first released in March 2020. Its stated emphasis is security, privacy, and transparency — the wallet surfaces UTXO-level detail and transaction graph information rather than abstracting it away. It is Apache-2.0 licensed, written in Java 25 / JavaFX, and is the de-facto reference desktop wallet for Bitcoin power users who prioritise self-sovereignty and hardware wallet integration.

Sparrow is a tab-based desktop application, deliberately not browser-based. The project philosophy treats browser-based wallets as having an unacceptably large attack surface compared to dedicated native applications.

- **GitHub:** https://github.com/sparrowwallet/sparrow
- **Website:** https://sparrowwallet.com
- **Latest version:** 2.5.3 (released 30 July 2026)
- **Created:** 22 March 2020
- **Author:** Craig Raw (sole primary maintainer)

---

## Adoption Metrics (as at 2026-08-12)

| Metric | Value | Source |
|--------|-------|--------|
| GitHub stars | 2,090 | GitHub API, 2026-08-12 |
| GitHub forks | 312 | GitHub API, 2026-08-12 |
| Contributors | 32 (GitHub page 1–32) | GitHub API, 2026-08-12 |
| MAU | [NOT FOUND] — no public data; desktop-only, no app-store analytics available | — |
| Total downloads (latest 20 releases, Apr 2024–Jul 2026) | ~1,325,097 | GitHub releases API, 2026-08-12 |
| Downloads of v2.5.3 (latest, 30 Jul 2026) | 56,279 | GitHub releases API, 2026-08-12 |
| Downloads of v2.5.2 (May 2026) | 101,039 | GitHub releases API, 2026-08-12 |
| Downloads of v2.3.1 (Nov 2025) | 110,776 | GitHub releases API, 2026-08-12 |
| Platforms | Windows 10+, macOS 11+ (Intel and Apple Silicon), Linux (deb, rpm, tar.gz; Intel and ARM64) | sparrowwallet.com/download/, 2026-08-12 |

Note: Sparrow is desktop-only. No mobile apps exist. No F-Droid listing. Download counts from GitHub releases are the best available proxy for user base.

---

## Key Management

### Seed and keystore types

- **BIP32 HD wallets** — fully supported; all derivation paths user-configurable
- **BIP39 mnemonic seeds** — standard 12/24-word import and creation
- **BIP39 passphrase (25th word)** — supported; Sparrow deliberately stores nothing derived from the passphrase in the wallet file (per BIP39 spec), so an incorrect passphrase silently derives a different address set
- **SLIP39 mnemonics** — import supported from v2.0.0 (File > New Wallet > Software Wallet)
- **Extended public key (xpub/zpub/Ypub etc.) watch-only keystores** — fully supported; master fingerprint can be set to a placeholder (e.g. `00000000`) when unknown
- **Output descriptors** — full import and export; wallets can be created from descriptors; considered the canonical backup format for multisig
- **BIP47 PayNym keystores** — nested wallets tracking BIP47 payment channels

### Wallet encryption

Wallet files are encrypted using **Argon2** (winner of the 2015 Password Hashing Competition), configured to take at least 500ms on modern hardware to derive the key — significantly stronger than the PBKDF2 used by many mobile wallets.

### Wallet file location

| Platform | Default location |
|----------|-----------------|
| macOS | `~/.sparrow` |
| Linux | `~/.sparrow` (or XDG locations if configured) |
| Windows | `%APPDATA%\Sparrow` |

XDG Base Directory Specification supported on macOS and Linux from v2.5.3 (opt-in, resolved independently per category).

---

## Script Types

All major Bitcoin script types are fully supported:

| Script type | Supported |
|-------------|-----------|
| P2PKH (Legacy) | Y |
| P2SH (nested Segwit multisig, etc.) | Y |
| P2WPKH (native SegWit, bech32) | Y |
| P2WSH (native SegWit multisig) | Y |
| P2TR (Taproot, bech32m) | Y — single sig from v1.4.3 (Jul 2021); full Taproot signing including script-path inspection |
| Silent Payments (BIP352) | Y — sending from v2.3.0 (Oct 2025); receiving wallets from v2.5.0 (May 2026); hardware wallet support via BIP375 DLEQ proofs from v2.4.0 |

---

## Signing

### PSBT

PSBT (BIP174 / Partially Signed Bitcoin Transactions) is the architectural foundation of Sparrow — the entire keystore design and transaction editor are built around PSBTs. PSBTv2 file loading is supported from v2.1.0.

Signing flow: user builds a transaction in the Send tab → Sparrow constructs a PSBT → hardware wallet plugins or software keystore sign → final signed transaction broadcast. At every step the full transaction fields (inputs, outputs, scripts, signatures, pubkeys, sighash types) are inspectable in the transaction editor.

Non-default sighash type warnings added in v2.5.2.

### Hardware wallet support

Sparrow supports the broadest hardware wallet coverage of any Bitcoin desktop wallet, via a combination of direct USB/HID (using the **Lark** library, replacing HWI in v2.1.0) and airgapped protocols:

**USB-connected (direct, no HWI required from v2.1.0):**
- Trezor (Model One, T, Safe 3, Safe 5) — USB
- Ledger (Nano S, S Plus, X, Stax, Flex) — USB
- BitBox02 — USB; native pairing from v2.1.0 (no external BitBoxApp required)
- Blockstream Jade / Jade Plus — USB and QR
- ERA — USB (added v2.5.3, July 2026)

**Airgapped — QR codes (BC-UR / UR standard):**
- Foundation Passport — QR
- SeedSigner — QR (seed QR, CompactSeedQR, ur:crypto-seed, ur:crypto-bip39)
- Keystone — QR
- Krux — QR (with optional BBQr from v2.0.0)
- Jade / Jade Plus — QR
- Specter DIY — QR
- Seed Tool — QR

**Airgapped — microSD (file-based PSBT):**
- Coldcard (Mk1–Mk4, Q1) — microSD PSBT file; BBQr from v1.8.3

**Airgapped — NFC smartcard:**
- Coinkite Tapsigner — NFC (via PC/SC reader; supported from v1.7.2)
- SatChip / Satochip — NFC and connected (from v1.8.0)
- Status Keycard — NFC (from docs airgapped smartcard guide)

**Deprecated (removed):**
- Cobo Vault — deprecated in v1.7.0; config flag `showDeprecatedImportExport` required

QR-based airgapped signing uses the **UR standard** (animated fountain codes / BC-UR v2), allowing arbitrary-length PSBTs to be transmitted. Webcam QR scanning is built in.

### Air-gapped flow

```
Watch-only wallet in Sparrow → constructs unsigned PSBT
    → exports via:
        (a) QR code (animated BC-UR fountain code) — scanned by hardware wallet camera
        (b) microSD file (.psbt) — for Coldcard
        (c) NFC tap — for smartcards

Hardware wallet → verifies outputs, amounts, and derivation paths on device screen
    → user confirms on device
    → device signs → signed PSBT returned via same channel

Sparrow → imports signed PSBT → broadcasts
```

---

## Multisig

- **Native m-of-n multisig** — all script types (P2SH, P2WSH, P2TR) for any combination of keystores
- **Mixed-vendor multisig** — any combination of hardware wallets, software keystores, and watch-only xpubs; Sparrow acts as PSBT coordinator
- **Multisig backup:** output descriptors (the canonical recovery format); Coldcard export format; Passport Multisig export; Jade multisig export; Specter DIY export; Unchained Capital JSON (import only)
- **Multisig coordinator flow:** Sparrow creates the unsigned PSBT → each cosigner adds their signature via their preferred channel (USB, QR, SD) → Sparrow finalises and broadcasts
- **Ledger multisig registrations** stored and resent to device from v2.1.0 to avoid re-registration

---

## Privacy Features

### CoinJoin — history and current state

**Whirlpool (Samourai):**
- Added: v1.5.0 (September 2021) — full Samourai Whirlpool CoinJoin integration
- Stonewallx2 (2-person simulated coinjoin) and Stowaway (payjoin via PayNym) — added v1.5.3 (December 2021)
- Removed: v1.9.0 (24 April 2024) — "Remove Whirlpool client, and other Soroban related features and dependencies" — this immediately followed the arrest of Samourai Wallet founders (Keonne Rodriguez and William Hill) by US authorities on 24 April 2024 on money laundering and unlicensed money transmission charges

**Current CoinJoin status (v2.5.3):**
- Whirlpool: REMOVED. No Whirlpool-style CoinJoin is available in current Sparrow.
- PayJoin (BIP78): Y — Sparrow can initiate PayJoin (v1) requests. Payjoin receiver error handling improved in v2.3.0. Note: PayJoin requires the receiver to run a compatible endpoint; this is not a coordinator-based CoinJoin but a 2-party transaction construction technique. v1 verification step adjusted in v2.1.0 per BIP78 update.
- Postmix accounts: Legacy postmix wallet accounts can still be imported and viewed (gap limit may need increasing) but no new mixing is possible without a coordinator.

### PayNym / BIP47

- Full BIP47 implementation from v1.6.0 (March 2022)
- Sending and receiving via PayNyms (payment codes); notification transaction required before sending
- PayNym service: switched from paynym.is to paynym.rs (and .onion equivalent when Tor active) in v2.1.0
- BIP47 collaborative sends (Stowaway) were tied to Samourai Soroban network — removed with Whirlpool in v1.9.0
- BIP47 PayNyms for independent sending/receiving remain functional
- BIP47 not supported on Taproot wallets (technical limitation)

### Silent Payments (BIP352)

- Sending to Silent Payment addresses: v2.3.0 (October 2025), including BIP353 Human Readable Names (HRNs)
- Silent Payment receiving wallets: v2.5.0 (May 2026)
- Hardware wallet support via BIP375 PSBT fields + DLEQ proof verification: v2.4.0 (February 2026)
- Airgapped hardware wallet signers for SP receiving: v2.5.0
- Dust detection for SP wallet UTXOs: v2.5.2

### Tor

- Built-in Tor proxy (bundled, no external Tor installation required)
- Activates automatically when a `.onion` server address is configured
- Can also use an external Tor SOCKS proxy (updated to support safe cookie authentication in v2.5.3)
- All external connections (Electrum server, exchange rate API, PayNym API) routed through Tor when configured
- Transaction broadcast over Tor: Sparrow broadcasts via a public server over Tor when Tor is active — this is more private than broadcasting from a personal node, as it obscures the originating IP
- Fiat exchange rate: fetched via Tor if configured; can be disabled entirely (set provider to 'None')
- Internal Tor updated to v0.4.8.16 in v2.2.0

### Server connectivity

Sparrow does not use SPV — it requires a server connection to obtain wallet data:

| Mode | Description | Privacy |
|------|-------------|---------|
| Public servers | Pre-configured list (Electrum-compatible); suitable for beginners | Low — server sees all wallet addresses |
| Private Electrum server | User-operated ElectrumX, Fulcrum, Electrs, Electrs-Esplora, EPS, BWT | High — user controls the server |
| Bitcoin Core (direct RPC) | Connects to user's Bitcoin Core node via Cormorant library; requires Bitcoin Core v24+ for Taproot; descriptor wallets | Maximum — no third party |

Fulcrum, ElectrumX, Electrs, Electrs-Esplora, EPS, and BWT are all supported over SSL. Server comparison documented at sparrowwallet.com/docs/server-performance.html.

---

## UTXO Management

- **Coin control:** UTXO tab — select specific UTXOs via Ctrl/Cmd+Click → "Send Selected"; fine-grained UTXO selection for all outgoing transactions
- **Labelling:** Comprehensive label system (BIP329 import/export from v1.7.2); labels applied to transactions, inputs, outputs, addresses, and UTXOs; labels automatically propagated when spending
- **Freezing UTXOs:** UTXOs can be frozen (BIP329 `spendable: false` exported for frozen coins from v2.5.3)
- **UTXO visualisation:** Transaction diagram showing inputs and outputs graphically, editable in real time during transaction construction; helps optimise UTXO consolidation and fee reduction
- **Branch and Bound / Knapsack coin selection** — same algorithms used by Bitcoin Core
- **Transaction graph explorer:** Inputs and spent outputs are hyperlinked — the wallet doubles as a private blockchain explorer, navigable back to the coinbase

---

## Transaction Features

- **Fee control:** Fee rate slider with fine 0.01 sat/vb adjustment; "Recent Blocks" view (v2.2.0) to understand current mempool; dynamic fee estimation from connected server
- **RBF (Replace-By-Fee):** Y — default on all Sparrow transactions; right-click "Increase Fee" on unconfirmed transactions from Transactions tab
- **CPFP (Child Pays For Parent):** Y — spend the unconfirmed output at a higher fee rate; CPFP rate shown only if it increases the effective fee rate (v2.1.0)
- **Batch sending (Send to Many):** Y — multiple outputs in a single transaction; Send To Many dialog
- **Transaction editor:** Full byte-level transaction editor; all metadata fields editable; hex/base64 copy/paste; PSBT save and export
- **BIP322 message signing:** Supported for single-sig addresses including P2TR (from v1.7.8)
- **Testnet / regtest / signet / testnet4:** All supported via `--network` flag or menu restart

---

## Backup and Recovery

- **Seed backup:** BIP39 mnemonic (12 or 24 words); SLIP39 import; Sparrow does not autogenerate SLIP39 output
- **Wallet file:** Encrypted (Argon2) JSON wallet file; can be stored on removable media using `-d` flag for plausible deniability
- **Output descriptors:** The recommended multisig backup format; full export/import; interoperable with Bitcoin Core, Electrum, and other descriptor-capable wallets
- **BIP329 labels:** Import/export of all wallet labels; preserves UTXO history context across wallet restores
- **Gap limit:** Configurable (default 20 for normal wallets, 40 for postmix); must be increased if many addresses were generated without transactions

---

## Open Source and Build Integrity

- **License:** Apache 2.0
- **Reproducible builds:** Supported from v1.5.0; `.tar.gz` and `.zip` binaries are reproducible (not installer packages: `.deb`, `.rpm`, `.msi` and macOS `.dmg` have minor variances or code-signing non-reproducibility). Instructions at `docs/reproducible.md`
- **Build attestation:** bitcoinbinary.org carries video-proof reproducible build attestations for v1.5.0 through v1.6.6 (Coinkite factory bot); later versions [NOT FOUND] at bitcoinbinary.org as of 2026-08-12 (the site appears to have stopped tracking recent releases)
- **GPG release signing:** All release binaries signed by Craig Raw's GPG key (D4D0D3202FC06849A257B38DE94618334C674B40). Signature verification built in to the Download Verifier dialog
- **SHA-pinned CI:** GitHub Actions workflows use SHA-pinned actions (added v2.5.3)
- **Security audits:** [NOT FOUND] — no public third-party security audit identified as of 2026-08-12
- **Java runtime:** Bundles Eclipse Temurin JDK 25.0.2+10 (from v2.4.0); does not require a separate Java installation
- **F-Droid:** N/A — desktop only, not distributed via F-Droid

---

## Limitations and Criticisms

1. **Desktop only.** No iOS or Android app. Not suitable for mobile-primary users or for quick on-the-go payments.
2. **Java runtime.** Bundles its own JDK (≈100–200 MB download); larger install footprint than native applications. Historically required explicit Java installation; now self-contained.
3. **Whirlpool removed (April 2024).** The loss of Samourai Whirlpool removes the primary trustless CoinJoin mechanism. PayJoin (BIP78) remains but requires a cooperative receiver endpoint — it is not a mixing protocol. This is a significant privacy regression for users who relied on Whirlpool for anonymity-set mixing.
4. **PayJoin v2 (BIP77):** [NOT FOUND] — PayJoin v2 (async serverless) not confirmed in any release notes or documentation as of v2.5.3.
5. **Lightning Network:** Not supported. Sparrow is on-chain only; no Lightning channels, no submarine swaps. Users needing Lightning must use a separate wallet.
6. **Complexity:** The comprehensive UTXO-level UI is a strength for advanced users but can be overwhelming for Bitcoin newcomers. The docs acknowledge this with a "Best Practices" guide and privacy journey framing.
7. **Single primary maintainer:** Craig Raw appears to be the sole primary developer; the contributor count of 32 reflects mostly minor contributions. Bus factor risk for a security-critical wallet.
8. **No mobile companion:** No ability to monitor balances or receive payments on a mobile device without a separate watch-only app.

---

## Release Cadence

Releasing consistently since March 2020. Sample frequency from recent major releases:

| Version | Date | Interval |
|---------|------|---------|
| 2.5.3 | 30 July 2026 | ~2 months after 2.5.2 |
| 2.5.2 | 31 May 2026 | ~10 days after 2.5.1 |
| 2.4.2 | 10 March 2026 | ~3 weeks after 2.4.1 |
| 2.4.0 | 10 February 2026 | ~3 months after 2.3.1 |
| 2.3.1 | 6 November 2025 | ~1 month after 2.3.0 |

Typical cadence: major feature releases every 2–4 months; patch releases as needed.

---

## Sources

- [GitHub API: sparrowwallet/sparrow](https://api.github.com/repos/sparrowwallet/sparrow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-sparrowwallet-sparrow.json)
- [GitHub API: releases (latest)](https://api.github.com/repos/sparrowwallet/sparrow/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-sparrowwallet-sparrow-releases-latest.json)
- [GitHub API: releases (20 most recent)](https://api.github.com/repos/sparrowwallet/sparrow/releases?per_page=20) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-sparrowwallet-sparrow-releases.json)
- [GitHub README: sparrowwallet/sparrow](https://raw.githubusercontent.com/sparrowwallet/sparrow/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-com-sparrowwallet-sparrow-README.md)
- [sparrowwallet.com homepage](https://sparrowwallet.com) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-home.html)
- [sparrowwallet.com/features/](https://sparrowwallet.com/features/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-features.html)
- [sparrowwallet.com/docs/](https://sparrowwallet.com/docs/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-docs.html)
- [sparrowwallet.com/docs/faq.html](https://sparrowwallet.com/docs/faq.html) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-docs-faq.html)
- [sparrowwallet.com/download/](https://sparrowwallet.com/download/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-download.html)
- [bitcoinbinary.org](https://bitcoinbinary.org/) — build attestation records for v1.5.0–v1.6.6 — accessed 2026-08-12
- [GitHub: docs/reproducible.md](https://raw.githubusercontent.com/sparrowwallet/sparrow/master/docs/reproducible.md) — accessed 2026-08-12
