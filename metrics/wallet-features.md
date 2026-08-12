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
| **Latest version** | 4.8.1 (August 2026) | [NF] | [NF] | [NF] |
| **Key management** | BIP32 HD; Electrum native seed or BIP39 import | BIP32 HD; BIP39 | BIP32 HD; BIP39; BIP47 (PayNym) | BIP32 HD; BIP39 |
| **Seed format** | Electrum native (version-embedded) or BIP39 | BIP39 | BIP39 | BIP39 |
| **Passphrase (BIP39 extension)** | Y | Y | Y | Y |
| **Watch-only wallet** | Y | Y | Y | N |
| **Hardware wallet** | Y (Trezor, Ledger, KeepKey, BitBox02, Coldcard, Jade, Safe-T, Bitbox01) | P (Ledger via BLE) | Y (Trezor, Ledger, Coldcard, BitBox02, Jade, Specter DIY, Passport) | N |
| **PSBT** | Y | [NF] | Y | N |
| **Multisig** | Y (native m-of-n; P2SH and P2WSH) | P (via third-party vault) | Y (native, all script types) | N |
| **Air-gapped signing** | Y (file, QR code, audio modem plugin) | [NF] | Y (QR, microSD, NFC) | N |
| **Lightning Network** | Y (built-in, trampoline via ACINQ) | Y (LNDhub, self-custodial requires own hub) | N | Y (native self-custodial) |
| **Submarine swaps** | Y (on-chain ↔ Lightning) | [NF] | N | Y |
| **Script types — P2PKH** | Y | Y | Y | N |
| **Script types — P2SH** | Y | [NF] | Y | N |
| **Script types — P2WPKH (bech32)** | Y | Y | Y | Y |
| **Script types — P2WSH** | Y (multisig) | [NF] | Y | N |
| **Script types — P2TR (Taproot receive)** | N (can send to P2TR; cannot create P2TR wallet) | [NF] | Y | N |
| **Coin control (UTXO selection)** | Y | [NF] | Y | N |
| **RBF (Replace-By-Fee)** | Y (default) | [NF] | Y | — |
| **CPFP** | Y | [NF] | Y | — |
| **CoinJoin** | N | N | Y (PayJoin; Whirlpool via Samourai coordinator) | N |
| **Tor support** | Y (SOCKS5 proxy to .onion servers) | [NF] | Y (built-in) | N |
| **Privacy-preserving routing** | Y (Tor, manual server) | [NF] | Y (Tor, self-hosted full node) | Y (trampoline) |
| **Fee estimation** | Y (dynamic; median of ~10 servers) | Y | Y | Y |
| **Payment batching** | Y | [NF] | Y | — |
| **2FA** | Y (TrustedCoin plugin, 2-of-3) | N | N | N |
| **Open-source** | Y (MIT) | Y (MIT) | Y (Apache-2.0) | Y (Apache-2.0) |
| **Reproducible builds** | Y (5 independent signers) | [NF] | [NF] | [NF] |
| **F-Droid** | Y | Y | N | Y |
| **Command-line / daemon** | Y (full RPC/JSON-RPC) | N | N | N |
| **Plugin system** | Y (hardware wallets, 2FA, NWC, watchtower, etc.) | N | P (limited) | N |

**Sources for Electrum column:**
- [electrum.org](https://electrum.org) — [archived](../sources/2026-08-10-electrum-org-home.html)
- [readthedocs](https://electrum.readthedocs.io) — [archived](../sources/2026-08-10-electrum-readthedocs-io-latest.html)
- [RELEASE-NOTES](https://raw.githubusercontent.com/spesmilo/electrum/master/RELEASE-NOTES) — [archived](../sources/2026-08-10-github-com-spesmilo-electrum-RELEASE-NOTES.txt)
- [electrum/bitcoin.py — WIF_SCRIPT_TYPES](https://github.com/spesmilo/electrum/blob/master/electrum/bitcoin.py) — source inspection
- [electrum/plugins/ directory](https://api.github.com/repos/spesmilo/electrum/contents/electrum/plugins) — source inspection

**Sources for other columns:** _index.md discovery research (2026-08-10). BlueWallet, Sparrow, and Phoenix columns will be filled in full during their respective deep-research runs.

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
| **Platforms** | iOS, Android, macOS, Linux | Windows, macOS, Linux | Windows, macOS, Linux, Tails, Whonix, Qubes | Android |
| **License** | MIT | BSD-3-Clause | BSD-3-Clause | MIT |
| **Architecture** | Flutter / remote node | C++/Qt/QML — full local node (advanced mode) or remote node | C++/Qt — remote node only | Android / remote node |
| **Latest version** | v6.4.0 (27 July 2026) | v0.18.5.2 "Fluorine Fermi" (21 July 2026) | v2.8.1 (14 April 2025) | [NF] |
| **Full node** | N (remote node) | Y (monerod bundled; optional bootstrap mode) | N (remote node) | N (remote node) |
| **Tor** | Y (opt-in; toggle in Settings) | Y (SOCKS5 proxy; daemon flags for p2p routing) | Y (bundled + on by default; switch-after-sync mode default; always-Tor option) | Y |
| **Hardware wallet** | Y (Ledger, Trezor, BitBox Android-only, Coldcard air-gapped) | Y (Ledger Nano S/S+/X/Flex/Gen5; Trezor Model T/Safe 3/Safe 5) | Y (Ledger Nano S/S+/X/Stax/Flex; Trezor Model T/Safe 3/Safe 5; passphrase entry on-device) | N |
| **Seed format** | Polyseed 16-word (default); legacy 25-word; BIP39 | 25-word Monero seed (standard; no Polyseed; no BIP39) | Polyseed 16-word (default); legacy 14-word (restore only); 25-word Monero (restore + export) | 25-word Monero seed |
| **Passphrase** | Y | N (wallet file password only; no seed passphrase extension for 25-word) | Y (Polyseed passphrase supported) | [NF] |
| **Lightning** | Y (Spark protocol, v6.0.0+) | N | N | N |
| **Watch-only** | Y (primary address + view key) | Y (createViewOnly — view key only; spend key withheld) | Y (primary address + secret view key; wizard-guided; QR transfer to offline device) | Y |
| **Multi-currency** | Y (17 coins: XMR, BTC, ETH, SOL, ZEC, LTC, others) | N (XMR only) | N (XMR only) | N (XMR only) |
| **Built-in swap** | Y (Chainflip non-custodial + custodial providers) | N | N | N |
| **Subaddresses** | Y | Y (full UI: generate, label, multi-account) | Y (generate, label; fresh subaddress per receive) | Y |
| **Multiple accounts** | [NF] | Y (HD-style sub-accounts per wallet) | Y (multiple sub-accounts per wallet; independent history and balance) | [NF] |
| **Coin control** | Y (BTC, LTC, BCH, DOGE, XMR, DCR) | N (GUI) — Y via bundled monero-wallet-cli | Y (sweep single/multi/all; output splitting; manual input selection; coin labeling; output blackballing) | [NF] |
| **Multisig** | [NF] | N (GUI) — Y via bundled monero-wallet-cli | N (planned; marked ✖* in feature table) | [NF] |
| **P2Pool mining** | N | Y (built-in; solo and P2Pool modes; updated to v4.17.1) | N (planned; marked ✖†) | N |
| **Offline / air-gapped signing** | N | Y (output export/import + key image workflow) | Y (animated QR / UR standard + file transfer; wizard-guided; webcam QR scanner built-in) | N |
| **Background sync** | [NF] | Y (v0.18.4.2+; syncs while wallet locked) | [NF] | [NF] |
| **Reproducible builds** | [NF] — no public process | Y (Windows + Linux Docker reproducible builds; SHA-256 + GPG hashes published) | Y (GNU Guix bootstrappable builds; multi-signer attestation in feather-sigs repo) | [NF] |
| **GPG release signing** | [NF] | Y (getmonero.org/downloads/hashes.txt) | Y (fingerprint 8185 E158 A333 30C7 FD61 BC0D 1F76 E155 CEFB A71C; .asc files per binary) | [NF] |
| **Security audit** | [NF] — none publicly disclosed | [NF] — no GUI-specific public audit found (HackerOne programme active) | [NOT FOUND via primary source] — Quarkslab 2022 audit referenced in community but report URL returns 404; Feather documentation does not link to report; bug bounty programme active (USD 100–1,500 in XMR for fund-loss vulnerabilities) | [NF] |
| **Package manager** | N | Y (Flathub, Arch, NixOS, Homebrew cask, Guix) | Y (Flatpak via release zip; AppImage self-contained) | N |
| **Tails OS support** | N | N | Y (dedicated Tails AppImage; persistent volume support; full installation guide in docs) | N |
| **Whonix / Qubes support** | N | N | Y (explicitly supported; system Tor override on Whonix; Qubes isolated-qube compatible) | N |
| **I2P support** | N | N | Y (node connections via I2P; site/docs available at .b32.i2p mirror) | N |
| **Transaction pusher** | N | N | Y (broadcast raw hex without connected wallet) | N |
| **Multibroadcasting** | N | N | Y (broadcast to multiple nodes simultaneously) | N |
| **Open-source** | Y (MIT) | Y (BSD-3-Clause) | Y (BSD-3-Clause) | Y (MIT) |

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
