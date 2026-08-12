# Cross-Wallet Feature Comparison

Feature comparison for self-custody software wallets across Bitcoin, Ethereum/EVM, Solana, Monero, and Zcash ecosystems. All figures current as of 2026-08-10; Rainbow column updated 2026-08-12.

**Key:** Y = Yes / Confirmed; N = No / Not supported; P = Partial / Plugin; — = Not applicable; [NF] = Not Found / unconfirmed

## Bitcoin wallets

| Feature | Electrum | BlueWallet | Sparrow Wallet | Phoenix |
|---------|----------|-----------|---------------|---------|
| **Platforms** | Windows, macOS, Linux, Android | iOS, Android | Windows, macOS, Linux | iOS, Android |
| **License** | MIT | MIT | Apache-2.0 | Apache-2.0 |
| **Architecture** | SPV (Electrum server) | SPV / Electrum server | Full node / Electrum server | Lightning-native |
| **Language** | Python | React Native | Java | Kotlin |
| **Latest version** | 4.8.1 (August 2026) | 8.0.1 (21 July 2026) | 2.5.3 (30 July 2026) | 2.8.1 (18 June 2026) |
| **Key management** | BIP32 HD; Electrum native seed or BIP39 import | BIP32 HD; BIP39 | BIP32 HD; BIP39; BIP39 passphrase; SLIP39 import; BIP47 (PayNym); xpub watch-only; output descriptor | BIP32 HD; BIP39 (12-word); on-chain: BIP84 path (m/84'/0'/0'); Lightning keys via lightning-kmp |
| **Seed format** | Electrum native (version-embedded) or BIP39 | BIP39 | BIP39 (12/24-word); SLIP39 import | BIP39 (12-word English) |
| **Passphrase (BIP39 extension)** | Y | Y | Y | [NF] — not mentioned in FAQ or release notes |
| **Watch-only wallet** | Y | Y | Y | N — Lightning-native; no xpub import |
| **Hardware wallet** | Y (Trezor, Ledger, KeepKey, BitBox02, Coldcard, Jade, Safe-T, Bitbox01) | P (Ledger via BLE) | Y — USB: Trezor, Ledger, BitBox02, Jade/Jade Plus, ERA; Airgapped QR: Passport, SeedSigner, Keystone, Krux, Jade, Specter DIY, Seed Tool; Airgapped SD: Coldcard; Airgapped NFC: Tapsigner, Satochip, Keycard | N |
| **PSBT** | Y | Y | Y | N |
| **Multisig** | Y (native m-of-n; P2SH and P2WSH) | Y (native m-of-n; P2WSH, P2SH-P2WSH, P2SH) | Y (native, all script types) | N |
| **Air-gapped signing** | Y (file, QR code, audio modem plugin) | Y (PSBT file, BC-UR v2 QR — v8.0.1) | Y (QR, microSD, NFC) | N |
| **Lightning Network** | Y (built-in, trampoline via ACINQ) | Y (LNDhub, self-custodial requires own hub) | N | Y — native self-custodial; single-channel model; trampoline routing via ACINQ node; BOLT11, BOLT12 offers, LNURL-pay, LNURL-withdraw, BIP353 |
| **Submarine swaps** | Y (on-chain ↔ Lightning) | Y (via Arkade/Boltz — v8.0.0+) | N | Y — splice-in (on-chain→LN) and splice-out (LN→on-chain); both trustless since v2.0.0; no external swap service |
| **Script types — P2PKH** | Y | Y | Y | N |
| **Script types — P2SH** | Y | Y (P2SH-P2WPKH wrapped SegWit; P2SH multisig) | Y | N |
| **Script types — P2WPKH (bech32)** | Y | Y | Y | Y — on-chain recovery via BIP84 path |
| **Script types — P2WSH** | Y (multisig) | Y (multisig via MultisigHDWallet) | Y | N |
| **Script types — P2TR (Taproot receive)** | N (can send to P2TR; cannot create P2TR wallet) | Y (HDTaprootWallet, BIP86, m/86'/0'/0') | Y (from v1.4.3, July 2021; full signing including script-path) | Y — default on-chain receive address since v2.2.0 (Feb 2024); taproot Lightning channels since v2.7.0 (Oct 2025) |
| **Coin control (UTXO selection)** | Y | Y (documented feature; fetchUtxo/getUtxo in source) | Y | N |
| **RBF (Replace-By-Fee)** | Y (default) | Y (BIP68 sequence 0x80000000; watch-only bech32 added v8.0.1) | Y | — |
| **CPFP** | Y | Y (documented in official support) | Y | — |
| **CoinJoin** | N | N | P — PayJoin (BIP78): Y; Whirlpool (Samourai): REMOVED v1.9.0 (Apr 2024, post-Samourai arrests); PayJoin v2 (BIP77): [NF] | N |
| **Tor support** | Y (SOCKS5 proxy to .onion servers) | N (not found in source inspection 2026-08-12) | Y (built-in) | N |
| **Privacy-preserving routing** | Y (Tor, manual server) | P (custom Electrum server only; no Tor) | Y (Tor, self-hosted full node) | P — trampoline via ACINQ node (ACINQ sees all payment metadata); Tor requires external Orbot VPN app since v2.5.0; custom Electrum server supported |
| **Fee estimation** | Y (dynamic; median of ~10 servers) | Y | Y | Y |
| **Payment batching** | Y | Y (Send to Many / batch documented in official support) | Y | — |
| **2FA** | Y (TrustedCoin plugin, 2-of-3) | N | N | N |
| **Open-source** | Y (MIT) | Y (MIT) | Y (Apache-2.0) | Y (Apache-2.0) |
| **Reproducible builds** | Y (5 independent signers) | P (APK + IPA released with detached .sig; full reproducible-build attestation [NOT FOUND]) | P — .tar.gz and .zip reproducible from v1.5.0; .deb/.rpm/.msi/.dmg have minor variances; bitcoinbinary.org attestations v1.5.0–v1.6.6 only; more recent releases [NF] at bitcoinbinary.org | [NF] — GPG-signed SHA256SUMS.asc published per release (key E04E48E72C205463); no full reproducible build process documented |
| **F-Droid** | Y | Y | N | Y |
| **Command-line / daemon** | Y (full RPC/JSON-RPC) | N | N | N |
| **Plugin system** | Y (hardware wallets, 2FA, NWC, watchtower, etc.) | N | P (limited) | N |
| **Silent Payments (BIP352)** | N | N | Y — sending v2.3.0 (Oct 2025); receiving wallets v2.5.0 (May 2026); HW support via BIP375 v2.4.0 | N |
| **Wallet encryption** | PBKDF2 | [NF] | Argon2 (≥500ms key derivation on modern hardware) | [NF] — mobile OS keystore used; no explicit encryption scheme documented |
| **Transaction graph explorer** | N | N | Y (built-in; all inputs/outputs hyperlinked to coinbase) | N |
| **Terminal / headless mode** | Y (full RPC/JSON-RPC) | N | Y (Sparrow Terminal TUI from v1.7.0; Sparrow Server headless builds) | N |

**Sources for Electrum column:**
- [electrum.org](https://electrum.org) — [archived](../sources/2026-08-10-electrum-org-home.html)
- [readthedocs](https://electrum.readthedocs.io) — [archived](../sources/2026-08-10-electrum-readthedocs-io-latest.html)
- [RELEASE-NOTES](https://raw.githubusercontent.com/spesmilo/electrum/master/RELEASE-NOTES) — [archived](../sources/2026-08-10-github-com-spesmilo-electrum-RELEASE-NOTES.txt)
- [electrum/bitcoin.py — WIF_SCRIPT_TYPES](https://github.com/spesmilo/electrum/blob/master/electrum/bitcoin.py) — source inspection
- [electrum/plugins/ directory](https://api.github.com/repos/spesmilo/electrum/contents/electrum/plugins) — source inspection

**Sources for BlueWallet column (updated 2026-08-12):**
- GitHub API: https://api.github.com/repos/bluewallet/bluewallet — [archived](../sources/2026-08-12-api-github-com-repos-bluewallet-bluewallet.json)
- v8.0.1 release notes: https://api.github.com/repos/bluewallet/bluewallet/releases/latest — [archived](../sources/2026-08-12-api-github-com-repos-bluewallet-bluewallet-releases-latest.json)
- Source inspection: class/wallets/ directory — hd-taproot-wallet.ts, multisig-hd-wallet.ts, lightning-custodian-wallet.ts, abstract-hd-electrum-wallet.ts — accessed 2026-08-12
- BlueWallet FAQ: https://github.com/BlueWallet/BlueWallet/blob/master/FAQ.md — accessed 2026-08-12
- Official support docs: https://bluewallet.io/lndhub/ — [archived](../sources/2026-08-12-bluewallet-io-lndhub.html)

**Sources for Sparrow Wallet column (updated 2026-08-12):**
- [GitHub API: sparrowwallet/sparrow](https://api.github.com/repos/sparrowwallet/sparrow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-sparrowwallet-sparrow.json)
- [GitHub releases (20 most recent)](https://api.github.com/repos/sparrowwallet/sparrow/releases?per_page=20) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-sparrowwallet-sparrow-releases.json)
- [sparrowwallet.com/features/](https://sparrowwallet.com/features/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-features.html)
- [sparrowwallet.com/docs/faq.html](https://sparrowwallet.com/docs/faq.html) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-docs-faq.html)
- [sparrowwallet.com/download/](https://sparrowwallet.com/download/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-download.html)
- [GitHub README](https://raw.githubusercontent.com/sparrowwallet/sparrow/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-com-sparrowwallet-sparrow-README.md)

**Sources for Phoenix column (updated 2026-08-12):**
- [GitHub API: ACINQ/phoenix](https://api.github.com/repos/ACINQ/phoenix) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenix.json)
- [GitHub releases (20 most recent)](https://api.github.com/repos/ACINQ/phoenix/releases?per_page=20) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenix-releases-20.json)
- [GitHub README](https://raw.githubusercontent.com/ACINQ/phoenix/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-com-acinq-phoenix-README.md)
- [Phoenix FAQ (full text)](https://phoenix.acinq.co/faq) — accessed 2026-08-12 — [archived](../sources/2026-08-12-phoenix-acinq-co-faq-full.txt)
- [Splicing blog post](https://acinq.co/blog/phoenix-splicing-update) — accessed 2026-08-12 — [archived](../sources/2026-08-12-acinq-co-blog-phoenix-splicing-update.txt)
- [PhoenixD GitHub API](https://api.github.com/repos/ACINQ/phoenixd) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenixd.json)

## Ethereum/EVM wallets

| Feature | MetaMask | Trust Wallet | Rabby | Rainbow |
|---------|---------|-------------|-------|---------|
| **Platforms** | Browser ext (Chrome, Firefox, Brave, Edge), iOS, Android | iOS, Android | Browser ext, iOS, Android, Desktop | iOS, Android, Browser ext (Chrome, Brave, Edge, Firefox, Arc) |
| **License** | Source-available, NOT open-source (ConsenSys proprietary; extension and mobile both; confirmed 2026-08-12 LICENSE inspection) | Apache-2.0 (wallet-core) | [NF] | GPL-3.0 (mobile + browser extension; confirmed via GitHub API 2026-08-12) |
| **Architecture** | Browser extension: MV3 service worker + injected `window.ethereum` content script; Mobile: React Native WebView + WalletConnect v2 | Mobile app | Browser-injected provider | React Native (iOS/Android); separate TypeScript browser extension codebase (`rainbow-me/browser-extension`); `window.ethereum` injection via extension |
| **Latest version** | Extension: v13.43.0 (11 Aug 2026); Mobile: v8.6.0 (7 Aug 2026) | [NF] | [NF] | v2.0.42 (11 Aug 2026) |
| **Language** | TypeScript | [NF] | TypeScript | TypeScript (React Native; confirmed via GitHub API language field 2026-08-12) |
| **Key management** | BIP39 SRP (12-word default, 24-word import); BIP44 m/44'/60'/0'/0/n; hardware accounts signed on device; imported single private keys; Bitcoin via Snap (BIP84 path) | [NF] | [NF] | BIP39 12-word mnemonic; BIP44 m/44'/60'/0'/0/n; multiple wallets per seed; multiple independent seeds; private key import; watch-only |
| **Seed format** | BIP39 12-word (default); 24-word import supported | [NF] | [NF] | BIP39 12-word English (standard Ethereum derivation) |
| **Passphrase (BIP39 extension)** | N — no 25th-word support in standard UI | [NF] | [NF] | [NF] — 25th-word passphrase not mentioned in support docs |
| **Hardware wallet** | Y — Ledger (WebHID/USB + Bluetooth); Trezor (WebUSB via TrezorConnect); Lattice1/GridPlus (gridplus-sdk); QR-based (Keystone via BC-UR v2). Confirmed via package.json `@ledgerhq/hw-transport-webhid`, `@trezor/connect-web`, `gridplus-sdk`, `eth-lattice-keyring` — accessed 2026-08-12 | [NF] | Y (Ledger, Trezor, OneKey, GridPlus) | Y — Mobile: Ledger Nano X/Stax/Flex via Bluetooth (`@ledgerhq/react-native-hw-transport-ble`); Extension: Ledger (USB) + Trezor (web interface; not on Firefox). No Keystone/airgap support. No Trezor on mobile. |
| **Multi-chain** | Y — EVM chains only by default; Bitcoin via `@metamask/bitcoin-wallet-snap` (Dec 2025); non-EVM chains possible via Snaps | Y (100+ chains) | Y (EVM chains) | Y — 23+ EVM-compatible chains (Ethereum mainnet, Base, Optimism, Arbitrum, Zora, Blast, Polygon, Avalanche, BNB Smart Chain, Linea, Scroll, Gnosis, ZkSync, Berachain, Sonic, Abstract, WorldChain, Unichain, Gravity, Degen Chain, Ink, ApeChain, Katana, HyperEVM — as at 2026-08-12); EVM-only; no Bitcoin, Solana, or Monero |
| **Signing** | EIP-191 (personal_sign), EIP-712 typed data (eth_signTypedData_v4), EIP-7702 authorisation, EIP-5792 batch; eth_sign disabled by default | [NF] | [NF] | EIP-191 (personal_sign), EIP-712 typed data, EIP-1559 transactions; custom gas editing confirmed |
| **Transaction simulation** | Y — Blockaid PPOM (`@blockaid/ppom_release` v1.5.3); on-device, no tx data sent remotely; detects drainers, phishing, approval exploits | [NF] | [NF] | [NF] — no transaction simulation feature found in support docs (unlike Rabby built-in or MetaMask Blockaid) |
| **MetaMask Snaps** | Y — extensibility platform; sandboxed JS; non-EVM key derivation; custom account types; 3rd-party audit required for key-deriving Snaps | N | N | N |
| **Built-in swap** | Y — MetaMask Swaps; 0.875% fee; aggregates 1inch, 0x, Paraswap; plus Perpetual Futures via Hyperliquid (Oct 2025) | [NF] | [NF] | Y — RainbowRouter DEX aggregator (Uniswap v3, SushiSwap, others); 0.85% fee; cross-chain bridging; perps via Hyperliquid (50x, BTC/S&P500/Gold/Oil) |
| **Smart accounts (ERC-4337)** | P — via Delegation Toolkit / Snaps; not in base UI; requires external dapp or `@metamask/gator-permissions-snap` | [NF] | [NF] | [NF] — no ERC-4337 smart account support found in docs |
| **Default RPC** | Infura (ConsenSys-owned; logs IP and queries by default) | [NF] | [NF] | Infura (ConsenSys; confirmed via README setup instructions) |
| **Custom RPC** | Y — any Ethereum-compatible endpoint per-network | [NF] | [NF] | [NF] — custom RPC not confirmed in support docs |
| **Tor support** | N — no SOCKS5/Tor proxy; no onion service | [NF] | [NF] | N — no Tor or SOCKS5 proxy support found |
| **Biometric unlock (mobile)** | Y — Touch ID / Face ID (iOS), fingerprint / face (Android); key wrapped in platform keychain | [NF] | [NF] | Y — `react-native-keychain` wraps iOS Secure Enclave / Android Keystore; biometric gate at app open |
| **NFT support** | Y — ERC-721, ERC-1155 display (toggle) | [NF] | [NF] | Y — ERC-721, ERC-1155; first-class showcase curation; NFT send; offer display; hide/unhide; large-collection rendering (known bug for very large collections) |
| **Reproducible builds** | N — no documented reproducible build process or multi-party attestation | [NF] | [NF] | [NF] — no reproducible build process or multi-party attestation documented |
| **Watch-only wallet** | P — hardware wallet accounts are effectively watch-only (no private key in extension); true watch-only xpub import: [NF] | [NF] | [NF] | Y — import address to view balance; confirmed in support docs |
| **Multisig** | N (native); smart-contract multisig via external Safe integration possible | [NF] | [NF] | N — no native multisig; no ERC-4337 smart account multisig in base UI |
| **Coin control** | N | [NF] | [NF] | N — EVM-native; no UTXO model; no manual output selection |
| **Air-gapped signing** | P — QR-based hardware wallets (Keystone via BC-UR v2); not a first-class air-gap workflow | [NF] | [NF] | N — no QR hardware wallet or airgap workflow; Ledger BLE only (mobile), Ledger/Trezor USB only (extension) |
| **Open-source** | N — source-available only (ConsenSys proprietary licence) | Y (wallet-core SDK) | Y | Y — GPL-3.0 (both mobile and browser extension repos) |
| **Security audit** | Y (component audits: Cure53, Least Authority, Diligence, OtterSec — see wallets/metamask.md) | [NF] | [NF] | [NF] — no public security audit report found |
| **Loyalty / rewards** | N | N | N | Y — RNBW token staking; 4 tiers (Silver 5K, Gold 10K, Diamond 15K, Black 20K RNBW staked); 25–100% swap fee cashback; 10% unstaking exit fee redistributed to stakers |
| **Backup method** | iCloud / Google Drive encrypted (mobile) | [NF] | [NF] | iCloud (iOS) or Google Drive (Android) encrypted AES + manual seed phrase; both recommended simultaneously; cloud key stored in device Secure Enclave / Keystore |
| **Browser extension launched** | 2019 | [NF] | [NF] | ~Sep 2022 (repo created 2022-09-29; confirmed via GitHub API) |
| **Fiat on-ramp** | [NF] | [NF] | [NF] | Y — Ramp (2.49–4.9%), MoonPay (2–4.89%), Coinbase Pay (0.5–2.5%); Apple Pay, ACH, debit, credit; ETH mainnet, Optimism, Arbitrum, Polygon, BSC, Base |

**Sources for Rainbow column (updated 2026-08-12):**
- [GitHub API: rainbow-me/rainbow](https://api.github.com/repos/rainbow-me/rainbow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-rainbow-me-rainbow.json)
- [GitHub API: rainbow-me/browser-extension](https://api.github.com/repos/rainbow-me/browser-extension) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-rainbow-me-browser-extension.json)
- [GitHub releases (latest 5)](https://api.github.com/repos/rainbow-me/rainbow/releases?per_page=5) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-rainbow-me-rainbow-releases.json)
- [package.json: rainbow-me/rainbow](https://raw.githubusercontent.com/rainbow-me/rainbow/develop/package.json) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-rainbow-me-rainbow-package-json.json)
- [Rainbow support: Supported Networks](https://rainbow.me/support/app/supported-networks) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-app-supported-networks.html)
- [Rainbow support: Backups](https://rainbow.me/support/app/the-importance-of-backups) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-app-backups.html)
- [Rainbow support: Ledger (mobile)](https://rainbow.me/support/app/add-a-wallet-from-ledger-to-rainbow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-app-ledger.html)
- [Rainbow support: Hardware Wallet (extension)](https://rainbow.me/support/extension/connect-your-hardware-wallet) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-extension-hardware-wallet.html)
- [Rainbow support: Staking RNBW](https://rainbow.me/support/app/staking-rnbw) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-support-staking-rnbw.html)
- [learn.rainbow.me: Buying Crypto with Rainbow](https://learn.rainbow.me/buying-crypto-with-rainbow) — accessed 2026-08-12 — [archived](../sources/2026-08-12-learn-rainbow-me-buying-crypto.html)
- [coinlaw.io: Rainbow Wallet Statistics 2026](https://coinlaw.io/rainbow-wallet-statistics/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinlaw-io-rainbow-wallet-statistics.html)
- [rainbow.me/privacy](https://rainbow.me/privacy) — accessed 2026-08-12 — [archived](../sources/2026-08-12-rainbow-me-privacy.html)

**Sources for MetaMask column (updated 2026-08-12):**
- [GitHub API: MetaMask/metamask-extension](https://api.github.com/repos/MetaMask/metamask-extension) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-metamask-metamask-extension.json)
- [GitHub API: MetaMask/metamask-mobile](https://api.github.com/repos/MetaMask/metamask-mobile) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-metamask-metamask-mobile.json)
- [GitHub releases/latest: extension v13.43.0](https://api.github.com/repos/MetaMask/metamask-extension/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-metamask-extension-releases-latest.json)
- [GitHub releases/latest: mobile v8.6.0](https://api.github.com/repos/MetaMask/metamask-mobile/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-metamask-mobile-releases-latest.json)
- [coinlaw.io/metamask-wallet-statistics/](https://coinlaw.io/metamask-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-metamask-wallet-statistics.html)
- [package.json dependency inspection: metamask-extension main](https://raw.githubusercontent.com/MetaMask/metamask-extension/main/package.json) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-metamask-extension-package-json.txt)
- [MetaMask extension LICENSE](https://raw.githubusercontent.com/MetaMask/metamask-extension/main/LICENSE) — accessed 2026-08-12
- [MetaMask mobile LICENSE](https://raw.githubusercontent.com/MetaMask/metamask-mobile/main/LICENSE) — accessed 2026-08-12
- [MetaMask docs: sign data (EIP-712, personal_sign)](https://docs.metamask.io/wallet/how-to/sign-data/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-metamask-io-sign-data.html)
- [MetaMask docs: Snaps introduction](https://docs.metamask.io/snaps/learn/about-snaps/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-metamask-io-about-snaps.html)
- [MetaMask extension README](https://raw.githubusercontent.com/MetaMask/metamask-extension/main/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-metamask-extension-README.md)

## Solana wallets

| Feature | Phantom | Solflare | Backpack |
|---------|---------|---------|---------|
| **Open-source** | N (Connect SDK only) | P | Y |
| **Hardware wallet** | Y (Ledger) | Y (Ledger) | [NF] |
| **Multi-chain** | Y (ETH, BTC, Base, Solana, Sui, Polygon, Hyperliquid, Monad) | P | Y |
| **Staking** | Y | Y ($14B SOL staked) | Y |

## Monero wallets

| Feature | Cake Wallet | Monero GUI | Feather Wallet | Monerujo |
|---------|------------|-----------|---------------|---------|
| **Platforms** | iOS, Android, macOS, Linux | Windows, macOS, Linux | Windows, macOS, Linux, Tails, Whonix, Qubes | Android |
| **License** | MIT | BSD-3-Clause | BSD-3-Clause | Apache-2.0 |
| **Architecture** | Flutter / remote node | C++/Qt/QML — full local node (advanced mode) or remote node | C++/Qt — remote node only | Java + C++ (Monero Core NDK) / remote node |
| **Latest version** | v6.4.0 (27 July 2026) | v0.18.5.2 "Fluorine Fermi" (21 July 2026) | v2.8.1 (14 April 2025) | v4.1.7 "Exolix" (17 June 2025) |
| **Full node** | N (remote node) | Y (monerod bundled; optional bootstrap mode) | N (remote node) | N (remote node) |
| **Tor** | Y (opt-in; toggle in Settings) | Y (SOCKS5 proxy; daemon flags for p2p routing) | Y (bundled + on by default; switch-after-sync mode default; always-Tor option) | Y (via Orbot; NetCipher integration; toggle on wallet list screen; .onion nodes supported) |
| **Hardware wallet** | Y (Ledger, Trezor, BitBox Android-only, Coldcard air-gapped) | Y (Ledger Nano S/S+/X/Flex/Gen5; Trezor Model T/Safe 3/Safe 5) | Y (Ledger Nano S/S+/X/Stax/Flex; Trezor Model T/Safe 3/Safe 5; passphrase entry on-device) | P — Ledger Nano S series via USB OTG confirmed (USB HID, firmware ≥1.6.0); Sidekick (Bluetooth 2nd-phone as software HW wallet, open-source companion app); Trezor: library compiled in but no confirmed UI path |
| **Seed format** | Polyseed 16-word (default); legacy 25-word; BIP39 | 25-word Monero seed (standard; no Polyseed; no BIP39) | Polyseed 16-word (default); legacy 14-word (restore only); 25-word Monero (restore + export) | 25-word Monero seed only (no Polyseed; no BIP39) |
| **Passphrase** | Y | N (wallet file password only; no seed passphrase extension for 25-word) | Y (Polyseed passphrase supported) | Y (CeasarSeed: Monero seed offset passphrase; supported at restore; confirmed in GenerateFragment.java) |
| **Lightning** | Y (Spark protocol, v6.0.0+) | N | N | N |
| **Watch-only** | Y (primary address + view key) | Y (createViewOnly — view key only; spend key withheld) | Y (primary address + secret view key; wizard-guided; QR transfer to offline device) | Y (address + view key; TYPE_VIEWONLY in LoginFragment; view key shown in wallet details) |
| **Multi-currency** | Y (17 coins: XMR, BTC, ETH, SOL, ZEC, LTC, others) | N (XMR only) | N (XMR only) | N (XMR only; Exolix swap for outgoing BTC/LTC/ETH/USDT/SOL only) |
| **Built-in swap** | Y (Chainflip non-custodial + custodial providers) | N | N | Y (Exolix KYC-free swap: XMR → BTC, LTC, ETH, USDT, SOL; centralised provider) |
| **Subaddresses** | Y | Y (full UI: generate, label, multi-account) | Y (generate, label; fresh subaddress per receive) | Y (generate, label; SubaddressFragment; per-account subaddress pool) |
| **Multiple accounts** | [NF] | Y (HD-style sub-accounts per wallet) | Y (multiple sub-accounts per wallet; independent history and balance) | Y (HD-style sub-accounts per wallet; sidebar drawer selection in WalletFragment; independent balance display per account) |
| **Coin control** | Y (BTC, LTC, BCH, DOGE, XMR, DCR) | N (GUI) — Y via bundled monero-wallet-cli | Y (sweep single/multi/all; output splitting; manual input selection; coin labeling; output blackballing) | P — sweep-all ("Send all confirmed funds in this account") only; no per-output manual selection |
| **Multisig** | [NF] | N (GUI) — Y via bundled monero-wallet-cli | N (planned; marked ✖* in feature table) | N |
| **P2Pool mining** | N | Y (built-in; solo and P2Pool modes; updated to v4.17.1) | N (no plans; marked ✖† = explicitly out of scope) | N |
| **Offline / air-gapped signing** | N | Y (output export/import + key image workflow) | Y (animated QR / UR standard + file transfer; wizard-guided; webcam QR scanner built-in) | N (Sidekick uses Bluetooth, not air-gap) |
| **Background sync** | [NF] | Y (v0.18.4.2+; syncs while wallet locked) | [NF] | Y (background wallet service keeps syncing; background lock feature; source: FAQ + strings.xml) |
| **Reproducible builds** | [NF] — no public process | Y (Windows + Linux Docker reproducible builds; SHA-256 + GPG hashes published) | Y (GNU Guix bootstrappable builds; multi-signer attestation in feather-sigs repo) | N (SHA-256 hash + APK signing cert published per release; no reproducible build process or multi-signer attestation found) |
| **GPG release signing** | [NF] | Y (getmonero.org/downloads/hashes.txt) | Y (fingerprint 8185 E158 A333 30C7 FD61 BC0D 1F76 E155 CEFB A71C; .asc files per binary) | N (APK signed by developer key CN=m2049r; SHA-256 hashes published per release; no GPG detached signatures) |
| **Security audit** | [NF] — none publicly disclosed | [NF] — no GUI-specific public audit found (HackerOne programme active) | [NOT FOUND via primary source] — Quarkslab 2022 audit referenced in community but report URL returns 404; Feather documentation does not link to report; bug bounty programme active (USD 100–1,500 in XMR for fund-loss vulnerabilities) | [NOT FOUND] — no public security audit; no bug bounty programme found |
| **Package manager** | N | Y (Flathub, Arch, NixOS, Homebrew cask, Guix) | Y (Flatpak via release zip; AppImage self-contained) | N (Google Play + official F-Droid repo + direct APK from GitHub releases) |
| **Tails OS support** | N | N | Y (dedicated Tails AppImage; persistent volume support; full installation guide in docs) | N |
| **Whonix / Qubes support** | N | N | Y (explicitly supported; system Tor override on Whonix; Qubes isolated-qube compatible) | N |
| **I2P support** | N | N | Y (node connections via I2P; site/docs available at .b32.i2p mirror) | N |
| **Transaction pusher** | N | N | Y (broadcast raw hex without connected wallet) | N |
| **Multibroadcasting** | N | N | Y (broadcast to multiple nodes simultaneously) | N |
| **Open-source** | Y (MIT) | Y (BSD-3-Clause) | Y (BSD-3-Clause) | Y (Apache-2.0) |
| **PocketChange** | N | N | N | Y (output pre-splitting: auto-creates ≥6 outputs at configurable amount with each tx to eliminate output-lock delay on repeat spending) |
| **Street Mode** | N | N | N | Y (hides balance and previous transactions while enabled; toggle in wallet UI) |
| **OpenAlias** | Y | Y | Y | Y (DNSSEC-verified human-readable addresses; OpenAliasHelper.java) |
| **F-Droid** | Y | N | N | Y (official Monerujo F-Droid repo at https://f-droid.monerujo.io/fdroid/repo/) |

**Sources for Monerujo column (updated 2026-08-12):**
- [GitHub API](https://api.github.com/repos/m2049r/xmrwallet) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet.json)
- [README.md](https://raw.githubusercontent.com/m2049r/xmrwallet/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-com-m2049r-xmrwallet-README.md)
- [FAQ.md](https://raw.githubusercontent.com/m2049r/xmrwallet/master/doc/FAQ.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-m2049r-xmrwallet-doc-FAQ.md)
- [Monerujo website](https://www.monerujo.app) — accessed 2026-08-12 — [archived](../sources/2026-08-12-monerujo-app-home.html)
- [Sidekick README](https://raw.githubusercontent.com/m2049r/sidekick/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-m2049r-sidekick-README.md)
- [GitHub releases API](https://api.github.com/repos/m2049r/xmrwallet/releases) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet-releases.json)
- Source inspection: `GenerateFragment.java` (CeasarSeed, Ledger), `LoginFragment.java` (watch-only, Ledger/Sidekick), `NetCipherHelper.java` + `OnionHelper.java` (Tor), `Ledger.java` (USB HID, Nano S), `ShiftService.java` + `ExolixApiImpl.java` (swap), `ZipBackup.java` (backup), `app/CMakeLists.txt` (Trezor lib), `Wallet.java` (device enum), `WalletFragment.java` (accounts), `SettingsFragment.java`, `help.xml`, `strings.xml`, `about.xml`, `privacy-policy.md`, `LICENSE` — accessed 2026-08-12 via GitHub raw API

**Sources for Feather Wallet column:**
- [GitHub API: feather-wallet/feather](https://api.github.com/repos/feather-wallet/feather) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather.json)
- [Docs: Feature comparison](https://docs.featherwallet.org/guides/features) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-features.html)
- [Docs: Tor support](https://docs.featherwallet.org/guides/tor-support) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-tor-support.html)
- [Docs: Seed scheme](https://docs.featherwallet.org/guides/seed-scheme) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-seed-scheme.html)
- [Docs: Offline transaction signing](https://docs.featherwallet.org/guides/offline-tx-signing) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-offline-signing.html)
- [Docs: Hardware wallet support](https://docs.featherwallet.org/guides/hardware-wallet-support) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-hardware-wallets.html)
- [Docs: About Feather Wallet (CCS funding)](https://docs.featherwallet.org/guides/about) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-about.html)
- [SECURITY.md](https://raw.githubusercontent.com/feather-wallet/feather/master/SECURITY.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-feather-wallet-security-md.md)
- [Guix reproducible builds README](https://raw.githubusercontent.com/feather-wallet/feather/master/contrib/guix/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-feather-wallet-guix-readme.md)
- [Feather download page](https://featherwallet.org/download/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-featherwallet-org-download.html)
- [Quarkslab audit report URL](https://blog.quarkslab.com/audit-of-feather-wallet.html) — returned 404 on 2026-08-12; report not accessible from primary source

**Sources for Monero GUI column:**
- [GitHub API: monero-project/monero-gui](https://api.github.com/repos/monero-project/monero-gui) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-api-monero-project-monero-gui.json)
- [getmonero.org/downloads/](https://www.getmonero.org/downloads/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-getmonero-org-downloads.html)
- [GitHub releases: v0.18.5.2](https://github.com/monero-project/monero-gui/releases/tag/v0.18.5.2) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-api-monero-gui-releases.json)
- [Source: src/libwalletqt/Wallet.h](https://raw.githubusercontent.com/monero-project/monero-gui/master/src/libwalletqt/Wallet.h) — Qt API inspection — accessed 2026-08-12
- [Source: pages/Mining.qml](https://raw.githubusercontent.com/monero-project/monero-gui/master/pages/Mining.qml) — P2Pool confirmed — accessed 2026-08-12
- [Source: pages/settings/SettingsLayout.qml](https://raw.githubusercontent.com/monero-project/monero-gui/master/pages/settings/SettingsLayout.qml) — SOCKS5 proxy confirmed — accessed 2026-08-12
- [Source: wizard/WizardModeSelection.qml](https://raw.githubusercontent.com/monero-project/monero-gui/master/wizard/WizardModeSelection.qml) — mode selection confirmed — accessed 2026-08-12
- [Flathub API: org.getmonero.Monero stats](https://flathub.org/api/v2/stats/org.getmonero.Monero) — accessed 2026-08-12 — [archived](../sources/2026-08-12-flathub-api-monero-gui-stats.json)

## Zcash wallets

| Feature | Zodl | YWallet | Nighthawk |
|---------|------|---------|---------|
| **Platforms** | iOS, Android | iOS, Android, Desktop | iOS, Android |
| **Shielded by default** | Y (Orchard) | Y | Y (Sapling) |
| **Orchard pool** | Y | Y | Y (v2) |
| **Hardware wallet** | N | N | N |
| **Open-source** | Y | Y | Y |
| **F-Droid** | [NF] | [NF] | Y |
