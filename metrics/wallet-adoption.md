# Cross-Wallet Adoption Metrics

Cross-wallet adoption comparison for self-custody software wallets. All figures retrieved 2026-08-10.

**Methodology notes:**
- MAU = Monthly Active Users, where self-reported or from credible analyst sources.
- GitHub stars = proxy metric where MAU unavailable.
- Download counts from app stores or AppBrain.
- [NOT FOUND] = no credible public figure located on 2026-08-10.
- All figures are point-in-time; wallet markets fluctuate significantly.

## Bitcoin wallets

| Wallet | GitHub stars | Forks | Contributors | MAU | Android installs | iOS installs | Date |
|--------|-------------|-------|--------------|-----|-----------------|--------------|------|
| Electrum | 8,561 | 3,463 | ~350 | [NOT FOUND] | 1M+ | N/A (no iOS app) | 2026-08-10 |
| BlueWallet | 3,268 | 1,045 | ~100 (top-100 API page) | [NOT FOUND] | [NOT FOUND] | [NOT FOUND] | 2026-08-12 |
| Sparrow Wallet | 2,090 | 312 | 32 | [NOT FOUND] — desktop only, no app-store analytics; GitHub downloads: ~1.33M across 20 recent releases (Apr 2024–Jul 2026); v2.5.3: 56,279; v2.5.2: 101,039; v2.3.1: 110,776 | N/A (desktop only) | N/A (desktop only) | 2026-08-12 |
| Phoenix | 857 | 143 | 38 | [NOT FOUND] — no self-reported MAU; Google Play: 100,000+ installs (164,112 precise from structured data), 867 reviews, 4.27★ rating | 100,000+ (Google Play) | [NOT FOUND] — removed from US App Store May 2024; global iOS store stats not accessible | 2026-08-12 |

**Sources:**
- Electrum: [GitHub API](https://api.github.com/repos/spesmilo/electrum) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-api-spesmilo-electrum.json)
- BlueWallet: [GitHub API](https://api.github.com/repos/bluewallet/bluewallet) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-bluewallet-bluewallet.json); forks/contributors from same API call; MAU and install counts [NOT FOUND]
- Sparrow: [GitHub API](https://api.github.com/repos/sparrowwallet/sparrow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-sparrowwallet-sparrow.json); [releases API](https://api.github.com/repos/sparrowwallet/sparrow/releases?per_page=20) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-sparrowwallet-sparrow-releases.json)
- Phoenix: [GitHub API](https://api.github.com/repos/ACINQ/phoenix) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenix.json); [GitHub releases API (10 most recent)](https://api.github.com/repos/ACINQ/phoenix/releases?per_page=10) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenix-releases.json); [Google Play Store listing](https://play.google.com/store/apps/details?id=fr.acinq.phoenix.mainnet) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-phoenix-mainnet.html) (install count: 100,000+/164,112 from structured data; rating: 4.27; reviews: 867)
- Electrum Android installs: _index.md citing AppBrain (credential required for live data); electrum.org notes "available on Google Play" with 1M+ badge

## Ethereum/EVM wallets

| Wallet | GitHub stars | MAU | Downloads/installs | Date |
|--------|-------------|-----|--------------------|------|
| MetaMask | 13,195 (metamask-extension); 3,008 (metamask-mobile) | 30M+ (plateau since March 2022; reconfirmed December 2025 Bitcoin Magazine; reconfirmed April 2026 BlockEden IPO analysis) | 143M lifetime downloads | 2026-08-12 |
| Trust Wallet | [NOT FOUND] (no single main repo) | 17M+ (2025, coinlaw.io) / 60M (SQ Magazine, early 2025 — figure not verified against archive) | 220M+ total | 2026-08-10 |
| Rabby | 1,878 (extension); 77 (mobile) | [NOT FOUND] — no self-reported MAU; ~1.4M current installs across Chrome (~800K), Google Play (~466.8K), iOS (~90K); self-reported "4.2M" (2025 milestone, all-time cumulative); Chrome: 4.0★; Play: 3.9★; iOS: 3.4★ (~1,200 ratings) | ~1.4M (2026 live store count); 4.2M (2025 self-reported all-time) | 2026-08-12 |
| Rainbow | 4,379 | [NOT FOUND] | [NOT FOUND] | 2026-08-10 |

**Sources:**
- MetaMask: [coinlaw.io/metamask-wallet-statistics/](https://coinlaw.io/metamask-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-metamask-wallet-statistics.html). Article last updated 2026-07-30. Key figures: 30M MAU (reconfirmed December 2025 Bitcoin Magazine, April 2026 BlockEden); $198.64M cumulative swap revenue; 143M lifetime downloads; 0.875% swap fee; CoinGecko 2026 #2 hot wallet. Mobile repo stars (3,008) from [GitHub API](https://api.github.com/repos/MetaMask/metamask-mobile) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-metamask-metamask-mobile.json).
- Trust Wallet: 17M+ MAU per [coinlaw.io/trust-wallet-statistics/](https://coinlaw.io/trust-wallet-statistics/) — accessed 2026-08-10 — [archived (MetaMask page as cross-reference)](../sources/2026-08-10-coinlaw-io-metamask-wallet-statistics.html); 60M figure per sqmagazine.co.uk — [archive unavailable]; conflicting figures recorded, neither independently verified against primary Trust Wallet disclosure
- Rabby: [coinlaw.io/rabby-wallet-statistics/](https://coinlaw.io/rabby-wallet-statistics/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinlaw-io-rabby-wallet-statistics.html) (last updated 20 July 2026; install breakdown: Chrome 800K, Play 466.8K, iOS 90K; ratings: Chrome 4.0, Play 3.9, iOS 3.4/1,200 ratings; 4.2M 2025 milestone self-reported); [GitHub API: RabbyHub/Rabby](https://api.github.com/repos/RabbyHub/Rabby) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-rabbyhub-rabby.json) (stars: 1,878); [GitHub API: RabbyHub/rabby-mobile](https://api.github.com/repos/RabbyHub/rabby-mobile) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-rabbyhub-rabby-mobile.json) (stars: 77)
- Rainbow: [GitHub API](https://api.github.com/repos/rainbow-me/rainbow) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-rainbow-me-rainbow.json)

## Solana wallets

| Wallet | GitHub stars | MAU | Market share | Date |
|--------|-------------|-----|--------------|------|
| Phantom | [NOT FOUND] (closed source) | 15–17M (2025 peak) | 39.4% Solana wallets | 2026-08-10 |
| Solflare | [NOT FOUND] | 4M (Jan 2026) | [NOT FOUND] | 2026-08-10 |
| Backpack | 1,650 | 100,000+ | [NOT FOUND] | 2026-08-10 |

**Sources:**
- Phantom: [coinlaw.io/phantom-wallet-statistics/](https://coinlaw.io/phantom-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-phantom-wallet-statistics.html)
- Solflare: [coinlaw.io/solflare-wallet-statistics/](https://coinlaw.io/solflare-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-solflare-wallet-statistics.html)
- Backpack: [GitHub API](https://api.github.com/repos/coral-xyz/backpack) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-coral-xyz-backpack.json)

## Monero wallets

| Wallet | GitHub stars | MAU | Date |
|--------|-------------|-----|------|
| Cake Wallet | 1,838 | [NOT FOUND] — structurally unavailable (Monero privacy); self-reported "1,750,000+ users" (unverified, all-time cumulative) | 2026-08-12 |
| Monero GUI | 2,285 | [NOT FOUND] — structurally unavailable (Monero privacy); Flathub (Linux only): 101,837 total installs, 2,135 last 30 days, 398 last 7 days | 2026-08-12 |
| Feather Wallet | 573 | [NOT FOUND] — structurally unavailable (Monero privacy); v2.8.1 release: 1,643 x64 AppImage downloads, 2,135 ARM64 AppImage downloads, 1,237 ARM32 AppImage downloads (GitHub release asset counters; partial proxy only) | 2026-08-12 |
| Monerujo | 688 | [NOT FOUND] — structurally unavailable (Monero privacy); Google Play tier: 100,000+; Play rating: 3.19/5 (1,176 ratings); latest GitHub APK (v4.1.7): 10,642 direct downloads | 2026-08-12 |

**Sources:**
- Cake Wallet: GitHub API — see _index.md and archived `.json` files in sources/ — accessed 2026-08-12
- Monero GUI: [GitHub API](https://api.github.com/repos/monero-project/monero-gui) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-api-monero-project-monero-gui.json); [Flathub API](https://flathub.org/api/v2/stats/org.getmonero.Monero) — accessed 2026-08-12 — [archived](../sources/2026-08-12-flathub-api-monero-gui-stats.json)
- Feather Wallet: [GitHub API](https://api.github.com/repos/feather-wallet/feather) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather.json); release asset download counts from [GitHub releases API (v2.8.1)](https://api.github.com/repos/feather-wallet/feather/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather-releases-latest.json)
- Monerujo: [GitHub API](https://api.github.com/repos/m2049r/xmrwallet) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet.json); [Google Play structured data](https://play.google.com/store/apps/details?id=com.m2049r.xmrwallet) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-monerujo.html); [GitHub releases API](https://api.github.com/repos/m2049r/xmrwallet/releases) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet-releases.json)

## Zcash wallets

| Wallet | GitHub stars | MAU | Date |
|--------|-------------|-----|------|
| Zodl (formerly Zashi) | 94 (zodl-android) | [NOT FOUND] | 2026-08-10 |
| YWallet | 62 (zwallet) | [NOT FOUND] | 2026-08-10 |
| Nighthawk | [NOT FOUND] | [NOT FOUND] | 2026-08-10 |

**Sources:** GitHub API — see _index.md and archived `.json` files in sources/.
