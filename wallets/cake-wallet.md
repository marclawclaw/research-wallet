---
tags: [wallet, monero, bitcoin, ethereum, mobile, desktop, cross-platform, flutter, mit]
category: self-custody software wallet
website: https://cakewallet.com
github: https://github.com/cake-tech/cake_wallet
launched: 2018
license: MIT
---

# Cake Wallet

Cake Wallet is the dominant Monero mobile wallet of 2026, developed by Cake Labs LLC and available on iOS, Android, macOS, and Linux. It is fully open-source (MIT licence) and non-custodial, holding no user keys server-side. Beyond Monero, it supports 17 additional cryptocurrencies — including Bitcoin with Lightning, Zcash shielded, Litecoin MWEB, Ethereum, and Solana — with built-in swap aggregation across decentralised and centralised exchange providers.

The website claims 1,750,000+ users as of 2026 and has been operating since 2018. The GitHub repository (cake-tech/cake_wallet) shows 1,838 stars, 389 forks, and 89+ contributors as of 12 August 2026, and has been updated continuously to v6.4.0 (27 July 2026).

---

## Adoption / usage metrics

| Metric | Value | Date | Source |
|--------|-------|------|--------|
| GitHub stars | 1,838 | 2026-08-12 | [GitHub API](https://api.github.com/repos/cake-tech/cake_wallet) — [archived](../sources/2026-08-12-api-github-com-cake-tech-cake-wallet.json) |
| GitHub forks | 389 | 2026-08-12 | [GitHub API](https://api.github.com/repos/cake-tech/cake_wallet) — [archived](../sources/2026-08-12-api-github-com-cake-tech-cake-wallet.json) |
| GitHub open issues | 308 | 2026-08-12 | [GitHub API](https://api.github.com/repos/cake-tech/cake_wallet) — [archived](../sources/2026-08-12-api-github-com-cake-tech-cake-wallet.json) |
| GitHub contributors | 89+ | 2026-08-12 | [GitHub contributors API](https://api.github.com/repos/cake-tech/cake_wallet/contributors?per_page=100) |
| Self-reported users | 1,750,000+ | 2026-08 | [cakewallet.com](https://cakewallet.com) — [archived](../sources/2026-08-12-cakewallet-com-home.html) |
| Total GitHub releases | 200+ (API page limit reached at 200) | 2026-08-12 | [GitHub releases API](https://api.github.com/repos/cake-tech/cake_wallet/releases) |
| Latest release | v6.4.0 | 2026-07-27 | [GitHub releases](https://github.com/cake-tech/cake_wallet/releases) |
| Earliest GitHub release | v4.1.5 | 2021-04-14 | [GitHub releases API](https://api.github.com/repos/cake-tech/cake_wallet/releases?per_page=100&page=2) |
| App Store (iOS) rating | 4.7/5 | 2026-07 | [Apple App Store](https://apps.apple.com/us/app/cake-wallet-monero-bitcoin/id1334702542) |
| App Store (iOS) ratings count | 4,200 | 2026-07 | [Apple App Store](https://apps.apple.com/us/app/cake-wallet-monero-bitcoin/id1334702542) |
| Google Play download count | [NOT FOUND] | — | — |
| Google Play rating | [NOT FOUND] | — | — |
| Monthly Active Users (MAU) | [NOT FOUND] — structurally unavailable for Monero; see note | — | — |
| Primary programming language | Dart (Flutter) | 2026-08-12 | [GitHub API](https://api.github.com/repos/cake-tech/cake_wallet) |
| Repo created on GitHub | 2020-01-04 | — | [GitHub API](https://api.github.com/repos/cake-tech/cake_wallet) |
| Last push to main | 2026-08-11 | — | [GitHub API](https://api.github.com/repos/cake-tech/cake_wallet) |

**Note on Monero MAU:** Monero's privacy architecture (RingCT, stealth addresses, no public address reuse) makes on-chain active-address attribution structurally impossible. No Monero wallet project publishes MAU. The self-reported "1,750,000+ users" figure on the Cake Wallet website is unverifiable and includes users of the affiliated Monero.com app (same codebase, XMR-only variant) and may include historical cumulative installs rather than active users. GitHub stars (1,838) are used as the primary adoption proxy for ranking purposes.

---

## How it works

### User perspective

1. **Install:** Available via Apple App Store, Google Play, F-Droid, Accrescent, direct APK (GitHub releases), and Linux Flatpak. Windows builds are paused pending a redesigned app (expected late 2026).
2. **Wallet creation:** User selects a coin type, chooses a seed format (Monero defaults to Polyseed 16-word; Bitcoin/ETH default to BIP39 12-word), optionally sets a passphrase, and is shown their seed phrase for backup.
3. **PIN / biometrics:** User sets a PIN (stored on-device only, not recoverable by Cake Labs). Optional biometric unlock. Optional Cake 2FA (TOTP, SHA-512, 8-digit) can be layered on top.
4. **Syncing:** Wallet syncs to an XMR/BTC/ETH node. Default nodes are Cake Labs-operated (closeable in Settings → Connections); user can switch to any custom node.
5. **Receive:** App generates a receive address or QR code. For Monero, the wallet auto-rotates subaddresses after each use (if enabled). For Bitcoin, it rotates addresses after each use.
6. **Send:** User enters address or scans QR, selects fee tier, optionally adds a transaction note. App constructs the transaction locally, signs it on-device, and broadcasts to the selected node.
7. **Swap:** User selects "Swap" tab, chooses source and destination coins, picks a provider (or "Best Rate"), and confirms. Swap is executed through the third-party provider; Cake Wallet routes the output to the user's destination address.

### System perspective (Monero transaction flow)

1. Wallet derives keys from the Polyseed (or legacy 25-word / BIP39 seed): spend key and view key pair.
2. Subaddresses are derived from the primary address + account index + subaddress index using Monero's key derivation scheme. Fresh subaddresses are generated after each use.
3. Transaction inputs are selected from unspent outputs visible to the view key (outputs addressed to any of the wallet's subaddresses).
4. **RingCT signing:** The wallet constructs a ring of 16 decoy outputs (the current Monero ring size) around the actual input. The Bulletproof+ range proof proves the transaction amount is non-negative without revealing the actual value. The transaction is signed with the spend key.
5. **Stealth address construction:** The recipient's stealth address is derived per-transaction from the recipient's public keys + a one-time transaction key, ensuring that only the recipient's view key can identify which outputs belong to them.
6. Signed transaction is broadcast to the Monero daemon (via the selected node). The transaction propagates through the Monero P2P network.
7. Once confirmed, new outputs become spendable after the unlock time (10 blocks for regular transactions).

---

## Key features

### Key management

- **Monero seed formats:** Three supported:
  - **Polyseed (16 words):** Default for new Monero wallets. Includes creation date encoded in seed; enables faster wallet restoration without needing to specify restore height.
  - **Legacy 25-word seed:** Monero's original seed format; compatible with all other Monero wallets (Feather, Monerujo, GUI).
  - **BIP39 (12 or 24 words):** Required for Monero wallets that join a wallet group (multi-currency shared seed). Non-default for Monero; compatible with standard BIP39 derivation.
- **Bitcoin seed formats:** BIP39 12-word default (derivation path `m/84'/0'/0'` for native SegWit); optional 24-word; optional Electrum seed type (`m/0'`).
- **Other coins:** BIP39 12-word default (24-word optional).
- **Seed passphrase:** Optional BIP39-style passphrase available under Advanced Settings for all seed types.
- **Wallet groups:** A single BIP39 seed can back multiple currency wallets simultaneously (one wallet per currency per group). Monero Polyseed/legacy wallets cannot join groups without recreating the Monero wallet with BIP39 seed.
- **View keys / watch-only wallets:** Full support for watch-only Monero wallets using the primary address + private view key (spend key field left blank). Restore height is optional but accelerates sync.
- **Key derivation:** Entirely on-device. The privacy policy explicitly states: "Your private keys (including your Monero private view keys), seeds, backup files, and wallet passcode are your own responsibility. This data is not received, collected, or stored by Cake Labs at any time, for any reason."

### Signing

- **Local, on-device signing** for all transactions across all supported coins.
- **Hardware wallet signing:** Ledger (via Bluetooth + USB on Android/macOS/Linux; Bluetooth only on iOS) supports Monero, BTC, LTC, ETH, and Polygon. Trezor (Bluetooth + USB on Android/macOS/Linux; Bluetooth only on iOS) supports Monero (added v6.2.1), BTC, LTC, ETH, and Polygon with full passphrase support added in v6.4.0. BitBox supports BTC, ETH, Polygon (Android only).
- **Air-gapped signing:** COLDCARD supported for Bitcoin via BCUR or BBQR QR-code format. Cupcake (Cake's companion air-gap signing app) also supported.
- **Monero RingCT:** Ring size 16 decoys (network default); Bulletproof+ range proofs; stealth addresses. All constructed on-device.
- **PSBT support:** For Bitcoin hardware wallet flows (implied by Sparrow-style COLDCARD integration).
- **Message signing:** Sign and verify messages feature available (documented in advanced section).

### UX

- **Setup flow:** Coin selection → seed type selection → passphrase (optional) → seed backup prompt → PIN setup → optional biometrics.
- **Address rotation:** Monero subaddresses auto-generated after each use (toggleable). Bitcoin addresses rotated after each use.
- **Receive address display:** QR code with copy-to-clipboard. OpenAlias and Unstoppable Domains supported for human-readable addresses.
- **Fee tiers:** Five Monero fee priority tiers (Slow / Automatic / Medium / Fast / Fastest; "Automatic" recommended). Network fee shown before confirmation.
- **Transaction notes:** User can add private notes to transactions (stored locally on-device only).
- **Address book:** Saved recipients with names; QR scanning supported.
- **Multiple wallets:** Unlimited wallets per app, per coin type.
- **Multiple accounts (Monero):** Multiple accounts within a single Monero wallet, each with independent transaction history and balance.
- **Batch sending:** Supported for Monero and Bitcoin.
- **AnyPay:** Payment protocol support for merchants.
- **Anypay / URI schemes:** BTC, XMR, ETH URI payment links supported.
- **Language support:** 24+ languages.
- **Customisable app:** Custom app icon, theme, display currency, and other visual settings.
- **Home screen transaction history:** Added in v6.3.0 (July 2026); recent transactions visible on the home screen without navigating to history.

### Backup / recovery

- **Seed phrase:** Primary recovery mechanism. Displayed during wallet creation; user must write it down.
- **Full app backup file:** Encrypted export of all wallets and settings; importable on a fresh install. Warning: restoring a backup requires a fresh install with no pre-existing wallets.
- **Recovery methods:** Seed phrase (with optional restore height / date for Monero to accelerate sync), private keys, hardware wallet reconnection, backup file import, QR code (air-gapped).
- **Duress PIN:** Secondary PIN that, when entered, performs an immediate, irreversible local wipe of all wallets (clears secure storage, wallet files, and auth data). Does not affect blockchain funds or backups stored externally. Returns app to fresh-install state with no visible wipe indicator.
- **Cloud backup:** Not offered natively. Documentation recommends end-to-end encrypted password managers (Bitwarden, Proton Pass, KeePassXC, 1Password) for seed storage.

### Privacy features

**Monero-native (structural, always on):**
- **RingCT:** Hides transaction amounts in all Monero transactions using Pedersen commitments. Mandatory network-level feature since Monero's October 2017 hard fork.
- **Stealth addresses:** Per-transaction, one-time recipient addresses derived from the recipient's public keys. Senders cannot identify each other's outputs.
- **Bulletproof+ range proofs:** Efficient zero-knowledge proofs confirming output amounts are positive without revealing values.
- **Subaddress rotation:** Fresh subaddress generated after each use (when enabled), preventing address linkability.
- **Multiple accounts:** Independent account compartmentalisation within one wallet.

**Bitcoin-specific (opt-in):**
- **Silent Payments:** Reusable, static address that generates unlinkable on-chain outputs per sender. Prevents address reuse tracking.
- **Payjoin:** Collaborative transaction construction that breaks the common-input-ownership heuristic.
- **Coin control:** Manual UTXO selection available for Bitcoin, Litecoin, Bitcoin Cash, Dogecoin, Monero, and Decred.
- **MEV protection (Ethereum/Base):** Transactions broadcast privately via Blink Labs before fallback to public mempool.

**Litecoin:**
- **MWEB (MimbleWimble Extension Blocks):** Confidential transactions hiding amounts and addresses on-chain; available on iOS and Android only (desktop platforms — macOS, Linux, Windows — do not get a Private MWEB address).

**Zcash:**
- **Shielded transactions:** Sender, receiver, and amount hidden on-chain using the Sapling/Orchard shielded pool.
- **Autoshielding:** Automatic movement of transparent ZEC to shielded pool.

**Network-level:**
- **Built-in Tor:** Routes wallet-to-node and exchange-rate check traffic through the Tor network. Single toggle in Settings → Connections. Onion node presets for Monero and Zcash. Experimental; slower sync. Does not protect against address-level analysis — only IP-level.
- **Custom node selection:** User can specify their own Monero/Bitcoin/ETH nodes to avoid using Cake Labs nodes entirely.
- **Fiat API toggle:** Can be disabled to prevent IP address and currency-pair data being sent to Cake Labs' price API.
- **Tor fail-closed modes:** Optional fail-closed for Fiat API and Swap services when Tor is enabled.

### Multi-currency support

Supported coins as of v6.4.0 (July 2026):

| Coin | Notes |
|------|-------|
| Monero (XMR) | Full Polyseed/legacy/BIP39 support; RingCT; subaddresses; background sync |
| Bitcoin (BTC) | Lightning via Spark protocol; Silent Payments; Payjoin; coin control; hardware wallet |
| Litecoin (LTC) | MWEB; coin control; hardware wallet |
| Bitcoin Cash (BCH) | Standard send/receive |
| Dogecoin (DOGE) | Standard send/receive |
| Ethereum (ETH) | ERC-20 tokens; custom token addition; MEV protection |
| Polygon (POL) | EVM; hardware wallet |
| Arbitrum (ARB) | EVM |
| Base (BASE) | EVM; MEV protection |
| BNB Smart Chain (BNB) | EVM |
| Solana (SOL) | Standard send/receive |
| Tron (TRX) | Standard send/receive |
| Zcash (ZEC) | Shielded (Sapling/Orchard/IronWood); autoshielding; Ironwood migration in v6.4.0 |
| Nano (XNO) | Standard send/receive |
| Zano (ZANO) | Standard send/receive |
| Decred (DCR) | Standard send/receive |
| Wownero (WOW) | View-only (wallet creation removed; seed/key viewing only) |

**17 active coins; Wownero in legacy view-only mode; Haven support removed.**

### Exchange / swap

**How it works:** Cake Wallet is a swap aggregator; it does not run its own exchange. When a user initiates a swap, Cake Wallet fetches quotes from multiple providers and routes the swap through the chosen provider. The user's funds go from their wallet directly to the provider's swap contract or address, and the output is sent to the user's destination address.

**Active providers (as of v6.4.0):**

| Provider | Type | Notes |
|----------|------|-------|
| Chainflip | Decentralised protocol | On-chain cross-chain swaps (e.g. BTC ↔ XMR, ETH ↔ BTC) using a decentralised liquidity network |
| ChangeNow | Centralised aggregator | Third-party exchange; receives source funds and sends output |
| Trocador | Centralised aggregator | Routes to best available exchange terms; acts as a meta-aggregator |
| LetsExchange | Centralised aggregator | Re-enabled in v6.2.1 |
| Jupiter | Decentralised (Solana) | On-chain Solana DEX aggregator |
| Near Intents | Decentralised (NEAR) | Decentralised intent-based swaps |

**Disabled providers:**
- Exolix: temporarily disabled as of v6.4.0
- Swaps.xyz, StealthEX: disabled as of v6.0.3

**Fixed vs floating rate:** User can toggle between floating-rate (best available rate, slippage risk) and fixed-rate (locked quote, slightly worse rate).

**Bridge:** USDT bridging between Ethereum, Polygon, and Arbitrum via in-app bridge (added v6.1.0).

**Custodial status of swap providers:** Centralised providers (ChangeNow, Trocador, LetsExchange) are custodial intermediaries for the duration of the swap — they briefly hold the source funds before sending the output. Chainflip, Jupiter, and Near Intents are non-custodial protocol-level swaps. Cake Wallet itself holds no funds at any point. KYC requirements: Cake Wallet itself requires no KYC; individual swap providers may have their own thresholds (amounts above which they require identity verification), but this is [NOT FOUND] in Cake Wallet's own documentation.

**Cake Pay:** Gift card purchase feature allowing users to spend crypto (including Monero) at retail merchants. Cake Pay is a separate product under the Cake Labs umbrella; the extent to which it is custodial or KYC-requiring is [NOT FOUND] from this research pass.

### Hardware wallet support

| Device | Connection | Supported coins |
|--------|------------|-----------------|
| Ledger | Bluetooth only (iOS); Bluetooth + USB (Android/macOS/Linux) | XMR, BTC, LTC, ETH, Polygon |
| Trezor | Bluetooth only (iOS); Bluetooth + USB (Android/macOS/Linux) | XMR (added v6.2.1), BTC, LTC, ETH, Polygon; full passphrase support in v6.4.0 |
| BitBox | USB (Android only) | BTC, ETH, Polygon |
| COLDCARD | Air-gapped QR (BCUR / BBQR) | BTC |
| Cupcake | Air-gapped QR | Cake's own companion app |
| SeedSigner | "Coming soon" | — |
| Keystone | "Coming soon" | — |
| Foundation Passport | "Coming soon" | — |

**Linux hardware wallet support:** Not currently available (noted in documentation as pending; desktop overhaul planned for late 2026).

---

## Architecture decisions

- **Flutter (Dart):** Single codebase across iOS, Android, macOS, and Linux. Enables rapid feature parity across platforms with one team. The trade-off is a large app binary (~356 MB on iOS) and some platform-specific gaps (e.g. hardware wallet USB not available on iOS due to platform restrictions; no Lightning on Linux).
- **Monolith with pluggable coin modules:** Separate Dart packages per coin (`cw_monero`, `cw_bitcoin`, `cw_evm`, `cw_solana`, `cw_zcash`, etc.) under one app shell. Allows per-coin dependency management and native library integration (Monero uses the `monero_c` native library via FFI).
- **Default Polyseed for Monero:** Departing from the Monero ecosystem's traditional 25-word legacy seed in favour of Polyseed (which encodes the wallet creation date, eliminating the need to remember a restore height). Trade-off: slightly less compatible with older Monero ecosystem tools.
- **Swap aggregation over atomic swaps:** Rather than implementing true atomic cross-chain swaps (which require on-chain smart contracts or HTLC scripts on both chains), Cake Wallet routes through swap providers. Chainflip is the closest to a trustless swap (decentralised liquidity pool); ChangeNow and others involve custodial intermediaries.
- **Tor as opt-in, not default:** Built-in Tor is available but not enabled by default; the documentation warns of significantly slower sync. This is a pragmatic choice prioritising usability for the majority over privacy-maximalism. Tor + Orbot combination documented for users who want comprehensive coverage.
- **Default Cake Labs nodes:** New users sync to Cake Labs' nodes by default. Privacy policy states IP, block height, and transaction data are received by Cake Labs nodes but not retained. Users can and are recommended to switch to their own nodes.
- **No reproducible builds documented:** No public reproducible build verification process or signed release provenance for Linux artifacts. An open issue (#3513, filed 11 August 2026) requests GitHub artifact attestations or Sigstore signing for Linux releases. The Flatpak bundle disables the standard Flatpak GPG verification flags (`gpg-verify-summary=false`, `gpg-verify=false`).

---

## Differentiators

Compared with other Monero wallets:

- **vs [[monero-gui]]:** Cake Wallet is mobile-first and significantly more beginner-friendly; Monero GUI requires running or connecting to a full Monero node and is desktop-only. Monero GUI is the official reference implementation (Monero Project team); Cake Wallet is a third-party product. Cake Wallet adds multi-currency support and built-in swaps; Monero GUI is XMR-only.
- **vs [[feather-wallet]]:** Feather is a privacy-maximalist desktop-only wallet (Windows, macOS, Linux, Tails/Qubes) with Tor built in by default and cold-signing capabilities. Feather does not support mobile and does not have built-in swap aggregation. Cake Wallet has wider platform reach (mobile) and multi-currency; Feather has deeper privacy tooling for desktop power users.
- **vs [[monerujo]]:** Monerujo is Android-only (open-source, Apache-2.0), simpler, and has a smaller feature surface. Cake Wallet has Flutter cross-platform parity, Polyseed, hardware wallet support, and multi-currency. Monerujo is a lighter, single-purpose option for Android users who want XMR-only.

Cake Wallet's main differentiators within the Monero ecosystem:
1. **Only major Monero wallet on iOS, Android, macOS, and Linux** from a single codebase.
2. **Polyseed by default** — reduces friction for new users (no restore-height management).
3. **Hardware wallet support** — Ledger, Trezor, and air-gapped options; unique in mobile Monero wallets.
4. **Built-in swap aggregation** — XMR-to-BTC and other pairs without leaving the app.
5. **Multi-currency** — 17 coins including BTC Lightning, Zcash shielded, and Litecoin MWEB.

---

## Limitations and criticisms

### Custodial elements in swap / exchange

- **ChangeNow, Trocador, and LetsExchange are centralised intermediaries.** During a swap, these providers temporarily hold user funds. They are not non-custodial. Cake Wallet's non-custodial claim refers specifically to key custody, not to the swap pipeline. Users performing BTC→XMR swaps via ChangeNow are trusting ChangeNow for the duration of the swap.
- **Chainflip is decentralised** but its cross-chain liquidity model involves its own protocol validators; it is not a pure atomic swap.
- **KYC threshold risk:** Centralised swap providers may have per-trade or cumulative KYC thresholds. This is not disclosed in Cake Wallet's own documentation. Users seeking maximum anonymity should use Chainflip or swap outside the app.
- **Cake Pay** (gift card spending) may involve KYC or identity requirements depending on the merchant and jurisdiction — [NOT FOUND] in documentation.

### Privacy risks

- **Default Cake Labs nodes:** New users have their IP address and block height visible to Cake Labs by default. Privacy policy states this data is received but not retained. Users who do not change to their own nodes are trusting Cake Labs' policy.
- **Transaction history APIs for EVM/Solana chains:** Cake Wallet queries Etherscan, PolygonScan, ArbiScan, BaseScan, BSCScan, and TronGrid APIs for transaction history for those chains. These APIs receive the user's wallet address and IP address. These toggles are in Settings → Connections.
- **Tor scope limitation:** Built-in Tor only covers wallet-to-node and price API requests. Application-layer traffic (swap quotes, app updates, error reports) may not be covered. The documentation acknowledges this and recommends combining with a VPN or Orbot for comprehensive coverage.
- **Coin selection fingerprinting (open issue #3407):** A reported privacy issue notes that the coin/UTXO selection algorithm follows a predictable pattern, creating a fingerprint that could correlate transactions. Status: open as of August 2026.
- **Bitcoin filter sync (open issue #3335):** Proposal to investigate compact filter sync for Bitcoin, suggesting the current synchronisation method may leak information.

### Reproducible builds and provenance

- No reproducible build verification process is publicly documented or actively maintained for any platform.
- The Linux Flatpak bundle disables GPG verification (`gpg-verify=false`, `gpg-verify-summary=false`), removing the standard Flatpak trust chain.
- An open issue (#3513, August 2026) formally requests signed release provenance (GitHub artifact attestations, Sigstore, or maintainer-key-signed checksums).
- SHA-256 hashes are published for APK releases, but these prove only integrity against GitHub release metadata — they do not bind the artifact to the source commit or the official build workflow.

### Security audits

- **No third-party security audit has been publicly disclosed.** The documentation contains no references to completed security audits or bug bounty programmes. This is a material gap for an RFP context.
- Cake Wallet's PRIVACY.md (last modified January 2024) is the most recent official privacy-related documentation.

### Platform gaps

- **Windows:** No current v6.x release; only pre-v6.0 builds are available. Desktop overhaul planned for late 2026. Not recommended for production use.
- **Linux:** No hardware wallet support. No Lightning support. No MWEB support. Desktop overhaul pending.
- **iOS:** No USB hardware wallet connection (Apple platform restriction); Bluetooth only. macOS and Linux support both Bluetooth and USB.
- **Tor on iOS:** Requires Orbot or a VPN for comprehensive coverage; built-in Tor has service compatibility issues with some providers on iOS.

### App store and community criticisms

- GapheneOS users have reported PIN authentication freezes and node connection instability on Android (GitHub issue #3222, May 2026). The reporter requested removal from the Accrescent store until baseline functionality is reliable on hardened Android.
- One prominent iOS App Store review (noted in data collection) cited exchange functionality issues (April 2023 — dated; may be resolved).
- Wownero support was dropped to view-only mode; Haven support was removed entirely. Users of those coins had to migrate.
- The Windows build lag (no v6.x available as of August 2026) is a recurring community complaint for desktop users.

---

## Sources

- [Cake Wallet homepage](https://cakewallet.com) — accessed 2026-08-12 — [archived](../sources/2026-08-12-cakewallet-com-home.html)
- [GitHub API: cake-tech/cake_wallet](https://api.github.com/repos/cake-tech/cake_wallet) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-cake-tech-cake-wallet.json)
- [Cake Wallet documentation (full LLM export)](https://docs.cakewallet.com/llms-full.txt) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-llms-full.txt)
- [Cake Wallet documentation sitemap](https://docs.cakewallet.com/sitemap.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-sitemap.md)
- [Cake Wallet PRIVACY.md (GitHub)](https://raw.githubusercontent.com/cake-tech/cake_wallet/main/PRIVACY.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-com-cake-tech-privacy-md.md)
- [Docs: Built-in Tor](https://docs.cakewallet.com/features/privacy-and-security/built-in-tor) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-tor.html)
- [Docs: Authentication and 2FA](https://docs.cakewallet.com/features/privacy-and-security/authentication) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-authentication.html)
- [Docs: Monero crypto page](https://docs.cakewallet.com/cryptos/monero.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-cryptos-monero.html)
- [Docs: Wallet Groups](https://docs.cakewallet.com/features/managing-your-wallet/wallet-groups) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-wallet-groups.html)
- [Docs: FAQ Common Questions](https://docs.cakewallet.com/faq/common-questions.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-faq-common-questions.html)
- [GitHub Releases page](https://github.com/cake-tech/cake_wallet/releases) — accessed 2026-08-12
- [Apple App Store listing](https://apps.apple.com/us/app/cake-wallet-monero-bitcoin/id1334702542) — accessed 2026-08-12
- [GitHub issue #3407 — coin selection fingerprinting](https://github.com/cake-tech/cake_wallet/issues/3407) — accessed 2026-08-12
- [GitHub issue #3513 — Linux release provenance request](https://github.com/cake-tech/cake_wallet/issues/3513) — accessed 2026-08-12
- [GitHub issue #3222 — GrapheneOS sync/auth issues](https://github.com/cake-tech/cake_wallet/issues/3222) — accessed 2026-08-12
