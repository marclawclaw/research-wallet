---
title: "Zcash Shielded Transactions"
pattern: zcash-shielded-transactions
tags: [zcash, orchard, sapling, privacy, zero-knowledge, unified-address, zip316, zip321, lightwalletd, shielded-pool]
created: 2026-08-12
accessed: 2026-08-12
---

# Zcash Shielded Transactions

Zcash's privacy model is layered across multiple "pools" (note-commitment trees). Understanding these pools is essential to evaluating any Zcash wallet's privacy guarantees.

## Pool Architecture

Zcash has historically supported three note-commitment pools, with a fourth (Ironwood) activated in 2026:

| Pool | Activation | Address prefix | Privacy | Status |
|------|------------|---------------|---------|--------|
| Transparent | Genesis | `t1`, `t3` | None — fully public UTXO | Active (legacy) |
| Sprout | Genesis | `zc` | zk-SNARK (Groth16 predecessor) | Deprecated; no new wallets |
| Sapling | NU2 (2018) | `zs` | zk-SNARK (Groth16, Jubjub curve) | Active but being superseded |
| Orchard | NU5 (2022) | Part of Unified Address | zk-SNARK (Halo2, Pasta curves) | Current recommended pool |
| Ironwood | NU6.3 (2026) | Part of Unified Address | TBD (Halo2-based, improved) | Newest pool (2026-07) |

## How Shielded Transactions Work

A shielded transaction consists of **notes** (UTXO-analogues) committed to the Orchard or Sapling note-commitment tree. Each note encodes:

- Value (amount of ZEC)
- Recipient's diversified payment address (derived from IVK)
- Randomness (nullifier seed)
- Memo field (512 bytes, encrypted to recipient)

When a note is spent, a **nullifier** is revealed — a one-way hash that lets the network detect double-spending without revealing which note was spent. The sender proves validity of the spend via a zero-knowledge proof.

The Zcash network broadcasts **compact blocks** (containing only note commitments and nullifiers) via `lightwalletd`, allowing light clients to trial-decrypt all notes with their Incoming Viewing Key (IVK) to find notes addressed to them — without revealing which notes they own to the server.

## Key Types

| Key | Acronym | Capability |
|-----|---------|-----------|
| Spending key | SK | Full spend authority |
| Unified Spending Key | USK | Derives keys for all pools from one seed |
| Full Viewing Key | FVK | See all sends and receives |
| Incoming Viewing Key | IVK | See only received notes |
| Outgoing Viewing Key | OVK | Prove you sent a transaction |
| Diversified payment address | — | Multiple addresses from one IVK (no linkage) |

Zcash uses **ZIP 32** for hierarchical deterministic key derivation (analogous to BIP32 for Bitcoin), with a different derivation path (`m/purpose'/coin_type'/account'`). A single 24-word BIP39 seed phrase generates a USK that controls all pools.

## Unified Addresses (ZIP 316)

ZIP 316 (activated NU5, 2022) defines **Unified Addresses** — a single address string that bundles multiple receivers:

```
u1[orchard_receiver][sapling_receiver][transparent_receiver]
```

When a sender with Orchard capability sends to a Unified Address, the Orchard receiver is used automatically. If the sender only supports transparent, the transparent receiver is used. This allows progressive privacy adoption without requiring users to manage multiple address types.

Unified Addresses are the recommended default for all Zcash wallets from 2022 onward. All major wallets (Zodl, YWallet, Nighthawk) use UAs as the default receive format.

## ZIP 321: Payment Request URIs

ZIP 321 defines a URI scheme for Zcash payment requests:

```
zcash:<address>?amount=<ZEC>&memo=<base64url>&message=<text>
```

Payment request QR codes encode ZIP 321 URIs, enabling point-of-sale flows and invoice generation. The memo field is encrypted end-to-end in shielded transactions.

## Shielded Pool Stats (2026)

- Shielded transactions as % of all Zcash transactions: 59.3% (all-time high, February 2026)
- Orchard pool: 4.2 million ZEC = 25.4% of circulating supply (May 2026)
- Total shielded supply: ~5 million ZEC out of 16.7 million circulating (May 2026; up from 8% in early 2024)
- Public transparent transactions: ~8,500/day (flat; all growth in shielded)

The surge in shielded adoption is attributed primarily to Zodl's Unified Address default routing, which automatically uses Orchard for capable senders.

## Lightwalletd Trust Model

Light clients (Zodl, Nighthawk, YWallet) connect to `lightwalletd` gRPC servers to download compact blocks. The trust model is:

- **Trusted lightwalletd:** Server is honest but the connection may be intercepted. This is the expected case; TLS protects against eavesdropping.
- **Compromised lightwalletd:** Server can feed false balance data, hide transactions, or replay old blocks. Light clients without ZIP 307 block-header validation (currently unimplemented in Zodl) cannot detect this attack class.
- **Mitigation:** Zodl runs its own lightwalletd servers; users can configure custom servers in Advanced Settings. Tor opt-in hides the user's IP from the lightwalletd operator.

## ZIP 320: Transparent-to-Transparent (TEX) Address Handling

ZIP 320 defines "transparent exchange" addresses (`t3...`) used by centralised exchanges that cannot accept shielded funds. When a user sends ZEC to a TEX address from a shielded wallet:

1. The wallet auto-deshields: creates a fresh ephemeral transparent address.
2. Shielded funds are sent to the ephemeral transparent address (shielded→transparent).
3. A second fully-transparent transaction sends from the ephemeral address to the TEX address.

This two-step process is designed to prevent on-chain linkage between the user's shielded balance and the exchange deposit address. Zodl implements ZIP 320 as of v1.3 (Q4 2024).

## Encrypted Memo Field

Every shielded transaction includes a 512-byte encrypted memo field, readable only by the recipient (or anyone with the FVK). Wallets use the memo for:

- Payment references or invoice numbers
- Human-readable notes
- ZIP 302 / ZIP 303 memo conventions (payment ID, contact addresses)

The memo is included in the zero-knowledge proof and is not visible to blockchain observers.

## Relation to Other Notes

- [[zodl]] — primary Zcash mobile wallet; Orchard-native, Ironwood migration support
- [[ywallet]] — power-user Zcash wallet; multi-account, multi-pool
- [[nighthawk]] — community Zcash mobile wallet; shielded-by-default

## Sources

| Source | URL | Accessed |
|--------|-----|----------|
| Zodl threat model (lightwalletd trust model) | https://raw.githubusercontent.com/zodl-inc/zodl-project/master/wallet_threat_model.md | 2026-08-12 |
| Zcash Android SDK README (lightwalletd, SDK architecture) | https://github.com/zcash/zcash-android-wallet-sdk | 2026-08-12 |
| ZIP 321: Payment Request URIs | https://zips.z.cash/zip-0321 | 2026-08-12 |
| CryptoNews: Zcash shielded pool 30% (shielded supply stats) | https://cryptonews.net/news/analytics/32936591/ | 2026-08-10 |
| zodl-android CHANGELOG (Ironwood, ZIP 320, auto-shielding) | https://raw.githubusercontent.com/zodl-inc/zodl-android/main/CHANGELOG.md | 2026-08-12 |
