---
tags: [pattern, solana, key-derivation, ed25519, slip10, bip44]
seen_in: [phantom, solflare, backpack]
---

# Solana Key Derivation (ed25519 / SLIP-10)

Solana uses **ed25519** keys for signing, not the secp256k1 curve used by Bitcoin and Ethereum. This makes standard BIP32 hierarchical deterministic (HD) derivation inapplicable, because BIP32 was designed for secp256k1.

## The Problem with BIP32 on ed25519

BIP32 requires intermediate key material to be valid curve points. Secp256k1 allows arbitrary scalar addition in its private key space; ed25519 does not share the same algebraic structure. Naively applying BIP32 to ed25519 produces invalid or insecure keys at certain derivation paths.

## The Solution: SLIP-10

[SLIP-10](https://github.com/satoshilabs/slips/blob/master/slip-0010.md) (Satoshi Labs Improvement Proposal 10) extends BIP32 to support ed25519. Key differences:

- Uses HMAC-SHA512 with a different master secret derivation (`ed25519 seed` as the key vs Bitcoin's `Bitcoin seed`)
- Restricts derivation to **hardened paths only** (all components must have the hardened flag `'`); non-hardened child derivation is explicitly not supported for ed25519
- All public key derivation happens from the private key; no public-key-only child derivation is possible

## Phantom's Derivation Path

Phantom uses the SLIP-10 derivation path:

```
m/44'/501'/0'/0'
```

Breakdown:
- `44'` — purpose (BIP44 coin type derivation, hardened)
- `501'` — coin type for Solana (registered in SLIP-44; hardened)
- `0'` — account index (hardened)
- `0'` — change index (hardened; ed25519 requires hardened here)

Additional accounts in Phantom use `m/44'/501'/1'/0'`, `m/44'/501'/2'/0'`, etc.

## Comparison with Solflare and Backpack

| Wallet | Default path | Curve | Notes |
|--------|-------------|-------|-------|
| Phantom | `m/44'/501'/0'/0'` | ed25519 (SLIP-10) | Standard Solana path |
| Solflare | `m/44'/501'/0'/0'` | ed25519 (SLIP-10) | Same path, compatible seeds |
| Backpack | Uses SLIP-10 ed25519 | ed25519 | xNFT wallet; path details [NF] |

Because Phantom and Solflare use the same derivation path, a seed phrase created in either wallet can restore accounts in the other.

## Wallet Recovery Compatibility

A Phantom 12-word BIP39 seed phrase can be imported into:
- **Solflare** — full compatibility (same path)
- **Any SLIP-10 ed25519 wallet** — with correct path specified
- **Standard BIP39 Ethereum/Bitcoin wallets** — the BIP39 seed works, but the Solana accounts will not appear (different curve and path); Ethereum accounts at `m/44'/60'/0'/0/n` will be accessible

## Contrast with Monero

Monero uses a completely different key system: 25-word mnemonic seeds (Monero-specific entropy encoding, not BIP39) and curve25519 (related but distinct from ed25519). Monero seeds are not compatible with Phantom or Solana key derivation.

## Privacy Implication

Because all Solana paths are hardened (required by SLIP-10/ed25519), an observer who knows one private key cannot derive sibling keys — even with knowledge of the parent public key. This is stronger than non-hardened BIP32 derivation used in some Bitcoin wallets.

## Sources

- [SLIP-10 specification](https://github.com/satoshilabs/slips/blob/master/slip-0010.md) — accessed 2026-08-12
- [SLIP-44 coin type registry](https://github.com/satoshilabs/slips/blob/master/slip-0044.md) — Solana: coin type 501 — accessed 2026-08-12
- [Phantom developer docs / phantom.com](https://phantom.com) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-home.html) (path inferred from Solana ecosystem conventions; Phantom is closed-source)
- [coinlaw.io/phantom-wallet-statistics/](https://coinlaw.io/phantom-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-phantom-wallet-statistics.html)
