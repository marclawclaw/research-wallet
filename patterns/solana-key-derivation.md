---
title: Solana Key Derivation Pattern
tags: [solana, cryptography, ed25519, bip39, slip-0010, key-derivation]
aliases: [Solana Ed25519 HD Key Derivation]
created: 2026-08-12
---

# Solana Key Derivation Pattern

Solana uses **Ed25519** elliptic-curve cryptography for all on-chain signing, not secp256k1 (which Bitcoin and Ethereum use). This shapes key derivation, seed formats, and hardware wallet compatibility across all Solana wallets.

## Core Cryptographic Primitives

| Property | Value |
|----------|-------|
| Signing algorithm | Ed25519 (Edwards-curve Digital Signature Algorithm) |
| Curve | Curve25519 (twisted Edwards form) |
| Address format | Base58-encoded 32-byte Ed25519 public key |
| HD derivation standard | SLIP-0010 (not BIP-32, which uses secp256k1) |
| Derivation path | `m/44'/501'/0'/0'` (Solana coin type 501 per SLIP-0044) |

## Mnemonic / Seed Phrase

- **Standard format:** BIP39 (both Phantom and Solflare confirmed)
- **Word count:** 24 words (confirmed: Solflare, Phantom; sources: coinbureau.com security review Aug 2026)
- **Entropy bits:** 256 bits (for 24-word BIP39)
- **Language wordlist:** English (BIP39 standard)
- **Derivation from mnemonic:** PBKDF2 → 512-bit seed → SLIP-0010 Ed25519 master key

## SLIP-0010 vs BIP-32

Bitcoin (secp256k1) wallets use BIP-32 for HD key derivation. Because Ed25519 does not support unhardened derivation (child keys cannot be derived from parent public keys), Solana uses **SLIP-0010**, which mandates all derivation paths be hardened (denoted with `'`):

- `m/44'/501'/0'/0'` — standard first account
- `m/44'/501'/1'/0'` — second account
- All path components must be hardened (`'`) — no unhardened child key derivation possible

This means Solana wallets cannot generate watch-only xpub-style account hierarchies the way Bitcoin wallets can.

## Account Model

- Each derived key pair = one Solana account address (public key)
- Multiple accounts can be created from one seed (sequential derivation index)
- Each account maintains its own SOL balance and token accounts (Associated Token Account per SPL token per wallet)

## Solflare Implementation

- 24-word BIP39 seed
- Multiple accounts per seed (sequential index)
- Separate seed phrase support for burner/isolated wallets
- Flutter-based mobile app uses `dart-ed25519-hd-key` library (public on GitHub)
- AES encryption of local key vault; keys stored on-device only

## Phantom Implementation

- 24-word BIP39 seed (confirmed coinbureau.com)
- Otherwise structurally identical to Solflare (shared Solana protocol)

## Hardware Wallet Compatibility

Ed25519 HD key derivation via SLIP-0010 requires firmware support. Ledger's Solana app (via Ledger Live or wallet adapters) implements SLIP-0010 for Solana accounts. Keystone also supports Solana Ed25519 accounts.

The screenless Solflare Shield proprietary NFC card uses a 12-word seed (lower entropy) rather than the standard 24-word; this is a deviation from the common Solana wallet pattern.

## Implications for Wallet Interoperability

- Any wallet implementing BIP39 + SLIP-0010 + Solana coin type 501 will derive identical keys from the same seed
- Solflare, Phantom, Backpack seeds are interoperable — same 24-word BIP39 generates the same accounts in any compliant wallet
- Exception: Solflare Shield (12-word seed) is not interoperable with 24-word wallets unless the recovery phrase is compatible

## Related Notes

- [[wallets/solflare]] — Solflare implementation
- [[patterns/wallet-core-signing]] — general multi-chain signing pattern (secp256k1 focus)

## Sources

- [coinbureau.com — Solflare vs Phantom](https://coinbureau.com/analysis/solflare-vs-phantom/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinbureau-com-solflare-vs-phantom.html) — confirms 24-word BIP39 for both Solflare and Phantom
- [coinbureau.com — Is Solflare Safe?](https://coinbureau.com/analysis/is-solflare-safe/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinbureau-com-is-solflare-safe.html) — key derivation and encryption model
- [github.com/solflare-wallet/dart-ed25519-hd-key](https://github.com/solflare-wallet/dart-ed25519-hd-key) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-orgs-solflare-repos-full.json) — Solflare's public Dart library for Ed25519 HD key derivation in Flutter mobile app
- SLIP-0010: https://github.com/satoshilabs/slips/blob/master/slip-0010.md — not archived (primary specification)
- SLIP-0044 (Solana coin type 501): https://github.com/satoshilabs/slips/blob/master/slip-0044.md — not archived (primary specification)
