---
title: Solflare Wallet
tags: [solana, wallet, staking, nft, hardware-wallet, defi]
aliases: [Solflare]
created: 2026-08-12
---

# Solflare Wallet

Solflare is the second-largest Solana wallet by monthly active users, founded in 2020 as the first wallet built exclusively for Solana. It is operated by Solflare Inc. and is Solana-first by design — it does not natively support EVM chains or Bitcoin, though EVM bridging is possible via a MetaMask Snap integration. Available as a browser extension, web app (solflare.com), iOS app, and Android app.

## Identity

| Field | Value |
|-------|-------|
| Founded | 2020 |
| Founder | Vidor (former gaming engineer) |
| Headquarters | [NOT FOUND] — US-incorporated |
| Chains supported | Solana (native); EVM via MetaMask Snap (bridge only) |
| Platforms | Browser extension (Chrome, Firefox, Edge, Brave), web app, iOS, Android |
| Licence | Partially open source — SDKs, MetaMask Snap, token lists, blocklists: public on GitHub; main extension, mobile apps, web wallet: not confirmed fully open source |
| GitHub org | [github.com/solflare-wallet](https://github.com/solflare-wallet) (40 public repos as of 2026-08-12) |

## Adoption Metrics

| Metric | Value | Date | Source |
|--------|-------|------|--------|
| Monthly active users | 4M+ | Mid-2025 / confirmed 2026 | Solflare homepage (self-reported); coinlaw.io (citing DeFi Dev Corp LOI, Aug 2025) |
| Assets under management | $14.1B | 2026-08-12 (live figure from solflare.com homepage) | solflare.com |
| AUM (earlier figure) | $8.5B | DOT Ads case study (date unspecified, ~2024) | coinlaw.io citing DOT campaign case study |
| iOS rating | 4.84 ★ (9,688 ratings) | 2026-07-30 (latest version date) | iTunes API — accessed 2026-08-12 |
| iOS version | 2.24.0 | 2026-07-30 | iTunes API |
| Android rating | 4.61 ★ (44,480 ratings) | 2026-08-12 | Google Play — accessed 2026-08-12 |
| Android installs | 1M+ | 2026-08-12 | Google Play — accessed 2026-08-12 |
| Ranking among Solana wallets | #2 (behind Phantom) | 2024–2026 | coinlaw.io citing DOT case study |
| App Store aggregate (homepage) | "51k Ratings" | 2026-08-12 | solflare.com (aggregate iOS + Android) |

### User Growth
- In a rebrand campaign, Solflare added 1.5M users in 6 months, growing to ~3.5M MAU; subsequent growth to 4M+ by mid-2025.
- DeFi Dev Corp signed a Letter of Intent (August 2025) to deploy Solflare as their official default wallet, citing Solflare's 4M user base.
- SimilarWeb ranks Solflare #337 in Finance apps in the US (28-day usage), #2,984 overall.

## GitHub Organisation Analysis

The `solflare-wallet` GitHub org has 40 public repos (all unlicensed unless noted) as of 2026-08-12. No single main application repository is public. Key public repos:

| Repo | Stars | Language | Notes |
|------|-------|----------|-------|
| `utl-aggregator` | 31 | TypeScript | Unified Token List aggregator |
| `solflare-snap` | 30 | JavaScript | MetaMask Snap (EVM bridge); ConsenSys Diligence audit Aug 2023 |
| `utl-sdk` | 26 | TypeScript | Token list SDK |
| `token-list` | 19 | JavaScript | Solana token registry |
| `utl-api` | 18 | TypeScript | Token list API |
| `solflare-sdk` | 9 | TypeScript | Wallet SDK |
| `dart-ed25519-hd-key` | 2 | Dart | Ed25519 HD key derivation (indicates Flutter-based mobile app) |

The Flutter-related repos (`flutter_nfc_kit`, `flutter_reactive_ble`, Dart libs) confirm the mobile app is built in Flutter/Dart. The main browser extension, mobile app, and web wallet source code are not in public repos — Solflare is **partially open source** only.

## Key Management and Seed Phrases

- **Seed phrase format:** 24-word BIP39 mnemonic (confirmed by coinbureau.com security review, Aug 2026; both Phantom and Solflare use 24-word BIP39)
- **Encryption:** AES encryption of the local seed vault (per coinbureau.com security review); keys never leave the device in the software wallet
- **HD derivation:** Solana standard Ed25519 / SLIP-0010 derivation path (Solana uses Ed25519, not secp256k1)
- **Multiple accounts:** Yes — multiple accounts derivable from a single seed phrase; separate seed phrases can be imported for isolation (burner wallet pattern is explicitly documented by Solflare)
- **Passphrase extension:** [NOT FOUND] — no explicit 25th-word BIP39 passphrase UI confirmed in archived sources
- **Watch-only:** [NOT FOUND] — not confirmed in archived sources

## Platforms and Architecture

| Platform | Available | Notes |
|---------|-----------|-------|
| Browser extension | Y | Chrome, Firefox, Edge, Brave |
| Web app | Y | solflare.com — full-featured; distinct from extension |
| iOS | Y | v2.24.0 (30 July 2026); built in Flutter |
| Android | Y | Flutter; 1M+ installs |
| Desktop native | N | — |

The architecture is **hot wallet** for all software configurations. The browser extension, web wallet, and mobile apps all keep signing keys on the internet-connected device. Solflare's own documentation explicitly labels these "hot wallets" in contrast to hardware-backed setups.

## Staking

Solflare's staking interface is its most differentiated feature among Solana wallets.

- **Native SOL staking:** Yes — delegates directly to validators via Solana's stake account system; creates a stake account and assigns delegation
- **Stake accounts:** Separate `stake authority` and `withdrawal authority` per stake account (following Solana protocol)
- **Validator selection UI:** Yes — lists validators with commission rates, APY, and uptime metrics; users select validators manually or by Solflare recommendations
- **Liquid staking:** Yes — integrates with Marinade Finance (mSOL) and Jito (jitoSOL); accessible without leaving the wallet
- **Instant unstake:** Yes — via liquidity providers (Marinade pool etc.); fee applies; faster than the standard deactivation epoch (~2-3 days)
- **Staking APY (network level):** ~7% APY (commonly cited network-level figure; actual varies by validator commission and network conditions)
- **Total SOL staked via Solflare:** [NOT FOUND in primary sources] — earlier marketing figures cited "$14B in SOL staked" but this appears to be total AUM, not specifically staked; coinlaw.io notes institutional staking on Solana grew 300% YoY (network-wide, not Solflare-specific)
- **Hardware-backed staking:** Recommended for significant positions; Ledger/Keystone integration covers staking approval flows

## Hardware Wallet Support

| Device | Support | Connection method |
|--------|---------|------------------|
| Ledger (all Nano/Stax/Flex series) | Y | USB (extension/desktop), Bluetooth (mobile) |
| Keystone | Y | Air-gapped QR code — transaction data passed without USB/Bluetooth |
| Solflare Shield | Y (proprietary) | NFC card; screenless; secure-element chip; 12-word seed; PIN protected |

**Solflare Shield** is Solflare's own proprietary hardware wallet — an NFC card with a secure-element chip, no screen, no battery. Signing confirmation appears on the phone. Priced from $49 USD. Recovery uses a 12-word seed (not 24-word). Shield stores private keys in the secure element; the tradeoff is screenless confirmation (phone screen only, not a trusted hardware display).

- Ledger integration: Solflare acts as the interface; private keys remain on the Ledger device at all times. The hardware wallet's recovery phrase should never be entered into Solflare.
- Keystone integration: QR-code workflow — fully air-gapped; no USB or Bluetooth required.

## NFT Support

- **NFT gallery:** Yes — dedicated "Collectibles" tab with filtering and bulk actions
- **Standards supported:** Metaplex NFT standard (Solana-native); compressed NFTs (cNFTs) — [NOT FOUND explicitly confirmed in primary sources; Metaplex cNFT standard is network-standard and implied by deep Solana integration]
- **NFT marketplace integration:** Magic Eden integration for listing and selling directly from wallet
- **NFT bulk actions:** Yes — batch operations supported in Collectibles tab

## DApp Browser and Connectivity

- **DApp browser (mobile):** Yes — built-in in-app browser connecting to Solana DApps and Solana Pay
- **DApp browser (extension):** Standard browser; extension injects Solflare provider into web pages (same model as MetaMask/Phantom)
- **Wallet standard:** Supports Solana Wallet Standard (the modern wallet adapter protocol replacing legacy `@solana/wallet-adapter`)
- **Solana Pay:** Yes — built-in support; scans QR codes for merchant payments

## Swap Feature

- **Built-in swap:** Yes — token swap aggregation within the wallet
- **Aggregator:** [NOT FOUND explicitly] — likely Jupiter (the dominant Solana DEX aggregator); CoinBureau review references "Serum and Jupiter" in context of Phantom, and notes Solflare as functionally comparable for swaps
- **Swap fee:** [NOT FOUND confirmed primary source]
- **Chains:** Solana tokens only (no cross-chain swap natively)

## Transaction Simulation and Security

### Solflare Guards
Solflare's security system, called "Solflare Guards," provides:
- Pre-transaction simulation: shows expected balance changes (token in/out) before signing
- Scam detection: blocklist of known malicious DApps and wallet drainer contracts
- Phishing warnings: domain-level checks; red flags on known phishing domains
- Delegation alerts: warns when a DApp requests spending delegations or stake authority changes
- Token credibility flags: marks unverified or suspicious tokens

### Audit History

| Component | Auditor | Date | Report status | Findings |
|-----------|---------|------|--------------|----------|
| MetaMask Snap (EVM bridge) | ConsenSys Diligence | August 2023 | Public (referenced; direct URL not cached) | 2 major findings: suppressed signing prompt, prompt injection — both marked fixed. Medium findings: derivation-path validation, public-key confirmation, origin validation — origin partially addressed. |
| Main extension / mobile / web wallet | [NOT FOUND] | — | No public report located | Not established |
| Solflare Shield hardware | [NOT FOUND] | — | No independent audit located | Not established |

**Bug bounty:** As of 1 August 2026, no formal public bug bounty program found on Solflare's website, GitHub, HackerOne, or Immunefi (per coinbureau.com security review, updated August 2026).

### Security Incident History

| Event | Date | Impact | Solflare fault? |
|-------|------|--------|----------------|
| Solana mass wallet drain | August 2022 | ~9,231 wallets drained; ~$4.1M lost | No — attributed to Slope wallet (seed phrase exfiltration to monitoring service); Solflare-created wallets not identified as affected |
| MetaMask Snap security findings | August 2023 | No user losses attributable to these findings | Yes (Snap component) — but findings fixed; losses: none confirmed |

Solflare's own infrastructure has not been compromised. The 2022 Solana drain affected wallets that had been created, imported, or used in Slope; not Solflare.

## Multi-Chain Status

- **Primary:** Solana only (SOL and all SPL tokens)
- **EVM:** Via `solflare-snap` MetaMask Snap — a MetaMask Snaps integration enabling EVM bridging. This is an add-on, not native.
- **Bitcoin, Ethereum, etc.:** Not natively supported
- **Cross-chain comparison:** Phantom expanded to Ethereum, Bitcoin, Polygon, Sui, Base, Monad in 2023–2025. Solflare deliberately stays Solana-first — a strategic differentiation rather than a gap.

## Unique Features

### Magic (AI Assistant)
A new AI wallet assistant (as of 2026, in closed beta). Features:
- Natural-language prompts to execute on-chain actions (swaps, alerts, DeFi strategies)
- Automated trade triggers based on price/market conditions
- Social sentiment and trend analysis on tokens
- "The model is not trained on your data, nor used to train third-party models" (Solflare privacy claim)
- "AI does NOT have access to your private keys" — user must sign each transaction
- Recommended for use with secondary/test wallet in beta

### Solflare Card
A debit card (Mastercard-backed) enabling spending USDC directly from the self-custody wallet:
- Spend $USDC on-chain at 100M+ Mastercard merchants worldwide
- True self-custody debit (on-chain settlement)
- First Solana self-custody debit card

### Private Send
Routes transfers through a privacy aggregator/third-party provider to reduce the direct sender-recipient blockchain link. Amount remains visible. Adds fees, metadata, and compliance risk. Not full anonymity.

## Privacy

- **Self-custody privacy:** Solflare holds no keys; wallet addresses are on public Solana ledger
- **Data collection (per privacy policy):** IP address, browser/device data, clickstream, wallet telemetry, customer support data, product order information
- **Analytics:** Third-party analytics providers may apply separate policies
- **Magic feature:** AI model does not train on user data (Solflare claim); private keys not accessed by AI

## Open-Source Status

Solflare is **partially open source**:
- Public: MetaMask Snap (`solflare-snap`), SDKs (`solflare-sdk`, `utl-sdk`), token lists, blocklist automation, Dart/Flutter library forks
- Not public: Main browser extension code, mobile app source, web wallet code
- No reproducible build process found — cannot independently verify store releases match reviewed code
- No software licence identified on the main app components

## Limitations

1. **Solana-only** (vs Phantom's full multi-chain): limits appeal to users diversified across EVM chains
2. **Partially closed source:** Main extension/mobile/web wallet not fully open source; no reproducible builds
3. **No public bug bounty:** No formal vulnerability disclosure program located (as of August 2026)
4. **Single-chain focus** may hinder growth as users diversify to Ethereum, Bitcoin
5. **Acquisition/independence:** Solflare Inc. remains nominally independent, but the Solana Foundation provides early backing and ongoing support; this creates an incentive alignment question around ecosystem neutrality
6. **Performance:** Some users report occasional mobile lags or delayed chart updates under load
7. **No confirmed BIP39 passphrase (25th word):** Feature not found in reviewed sources
8. **Magic (AI) in beta:** Experimental; recommended with limited-balance secondary wallets only

## Relations and Ecosystem Position

- **Phantom** is the primary competitor; 15–17M MAU vs Solflare's 4M (early 2025)
- **Backpack** is a distant third among Solana wallets (100K+ MAU)
- **Solana Foundation:** Early backer; Solflare is often positioned as the "flagship Solana wallet" for advanced/institutional users
- **DeFi Dev Corp (Nasdaq: DFDV):** Signed LOI (August 2025) to make Solflare the default wallet across their ecosystem and co-create educational content
- **Squads Protocol:** Solflare does not have built-in multisig; external multisig via Squads is the recommended path for institutional users
- **2022 Slope incident resilience:** Solflare's separate architecture meant its users were not exposed to the Slope key-exfiltration bug

## Sources

- [solflare.com](https://solflare.com) — accessed 2026-08-12 — [archived](../sources/2026-08-12-solflare-com-home.html)
- [solflare.com/magic](https://solflare.com/magic) — accessed 2026-08-12 — [archived](../sources/2026-08-12-solflare-com-magic.html)
- [solflare.com/card](https://solflare.com/card) — accessed 2026-08-12 — [archived](../sources/2026-08-12-solflare-com-card.html)
- [coinlaw.io/solflare-wallet-statistics/](https://coinlaw.io/solflare-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-solflare-wallet-statistics.html) — last modified 2026-04-02; author: Barry Elad
- [coinbureau.com — Is Solflare Safe?](https://coinbureau.com/analysis/is-solflare-safe/) — updated August 2026 — [archived](../sources/2026-08-12-coinbureau-com-is-solflare-safe.html)
- [coinbureau.com — Solflare vs Phantom](https://coinbureau.com/analysis/solflare-vs-phantom/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinbureau-com-solflare-vs-phantom.html)
- [GitHub: solflare-wallet org repos](https://api.github.com/orgs/solflare-wallet/repos?per_page=100) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-orgs-solflare-repos-full.json)
- [GitHub: solflare-wallet/solflare-snap](https://api.github.com/repos/solflare-wallet/solflare-snap) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-solflare-wallet-snap.json) — stars: 30; forks: 15
- [iTunes API: com.solflare.mobile](https://itunes.apple.com/lookup?bundleId=com.solflare.mobile) — accessed 2026-08-12 — [archived](../sources/2026-08-12-itunes-apple-com-solflare.json) — v2.24.0; rating 4.84; 9,688 ratings
- [Google Play: com.solflare.mobile](https://play.google.com/store/apps/details?id=com.solflare.mobile) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-solflare.html) — 1M+ installs; 44,480 reviews; rating 4.61
- ConsenSys Diligence MetaMask Snap audit (August 2023) — referenced in coinbureau.com security review and in coinlaw.io — direct report URL not accessible from primary source in this session
