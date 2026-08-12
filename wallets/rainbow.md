---
tags: [wallet, ethereum, evm, mobile, browser-extension]
category: self-custody software wallet
website: https://rainbow.me
docs: https://learn.rainbow.me
support: https://rainbow.me/support
github_mobile: https://github.com/rainbow-me/rainbow
github_extension: https://github.com/rainbow-me/browser-extension
launched: 2019
licence: GPL-3.0
platforms: iOS, Android, Browser Extension (Chrome, Brave, Edge, Firefox, Arc)
ecosystem: Ethereum/EVM
---

# Rainbow Wallet

Rainbow is a consumer-focused, mobile-first, self-custody Ethereum and EVM wallet built by Rainbow Studio Inc. (San Francisco). Originally launched in 2019 as an iOS-only NFT and DeFi wallet, it expanded to Android and a separate browser extension. Its design philosophy emphasises aesthetics, animation, and UX simplicity over power-user features; the codebase is open-source under GPL-3.0. As of 2026, the product has pivoted to position itself as an "any financial market from your phone" app, adding perpetual futures, prediction markets, and a rewards/loyalty system alongside its original Ethereum wallet.

## Adoption / usage metrics

| Metric | Value | Date | Source |
|--------|-------|------|--------|
| GitHub stars (mobile repo) | 4,379 | 2026-08-12 | [GitHub API](https://api.github.com/repos/rainbow-me/rainbow) |
| GitHub forks (mobile repo) | 753 | 2026-08-12 | [GitHub API](https://api.github.com/repos/rainbow-me/rainbow) |
| GitHub stars (browser extension repo) | 194 | 2026-08-12 | [GitHub API](https://api.github.com/repos/rainbow-me/browser-extension) |
| MAU | [NOT FOUND] — no public figure from Rainbow or credible analyst | — | — |
| iOS App Store rating | 4.64 / 5 | 2026-08-12 | [iTunes lookup API](https://itunes.apple.com/search?term=rainbow+ethereum+wallet&country=us&entity=software) |
| iOS App Store review count | 4,199 ratings | 2026-08-12 | [iTunes lookup API](https://itunes.apple.com/search?term=rainbow+ethereum+wallet&country=us&entity=software) |
| iOS bundle ID | me.rainbow | 2026-08-12 | iTunes API |
| Android (Google Play) rating | 4.6 / 5 | 2026-08-12 | [Google Play Store](https://play.google.com/store/apps/details?id=me.rainbow) |
| Android (Google Play) review count | 24,200 | 2026-08-12 | [Google Play Store](https://play.google.com/store/apps/details?id=me.rainbow) |
| Android (Google Play) installs | 500,000+ | 2026-08-12 | [Google Play Store](https://play.google.com/store/apps/details?id=me.rainbow) |
| Chrome extension users | 90,000 | 2026-08-12 | [Chrome Web Store](https://chromewebstore.google.com/detail/rainbow-wallet/opfgelmcmbiajamepnmloijbpoleiama) |
| Chrome extension rating | 4.1 / 5 | 2026-08-12 | [Chrome Web Store](https://chromewebstore.google.com/detail/rainbow-wallet/opfgelmcmbiajamepnmloijbpoleiama) |
| Swap volume (one reported month, mid-2025) | ~USD 5.4 million | 2025 | [coinlaw.io](https://coinlaw.io/rainbow-wallet-statistics/) — self-reported/analyst |
| Swap fee | 0.85% | 2026-08-12 | [coinlaw.io](https://coinlaw.io/rainbow-wallet-statistics/) — community-observed |
| Protocol fee revenue (30-day) | ~USD 176,979 | ~2025 | [coinlaw.io](https://coinlaw.io/rainbow-wallet-statistics/) |
| Protocol fee revenue (annualised estimate) | ~USD 2.16 million | ~2025 | [coinlaw.io](https://coinlaw.io/rainbow-wallet-statistics/) |
| Protocol fee revenue (Q3 2025) | USD 833,625 | Q3 2025 | [coinlaw.io](https://coinlaw.io/rainbow-wallet-statistics/) |
| Cumulative protocol fees (all-time) | >USD 7 million | ~2025 | [coinlaw.io](https://coinlaw.io/rainbow-wallet-statistics/) |
| Wallets using swap (one reported month) | ~2,100 | November (year unspecified) | [coinlaw.io](https://coinlaw.io/rainbow-wallet-statistics/) |
| Total funding raised | ~USD 21 million (4 rounds) | 2026-08-12 | [coinlaw.io](https://coinlaw.io/rainbow-wallet-statistics/) |
| Series A | USD 18 million, led by Seven Seven Six | 2021 | [coinlaw.io](https://coinlaw.io/rainbow-wallet-statistics/) |
| Development cadence | ~14 commits/week, ~304/month | mid-2025 | [coinlaw.io](https://coinlaw.io/rainbow-wallet-statistics/) |

**Note on coinlaw.io figures:** coinlaw.io aggregates swap-fee data sourced from Bankless and on-chain analytics; figures are third-party estimates, not Rainbow self-reported disclosures. Treat as indicative rather than exact.

## How it works

### User perspective

1. **Onboarding:** Download iOS or Android app. Generate a new wallet (BIP39 12-word seed) or import an existing one via seed phrase or private key. Optionally name the wallet with an emoji identifier.
2. **Backup:** Choose iCloud (iOS) or Google Drive (Android) encrypted cloud backup, or manually record the seed phrase. Both can be used simultaneously.
3. **Receiving:** Share address (derived from seed via BIP44 m/44'/60'/0'/0/n path) or ENS name. Same address works across all supported EVM chains.
4. **Sending:** Tap Send, enter recipient address or ENS name, select token and amount. Custom gas editing available (see [[patterns/custom-gas-evm]]).
5. **Swapping:** Tap Swap, choose token pair. RainbowRouter aggregates DEX liquidity (Uniswap v3, SushiSwap, and others); shows gas-inclusive price impact, route, and minimum received before confirmation.
6. **Bridging:** Select cross-chain swap pair; the app routes through bridging infrastructure and delivers to the destination chain.
7. **Connecting to dApps:** Via WalletConnect v2 (scan QR or deep link) or Rainbow's in-app browser (mobile). Browser extension injects `window.ethereum` provider.
8. **NFTs:** Portfolio tab shows ERC-721 and ERC-1155 tokens; users can organise a showcase, hide items, send NFTs, and view offers.
9. **Hardware wallet (mobile):** Tap Add Wallet → Connect hardware wallet → connect Ledger Nano X, Stax, or Flex via Bluetooth. Signing occurs on the device.
10. **Hardware wallet (extension):** Add from hardware wallet → select Ledger (USB: Nano S Plus, Nano X, Stax) or Trezor (web interface). Trezor not supported on Firefox.

### System perspective

- **Key storage (mobile):** `react-native-keychain` wraps iOS Secure Enclave / Android Keystore; private keys never leave the device keychain in plaintext.
- **Swap routing:** `@rainbow-me/swaps` package calls the RainbowRouter aggregator contract (`0x00000000009726632680FB29d3F7A9734E3010E2` on mainnet, deployed on Arbitrum, Optimism, Polygon). Contract queries multiple DEXs and returns best quote net of gas.
- **WalletConnect:** Uses `@walletconnect/core` and `@reown/walletkit` (WalletConnect v2 protocol); dApp session is E2E encrypted via Noise protocol.
- **ENS:** Uses `@ensdomains/eth-ens-namehash` and `@ensdomains/address-encoder` for reverse resolution and multi-chain address encoding. ENS names resolve to on-chain registry.
- **Notifications:** Firebase Messaging (`@react-native-firebase/messaging`) for push notifications on transaction confirmations and price alerts.
- **Cloud backup:** Seed phrase encrypted with AES before uploading to iCloud / Google Drive; encryption key is device-specific (Secure Enclave / Android Keystore). Rainbow servers never receive the plaintext seed.
- **Browser extension:** Separate codebase at `rainbow-me/browser-extension`; TypeScript; also GPL-3.0. Injects `window.ethereum`; connects via keyboard-navigable Cmd+K/Ctrl+K command palette.

## Key behaviours

- [[patterns/walletconnect-integration]] — WalletConnect v2 as primary dApp connection protocol on mobile and extension
- [[patterns/cloud-seed-backup]] — iCloud/Google Drive encrypted seed backup as primary recovery path
- [[patterns/dex-aggregation-evm]] — RainbowRouter contract aggregates DEX liquidity for best swap price
- [[patterns/ledger-ble-mobile]] — Ledger hardware wallet connected via Bluetooth on mobile
- [[patterns/ens-as-identity]] — ENS name as primary user identity across dApps; in-app ENS registration and profile editing

## Architecture decisions

- **React Native for mobile:** Enables iOS/Android from a single codebase; TypeScript throughout. Consistent with UX-first philosophy.
- **Separate extension codebase:** Browser extension (`rainbow-me/browser-extension`) is a distinct product, not a wrapper around the mobile app. Allows extension-specific UX (keyboard-driven, Cmd+K) without mobile constraints.
- **GPL-3.0 licence:** Copyleft licence requires derivative works to remain open-source. Intentionally stronger than MIT; limits commercial forks without contribution.
- **No Trezor on mobile:** Trezor requires a web bridge (WebUSB) that does not translate to a native mobile app; only Ledger (BLE) is supported on mobile.
- **Firebase for notifications:** Pragmatic choice for push delivery; introduces a dependency on Google services infrastructure.
- **RNBW token and staking:** Loyalty programme built on-chain. Swap fee cashback distributed as staked RNBW, compounding automatically. Four tiers (Silver 5K, Gold 10K, Diamond 15K, Black 20K RNBW staked). Unstaking incurs a 10% exit fee redistributed to remaining stakers.
- **EVM-only scope:** No native Bitcoin, Solana, or non-EVM chain support as of 2026-08-12. All supported networks are EVM-compatible.

## Supported networks

As of 2026-08-12 (from official support page), Rainbow supports 23 EVM-compatible networks:

Ethereum mainnet, Base, Optimism, Arbitrum, Zora, Blast, ApeChain, Polygon, Avalanche, Degen Chain, BNB Smart Chain, Ink, Berachain, Gravity, Unichain, Sonic, Linea, Abstract, WorldChain, Scroll, Gnosis, Katana, ZkSync, HyperEVM.

## Key management

| Aspect | Detail |
|--------|--------|
| Seed format | BIP39 12-word mnemonic (English wordlist) |
| Derivation path | BIP44 m/44'/60'/0'/0/n (standard Ethereum) |
| Multiple accounts | Y — multiple wallets per seed; multiple independent seeds supported |
| Wallet groups | Y — wallet groups explained in-app; each group has its own seed |
| Watch-only | Y — import address to view balance without exposing keys |
| Private key import | Y — import single private key |
| Passphrase (BIP39 25th word) | [NOT FOUND] — not mentioned in support docs |
| Keychain storage | react-native-keychain → iOS Secure Enclave / Android Keystore |

## Signing

| Standard | Support |
|----------|---------|
| EIP-191 (personal_sign) | Y |
| EIP-712 (typed data) | Y — confirmed via WalletConnect dApp integration |
| EIP-1559 transactions | Y |
| Custom gas | Y — gas editing UI confirmed in support docs |
| Blind signing (Ledger) | Y — required for Ledger hardware wallet on mobile |

## Backup and recovery

- **iCloud backup (iOS):** Encrypted AES backup to user's iCloud account. Opt-in at setup; can be enabled later via Settings → Wallets & Backup. Green status indicator when up-to-date. Source: [support page](https://rainbow.me/support/app/the-importance-of-backups)
- **Google Drive backup (Android):** Equivalent cloud backup path.
- **Manual seed phrase:** Export 12-word seed via Settings → Wallets & Backup. Multiple independent seed groups require separate exports.
- **Phone Swap:** Support article explains migration flow between devices using cloud backup.
- **Recommendation:** Rainbow docs advise using both cloud backup AND manual seed phrase for redundancy.

## Fiat on-ramp

Three providers integrated in-app: Ramp (2.49–4.9% fee, global, instant), MoonPay (2–4.89% fee, global), Coinbase Pay (0.5–2.5% fee, global). Supported networks for on-ramp: Ethereum mainnet, Optimism, Arbitrum, Polygon, BSC, Base. Payment methods: Apple Pay, ACH, debit, credit, Coinbase balance. Source: [learn.rainbow.me](https://learn.rainbow.me/buying-crypto-with-rainbow)

## DeFi and product expansion (2024–2026)

The homepage as of 2026-08-12 positions Rainbow as a trading platform beyond crypto wallets:

- **Perps:** Perpetual futures up to 50x leverage on BTC, S&P 500, Gold, Oil (long/short). Powered by Hyperliquid.
- **Predictions:** Polymarket-integrated prediction markets (sports, elections, rate cuts).
- **Live Data:** Real-time price charts within app.
- **RNBW rewards:** Every trade earns RNBW tokens; Rainbow Black tier gives 100% fee cashback and "free instant deposits".

## Privacy

- **No PII required:** No email, name, or KYC required to create or use a wallet. Source: [privacy policy](https://rainbow.me/privacy), [is-rainbow-wallet-safe](https://rainbow.me/support/safety/is-rainbow-wallet-safe)
- **Telemetry:** App collects error/log data (IP address, device name, OS version, app config, timestamps) via third-party analytics on crashes. Source: [privacy policy](https://rainbow.me/privacy) (dated May 2020 — may be outdated)
- **coinlaw.io note:** "The wallet adds telemetry and diagnostics in updates" — third-party observation. Exact scope unknown.
- **Default RPC:** Uses Infura (ConsenSys) by default, per README setup instructions. Custom RPC endpoints not confirmed in support docs.
- **No Tor support:** [NOT FOUND] — no evidence of SOCKS5 proxy or Tor routing.

## Differentiators vs MetaMask and Rabby

| Aspect | Rainbow | MetaMask | Rabby |
|--------|---------|---------|-------|
| Primary platform | Mobile-first (iOS/Android) | Browser extension-first | Browser extension |
| Open-source | Y (GPL-3.0) | N (ConsenSys proprietary licence) | Y |
| NFT gallery | First-class, showcase curation | Toggle display only | Basic |
| ENS integration | In-app registration + profile editing | Display only | Display only |
| Swap fee | 0.85% | 0.875% | [NF] |
| Hardware wallet (mobile) | Ledger via Bluetooth | N (extension only) | N (extension only) |
| Hardware wallet (extension) | Ledger + Trezor | Ledger + Trezor + Lattice1 + Keystone | Ledger + Trezor + OneKey + GridPlus |
| Loyalty/rewards | RNBW staking, tiered cashback | N | N |
| Transaction simulation | [NOT FOUND] | Y (Blockaid PPOM) | Y (built-in) |
| MetaMask Snaps compatibility | N | Y | N |
| Reproducible builds | [NOT FOUND] | N | [NF] |

See [[metrics/wallet-features]] for the full feature comparison table.

## Limitations and criticisms

- **Smaller user base:** With ~2,100 wallets actively using swaps in a reported month, Rainbow's actual active user base is substantially smaller than MetaMask (30M MAU) or Trust Wallet (17M+ MAU). Fee revenue (~USD 2.16M annualised) is a small fraction of MetaMask or Phantom.
- **Android distribution gaps:** coinlaw.io notes some listings showed only "1K" Android installs, attributing this to distribution gaps or analytics issues. Google Play shows 500K+ which is the verified figure.
- **EVM-only scope:** No Bitcoin, Solana, or Monero support. Users who want multi-ecosystem coverage must use a separate wallet.
- **Mobile-first limits extension:** Browser extension is a secondary product; as of August 2026, Trezor support on Firefox is disabled in the extension.
- **Privacy policy outdated:** Published May 2020; does not reflect current telemetry or analytics practices. Third-party observers note diagnostic data collection in recent versions.
- **No transaction simulation (confirmed):** Unlike Rabby's built-in transaction simulator or MetaMask's Blockaid PPOM, no equivalent feature confirmed in Rainbow's support documentation. [NOT FOUND]
- **Perps pivot risk:** The 2025–2026 pivot toward prediction markets and perpetual futures trading represents a significant product direction change that may dilute the wallet's original self-custody focus.
- **Swap fee cashback model:** RNBW staking has a 10% exit fee distributed to remaining stakers — a mechanism that benefits long-term holders but penalises users who need liquidity.
- **App stability (anecdotal):** Community reports of crashes in Android secure folders and NFT display failures in large collections. Official release notes include regular UX and NFT rendering bug fixes.
- **No BIP39 passphrase:** 25th-word extension not documented; users who rely on passphrase-based security cannot use Rainbow for that purpose.

## Sources

- [GitHub API: rainbow-me/rainbow](https://api.github.com/repos/rainbow-me/rainbow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-rainbow-me-rainbow.json)
- [GitHub API: rainbow-me/browser-extension](https://api.github.com/repos/rainbow-me/browser-extension) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-rainbow-me-browser-extension.json)
- [GitHub README: rainbow-me/rainbow](https://raw.githubusercontent.com/rainbow-me/rainbow/develop/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-rainbow-me-rainbow-README.md)
- [package.json: rainbow-me/rainbow](https://raw.githubusercontent.com/rainbow-me/rainbow/develop/package.json) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-rainbow-me-rainbow-package-json.json)
- [rainbow.me homepage](https://rainbow.me) — accessed 2026-08-12 — [screenshot archived](../sources/2026-08-12-rainbow-me-homepage.png)
- [rainbow.me/privacy](https://rainbow.me/privacy) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-privacy.html)
- [Rainbow support: Supported Networks](https://rainbow.me/support/app/supported-networks) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-app-supported-networks.html)
- [Rainbow support: Backups](https://rainbow.me/support/app/the-importance-of-backups) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-app-backups.html)
- [Rainbow support: Ledger (mobile)](https://rainbow.me/support/app/add-a-wallet-from-ledger-to-rainbow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-app-ledger.html)
- [Rainbow support: Hardware Wallet (extension)](https://rainbow.me/support/extension/connect-your-hardware-wallet) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-extension-hardware-wallet.html)
- [Rainbow support: Is Rainbow Wallet Safe?](https://rainbow.me/support/safety/is-rainbow-wallet-safe) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-safety-is-safe.html)
- [Rainbow support: Staking RNBW](https://rainbow.me/support/app/staking-rnbw) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-staking-rnbw.html)
- [Rainbow support: Perps on Rainbow](https://rainbow.me/support/app/perps-on-rainbow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-perps.html)
- [learn.rainbow.me: Buying Crypto with Rainbow](https://learn.rainbow.me/buying-crypto-with-rainbow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-learn-rainbow-me-buying-crypto.html)
- [learn.rainbow.me: Cross Chain Swaps and Bridging](https://learn.rainbow.me/cross-chain-swaps-and-bridging-with-rainbow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-learn-rainbow-me-cross-chain-swaps.html)
- [learn.rainbow.me: Swap with Confidence](https://learn.rainbow.me/swap-with-confidence-with-rainbow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-learn-rainbow-me-swap-with-confidence.html)
- [iTunes App Store lookup](https://itunes.apple.com/search?term=rainbow+ethereum+wallet&country=us&entity=software) — accessed 2026-08-12 — [archived](../sources/2026-08-12-itunes-apple-com-search-rainbow-wallet.json)
- [Google Play Store: me.rainbow](https://play.google.com/store/apps/details?id=me.rainbow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-rainbow-wallet.html)
- [Chrome Web Store: Rainbow Wallet](https://chromewebstore.google.com/detail/rainbow-wallet/opfgelmcmbiajamepnmloijbpoleiama) — accessed 2026-08-12 — [archived](../sources/2026-08-12-chromewebstore-rainbow-wallet.html)
- [coinlaw.io: Rainbow Wallet Statistics 2026](https://coinlaw.io/rainbow-wallet-statistics/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinlaw-io-rainbow-wallet-statistics.html) — third-party analyst; figures not independently verified against on-chain primary sources
- [GitHub releases (latest 5)](https://api.github.com/repos/rainbow-me/rainbow/releases?per_page=5) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-rainbow-me-rainbow-releases.json)
