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
| **Platforms** | iOS, Android, macOS, Linux | Windows, macOS, Linux | Windows, macOS, Linux, Tails | Android |
| **License** | MIT | BSD-3-Clause | BSD-3-Clause | MIT |
| **Full node** | N (remote node) | Y | N (remote node) | N (remote node) |
| **Tor** | Y | Y | Y (built-in) | Y |
| **Hardware wallet** | Y (Ledger, Trezor, BitBox Android-only, Coldcard air-gapped) | Y (Ledger) | N | N |
| **Seed format** | Polyseed 16-word (default); legacy 25-word; BIP39 | 25-word Monero seed | [NF] | 25-word Monero seed |
| **Lightning** | Y (Spark protocol, v6.0.0+) | N | N | N |
| **Watch-only** | Y (primary address + view key) | Y | Y | Y |
| **Multi-currency** | Y (17 coins: XMR, BTC, ETH, SOL, ZEC, LTC, others) | N (XMR only) | N (XMR only) | N (XMR only) |
| **Built-in swap** | Y (Chainflip non-custodial + custodial providers) | N | N | N |
| **Coin control** | Y (BTC, LTC, BCH, DOGE, XMR, DCR) | [NF] | Y | [NF] |
| **Reproducible builds** | [NF] — no public process | N | [NF] | [NF] |
| **Security audit** | [NF] — none publicly disclosed | [NF] | Y (Quarkslab 2022) | [NF] |

## Zcash wallets

| Feature | Zodl | YWallet | Nighthawk |
|---------|------|---------|---------|
| **Platforms** | iOS, Android | iOS, Android, Desktop | iOS, Android |
| **Shielded by default** | Y (Orchard) | Y | Y (Sapling) |
| **Orchard pool** | Y | Y | Y (v2) |
| **Hardware wallet** | N | N | N |
| **Open-source** | Y | Y | Y |
| **F-Droid** | [NF] | [NF] | Y |
