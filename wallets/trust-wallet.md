---
tags: [wallet, ethereum, evm, multi-chain, mobile]
category: self-custody software wallet
website: https://trustwallet.com
docs: https://developer.trustwallet.com
github_org: https://github.com/trustwallet
launched: 2017
acquired_by: Binance (2018)
license: Apache-2.0 (wallet-core); mobile app source not public
platforms: iOS, Android, Browser Extension (Chromium)
---

# Trust Wallet

Non-custodial mobile wallet founded in 2017 and acquired by Binance in 2018. Supports 100+ blockchains (167 in wallet-core registry as of August 2026) via an open-source C++ signing library (wallet-core). The mobile app itself is not open-source; only the underlying cryptographic library is published under Apache-2.0. As of August 2026 the official homepage cites 200 million users, but MAU figures from third-party sources conflict: 17M (coinlaw.io, early 2025) vs. 60M (per coinlaw.io, original source cited as sqmagazine.co.uk — not independently confirmed, early 2025). Both figures are recorded below without resolution.

## Adoption / usage metrics

| Metric | Value | Date | Source |
|--------|-------|------|--------|
| Self-reported total users | 200M+ | 2025 (March) | [trustwallet.com homepage](https://trustwallet.com) — accessed 2026-08-12 — [archived](../sources/2026-08-12-trustwallet-com-home.html) |
| Self-reported total users (Google Play listing) | 200M+ | 2026-08-06 (last updated) | [Google Play structured data](https://play.google.com/store/apps/details?id=com.wallet.crypto.trustapp) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-trust-wallet.html) |
| MAU (source A) | ~17M (early 2025) | 2025 | [coinlaw.io/trust-wallet-statistics/](https://coinlaw.io/trust-wallet-statistics/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinlaw-io-trust-wallet-statistics.html) — **third-party analyst, not Trust Wallet disclosure** |
| MAU (source B) | 60M ("global users", conflates MAU with total users) | early 2025 | per coinlaw.io (original source cited as sqmagazine.co.uk — not independently confirmed) — **not independently archived; URL not confirmed**; coinlaw.io itself also cites this figure in a comparison table as "60 million users" separate from 17M MAU |
| Google Play install tier | 50M+ | 2026-08-12 | [Google Play listing](https://play.google.com/store/apps/details?id=com.wallet.crypto.trustapp) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-trust-wallet.html) |
| Google Play rating | 4.48 / 5 | 2026-08-12 | [Google Play structured JSON-LD](https://play.google.com/store/apps/details?id=com.wallet.crypto.trustapp) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-trust-wallet.html) |
| Google Play review count | 2,541,392 | 2026-08-12 | [Google Play structured JSON-LD](https://play.google.com/store/apps/details?id=com.wallet.crypto.trustapp) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-trust-wallet.html) |
| Total downloads (self-reported) | 200M+ (March 2025) / 140M+ (Sept 2024) | 2025 | [coinlaw.io/trust-wallet-statistics/](https://coinlaw.io/trust-wallet-statistics/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinlaw-io-trust-wallet-statistics.html) — citing Trust Wallet marketing material |
| Market share (global wallet downloads) | ~35% (March 2025); ~20% (Jan 2025) | 2025 | [coinlaw.io/trust-wallet-statistics/](https://coinlaw.io/trust-wallet-statistics/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinlaw-io-trust-wallet-statistics.html) |
| U.S. market share | 16.9% | 2025 | [coinlaw.io/trust-wallet-statistics/](https://coinlaw.io/trust-wallet-statistics/) — citing Coinweb.com — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinlaw-io-trust-wallet-statistics.html) |
| Security scanner blocked | $162M in potentially harmful transactions | 2025 | [coinlaw.io/trust-wallet-statistics/](https://coinlaw.io/trust-wallet-statistics/) — citing Trust Wallet internal data — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinlaw-io-trust-wallet-statistics.html) — **self-reported** |
| wallet-core GitHub stars | 3,553 | 2026-08-12 | [GitHub API](https://api.github.com/repos/trustwallet/wallet-core) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-wallet-core.json) |
| wallet-core forks | 1,973 | 2026-08-12 | [GitHub API](https://api.github.com/repos/trustwallet/wallet-core) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-wallet-core.json) |
| assets repo GitHub stars | 5,361 | 2026-08-12 | [GitHub API](https://api.github.com/repos/trustwallet/assets) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-assets.json) |
| Supported blockchains (wallet-core) | 167 (per registry.json) | 2026-08-12 | [wallet-core registry.json](https://raw.githubusercontent.com/trustwallet/wallet-core/master/registry.json) — accessed 2026-08-12 — [archived](../sources/2026-08-12-trustwallet-wallet-core-registry.json) |
| wallet-core latest version | 4.7.3 (7 August 2026) | 2026-08-12 | [GitHub releases API](https://api.github.com/repos/trustwallet/wallet-core/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-wallet-core-releases-latest.json) |
| ISO certification | Cited ("ISO Certified") | 2026-08-12 | [trustwallet.com homepage](https://trustwallet.com) — accessed 2026-08-12 — [archived](../sources/2026-08-12-trustwallet-com-home.html) — **no certificate number or standard specified on page** |

### MAU conflict note

Two figures circulate: ~17M MAU (coinlaw.io, attributed to a Trust Wallet statement, early 2025) and 60M (per coinlaw.io, original source cited as sqmagazine.co.uk — not independently confirmed; a wallet-comparison table entry labelled "users" not "MAU"). The distinction is important: the 60M figure may conflate cumulative registered accounts with monthly active users. Neither figure is supported by an independently audited primary disclosure from Trust Wallet (e.g. a company press release with a specific date and methodology). Both are recorded here as sourced. The 200M figure on the homepage and Google Play listing is a "total users" or "total downloads" metric, not MAU.

## How it works

### User perspective

1. Download the Trust Wallet app (iOS or Android) or Chromium browser extension.
2. Create a new wallet: the app generates a BIP39 12-word mnemonic. The user is prompted to write it down. No account or email is required.
3. The app derives addresses for each supported blockchain using BIP44 HD derivation paths (e.g. `m/44'/60'/0'/0/0` for EVM chains, `m/44'/501'/0'/0'` for Solana) via the wallet-core C++ library.
4. All signing operations occur locally. The private keys and mnemonic are stored on-device and are never transmitted to Trust Wallet servers.
5. To recover, import the 12-word mnemonic on any device. An optional Encrypted Cloud Backup stores an AES-256-GCM encrypted copy of the mnemonic in the user's cloud (iCloud / Google Drive) protected by a user-set password — Trust Wallet does not hold the decryption key.
6. DApp interaction: the in-app DApp browser (mobile) or browser extension injects `window.trustwallet.ethereum` (EVM) and equivalent providers. WalletConnect v2 is supported for mobile connection to external DApps.

### System perspective

- **wallet-core** (C++, Apache-2.0): cross-platform library that handles all key derivation, address generation, and transaction signing. Bindings exist for Swift (iOS), Java/Kotlin (Android), Rust, Go, WebAssembly, and npm. It does not handle networking or UI.
- **Derivation**: BIP39 mnemonic → BIP32 root seed → BIP44 per-coin derivation path as specified in `registry.json`. Supported curves: secp256k1, ed25519, ed25519Blake2bNano (NANO), nist256p1, ed25519ExtendedCardano.
- **trust-web3-provider**: TypeScript library (915 GitHub stars, forks: 497) that wraps wallet-core signing for DApp browser injection on iOS and Android.
- **Barz**: ERC-4337 smart contract wallet (account abstraction) — Trust Wallet's "SWIFT" feature. Separate from the EOA wallet; uses modular upgradeable smart wallet design.
- **Privacy**: per the October 2025 Privacy Notice, wallet private keys and transaction data are stored locally; IP addresses are used transiently for traffic routing and not retained; device/usage information (model, OS, browser type) is collected for service improvement.

## Key features

| Feature | Support |
|---------|---------|
| Seed format | BIP39 12-word mnemonic (English wordlist) |
| HD derivation | BIP32/BIP44 per-chain paths via wallet-core |
| Supported blockchains | 167 (wallet-core registry, Aug 2026); described as "100+" officially |
| EVM chains | Yes — Ethereum, BNB Smart Chain, Polygon, Arbitrum, Optimism, Base, Avalanche, and all EVM-compatible chains |
| Non-EVM chains | Yes — Bitcoin, Solana, XRP, Cosmos, TON, Sui, Tron, Cardano, NEAR, Aptos, and many others |
| Zcash | Yes (secp256k1 signing; shielded pool support [NOT FOUND] — transparent only likely) |
| Monero | **No** — XMR is absent from registry.json; not confirmed in wallet-core chain registry (accessed 2026-08-12) |
| WalletConnect | Yes — WalletConnect v2 (CAIP-25) for mobile DApp connections |
| DApp browser | Yes (in-app browser, mobile); browser extension for desktop |
| NFT support | Yes (view, manage) |
| Staking | Yes (ETH, BNB, SOL, ATOM, and others via native and liquid staking) |
| In-app swap | Yes (DEX aggregators; "Swaps" feature) |
| Buy crypto | Yes (fiat on-ramp integrations including Coinbase Pay, Binance Pay) |
| Hardware wallet | **No** — no confirmed hardware wallet integration in mobile app or extension (as at 2026-08-12; [NOT FOUND]) |
| Air-gapped / PSBT signing | **No** — mobile-first; no PSBT or offline signing workflow |
| Multisig (EOA) | **No** — EOA wallet is single-key; Barz smart wallet supports ERC-4337 multi-signer |
| Browser extension | Yes (Chromium) — separate codebase from mobile; injects EVM, Solana, and Cosmos providers |
| Tor support | **No** — no built-in Tor or proxy configuration found |
| Encrypted cloud backup | Yes (optional) — AES-256-GCM, user-controlled password; stored in iCloud / Google Drive; Trust Wallet does not hold the key |
| Manual SRP backup | Yes (mandatory during wallet creation) |
| Reproducible builds | [NOT FOUND] — no public reproducible-build process or multi-signer attestation documented for mobile app; wallet-core builds are open-source |
| F-Droid | [NOT FOUND] — no official F-Droid listing found (2026-08-12) |
| Open-source (signing library) | Yes — wallet-core Apache-2.0 |
| Open-source (mobile app) | **No** — iOS app repo (trust-wallet-ios) and Android app repo (trust-wallet-android-source) are archived and no longer actively published; current app code not public |
| Account abstraction | Yes — Barz (ERC-4337 smart wallet), marketed as "SWIFT" |
| Security scanner | Yes (in-app; flags risky addresses and DApp connections) |

## Architecture decisions

- **C++ core, language-native bindings**: wallet-core is written in C++ for maximum code reuse across platforms, with generated Swift and Kotlin/Java bindings. This means iOS and Android share the same signing logic, reducing platform-divergence bugs.
- **No single "main" GitHub repo**: the app code is not published. wallet-core is the primary open-source artefact. Stars are split across wallet-core (3,553), assets (5,361), trust-web3-provider (915), and now-archived iOS/Android repos. Any stars comparison with wallets that publish full app source is not apples-to-apples.
- **Registry-driven multi-chain**: chain support is defined declaratively in `registry.json`, which specifies derivation paths, address encoding, explorer URLs, and elliptic curve per chain. Adding a new chain is a registry + C++ implementation PR, not a new app release.
- **Account abstraction layer**: "SWIFT" (Barz) is a separate product from the core EOA wallet — users can choose to use Barz for ERC-4337 features (social recovery, session keys, etc.) independently of their seed-based accounts.

## Differentiators

- **Broadest confirmed chain count** in this research set (167 in registry, 100+ marketed): competes with MetaMask (EVM-only) and Phantom (Solana-focused). See [[metrics/wallet-features]].
- **wallet-core as a public good**: other wallets (Crypto.com Wallet, Frontier) also use wallet-core — the library has an ecosystem beyond Trust Wallet. This is unique among the wallets in this research set.
- **Mobile-first with browser extension**: unlike Sparrow (desktop-only) or Electrum (desktop+Android), Trust Wallet is designed for mobile with an optional desktop extension. Unlike MetaMask (extension-first), Trust Wallet is app-first.
- **200M claimed user base**: largest claimed user base of any wallet in the research set by self-reported total users (though MAU is disputed). See [[metrics/wallet-adoption]].
- **In-app security scanner**: proactive transaction risk scoring is an unusual feature at this scale; reportedly blocked $162M in 2025 (self-reported).

## Binance ownership — sovereignty implications

Binance acquired Trust Wallet in 2018. The legal entity operating Trust Wallet as of October 2025 is "Dapps Platform Bahrain W.L.L doing business as Trust Wallet" (per Privacy Notice). The following is recorded from primary sources:

- The Privacy Notice (October 2025) does not name Binance as a data recipient; "Affiliates" are mentioned as potential Personal Data recipients.
- The homepage (August 2026) presents Trust Wallet as independent and does not reference Binance ownership.
- The developer docs list `"research": "https://research.binance.com/en/projects/trustwallet"` as Trust Wallet's research URL — confirming the ongoing Binance relationship at the infrastructure level.
- **Self-custody claim**: the Privacy Notice confirms wallet private keys are stored locally and not transmitted to Trust Wallet servers. No Binance account is required to use the wallet.
- **Regulatory risk**: Binance has faced regulatory actions in multiple jurisdictions (CFTC, DOJ, SEC, etc.). Whether Binance ownership creates legal compulsion risk for Trust Wallet user data is [NOT FOUND] in publicly available disclosures. The Bahrain entity structure may create some legal insulation; this is unverified.
- **Operational independence**: Trust Wallet operates under its own brand, codebase, and support structure. However, integration hooks exist (Binance Pay on-ramp, Binance research link). Full operational independence is not independently confirmed.

## Limitations and criticisms

- **Mobile app is closed source**: despite wallet-core being Apache-2.0, the actual iOS and Android app code is not published. Users cannot independently verify what the app does beyond the signing library. The iOS repo (trust-wallet-ios, 1,608 stars) and Android repo (trust-wallet-android-source, 402 stars) are archived and contain old, inactive source only.
- **No hardware wallet support**: Trust Wallet has no confirmed hardware wallet (Ledger, Trezor, etc.) integration. This is a significant limitation for high-value self-custody compared to Sparrow (14+ devices) or Electrum (8+ devices).
- **No Tor / privacy routing**: unlike Feather Wallet (Tor by default) or Electrum (SOCKS5 proxy), Trust Wallet has no built-in Tor support or network privacy features beyond the IP non-retention policy.
- **No Monero support**: XMR is absent from wallet-core's chain registry. Users requiring Monero must use a separate wallet (e.g. Cake Wallet, Feather Wallet).
- **2022 browser extension WASM entropy vulnerability**: a high-severity entropy weakness in the browser extension's WebAssembly implementation caused predictable private keys to be generated for wallets created between certain dates. Approximately $170,000 in user funds was drained from affected wallets in November–December 2022. Trust Wallet's own post-mortem blog post is no longer accessible at the URLs attempted (HTTP 500/404 as of 2026-08-12). The incident is documented in secondary sources including CoinDesk (17 November 2022: "Trust Wallet Fixes Vulnerability That Drained $170,000 From User Funds" — [archived](../sources/2026-08-12-coindesk-com-trust-wallet-wasm-170k.html)). The mobile wallet was not affected. Users were advised to move funds from wallets created by the extension between certain dates.
- **2023 Kudelski Security audit (Starknet library, wallet-core)**: a separate, unrelated cryptographic review commissioned by Trust Wallet in 2023 covered the Starknet-related `starknet-rs` crates (`starknet-crypto`, `starknet-curve`, `starknet-ff`) integrated in wallet-core. Testing period: 20 July – 7 August 2023; report v2.0 dated 15 September 2023. Findings: 6 low-severity issues (no critical, high, or medium) — inconsistency between formal algorithm definitions and implementation; hardcoded cryptographic parameters. This audit is unrelated to the 2022 WASM entropy incident.
- **MAU figure conflict**: 17M vs 60M MAU from different 2025 sources; neither is a primary Trust Wallet audited disclosure. The 60M figure may be a "registered users" or "total accounts" metric mislabelled. Resolution is [NOT FOUND].
- **Reproducible builds**: no public reproducible-build process documented for the mobile app. No multi-signer attestation comparable to Electrum or Feather Wallet.
- **No F-Droid listing**: no official F-Droid distribution found. Google Play is the primary Android distribution channel, raising the usual Play Services dependency concern.
- **Encrypted cloud backup is optional, not a replacement for SRP**: the cloud backup feature stores an encrypted mnemonic in iCloud/Google Drive. If the user loses both their SRP and their cloud backup password, funds are unrecoverable. The trust model is better than custodial cloud backup, but the feature may create false reassurance for users who treat it as a full recovery solution.

## Security audit

- **Kudelski Security, September 2023 (Starknet library, wallet-core)**: cryptographic review of `starknet-rs` crates (`starknet-crypto`, `starknet-curve`, `starknet-ff`) and their integration in wallet-core. Testing period: 20 July – 7 August 2023; report v2.0 dated 15 September 2023. Findings: 6 low-severity issues (no critical, high, or medium). Key themes: inconsistency between formal algorithm definitions and Starknet library implementation; hardcoded cryptographic parameters. Positive observations: code was "clean, concise, and commented throughout." The audit covered wallet-core Starknet integration only, not the full mobile app or browser extension, and is unrelated to the 2022 WASM entropy incident. Report publicly available in the wallet-core repository.
- Source: [wallet-core audit directory](https://api.github.com/repos/trustwallet/wallet-core/contents/audit) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-wallet-core-audit.json); [PDF report](https://raw.githubusercontent.com/trustwallet/wallet-core/master/audit/2023-09-15_TrustWallet_SecureCodeReviewReport_Public_v2.00.pdf) — accessed 2026-08-12 — [archived](../sources/2026-08-12-trustwallet-wallet-core-audit-2023-09.pdf)
- **HackerOne bug bounty**: a HackerOne programme for Trust Wallet was reported by secondary sources (coinlaw.io); the programme page returned 404 at `hackerone.com/trust_wallet` (accessed 2026-08-12). Whether the programme is active or has moved is [NOT FOUND].

## Sources

- [trustwallet.com homepage](https://trustwallet.com) — accessed 2026-08-12 — [archived](../sources/2026-08-12-trustwallet-com-home.html)
- [GitHub API: trustwallet/wallet-core](https://api.github.com/repos/trustwallet/wallet-core) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-wallet-core.json)
- [GitHub API: trustwallet/trust-wallet-ios](https://api.github.com/repos/trustwallet/trust-wallet-ios) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-trust-wallet-ios.json)
- [GitHub API: trustwallet/trust-wallet-android-source](https://api.github.com/repos/trustwallet/trust-wallet-android-source) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-trust-wallet-android-source.json)
- [GitHub API: trustwallet org repos (sorted by updated)](https://api.github.com/orgs/trustwallet/repos?per_page=100&sort=updated) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-orgs-trustwallet-repos-updated.json)
- [GitHub API: wallet-core latest release](https://api.github.com/repos/trustwallet/wallet-core/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-wallet-core-releases-latest.json)
- [GitHub API: trustwallet/assets](https://api.github.com/repos/trustwallet/assets) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-assets.json)
- [GitHub API: trustwallet/trust-web3-provider](https://api.github.com/repos/trustwallet/trust-web3-provider) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-trust-web3-provider.json)
- [wallet-core README](https://raw.githubusercontent.com/trustwallet/wallet-core/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-com-trustwallet-wallet-core-README.md)
- [wallet-core registry.md (chain list)](https://raw.githubusercontent.com/trustwallet/wallet-core/master/docs/registry.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-com-trustwallet-wallet-core-registry.md)
- [wallet-core registry.json (machine-readable)](https://raw.githubusercontent.com/trustwallet/wallet-core/master/registry.json) — accessed 2026-08-12 — [archived](../sources/2026-08-12-trustwallet-wallet-core-registry.json)
- [wallet-core audit directory (GitHub API)](https://api.github.com/repos/trustwallet/wallet-core/contents/audit) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-wallet-core-audit.json)
- [Kudelski Security audit PDF (2023-09-15)](https://raw.githubusercontent.com/trustwallet/wallet-core/master/audit/2023-09-15_TrustWallet_SecureCodeReviewReport_Public_v2.00.pdf) — accessed 2026-08-12 — [archived](../sources/2026-08-12-trustwallet-wallet-core-audit-2023-09.pdf)
- [Google Play listing: Trust: Crypto & Bitcoin Wallet](https://play.google.com/store/apps/details?id=com.wallet.crypto.trustapp) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-trust-wallet.html)
- [coinlaw.io: Trust Wallet Statistics](https://coinlaw.io/trust-wallet-statistics/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinlaw-io-trust-wallet-statistics.html)
- [Trust Wallet Privacy Notice (Oct 2025)](https://trustwallet.com/privacy-policy) — accessed 2026-08-12 — [archived](../sources/2026-08-12-trustwallet-com-privacy-policy.html)
- [Trust Wallet Developer Docs](https://developer.trustwallet.com) — accessed 2026-08-12 — [archived](../sources/2026-08-12-developer-trustwallet-com-index.html)
- [Trust Wallet Developer Docs (full text)](https://developer.trustwallet.com/developer/llms-full.txt) — accessed 2026-08-12 — [archived](../sources/2026-08-12-developer-trustwallet-com-llms-full.txt)
- [CoinDesk: "Trust Wallet Fixes Vulnerability That Drained $170,000 From User Funds" (17 November 2022)](https://www.coindesk.com/tech/2022/11/17/trust-wallet-fixes-vulnerability-that-drained-170000-from-user-funds/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coindesk-com-trust-wallet-wasm-170k.html) — **paywalled; URL slug confirms $170K figure; used as secondary source for the 2022 WASM entropy incident**
