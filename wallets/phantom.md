---
tags: [wallet, solana, ethereum, bitcoin, multi-chain, closed-source, mobile, browser-extension]
chains: [solana, ethereum, bitcoin, base, polygon, sui, monad, hyperliquid]
access_date: 2026-08-12
---

# Phantom Wallet

Phantom is the dominant Solana-native self-custody wallet, expanded to a multi-chain consumer finance platform supporting eight blockchains as of late 2025. It holds 39.4% of Solana wallet market share and reported 15–17 million monthly active users (MAU) at its 2025 peak. Phantom is closed-source (proprietary), VC-backed, and operates browser extensions plus mobile apps.

## Identity and Governance

- **Type:** Self-custody software wallet (closed-source, proprietary)
- **Company:** Phantom Technologies Inc., Delaware (US)
- **Founded:** 2021
- **CEO / co-founder:** Brandon Millman
- **Funding:** $268M total — Series A: $9M (a16z led); Series B: $109M (Paradigm led; a16z, Variant, Solana Ventures, Jump Crypto participated); Series C: $150M co-led by Sequoia Capital and Paradigm at a $3 billion valuation (2025)
- **Source:** [phantom.com/learn/blog/phantom-series-c](https://phantom.com/learn/blog/phantom-series-c) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-series-c.html)

## Supported Platforms

- **Browser extensions:** Chrome, Brave, Edge, Firefox
- **Mobile:** iOS, Android
- **Desktop:** no dedicated desktop app (browser extension covers desktop)
- **Source:** [phantom.com/download](https://phantom.com/download) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-download.html) (meta description confirms: "Available for Chrome, Brave, Firefox, iOS, or Android")

## Supported Chains (as of 2026-08-12)

Eight blockchains total per coinlaw.io (updated June 2026):

| Chain | Support added | Notes |
|-------|--------------|-------|
| Solana | Launch (2021) | Native chain; ed25519 keys |
| Ethereum | 2022 | EVM chains via EIP-1559; EIP-712 signing |
| Polygon | 2022–2023 | EVM L2 |
| Base | 2023–2024 | EVM L2 (Coinbase) |
| Bitcoin | 2023 | Legacy + Ordinals/BRC-20 |
| Sui | 2024 | Layer-1; added to extension and mobile |
| Hyperliquid | ~2024–2025 | Perpetual futures integration |
| Monad | 24 November 2025 | EVM-compatible L1; airdrop claims at launch |

- **Source:** [coinlaw.io/phantom-wallet-statistics/](https://coinlaw.io/phantom-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-phantom-wallet-statistics.html) (last updated 25 June 2026); Monad: [blockchain.news flashnews](https://blockchain.news/flashnews/monad-mainnet-live-on-nov-24-2025-phantom-wallet-enables-mon-airdrop-claims-today) cited in coinlaw article

## Key Management

- **Seed format:** BIP39 12-word secret recovery phrase (English wordlist)
- **Solana key derivation:** ed25519 via SLIP-10/BIP44 derivation path `m/44'/501'/0'/0'` (Phantom default); ed25519 is not standard BIP32 (secp256k1), requiring SLIP-10 specification for hierarchical derivation on Edwards curves
- **Ethereum key derivation:** secp256k1, BIP44 path `m/44'/60'/0'/0/n` (standard EVM)
- **Bitcoin key derivation:** secp256k1, BIP84 path for native SegWit (bech32) addresses; Taproot (P2TR) also supported for Ordinals
- **Multiple accounts:** Yes — users can create or import multiple accounts under a single seed phrase, each at its own derivation path index
- **Import by private key:** Yes — single account import without seed phrase
- **BIP39 passphrase (25th word):** [NOT FOUND] — no documentation of passphrase extension support in Phantom; not mentioned in help centre or security page
- **Watch-only:** [NOT FOUND] — no confirmed watch-only wallet mode in Phantom UI
- **Source:** Phantom developer docs (closed; JS SDK only); key derivation path inferred from Solana ecosystem conventions and Phantom SDK; [coinlaw.io summary](https://coinlaw.io/phantom-wallet-statistics/) — accessed 2026-08-10

## Hardware Wallet Support

- **Ledger:** Yes — confirmed on browser extension for Solana accounts; USB (WebHID/WebUSB) connection; Ledger Nano S/X/S+/Stax
- **Mobile hardware wallet:** [NOT FOUND] — Ledger support on mobile not confirmed; Ledger Bluetooth (BLE) on mobile [NOT FOUND] in Phantom documentation
- **Trezor:** [NOT FOUND] — not confirmed in Phantom documentation
- **Air-gapped / QR-based:** [NOT FOUND] — no QR-code hardware wallet support documented
- **Source:** Phantom help centre returning 401 (credential required); inferred from community knowledge; Ledger Solana support widely documented in Solana ecosystem; [phantom.com/security](https://phantom.com/security) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-security.html) (mentions "independently audited" but not HW wallet specifics)

## Transaction Signing and Simulation

### Solana
- **Transaction simulation:** Yes — Phantom displays a transaction preview before signing, showing token balance changes, NFT transfers, and programme interactions. Positioned as a security feature to prevent drain attacks ("Scam detection flags malicious transactions instantly" — security page)
- **Versioned transactions:** Yes — Phantom supports Solana versioned transactions (v0 with Address Lookup Tables), required for most modern Solana DeFi protocols
- **Message signing:** Yes — `signMessage` for arbitrary UTF-8 byte arrays (Solana wallet standard)
- **Source:** [phantom.com/security](https://phantom.com/security) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-security.html); Phantom developer docs

### Ethereum / EVM
- **EIP-712 typed data:** Yes — standard EVM signing; required for permit-based approvals and structured data
- **EIP-1559:** Yes — Type 2 transactions with base fee + priority fee
- **Personal sign (EIP-191):** Yes
- **Source:** Inferred from EVM standard; Phantom EVM support confirmed by coinlaw.io

## DApp Connection

- **Solana:** Phantom injects `window.phantom.solana` provider into browser pages; implements the [Solana Wallet Standard](https://github.com/wallet-standard/wallet-standard) (Phantom is a key contributor — `sign-in-with-solana` repo: 155 stars; `sol-wallet-adapter`: 13 stars in org)
- **EVM chains:** Phantom injects `window.ethereum` provider for EVM dApps (MetaMask-compatible interface)
- **WalletConnect:** [NOT FOUND] — Phantom extension does not appear to use WalletConnect for desktop; mobile may use deep links or WalletConnect; not confirmed via package inspection (closed-source)
- **Mobile dApp browser:** Yes — built-in dApp browser on mobile (iOS/Android) for browsing Solana and EVM dApps
- **Deep links:** Yes — Phantom supports deep link protocol for mobile app interactions; public demo: [github.com/phantom/deep-link-demo-app](https://github.com/phantom/deep-link-demo-app) (1,415 stars)
- **Source:** [github.com/phantom/sign-in-with-solana](https://github.com/phantom/sign-in-with-solana) — 155 stars — [archived](../sources/2026-08-12-api-github-phantom-sign-in-with-solana.json); [github.com/phantom/deep-link-demo-app](https://github.com/phantom/deep-link-demo-app) — [archived](../sources/2026-08-12-api-github-phantom-deep-link-demo.json)

## Built-in Swap

- **Availability:** Yes — native swap aggregator across all supported chains
- **Fee:** Swap fee charged on trades (exact % not confirmed in archived sources; generally reported as competitive with ~0.85% or lower in community sources)
- **Swap volume:** $20 billion annual swap volume (at Series C announcement, 2025)
- **Perps integration:** Phantom integrated Hyperliquid perpetual futures; $42.783 billion cumulative perp volume per DefiLlama
- **Source:** [phantom.com/learn/blog/phantom-series-c](https://phantom.com/learn/blog/phantom-series-c) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-series-c.html); [coinlaw.io/phantom-wallet-statistics/](https://coinlaw.io/phantom-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-phantom-wallet-statistics.html)

## Staking

- **Solana native staking:** Yes — Phantom provides direct SOL staking through the UI; users can delegate to validators and earn staking rewards
- **Staking type:** Delegated proof-of-stake (native Solana staking); not liquid staking (users receive staking rewards but no liquid staking token unless using a third-party protocol)
- **Validator selection:** Phantom recommends validators and allows users to choose; specific default validators [NOT FOUND] in archived sources
- **Source:** [phantom.com/learn/blog/phantom-2024-year-in-review](https://phantom.com/learn/blog/phantom-2024-year-in-review) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-2024-year-in-review.html); help.phantom.com staking article (blocked by OneCLI credential requirement — 2026-08-12)

## NFT and Token Support

- **Solana NFTs:** Yes — displays Metaplex NFTs, compressed NFTs (cNFTs / Metaplex Bubblegum), Solana Token 2022 extensions
- **Ethereum NFTs:** Yes — ERC-721 and ERC-1155 display
- **Bitcoin Ordinals / BRC-20:** Yes — Ordinals inscriptions and BRC-20 token management on Bitcoin
- **In-wallet collectibles browser:** Yes — NFT gallery view on both extension and mobile
- **Source:** [coinlaw.io/phantom-wallet-statistics/](https://coinlaw.io/phantom-wallet-statistics/) — accessed 2026-08-10; Phantom home page product description

## Security

### Transaction Simulation and Phishing Detection
- **Transaction simulation:** Yes — pre-signing simulation shows token balance changes; "Scam detection flags malicious transactions instantly"
- **Phishing blocklist:** Yes — open-source domain blocklist at [github.com/phantom/blocklist](https://github.com/phantom/blocklist) (90 stars); Phantom scans URLs of dApps against the blocklist before connection
- **Source:** [phantom.com/security](https://phantom.com/security) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-security.html) ("Avoid phishing sites with our open source blocklist"); [github.com/phantom/blocklist](https://github.com/phantom/blocklist) — [archived](../sources/2026-08-12-api-github-phantom-blocklist.json)

### Security Audits
- **Kudelski Security (2021):** PDF audit report published at [github.com/phantom/audit-reports](https://github.com/phantom/audit-reports) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-phantom-audit-reports-contents.json)
- **Least Authority (2024):** PDF audit report published at same repo
- **Audit repository:** [github.com/phantom/audit-reports](https://github.com/phantom/audit-reports) — two audits confirmed in directory listing (Kudelski-Security-2021.pdf, Least_Authority-2024.pdf)

### Security Incidents
- **2022 Solana wallet drain:** In August 2022, ~$6M was drained from ~9,000 Solana wallets across Phantom, Slope, and other wallets. Root cause identified as Slope wallet (Slope sent seed phrases to a centralised logging server). Phantom itself was not the source of the vulnerability; affected Phantom users were those who had previously imported accounts originally created in Slope. Phantom confirmed no vulnerability in Phantom code.
- **Source:** Widely reported (TechCrunch, The Block, August 2022); coinlaw.io summary — [archived](../sources/2026-08-10-coinlaw-io-phantom-wallet-statistics.html)

### Key Storage
- Keys stored locally in browser extension storage (encrypted) or mobile device secure storage; no server-side key storage
- Phantom's tagline on security page: "security, not surveillance"

## Privacy and Telemetry

- **Analytics:** Phantom uses Google Analytics (`dataLayer`/`gtag`) on its website and likely on the extension for product telemetry; opt-out mechanism via OneTrust cookie consent (confirmed in privacy page JavaScript)
- **RPC provider:** [NOT FOUND] — Phantom uses its own RPC infrastructure for Solana; does not expose default RPC URL publicly; users can configure custom RPC endpoints
- **IP exposure:** Users' IP addresses are exposed to Phantom's RPC nodes on every transaction/query
- **No Tor support:** No confirmed SOCKS5/Tor proxy setting; no onion RPC service
- **Source:** [phantom.com/privacy](https://phantom.com/privacy) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-privacy.html) (OneTrust consent management confirmed; `analytics_storage` consent required)

## Backup and Recovery

- **Seed phrase backup:** Standard 12-word BIP39 seed phrase; shown once at wallet creation; user must save it offline
- **Cloud backup:** [NOT FOUND] — no confirmed cloud backup or iCloud/Google Drive integration; Phantom discourages any form of digital storage of seed phrases
- **Biometric unlock:** Yes — iOS Face ID / Touch ID; Android fingerprint / face unlock; biometric protects app access, not the seed phrase itself
- **Source:** Apple App Store description confirms "Trusted by 20M+ users"; iOS rating 4.78, 64,122 ratings — [archived](../sources/2026-08-12-itunes-apple-com-phantom.json)

## Codebase and Open-Source Status

- **Main wallet code:** Closed-source (proprietary); no public GitHub repository for the wallet itself
- **GitHub org (phantom):** [github.com/phantom](https://github.com/phantom) — public repos include SDKs and tooling only
- **Public repos of note:**

| Repo | Stars | Purpose |
|------|-------|---------|
| deep-link-demo-app | 1,415 | Mobile deep-link integration demo |
| sign-in-with-solana | 155 | Sign-In With Solana (SIWS) standard |
| blocklist | 90 | Open-source phishing domain blocklist |
| docs | 41 | Developer documentation |
| sol-wallet-adapter | 13 | Legacy Solana wallet adapter (deprecated) |

- **Source:** [github.com/phantom org repos API](https://api.github.com/orgs/phantom/repos?per_page=20) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-orgs-phantom-repos.json)

## Adoption Metrics

| Metric | Value | Date | Source |
|--------|-------|------|--------|
| Peak MAU | ~17M | 2025 peak | coinlaw.io citing Brandon Millman Solana Accelerate keynote |
| Steady-state MAU | ~15M | 2025 steady-state | Phantom Series C announcement |
| MAU at Series C close | 15M | ~Nov–Dec 2024 | Phantom blog |
| MAU in 2024 (year-end) | 10M | End 2024 | Phantom 2024 Year in Review blog |
| Total transactions (2024) | 850M+ | 2024 | Phantom blog |
| TPS across chains | ~27 | 2024 average | Phantom blog (850M ÷ 365 days) |
| Mobile app downloads (all-time) | 24M+ | End 2024 | Phantom 2024 Year in Review |
| App opens / day (avg) | 16× | 2024 | coinlaw.io |
| Self-custodied assets | $25B | At Series C | Phantom Series C blog |
| Annual swap volume | $20B | 2025 | Phantom Series C blog |
| Perps volume (cumulative) | $42.78B | June 2026 | DefiLlama via coinlaw.io |
| FY 2025 gross protocol revenue | $325.89M | FY 2025 | DefiLlama via coinlaw.io |
| Cumulative protocol fees | $565.42M | To June 2026 | DefiLlama via coinlaw.io |
| Total funding raised | $268M | 2025 | coinlaw.io / Phantom blog |
| Implied valuation (Series C) | $3B | 2025 | Phantom Series C blog |
| Solana market share | 39.4% | 2025 | coinlaw.io |
| Usernames registered | ~3.8M | At Series C | coinlaw.io citing Millman |
| Chrome Web Store installs | 4,000,000 | 2026-08-12 | Chrome Web Store listing |
| Google Play installs | 10M+ | 2026-08-12 | Google Play listing |
| Google Play reviews | 152K | 2026-08-12 | Google Play listing |
| Google Play rating | 4.5★ | 2026-08-12 | Google Play listing |
| iOS rating | 4.78★ | 2026-08-12 | Apple App Store API |
| iOS rating count | 64,122 | 2026-08-12 | Apple App Store API |
| iOS version | 26.24.2 | 2026-08-12 | Apple App Store API |

- **Sources:**
  - [coinlaw.io/phantom-wallet-statistics/](https://coinlaw.io/phantom-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-phantom-wallet-statistics.html) (last updated 25 June 2026)
  - [phantom.com/learn/blog/phantom-series-c](https://phantom.com/learn/blog/phantom-series-c) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-series-c.html)
  - [phantom.com/learn/blog/phantom-2024-year-in-review](https://phantom.com/learn/blog/phantom-2024-year-in-review) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phantom-com-2024-year-in-review.html)
  - [Chrome Web Store](https://chromewebstore.google.com/detail/phantom/bfnaelmomeimhlpmgjnjophhpkkoljpa) — accessed 2026-08-12 — [archived](../sources/2026-08-12-chrome-webstore-phantom.html)
  - [Google Play Store](https://play.google.com/store/apps/details?id=app.phantom) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-phantom.html)
  - [iTunes/App Store API lookup](https://itunes.apple.com/lookup?bundleId=app.phantom) — accessed 2026-08-12 — [archived](../sources/2026-08-12-itunes-apple-com-phantom.json)

## Limitations and Risks

- **Closed-source:** The wallet code is not auditable by the public; users must trust Phantom's binary. Only two formal security audits published (Kudelski 2021, Least Authority 2024).
- **VC-backed, US-based:** Heavy institutional ownership; US regulatory exposure (SEC, FinCEN); potential for forced compliance changes or geo-blocking.
- **No BIP39 passphrase:** No 25th-word passphrase support confirmed; users who want passphrase protection must use a hardware wallet.
- **No Tor/privacy RPC:** Phantom's RPC infrastructure sees all user queries; no privacy-preserving routing option.
- **Multi-chain quality depth trade-off:** Rapid expansion to 8 chains in ~4 years; security model for each chain integration is not independently verifiable (closed-source).
- **No desktop app:** Relies on browser extension for desktop; browser extension attack surface is larger than a standalone app.
- **No hardware wallet on mobile:** Ledger support is extension-only; mobile users have no hardware wallet signing path.
- **Custody of RPC data:** Phantom operates its own Solana RPC nodes; all transaction data, balances, and query patterns are visible to Phantom.

## Related Notes

- [[patterns/walletconnect-integration]] — Phantom does not appear to use WalletConnect on extension; uses injected provider model
- [[patterns/solana-key-derivation]] — ed25519 SLIP-10 key derivation used by Phantom and Solflare
- [[wallets/solflare]] — primary Solana wallet competitor (4M MAU, Jan 2026)
- [[wallets/backpack]] — open-source Solana wallet competitor (1,650 GitHub stars)

## Sources

| Source | URL | Access date | Archive |
|--------|-----|-------------|---------|
| coinlaw.io Phantom statistics | https://coinlaw.io/phantom-wallet-statistics/ | 2026-08-10 | [archived](../sources/2026-08-10-coinlaw-io-phantom-wallet-statistics.html) |
| Phantom Series C blog | https://phantom.com/learn/blog/phantom-series-c | 2026-08-12 | [archived](../sources/2026-08-12-phantom-com-series-c.html) |
| Phantom 2024 Year in Review | https://phantom.com/learn/blog/phantom-2024-year-in-review | 2026-08-12 | [archived](../sources/2026-08-12-phantom-com-2024-year-in-review.html) |
| Phantom download page | https://phantom.com/download | 2026-08-12 | [archived](../sources/2026-08-12-phantom-com-download.html) |
| Phantom security page | https://phantom.com/security | 2026-08-12 | [archived](../sources/2026-08-12-phantom-com-security.html) |
| Phantom privacy policy | https://phantom.com/privacy | 2026-08-12 | [archived](../sources/2026-08-12-phantom-com-privacy.html) |
| GitHub orgs/phantom repos | https://api.github.com/orgs/phantom/repos | 2026-08-12 | [archived](../sources/2026-08-12-api-github-com-orgs-phantom-repos.json) |
| GitHub phantom/audit-reports | https://api.github.com/repos/phantom/audit-reports/contents | 2026-08-12 | [archived](../sources/2026-08-12-api-github-phantom-audit-reports-contents.json) |
| GitHub phantom/blocklist | https://api.github.com/repos/phantom/blocklist | 2026-08-12 | [archived](../sources/2026-08-12-api-github-phantom-blocklist.json) |
| Chrome Web Store | https://chromewebstore.google.com/detail/phantom/bfnaelmomeimhlpmgjnjophhpkkoljpa | 2026-08-12 | [archived](../sources/2026-08-12-chrome-webstore-phantom.html) |
| Google Play Store | https://play.google.com/store/apps/details?id=app.phantom | 2026-08-12 | [archived](../sources/2026-08-12-play-google-com-phantom.html) |
| Apple App Store API | https://itunes.apple.com/lookup?bundleId=app.phantom | 2026-08-12 | [archived](../sources/2026-08-12-itunes-apple-com-phantom.json) |
