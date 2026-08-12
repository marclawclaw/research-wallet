---
tags: [pattern, monero, key-management, seed, recovery, polyseed]
applies-to: [cake-wallet]
related: [monero-stealth-address]
---

# Pattern: Polyseed — Monero's Date-Encoded 16-Word Seed

## What it is

Polyseed is a 16-word mnemonic seed format for Monero wallets that encodes the wallet creation date within the seed itself. It was developed as a modern alternative to Monero's original 25-word legacy seed and is the default seed format for new wallets created in Cake Wallet.

## Why it matters

The primary practical pain point with Monero wallet recovery is that restoring a seed phrase requires specifying a **restore height** — the block number at which the wallet was created — to avoid scanning the entire Monero blockchain from block 0 (which takes hours or days). Legacy 25-word seeds do not encode this date; users must record it separately.

Polyseed encodes the creation date within its 16 words, eliminating the need to record a separate restore height. This substantially reduces the onboarding friction for new users and reduces the risk of failed recovery due to forgotten restore heights.

## Technical details

- **Word count:** 16 words (vs 25 for legacy Monero seed, vs 12/24 for BIP39).
- **Information encoded:** Monero spend key seed + wallet creation date (encoded as weeks since a reference epoch).
- **Passphrase support:** Optional seed passphrase supported (appended during key derivation, similar to BIP39's 25th word).
- **Key derivation:** Spend key derived from Polyseed using a deterministic function; view key derived from spend key using Monero's standard key derivation.
- **Compatibility:** Polyseed is not compatible with Monero's legacy 25-word seed format. Polyseed wallets can be restored in any Polyseed-compatible wallet (Cake Wallet, and others that have implemented the standard). Legacy 25-word wallets cannot be imported as Polyseed.

## Trade-offs

| Dimension | Polyseed (16-word) | Legacy (25-word) | BIP39 (12/24-word) |
|-----------|-------------------|------------------|---------------------|
| Word count | 16 | 25 | 12 or 24 |
| Creation date encoded | Yes | No | No (BIP39 has no date concept) |
| Restore height required | No | Yes (must record separately) | Yes (for Monero) |
| Ecosystem compatibility | Polyseed-supporting wallets only | All Monero wallets | Only wallets supporting BIP39 Monero derivation |
| Wallet group support | No (cannot join Cake Wallet groups) | No | Yes (required for multi-currency shared seed) |
| Standard | Polyseed open spec | Monero Project standard | BIP39 open standard |

## Usage in Feather Wallet

Feather Wallet was the **first production Monero wallet to ship Polyseed** (introduced in Feather v2.0.0). It remains the default seed format for all new wallets.

- **Default:** Polyseed (16-word) is the default for new wallets. Feather explicitly describes itself as a "testing grounds for experimental features that may later be adopted in reference wallets."
- **Also supports:** 25-word legacy seed (full restore); 14-word legacy seed (the older `monero-seed` library format, supported for restore indefinitely); converting a Polyseed wallet's keys to a 25-word representation (Wallet → Seed → Show 25 word seed).
- **Passphrase:** Supported for Polyseed via an optional additional entropy input.
- **Conversion (Polyseed → 25-word):** Possible via Wallet → Seed. The 25-word representation and the restore height can then be used with any Monero-compatible wallet.
- **Conversion (25-word → Polyseed):** Not possible — Polyseed uses a one-way KDF.
- **Damaged seed recovery:** Feather includes a guide and tooling (`guides/damaged-seed`) for recovering from a partially damaged Polyseed.

## Non-usage in Monero GUI

Monero GUI uses only the traditional **25-word Monero mnemonic** (Monero Project standard). Polyseed is not supported.

- Users creating a new wallet in Monero GUI receive a 25-word mnemonic with no embedded date.
- The restore height must be recorded separately by the user.
- If the restore height is unknown, the wallet scans from block 0 — potentially many hours of sync time.
- Polyseed wallets created in Cake Wallet cannot be imported into Monero GUI. Users must migrate by sweeping funds to a new 25-word wallet.
- This is a meaningful usability disadvantage of Monero GUI relative to Cake Wallet for new users and for users who may need to recover their wallet.

## Usage in Cake Wallet

- **Default:** Polyseed is the default seed format when creating a new Monero wallet in Cake Wallet.
- **Alternative selection:** During wallet creation, users can switch to legacy 25-word or BIP39 12-word format in Advanced Settings.
- **Wallet groups requirement:** Monero wallets must use BIP39 seed format to join a Cake Wallet wallet group (one seed for all currencies). A Polyseed Monero wallet cannot be added to a group; the user must create a new Monero wallet with BIP39 seed.
- **Passphrase:** Optional additional passphrase can be set for Polyseed wallets via Advanced Settings.

## Sources

- [Cake Wallet docs: LLM full export — seed formats section](https://docs.cakewallet.com/llms-full.txt) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-llms-full.txt)
- [Polyseed GitHub](https://github.com/tevador/polyseed) — tevador (Monero developer); open specification
- [Cake Wallet docs: Wallet Groups](https://docs.cakewallet.com/features/managing-your-wallet/wallet-groups) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-wallet-groups.html)
- [Feather Wallet docs: Seed scheme](https://docs.featherwallet.org/guides/seed-scheme) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-seed-scheme.html)
- [Feather Wallet docs: Feature comparison — Seed scheme row](https://docs.featherwallet.org/guides/features) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-features.html)
