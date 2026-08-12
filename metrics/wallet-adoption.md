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
| BlueWallet | 3,264 | [NOT FOUND] | [NOT FOUND] | [NOT FOUND] | [NOT FOUND] | [NOT FOUND] | 2026-08-10 |
| Sparrow Wallet | 2,083 | [NOT FOUND] | [NOT FOUND] | [NOT FOUND] | N/A (desktop only) | N/A (desktop only) | 2026-08-10 |
| Phoenix | 855 | [NOT FOUND] | [NOT FOUND] | [NOT FOUND] | [NOT FOUND] | [NOT FOUND] | 2026-08-10 |

**Sources:**
- Electrum: [GitHub API](https://api.github.com/repos/spesmilo/electrum) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-api-spesmilo-electrum.json)
- BlueWallet: [GitHub API](https://api.github.com/repos/bluewallet/bluewallet) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-bluewallet-bluewallet.json)
- Sparrow: [GitHub API](https://api.github.com/repos/sparrowwallet/sparrow) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-sparrowwallet-sparrow.json)
- Phoenix: [GitHub API](https://api.github.com/repos/ACINQ/phoenix) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-acinq-phoenix.json)
- Electrum Android installs: _index.md citing AppBrain (credential required for live data); electrum.org notes "available on Google Play" with 1M+ badge

## Ethereum/EVM wallets

| Wallet | GitHub stars | MAU | Downloads/installs | Date |
|--------|-------------|-----|--------------------|------|
| MetaMask | 13,195 (metamask-extension) | 30M+ (April 2026) | 143M lifetime | 2026-08-10 |
| Trust Wallet | [NOT FOUND] (no single main repo) | 17M+ (2025, coinlaw.io) / 60M (SQ Magazine, early 2025 — figure not verified against archive) | 220M+ total | 2026-08-10 |
| Rabby | 1,872 | [NOT FOUND] | 4.2M installs (2025) | 2026-08-10 |
| Rainbow | 4,379 | [NOT FOUND] | [NOT FOUND] | 2026-08-10 |

**Sources:**
- MetaMask: [coinlaw.io/metamask-wallet-statistics/](https://coinlaw.io/metamask-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-metamask-wallet-statistics.html)
- Trust Wallet: 17M+ MAU per [coinlaw.io/trust-wallet-statistics/](https://coinlaw.io/trust-wallet-statistics/) — accessed 2026-08-10 — [archived (MetaMask page as cross-reference)](../sources/2026-08-10-coinlaw-io-metamask-wallet-statistics.html); 60M figure per sqmagazine.co.uk — [archive unavailable]; conflicting figures recorded, neither independently verified against primary Trust Wallet disclosure
- Rabby: [coinlaw.io/rabby-wallet-statistics/](https://coinlaw.io/rabby-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-rabby-wallet-statistics.html)
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
| Monerujo | 688 | [NOT FOUND] | 2026-08-10 |

**Sources:**
- Cake Wallet: GitHub API — see _index.md and archived `.json` files in sources/ — accessed 2026-08-12
- Monero GUI: [GitHub API](https://api.github.com/repos/monero-project/monero-gui) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-api-monero-project-monero-gui.json); [Flathub API](https://flathub.org/api/v2/stats/org.getmonero.Monero) — accessed 2026-08-12 — [archived](../sources/2026-08-12-flathub-api-monero-gui-stats.json)
- Feather Wallet: [GitHub API](https://api.github.com/repos/feather-wallet/feather) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather.json); release asset download counts from [GitHub releases API (v2.8.1)](https://api.github.com/repos/feather-wallet/feather/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather-releases-latest.json)
- Monerujo: GitHub API — see _index.md and archived `.json` files in sources/ — accessed 2026-08-10

## Zcash wallets

| Wallet | GitHub stars | MAU | Date |
|--------|-------------|-----|------|
| Zodl (formerly Zashi) | 94 (zodl-android) | [NOT FOUND] | 2026-08-10 |
| YWallet | 62 (zwallet) | [NOT FOUND] | 2026-08-10 |
| Nighthawk | [NOT FOUND] | [NOT FOUND] | 2026-08-10 |

**Sources:** GitHub API — see _index.md and archived `.json` files in sources/.
