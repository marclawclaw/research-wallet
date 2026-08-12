# Cross-Wallet Feature Comparison

Feature comparison for self-custody software wallets across Bitcoin, Ethereum/EVM, Solana, Monero, and Zcash ecosystems. All figures current as of 2026-08-10.

**Key:** Y = Yes / Confirmed; N = No / Not supported; P = Partial / Plugin; — = Not applicable; [NF] = Not Found / unconfirmed

## Bitcoin wallets

| Feature | Electrum | BlueWallet | Sparrow Wallet | Phoenix |
|---------|----------|-----------|---------------|---------|
| **Platforms** | Windows, macOS, Linux, Android | iOS, Android | Windows, macOS, Linux | iOS, Android |
| **License** | MIT | MIT | Apache-2.0 | Apache-2.0 |
| **Architecture** | SPV (Electrum server) | SPV / Electrum server | Full node / Electrum server | Lightning-native |
| **Language** | Python | React Native | Java | Kotlin |
| **Latest version** | 4.8.1 (August 2026) | 8.0.1 (21 July 2026) | 2.5.3 (30 July 2026) | [NF] |
| **Key management** | BIP32 HD; Electrum native seed or BIP39 import | BIP32 HD; BIP39 | BIP32 HD; BIP39; BIP39 passphrase; SLIP39 import; BIP47 (PayNym); xpub watch-only; output descriptor | BIP32 HD; BIP39 |
| **Seed format** | Electrum native (version-embedded) or BIP39 | BIP39 | BIP39 (12/24-word); SLIP39 import | BIP39 |
| **Passphrase (BIP39 extension)** | Y | Y | Y | Y |
| **Watch-only wallet** | Y | Y | Y | N |
| **Hardware wallet** | Y (Trezor, Ledger, KeepKey, BitBox02, Coldcard, Jade, Safe-T, Bitbox01) | P (Ledger via BLE) | Y — USB: Trezor, Ledger, BitBox02, Jade/Jade Plus, ERA; Airgapped QR: Passport, SeedSigner, Keystone, Krux, Jade, Specter DIY, Seed Tool; Airgapped SD: Coldcard; Airgapped NFC: Tapsigner, Satochip, Keycard | N |
| **PSBT** | Y | Y | Y | N |
| **Multisig** | Y (native m-of-n; P2SH and P2WSH) | Y (native m-of-n; P2WSH, P2SH-P2WSH, P2SH) | Y (native, all script types) | N |
| **Air-gapped signing** | Y (file, QR code, audio modem plugin) | Y (PSBT file, BC-UR v2 QR — v8.0.1) | Y (QR, microSD, NFC) | N |
| **Lightning Network** | Y (built-in, trampoline via ACINQ) | Y (LNDhub, self-custodial requires own hub) | N | Y (native self-custodial) |
| **Submarine swaps** | Y (on-chain ↔ Lightning) | Y (via Arkade/Boltz — v8.0.0+) | N | Y |
| **Script types — P2PKH** | Y | Y | Y | N |
| **Script types — P2SH** | Y | Y (P2SH-P2WPKH wrapped SegWit; P2SH multisig) | Y | N |
| **Script types — P2WPKH (bech32)** | Y | Y | Y | Y |
| **Script types — P2WSH** | Y (multisig) | Y (multisig via MultisigHDWallet) | Y | N |
| **Script types — P2TR (Taproot receive)** | N (can send to P2TR; cannot create P2TR wallet) | Y (HDTaprootWallet, BIP86, m/86'/0'/0') | Y (from v1.4.3, July 2021; full signing including script-path) | N |
| **Coin control (UTXO selection)** | Y | Y (documented feature; fetchUtxo/getUtxo in source) | Y | N |
| **RBF (Replace-By-Fee)** | Y (default) | Y (BIP68 sequence 0x80000000; watch-only bech32 added v8.0.1) | Y | — |
| **CPFP** | Y | Y (documented in official support) | Y | — |
| **CoinJoin** | N | N | P — PayJoin (BIP78): Y; Whirlpool (Samourai): REMOVED v1.9.0 (Apr 2024, post-Samourai arrests); PayJoin v2 (BIP77): [NF] | N |
| **Tor support** | Y (SOCKS5 proxy to .onion servers) | N (not found in source inspection 2026-08-12) | Y (built-in) | N |
| **Privacy-preserving routing** | Y (Tor, manual server) | P (custom Electrum server only; no Tor) | Y (Tor, self-hosted full node) | Y (trampoline) |
| **Fee estimation** | Y (dynamic; median of ~10 servers) | Y | Y | Y |
| **Payment batching** | Y | Y (Send to Many / batch documented in official support) | Y | — |
| **2FA** | Y (TrustedCoin plugin, 2-of-3) | N | N | N |
| **Open-source** | Y (MIT) | Y (MIT) | Y (Apache-2.0) | Y (Apache-2.0) |
| **Reproducible builds** | Y (5 independent signers) | P (APK + IPA released with detached .sig; full reproducible-build attestation [NOT FOUND]) | P — .tar.gz and .zip reproducible from v1.5.0; .deb/.rpm/.msi/.dmg have minor variances; bitcoinbinary.org attestations v1.5.0–v1.6.6 only; more recent releases [NF] at bitcoinbinary.org | [NF] |
| **F-Droid** | Y | Y | N | Y |
| **Command-line / daemon** | Y (full RPC/JSON-RPC) | N | N | N |
| **Plugin system** | Y (hardware wallets, 2FA, NWC, watchtower, etc.) | N | P (limited) | N |
| **Silent Payments (BIP352)** | N | N | Y — sending v2.3.0 (Oct 2025); receiving wallets v2.5.0 (May 2026); HW support via BIP375 v2.4.0 | N |
| **Wallet encryption** | PBKDF2 | [NF] | Argon2 (≥500ms key derivation on modern hardware) | [NF] |
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

**Sources for other columns:** _index.md discovery research (2026-08-10). Phoenix column will be filled in during its respective deep-research run.

## Ethereum/EVM wallets

| Feature | MetaMask | Trust Wallet | Rabby | Rainbow |
|---------|---------|-------------|-------|---------|
| **Platforms** | Browser ext, iOS, Android | iOS, Android | Browser ext, iOS, Android, Desktop | iOS, Android, Browser ext |
| **License** | [NF] | Apache-2.0 (wallet-core) | [NF] | [NF] |
| **Architecture** | Browser-injected provider | Mobile app | Browser-injected provider | Mobile app |
| **Lightning** | — | — | — | — |
| **Hardware wallet** | Y (via MetaMask accounts) | [NF] | Y (Ledger, Trezor, OneKey, GridPlus) | [NF] |
| **Multi-chain** | Y (EVM chains) | Y (100+ chains) | Y (EVM chains) | Y (ETH + Base + Solana) |
| **Open-source** | Y (metamask-extension) | Y (wallet-core SDK) | Y | Y |

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
| **Platforms** | iOS, Android, macOS, Linux | Windows, macOS, Linux | Windows, macOS, Linux, Tails | Android |
| **License** | MIT | BSD-3-Clause | BSD-3-Clause | MIT |
| **Architecture** | Flutter / remote node | C++/Qt/QML — full local node (advanced mode) or remote node | C++/Qt — remote node | Android / remote node |
| **Latest version** | [NF] | v0.18.5.2 "Fluorine Fermi" (21 July 2026) | [NF] | [NF] |
| **Full node** | N (remote node) | Y (monerod bundled; optional bootstrap mode) | N (remote node) | N (remote node) |
| **Tor** | Y | Y (SOCKS5 proxy; daemon flags for p2p routing) | Y (built-in) | Y |
| **Hardware wallet** | Y (Ledger, Trezor, BitBox Android-only, Coldcard air-gapped) | Y (Ledger Nano S/S+/X/Flex/Gen5; Trezor Model T/Safe 3/Safe 5) | N | N |
| **Seed format** | Polyseed 16-word (default); legacy 25-word; BIP39 | 25-word Monero seed (standard; no Polyseed; no BIP39) | [NF] | 25-word Monero seed |
| **Passphrase** | Y | N (wallet file password only; no seed passphrase extension for 25-word) | [NF] | [NF] |
| **Lightning** | Y (Spark protocol, v6.0.0+) | N | N | N |
| **Watch-only** | Y (primary address + view key) | Y (createViewOnly — view key only; spend key withheld) | Y | Y |
| **Multi-currency** | Y (17 coins: XMR, BTC, ETH, SOL, ZEC, LTC, others) | N (XMR only) | N (XMR only) | N (XMR only) |
| **Built-in swap** | Y (Chainflip non-custodial + custodial providers) | N | N | N |
| **Subaddresses** | Y | Y (full UI: generate, label, multi-account) | Y | Y |
| **Multiple accounts** | [NF] | Y (HD-style sub-accounts per wallet) | [NF] | [NF] |
| **Coin control** | Y (BTC, LTC, BCH, DOGE, XMR, DCR) | N (GUI) — Y via bundled monero-wallet-cli | Y | [NF] |
| **Multisig** | [NF] | N (GUI) — Y via bundled monero-wallet-cli | [NF] | [NF] |
| **P2Pool mining** | N | Y (built-in; solo and P2Pool modes; updated to v4.17.1) | N | N |
| **Cold signing** | N | Y (output export/import + key image workflow) | [NF] | N |
| **Background sync** | [NF] | Y (v0.18.4.2+; syncs while wallet locked) | [NF] | [NF] |
| **Reproducible builds** | [NF] — no public process | Y (Windows + Linux Docker reproducible builds; SHA-256 + GPG hashes published) | [NF] | [NF] |
| **GPG release signing** | [NF] | Y (getmonero.org/downloads/hashes.txt) | [NF] | [NF] |
| **Security audit** | [NF] — none publicly disclosed | [NF] — no GUI-specific public audit found (HackerOne programme active) | Y (Quarkslab 2022) | [NF] |
| **Package manager** | N | Y (Flathub, Arch, NixOS, Homebrew cask, Guix) | [NF] | N |
| **Open-source** | Y (MIT) | Y (BSD-3-Clause) | Y (BSD-3-Clause) | Y (MIT) |

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
