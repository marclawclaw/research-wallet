---
tags: [pattern, monero, privacy, cryptography, key-management]
applies-to: [cake-wallet, monero-gui, feather-wallet, monerujo]
---

# Pattern: Monero Stealth Address

## What it is

A one-time recipient address derived per-transaction from the recipient's public keys and a random transaction key chosen by the sender. On the Monero network, every transaction output is sent to a fresh stealth address, meaning the public blockchain never reveals the recipient's wallet address or links multiple payments to the same entity.

## Why it matters

Unlike Bitcoin (where you can see all transactions to a given address) or Ethereum (where all activity is visible on-chain), Monero's stealth address scheme makes it impossible for a third-party observer — or even the sender — to identify which blockchain outputs belong to a given wallet. The only party who can identify their own outputs is the recipient (using their private view key).

## How it works

**Parties involved:**

- **Recipient** publishes two public keys: a **view public key** (`B`) and a **spend public key** (`A`).
- **Sender** generates a random one-time transaction key `r`.

**Stealth address derivation:**

1. Sender computes the shared secret: `S = r × B` (scalar multiplication on the Ed25519 curve — the Monero curve).
2. Sender derives the one-time stealth address: `P = H_s(S) × G + A`, where `H_s` is a hash function and `G` is the curve generator point. This is the address that appears on-chain as the output's destination.
3. Sender includes the transaction public key `R = r × G` in the transaction, visible to anyone.

**Recipient scanning:**

1. Recipient's wallet downloads all transaction public keys from the blockchain.
2. For each transaction, wallet computes `S' = b × R` (using the private view key `b`), then checks if `P = H_s(S') × G + A`.
3. If it matches, the output belongs to the recipient. The private spend key `a` can then derive the one-time private key for spending: `x = H_s(S') + a`.

**Result:** No two stealth addresses for the same recipient are related — the blockchain shows a different address for every incoming payment. External observers cannot link payments to the same wallet.

## Implementation in Cake Wallet

- Stealth address generation is handled by the `monero_c` native library, integrated via Flutter FFI in the `cw_monero` package.
- **Subaddress rotation:** Cake Wallet auto-generates a fresh subaddress after each use (when enabled). Subaddresses are a variant of the stealth address scheme where the recipient pre-generates a pool of addresses, each binding to a specific account + index pair. This allows separating funds without exposing the primary address.
- **View keys for watch-only wallets:** The private view key alone (without the spend key) allows a wallet to identify all incoming transactions — enabling watch-only monitoring — but not to spend funds.

## Limitations

- The scheme requires the recipient's wallet to scan every transaction on the blockchain using their private view key. This is why Monero wallets require a "sync" period after restoration: the wallet must replay all blocks from the restore height to find outputs addressed to it.
- **Restore height:** If the restore height (the block at which the wallet was created) is not recorded, the wallet must scan from block 0, which can take hours or days. Polyseed mitigates this by encoding the creation date within the seed itself.
- The stealth address scheme protects recipient privacy, but does not by itself hide the **sender** (RingCT and ring signatures handle that) or **amounts** (Bulletproof+ range proofs handle that).

## Sources

- [Monero Research Lab: Stealth Addresses](https://www.getmonero.org/resources/moneropedia/stealthaddress.html) — Monero Project documentation
- [Cake Wallet docs: Monero crypto](https://docs.cakewallet.com/cryptos/monero.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-cryptos-monero.html)
- [Cake Wallet docs: Seed and keys](https://docs.cakewallet.com/features/managing-your-wallet/seed-keys.md) — accessed 2026-08-12
