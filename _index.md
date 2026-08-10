# Research Index: Self-Custody Software Wallets

**Domain definition:** A self-custody software wallet is a non-custodial application — desktop, mobile, or command-line — that generates and holds private keys entirely under the user's control, with no third-party key custody. This index covers pure software wallets only: browser extensions with no embedded custodian, native desktop clients, and mobile apps that store keys on-device. Excluded: hardware wallets (Ledger, Trezor, Coldcard), custodial exchange wallets (Binance, Kraken), and browser extensions that proxy key operations through a centralised server. The five ecosystems covered are Monero (XMR), Bitcoin (BTC), Ethereum (ETH/EVM), Solana (SOL), and Zcash (ZEC).

**Primary metric:** Monthly Active Users (MAU) where reported by the project or credible third-party analytics (e.g. CoinLaw, SQ Magazine, DappRadar). Where MAU is unavailable (privacy coins, smaller wallets), GitHub stars are used as a proxy for developer and community adoption, supplemented by download counts where obtainable. Rationale: MAU most directly reflects real-world usage relevant to an RFP; GitHub stars are a weak but comparable proxy when MAU is not disclosed. On-chain active-address counts are structurally unavailable for Monero (privacy) and partially obscured for Zcash (shielded pool). All [NOT FOUND] entries indicate no credible public figure was located on 2026-08-10.

**Discovery sources:**
- CoinLaw wallet statistics pages (https://coinlaw.io) — accessed 2026-08-10
- SQ Magazine statistics (https://sqmagazine.co.uk) — accessed 2026-08-10
- Monero.how wallet guide (https://www.monero.how/best-monero-wallets) — accessed 2026-08-10
- Coin Bureau Zcash wallet review (https://coinbureau.com/analysis/best-zcash-wallets) — accessed 2026-08-10
- Bitcoin Magazine self-custody guide (https://bitcoinmagazine.com/business/top-self-custody-bitcoin-wallets-for-2026) — accessed 2026-08-10
- GitHub API (https://api.github.com/repos/<owner>/<repo>) — star counts fetched 2026-08-10
- PrivacyTools.io Cake Wallet review (https://privacytools.io/app/cake-wallet) — accessed 2026-08-10
- Crypto.news Zcash shielded pool article (https://cryptonews.net/news/analytics/32936591/) — accessed 2026-08-10
- CoinDesk Zcash shielded transaction surge reporting — referenced in search 2026-08-10

---

## Monero (XMR) Wallets

**Ecosystem context:** Monero's privacy architecture (RingCT, stealth addresses) means on-chain active-address counts are fundamentally unavailable — the network is designed to prevent address tracing. Network-level data: ~27,959 daily transactions (December 2025), ~18,000 daily active addresses estimated (proxy figure). MAU by wallet is not publicly disclosed by any Monero wallet project. GitHub stars are the primary proxy.

| Rank | Name | Platform | Primary metric | Source | Selected |
|------|------|----------|----------------|--------|----------|
| 1 | Cake Wallet | iOS, Android, macOS, Linux | 1,835 GitHub stars (cake-tech/cake_wallet) | https://api.github.com/repos/cake-tech/cake_wallet (2026-08-10) | ✓ |
| 2 | Monero GUI | Windows, macOS, Linux | 2,284 GitHub stars (monero-project/monero-gui) | https://api.github.com/repos/monero-project/monero-gui (2026-08-10) | ✓ |
| 3 | Feather Wallet | Windows, macOS, Linux, Tails | 572 GitHub stars (feather-wallet/feather) | https://api.github.com/repos/feather-wallet/feather (2026-08-10) | ✓ |
| 4 | Monerujo | Android | 688 GitHub stars (m2049r/xmrwallet) | https://api.github.com/repos/m2049r/xmrwallet (2026-08-10) | ✓ |
| 5 | Stack Wallet | iOS, Android | [NOT FOUND] GitHub stars for fluffypony/stack_wallet; listed as candidate on monero.how | https://www.monero.how/best-monero-wallets (2026-08-10) | — |
| — | MyMonero | iOS, Android, Desktop | Deprecated — project directed users to migrate to Cake Wallet by Feb 2026 | https://www.monero.how/best-monero-wallets (2026-08-10) | — |

**Note on ranking:** Monero GUI has more stars than Cake Wallet but those stars partly reflect its status as the official reference implementation (maintained by the Monero Project core team) rather than necessarily greater end-user adoption. Cake Wallet is consistently cited as the most-used wallet for daily mobile use in 2026. Stars-based ranking is approximate.

### Shortlist rationale — Monero

- **Cake Wallet:** The dominant Monero mobile wallet in 2026, available on iOS, Android, macOS, and Linux. Open-source (MIT). Supports BTC-to-XMR atomic-style swaps built in. Covers mobile and desktop with a single seed. Highest community recommendation frequency.
- **Monero GUI:** Official reference implementation maintained by the Monero Project. Supports full node (maximum privacy) or remote node. The de-facto standard for users who want the full validation stack. Desktop only.
- **Feather Wallet:** Lightweight desktop client with Tor built in, cold-signing support, and compatibility with Tails/Qubes OS. Favoured by power users who want privacy-hardened desktop operation without running a full node. Represents the "privacy-maximalist desktop" architectural category.
- **Monerujo:** The only major Monero-native Android wallet besides Cake Wallet, open-source (Apache-2.0). Represents the Android-only / lightweight mobile category distinct from Cake Wallet (which is cross-platform Flutter).

---

## Bitcoin (BTC) Wallets

**Ecosystem context:** Bitcoin on-chain active addresses are publicly traceable. However, no Bitcoin wallet project publishes wallet-attributed MAU; the figures below are from wallet-reported or analyst-estimated data. Electrum is Bitcoin-only and desktop-first; BlueWallet and Phoenix are mobile-first; Sparrow is privacy/power-user desktop.

| Rank | Name | Platform | Primary metric | Source | Selected |
|------|------|----------|----------------|--------|----------|
| 1 | Electrum | Windows, macOS, Linux, Android | 8,556 GitHub stars; 1M+ Android downloads | https://api.github.com/repos/spesmilo/electrum (2026-08-10); https://www.appbrain.com/app/electrum-bitcoin-wallet/org.electrum.electrum (2026-08-10) | ✓ |
| 2 | BlueWallet | iOS, Android | 3,264 GitHub stars (bluewallet/bluewallet); v8.0.0 released June 2026 | https://api.github.com/repos/bluewallet/bluewallet (2026-08-10) | ✓ |
| 3 | Sparrow Wallet | Windows, macOS, Linux | 2,083 GitHub stars (sparrowwallet/sparrow) | https://api.github.com/repos/sparrowwallet/sparrow (2026-08-10) | ✓ |
| 4 | Phoenix | iOS, Android | 855 GitHub stars (ACINQ/phoenix); ACINQ is a Lightning Network pioneer | https://api.github.com/repos/ACINQ/phoenix (2026-08-10) | ✓ |
| 5 | Cake Wallet (BTC) | iOS, Android, macOS, Linux | See Monero row; also supports BTC with Silent Payments + Payjoin | https://bitcoinmagazine.com/business/top-self-custody-bitcoin-wallets-for-2026 (2026-08-10) | — |
| 6 | Zeus | iOS, Android | [NOT FOUND] MAU; Lightning-node self-custody focus | https://bitcoinmagazine.com/business/top-self-custody-bitcoin-wallets-for-2026 (2026-08-10) | — |
| 7 | Muun | iOS, Android | [NOT FOUND] MAU; self-custodial hybrid on-chain/Lightning | https://www.spark.money/tools/lightning-wallet-comparison (2026-08-10) | — |

### Shortlist rationale — Bitcoin

- **Electrum:** The longest-tenured Bitcoin software wallet (2011), still the most-starred Bitcoin-only wallet on GitHub (8,556 stars). Desktop-first with Android app. Hardware wallet compatible. SPV-based lightweight architecture (representative of the "server-backed lightweight" category). 1M+ Android downloads is the best available proxy for install base.
- **BlueWallet:** Most popular open-source mobile Bitcoin wallet with Lightning support (3,264 stars). Represents the "mobile-first, Lightning-enabled" category. v8.0.0 shipped June 2026 indicates active maintenance.
- **Sparrow Wallet:** The go-to desktop wallet for privacy and UTXO management — CoinJoin (PayJoin), PSBT, hardware wallet coordination, full-node or Electrum-server connectivity. 2,083 stars. Represents the "privacy/power-user desktop" architectural category.
- **Phoenix:** The leading self-custodial Lightning wallet (855 stars, ACINQ — the Lightning Network infrastructure company). Represents the "Lightning-native mobile" category; fully self-custodial channel management without a custodial Lightning provider.

---

## Ethereum (ETH/EVM) Wallets

**Ecosystem context:** MetaMask dominates with 30M+ reported MAU. Trust Wallet reports 60M MAU across all chains but is primarily a multi-chain wallet; its ETH-specific share is not broken out. All figures are self-reported or analyst-estimated, not on-chain wallet attribution.

| Rank | Name | Platform | Primary metric | Source | Selected |
|------|------|----------|----------------|--------|----------|
| 1 | MetaMask | Browser extension, iOS, Android | 30M+ MAU (April 2026); 143M lifetime downloads | https://coinlaw.io/metamask-wallet-statistics/ (2026-08-10) | ✓ |
| 2 | Trust Wallet | iOS, Android | 60M MAU (early 2025, all chains); 220M+ total downloads | https://coinlaw.io/trust-wallet-statistics/ (2026-08-10) | ✓ |
| 3 | Rabby | Browser extension, iOS, Android, Desktop | 4.2M installs (2025); 1,872 GitHub stars | https://coinlaw.io/rabby-wallet-statistics/ (2026-08-10); https://api.github.com/repos/RabbyHub/Rabby (2026-08-10) | ✓ |
| 4 | Rainbow | iOS, Android, Browser extension | 4,379 GitHub stars; MAU [NOT FOUND] | https://api.github.com/repos/rainbow-me/rainbow (2026-08-10) | ✓ |
| 5 | Coinbase Wallet | Browser extension, iOS, Android | 3.2M MAU (2025); 15M+ installs | https://coinlaw.io/coinbase-wallet-statistics/ (2026-08-10) | — |
| 6 | Frame | Windows, macOS, Linux (system-level proxy) | 1,199 GitHub stars; MAU [NOT FOUND] | https://api.github.com/repos/floating/frame (2026-08-10) | — |

### Shortlist rationale — Ethereum

- **MetaMask:** The Ethereum wallet with the largest verifiable MAU (30M+, April 2026). Browser extension + mobile. Open-source (13,195 stars on metamask-extension). Represents the "browser extension DeFi" category and is the de-facto baseline for EVM compatibility.
- **Trust Wallet:** Largest self-custody multi-chain wallet by download volume (220M+ total, 60M MAU). Mobile-first. Supports 100+ chains. Represents the "mobile multi-chain" category.
- **Rabby:** Fastest-growing EVM wallet in 2025–2026; 4.2M installs; transaction simulation and risk-warning features. Browser extension + mobile + desktop. Represents the "power-user EVM" category distinct from MetaMask's generalist approach.
- **Rainbow:** Most-starred mobile-first Ethereum wallet (4,379 stars) with a mobile + browser-extension presence. Represents the "design-first mobile" architectural category. Self-custodial, open-source.

---

## Solana (SOL) Wallets

**Ecosystem context:** Solana wallet market share is better documented than other ecosystems due to on-chain activity attribution. Phantom is clearly dominant at 39.4% of Solana wallets (early 2025) with 15–17M MAU.

| Rank | Name | Platform | Primary metric | Source | Selected |
|------|------|----------|----------------|--------|----------|
| 1 | Phantom | Browser extension, iOS, Android | 15–17M MAU (2025 peak); 39.4% Solana wallet market share; 24M mobile downloads | https://coinlaw.io/phantom-wallet-statistics/ (2026-08-10) | ✓ |
| 2 | Solflare | Browser extension, iOS, Android | 4M MAU (Jan 2026); $14B SOL staked | https://coinlaw.io/solflare-wallet-statistics/ (2026-08-10) | ✓ |
| 3 | Backpack | Browser extension, iOS, Android | 100,000+ MAU; 1,650 GitHub stars (coral-xyz/backpack) | https://solanacompass.com/projects/Backpack (2026-08-10); https://api.github.com/repos/coral-xyz/backpack (2026-08-10) | ✓ |
| 4 | Phantom (also covers Ethereum/BTC/Base) | Multi-chain extension | Also supports ETH, Bitcoin, Base, Polygon, Sui, Hyperliquid, Monad | https://coinlaw.io/phantom-wallet-statistics/ (2026-08-10) | — |

**Note on Phantom open-source status:** Phantom's core wallet is not open-source; only the Connect SDK is public (https://github.com/phantom/phantom-connect-sdk). This is a material difference from Backpack (coral-xyz/backpack, open-source but carries an "unaudited" disclaimer) and Solflare (partially open-source SDK).

### Shortlist rationale — Solana

- **Phantom:** Dominant Solana wallet by every available metric — 15–17M MAU, 39.4% Solana market share, 24M mobile downloads, $325M FY2025 protocol revenue. Represents the "mainstream multi-chain" category.
- **Solflare:** Second-largest Solana wallet (4M MAU); strong staking tooling with $14B SOL staked. Partnered with Mastercard for a self-custody debit card (November 2025). Represents the "staking/validator power-user" category.
- **Backpack:** Power-user DeFi wallet from Solana veteran Armani Ferrante; open-source (coral-xyz/backpack). Connected to the Backpack Exchange (Dubai VARA-licensed). Represents the "open-source DeFi-native" category distinct from Phantom's partially closed architecture.

---

## Zcash (ZEC) Wallets

**Ecosystem context:** Zcash has ~8,500 public transparent transactions per day (2026); shielded transactions reached 59.3% of all transactions in February 2026 (up from ~30% at start of 2025), driven largely by Zodl's default routing to the Orchard shielded pool. Wallet-level MAU is not publicly disclosed. GitHub stars are the primary proxy; the ecosystem is small (all repos under 100 stars except for the core zcash node).

Major January–February 2026 event: the core wallet development team split from Electric Coin Company (ECC) and formed the Zcash Open Development Lab (ZODL), rebranding the Zashi wallet to Zodl and raising $25M from Paradigm/a16z.

| Rank | Name | Platform | Primary metric | Source | Selected |
|------|------|----------|----------------|--------|----------|
| 1 | Zodl (formerly Zashi) | iOS, Android | 94 GitHub stars (zodl-inc/zodl-android); driver of shielded pool growth to 59.3% in Feb 2026 | https://api.github.com/repos/zodl-inc/zodl-android (2026-08-10); https://cryptonews.net/news/analytics/32936591/ (2026-08-10) | ✓ |
| 2 | YWallet | iOS, Android, Desktop | 62 GitHub stars (hhanh00/zwallet — superseded by zkool2, 34 stars) | https://api.github.com/repos/hhanh00/zwallet (2026-08-10) | ✓ |
| 3 | Nighthawk Wallet | iOS, Android | v2 at nighthawk-apps/nighthawk-android-wallet; old repo: 32 stars; new repo star count [NOT FOUND] | https://github.com/nighthawk-apps (2026-08-10) | ✓ |
| 4 | ZecWallet Lite | Windows, macOS, Linux | Deprecated — no Orchard support shipped; legacy Sapling flows only | https://coinbureau.com/analysis/best-zcash-wallets (2026-08-10) | — |
| 5 | Zashi (ECC — original, pre-rebrand) | iOS, Android | 90 GitHub stars (Electric-Coin-Company/zashi-ios); superseded by Zodl | https://api.github.com/repos/Electric-Coin-Company/zashi-ios (2026-08-10) | — |
| 6 | ZkOol2 | Desktop | 34 GitHub stars — successor to YWallet by same author (hhanh00) | https://api.github.com/repos/hhanh00/zkool2 (2026-08-10) | — |

### Shortlist rationale — Zcash

- **Zodl (formerly Zashi):** The most consequential Zcash wallet by on-chain impact — its Unified Address default routing is the documented driver of the shielded-pool surge to 59.3% of transactions (February 2026). Backed by the team that built Zashi under ECC; now independent with $25M funding. Shielded-by-default mobile wallet.
- **YWallet:** The power-user choice for multi-pool, multi-account, and advanced Zcash operations. The author (hhanh00) is also developing the successor Zkool2; YWallet receives security fixes only. Selected as the most-capable existing alternative to Zodl with a different architectural approach (desktop + advanced key management).
- **Nighthawk Wallet:** Community-built shielded-by-default mobile wallet with an independent development team; v2 at nighthawk-apps org. Represents the "community-independent mobile" category distinct from Zodl's ZODL-Lab origin. F-Droid distribution (degoogled Android support).

---

## Gaps and Uncertainties

1. **Monero wallet MAU is structurally unavailable.** No Monero wallet discloses MAU; GitHub stars are used as a proxy. The ranking within the Monero table is approximate and directional only. Cake Wallet vs Monero GUI star counts may not reflect actual end-user adoption ratios.

2. **Phantom is not open-source.** The core wallet application is closed-source. This is a material consideration for an RFP evaluating self-custody software wallets. Only Phantom's Connect SDK is public.

3. **Trust Wallet MAU figures are inconsistent.** Conflicting reports of 17M vs 60M MAU circulate; both may reflect different measurement methodologies (crypto-only MAU vs all-asset MAU). Both figures are noted: 17M (EarthWeb, 2025) and 60M (SQ Magazine, early 2025). The Trust Wallet wallet-core GitHub repo (3,600 stars) is separate from the app itself (no dedicated public app repo).

4. **Zcash wallet ecosystem is small and in transition.** The Zashi → Zodl rebrand (February 2026) and the ECC/ZODL split create uncertainty about long-term maintenance of legacy ECC-branded repos. YWallet is entering maintenance-only mode. The ecosystem has no wallet with >100 GitHub stars.

5. **Solflare GitHub repositories are fragmented.** Solflare's main wallet app has no single prominent public source repo (the `solflare-wallet` org has 40 repositories). Star count for the primary wallet is [NOT FOUND] from the org overview; only the Solana Snap repo (30 stars) and SDK repo were found.

6. **BlueWallet Lightning.** BlueWallet discontinued its own custodial Lightning hub in 2023. Users must self-host LNDhub or connect to a third-party hub. This architectural shift affects how "self-custodial" the Lightning functionality is; this distinction should be surfaced in deep research.

7. **Frame wallet.** Frame (floating/frame, 1,199 stars) is a notable Ethereum desktop system-level proxy wallet (EVM-native, hardware-wallet integrated, privacy-focused) that did not make the ETH shortlist but should be considered as an alternative in deep research if hardware-wallet coordination or Linux desktop support is a priority.

8. **Zkool2 (YWallet successor).** Only 34 stars as of 2026-08-10 but it is the active development path from the YWallet author. If the RFP cares about Zcash desktop power-users, Zkool2 warrants monitoring.

---

## Sources Archive

All sources archived to `/workspace/extra/marclawclaw/research-wallet/sources/` on 2026-08-10.

| File | URL |
|------|-----|
| `2026-08-10-coinlaw-io-phantom-wallet-statistics.html` | https://coinlaw.io/phantom-wallet-statistics/ |
| `2026-08-10-coinlaw-io-metamask-wallet-statistics.html` | https://coinlaw.io/metamask-wallet-statistics/ |
| `2026-08-10-coinlaw-io-solflare-wallet-statistics.html` | https://coinlaw.io/solflare-wallet-statistics/ |
| `2026-08-10-coinlaw-io-rabby-wallet-statistics.html` | https://coinlaw.io/rabby-wallet-statistics/ |
| `2026-08-10-coinlaw-io-coinbase-wallet-statistics.html` | https://coinlaw.io/coinbase-wallet-statistics/ |
| `2026-08-10-sqmagazine-co-uk-monero-statistics.html` | https://sqmagazine.co.uk/monero-statistics/ |
| `2026-08-10-monero-how-best-wallets.html` | https://www.monero.how/best-monero-wallets |
| `2026-08-10-coinbureau-com-best-zcash-wallets.html` | https://coinbureau.com/analysis/best-zcash-wallets |
| `2026-08-10-bitcoinmagazine-com-top-self-custody-bitcoin-wallets-2026.html` | https://bitcoinmagazine.com/business/top-self-custody-bitcoin-wallets-for-2026 |
| `2026-08-10-crypto-news-net-zcash-shielded-pool-30pct.html` | https://cryptonews.net/news/analytics/32936591/ |
| `2026-08-10-privacytools-io-cake-wallet-review.html` | https://privacytools.io/app/cake-wallet |
| `2026-08-10-github-com-cake-tech-cake-wallet.json` | https://api.github.com/repos/cake-tech/cake_wallet |
| `2026-08-10-github-com-feather-wallet-feather.json` | https://api.github.com/repos/feather-wallet/feather |
| `2026-08-10-github-com-monero-project-monero-gui.json` | https://api.github.com/repos/monero-project/monero-gui |
| `2026-08-10-github-com-m2049r-xmrwallet.json` | https://api.github.com/repos/m2049r/xmrwallet |
| `2026-08-10-github-com-spesmilo-electrum.json` | https://api.github.com/repos/spesmilo/electrum |
| `2026-08-10-github-com-bluewallet-bluewallet.json` | https://api.github.com/repos/bluewallet/bluewallet |
| `2026-08-10-github-com-sparrowwallet-sparrow.json` | https://api.github.com/repos/sparrowwallet/sparrow |
| `2026-08-10-github-com-acinq-phoenix.json` | https://api.github.com/repos/ACINQ/phoenix |
| `2026-08-10-github-com-metamask-metamask-extension.json` | https://api.github.com/repos/MetaMask/metamask-extension |
| `2026-08-10-github-com-rabby-hub-rabby.json` | https://api.github.com/repos/RabbyHub/Rabby |
| `2026-08-10-github-com-rainbow-me-rainbow.json` | https://api.github.com/repos/rainbow-me/rainbow |
| `2026-08-10-github-com-floating-frame.json` | https://api.github.com/repos/floating/frame |
| `2026-08-10-github-com-coral-xyz-backpack.json` | https://api.github.com/repos/coral-xyz/backpack |
| `2026-08-10-github-com-zodl-inc-zodl-android.json` | https://api.github.com/repos/zodl-inc/zodl-android |
| `2026-08-10-github-com-hhanh00-zwallet.json` | https://api.github.com/repos/hhanh00/zwallet |
| `2026-08-10-github-com-hhanh00-zkool2.json` | https://api.github.com/repos/hhanh00/zkool2 |
| `2026-08-10-github-com-ecc-zashi-ios.json` | https://api.github.com/repos/Electric-Coin-Company/zashi-ios |
