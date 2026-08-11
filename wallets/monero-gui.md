---
tags: [wallet, monero, desktop, reference-wallet, full-node, hardware-wallet]
related: [monero-stealth-address, polyseed-monero-seed, full-node-wallet]
ecosystem: monero
status: active
---

# Monero GUI

The official reference wallet for the Monero network, maintained by the Monero Project core team. Desktop-only (Windows, macOS, Linux). The only Monero wallet to bundle a full local node (monerod daemon), making it the recommended choice for users who require maximum privacy and self-sovereignty. Open-source under BSD-3-Clause.

## Adoption Metrics

| Metric | Value | Source | Date |
|--------|-------|--------|------|
| GitHub stars | 2,285 | [GitHub API](https://api.github.com/repos/monero-project/monero-gui) | 2026-08-12 |
| GitHub forks | 920 | GitHub API | 2026-08-12 |
| GitHub contributors | ~235 (pagination inference) | GitHub API (`/contributors?per_page=1` returns 235 pages) | 2026-08-12 |
| GitHub watchers/subscribers | 109 | GitHub API | 2026-08-12 |
| Open issues | 470 | GitHub API | 2026-08-12 |
| Flathub installs (total, Linux) | 101,837 | [Flathub API](https://flathub.org/api/v2/stats/org.getmonero.Monero) | 2026-08-12 |
| Flathub installs (last 30 days) | 2,135 | Flathub API | 2026-08-12 |
| Flathub installs (last 7 days) | 398 | Flathub API | 2026-08-12 |
| GitHub download counts | [NOT FOUND] — releases served via downloads.getmonero.org, not GitHub assets; no public download counter | — | 2026-08-12 |
| MAU | [NOT FOUND] — structurally unavailable. Monero's RingCT protocol and stealth addresses make it impossible to attribute on-chain activity to individual wallet software. No opt-in telemetry is present in Monero GUI. | — | — |
| Latest version | v0.18.5.2 ("Fluorine Fermi") | [GitHub releases](https://github.com/monero-project/monero-gui/releases/tag/v0.18.5.2) | 2026-08-12 |
| Release date (latest) | 21 July 2026 | GitHub releases | 2026-08-12 |
| Repo created | 1 April 2015 | GitHub API | 2026-08-12 |
| Last push | 6 August 2026 | GitHub API | 2026-08-12 |

**Note on MAU:** Monero MAU is structurally unavailable for all Monero wallets. RingCT (confidential amounts), ring signatures, and stealth addresses together make it impossible to identify the wallet software behind any on-chain transaction. Monero GUI contains no telemetry. The Flathub monthly install figure (2,135 for July–August 2026) is a lower-bound proxy covering Linux Flatpak installs only; Windows and macOS installs via getmonero.org and Homebrew are not counted.

## How It Works

Monero GUI is a Qt/QML front-end written in C++ that wraps the official Monero wallet library (`libwallet`). It communicates with a Monero daemon (monerod) via a local RPC connection. Three operational modes are available, selected at first launch:

1. **Simple mode** — connects automatically to a remote node (no local blockchain). Lower privacy; faster startup. Not available on Tails OS.
2. **Simple mode (bootstrap)** — downloads blockchain locally while initially using a remote node as a bootstrap; switches to local node when sync is complete. Best of both: usable immediately, eventually sovereign.
3. **Advanced mode** — requires a fully synced local node. Includes mining (solo or P2Pool), message verification, and Shared Ring DB. Maximum privacy and control.

There is also a **Portable mode** option for running from external storage (USB drive).

## Key Features

### Key Management
- **Seed format:** 25-word Monero mnemonic (Electrum-style, Monero's native format). Not Polyseed (16-word); not BIP39 by default. The 25-word seed includes a checksum word. Language selectable (25+ languages).
- **Key display:** Secret view key, public view key, secret spend key, and public spend key are all viewable in the Keys page (wallet/pages/Keys.qml).
- **Watch-only wallets:** Supported. Creating a view-only wallet (`createViewOnly`) exports only the private view key; spend key is not retained, allowing watch-only monitoring without spend capability.
- **Subaddresses:** Full subaddress support. Users can generate new subaddresses on the Receive page, label them, and manage multiple accounts. The wallet auto-generates subaddresses per account/index pair.
- **Multiple accounts:** Multiple HD-style accounts within a single wallet, each with its own set of subaddresses. Accounts are switched via the left panel.
- **Wallet file encryption:** Wallet files encrypted with user passphrase (KDF rounds configurable).
- **Seed language:** 25+ language wordlists available at wallet creation.

### Node Options
- **Local full node (daemon):** Monero GUI is unique among Monero wallets in bundling `monerod` and providing a GUI to start/stop it with configurable flags. Daemon flags are set via Settings > Node. Blockchain pruning supported (downloads ~1/3 of full blockchain).
- **Remote node:** Custom remote node address and port configurable. Authentication supported.
- **Bootstrap node:** While syncing locally, use a remote node to remain usable.
- **Trusted/untrusted daemon:** Users can mark a remote daemon as trusted or untrusted, affecting privacy-protection behaviours (e.g., refusing to share view key with untrusted daemon).
- **P2Pool sidechain mining:** Advanced mode includes built-in P2Pool integration (updated to P2Pool v4.17.1 as of v0.18.5.1). Both solo mining and P2Pool decentralised pool mining are available from the Mining page.

### Hardware Wallet Integration
- **Ledger:** Nano S, Nano S Plus, Nano X, Flex, and Nano Gen 5 supported. Ledger Monero app required. Key operations performed on device. v0.18.4.3 added Ledger Flex; v0.18.4.5 added Ledger Nano Gen 5.
- **Trezor:** Model T, Safe 3, Safe 5 supported. Open issue (as at August 2026) to add Trezor Safe 7 (#4665). Trezor support integrated at C++ level in the core Monero daemon (`device_trezor`).
- **Connection method:** USB HID. Both are identified at wallet creation; seed is protected on device; the GUI shows "Mnemonic seed protected by hardware device" in the Keys view.
- **Operations on device:** Transaction signing, address verification (`deviceShowAddressAsync`). The wallet tracks `isLedger()` and `isTrezor()` flags.
- **Limitation:** Hardware wallets do not expose secret spend key to the GUI (expected behaviour). View key can be optionally exported for watch-only monitoring (Ledger: fixed bug in v0.18.4.4 where rejecting secret view key export caused a crash).

### Signing and Transaction Construction
- **Standard Monero RingCT signing:** All transactions use Ring Confidential Transactions (RingCT) with Bulletproof+ range proofs (via the core `libmonero` library). Ring size is 16 (as of Monero network protocol v15/16). Decoy selection performed locally.
- **Offline / cold wallet signing:** The GUI supports a two-step cold signing workflow: (1) export outputs from view-only wallet; (2) import outputs and export key images from cold/offline wallet; (3) sign transaction offline; (4) submit signed transaction via view-only wallet. The `UnsignedTransaction` / `loadTxFile` / `submitTxFile` API is exposed in the GUI's Transfer page.
- **Sweep unmixable outputs:** Dedicated "Sweep" function to consolidate old outputs that cannot be adequately ring-signed (pre-RingCT outputs).
- **Sweep all:** `createTransactionAll` sends entire wallet balance.
- **Fee tiers:** Four tiers — Slow (×0.2), Normal (×1), Fast (×5), Fastest (×200) — with real-time estimated fee display including fiat conversion.
- **Payment ID:** Long payment IDs deprecated; integrated payment IDs supported.
- **QR code scanning:** Built-in QR code scanner for receiving addresses. Security fix applied in v0.18.5.1 (memory safety issue in QR scanning).

### Backup and Recovery
- **Seed backup:** 25-word mnemonic shown during wallet creation wizard; user records manually. GUI warns against copying to clipboard (clipboard exposure risk).
- **Restore:** Wallet restoration from 25-word seed with manual restore height input. Restore height specifies the block from which scanning begins; entering too early a height extends sync time proportionally.
- **Wallet creation height:** Stored in wallet metadata (`getWalletCreationHeight`). Can be reset in Settings > Info for rescan.
- **Wallet file backup:** `.keys` file and accompanying wallet data file; both required for wallet backup.
- **Key image export/import:** Supports export of key images from offline wallet for use in watch-only wallet (`exportKeyImages`, `importKeyImages`).
- **Background sync:** v0.18.4.2 added background sync while wallet is locked (`setupBackgroundSync`), allowing the wallet to remain in sync without the spend password being held in memory.

### Privacy Features
- **SOCKS5 proxy (Tor / I2P):** Configurable in Settings > Layout. Proxy applies to remote node connections, update downloads, and fiat price source fetching. In advanced mode, the daemon itself is configured separately (daemon flags field) to route p2p traffic over Tor/I2P. The getmonero.org downloads page explicitly lists "Transactions over Tor/I2P" as a feature.
- **Shared Ring DB:** A dedicated GUI page (`pages/SharedRingDB.qml`) for managing the shared ring signature database — used to avoid revealing which outputs are spent by reusing ring members from known spent outputs.
- **Sign/verify:** Message signing and verification page (`pages/Sign.qml`).
- **Prove/check:** Transaction proof generation and verification page (part of Advanced).
- **Trusted daemon flag:** Users can flag a remote daemon as untrusted to prevent the GUI from exposing sensitive information (e.g., view key export).
- **Fiat price:** Optional in-app fiat conversion with configurable price source.

### UX Modes
- **Simple vs Advanced:** Mode selection at first run. Simple mode hides daemon management, mining, ring DB, and advanced transaction options. Advanced mode exposes the full feature set.
- **Merchant page:** Dedicated XMR payment request page with QR code generation (navigable from left panel).
- **Address book:** Built-in address book (`pages/AddressBook.qml`).
- **Transaction history:** Filterable transaction list (`pages/History.qml`).
- **Localisation:** 30+ languages via Weblate.
- **Fiat conversion:** In-app fiat price display.

### Multisig
- **Status:** [NOT FOUND] in the GUI interface. The core Monero daemon (`libmonero`) supports native M-of-N multisig, and the CLI wallet fully exposes it. Monero GUI's libwalletqt API (`Wallet.h`) does not expose multisig methods as `Q_INVOKABLE` functions (reviewed August 2026). Multisig in Monero GUI requires using it via the CLI wallet (`monero-wallet-cli`) bundled in the release archive.
- The GUI release archive includes both `monero-wallet-gui` and the supporting binaries (`monerod`, `monero-wallet-cli`), so CLI-based multisig is available to GUI users who are comfortable with the terminal.

### Open-Source and Reproducibility
- **Licence:** BSD-3-Clause (confirmed from LICENSE file).
- **Reproducible builds:** Supported for Windows (Docker-based deterministic build, `Dockerfile.windows`) and Linux (`Dockerfile.linux`). macOS builds are not listed as reproducible in the README. The README references reproducibility for Windows and Linux static binaries with SHA-256 hash verification.
- **Release signing:** Each release ships GPG-signed SHA-256 hashes at `getmonero.org/downloads/hashes.txt`. GPG keys are stored in the repo at `/utils/gpg_keys`.
- **Bug bounty:** HackerOne programme at `hackerone.com/monero`. Responsible disclosure process documented in `monero-project/meta`.
- **Audit history:** [NOT FOUND] — no formal third-party code audit of the GUI specifically is publicly documented. The core Monero library has received research scrutiny from Monero Research Lab but no public GUI-specific audit report was located as at 2026-08-12.
- **Flatpak:** Available on Flathub (`org.getmonero.Monero`).
- **Package managers:** Arch Linux (AUR, `monero-gui`), NixOS (`nix-shell -p monero-gui`), Homebrew cask (`brew install --cask monero-wallet`), Guix.

## Architecture Decisions

1. **Full node bundled by default (Advanced mode):** Unlike every other Monero wallet, Monero GUI ships a full `monerod` daemon and makes running it the default for advanced users. This eliminates dependence on any third party for transaction broadcast and blockchain data — a significant privacy and censorship-resistance advantage.

2. **C++/Qt/QML stack:** The wallet engine (`libwallet`) is in C++, consistent with the core Monero daemon. The UI is Qt5 QML, enabling cross-platform desktop support without a web browser runtime. This contrasts with Flutter (Cake Wallet), React Native (BlueWallet), or Electron (many others).

3. **No Polyseed:** Monero GUI uses only the traditional 25-word Monero mnemonic. Polyseed (16-word, date-encoded) is not supported. Users must manually record their restore height or scan from block 0.

4. **P2Pool integration:** The GUI bundles P2Pool, a decentralised mining pool protocol, and offers a one-click P2Pool mining mode. This is unusual for a user-facing wallet and reflects the Monero community's emphasis on decentralised mining.

5. **Offline/cold wallet workflow:** The GUI supports an outputs-export / key-images-import workflow for air-gapped cold signing, without requiring a separate application. This is more ergonomic than comparable Bitcoin wallets that require PSBT-aware coordinators.

6. **Mode selection at first run:** Rather than offering a single complex UI, Monero GUI presents a mode selection wizard at first launch. Simple mode deliberately hides features to reduce friction for new users.

## Differentiators vs Other Monero Wallets

| | Monero GUI | Cake Wallet | Feather Wallet | Monerujo |
|---|---|---|---|---|
| Local full node | **Y** | N | N | N |
| P2Pool mining | **Y** | N | N | N |
| Hardware wallet | Ledger + Trezor | Ledger + Trezor | N | N |
| Multi-currency | N | Y (17 coins) | N | N |
| Polyseed | N | Y (default) | [NF] | N |
| Mobile | N | Y | N | Y (Android) |
| Coin control | N (GUI) / Y (bundled CLI) | Y | Y | [NF] |
| Reproducible builds | Y (Windows + Linux) | N | [NF] | [NF] |
| Tor (built-in) | SOCKS5 proxy | Y | Y (built-in) | Y |

## Limitations and Criticisms

1. **Sync time:** A full local node requires downloading the entire Monero blockchain (currently ~180 GB+ uncompressed; ~60 GB pruned). Initial block download on a consumer connection can take days. Remote node mode avoids this but reduces privacy.

2. **Restore height friction:** Unlike Polyseed (used by Cake Wallet), the 25-word Monero seed does not encode the wallet creation date. Users must record the restore height separately; if forgotten, the wallet rescans from block 0, which can take many hours.

3. **Desktop-only:** No mobile application. Monero GUI explicitly targets Windows, macOS, and Linux desktops. An experimental Android APK build exists in the Dockerfile but is not an officially supported release.

4. **No coin control UI:** The GUI does not expose a graphical coin control (UTXO/output selection) interface. Users who require fine-grained output selection must use the bundled `monero-wallet-cli`.

5. **No multisig UI:** Monero's native multisig (M-of-N) is not exposed in the GUI. Available via the CLI wallet only.

6. **No Polyseed support:** Cannot create or restore Polyseed (16-word) wallets. Users migrating from Cake Wallet with a Polyseed seed must either convert their Cake Wallet to a 25-word seed or use a different wallet.

7. **UX complexity for new users:** Despite the Simple mode, the range of concepts (restore height, ring size, daemon management, view keys, etc.) creates a steeper learning curve compared to mobile Monero wallets.

8. **Security issues (historical):** v0.18.5.1 fixed a memory safety issue in QR code scanning and CSV formula injection in transaction export. v0.18.5.1 also fixed an HTML-escaping gap in rich text views. These were found and fixed by the core team; no external CVE was issued.

## Sources

- [GitHub: monero-project/monero-gui](https://github.com/monero-project/monero-gui) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-monero-gui-README.md)
- [GitHub API: repo metadata](https://api.github.com/repos/monero-project/monero-gui) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-api-monero-project-monero-gui.json)
- [GitHub API: releases (10 most recent)](https://api.github.com/repos/monero-project/monero-gui/releases?per_page=10) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-api-monero-gui-releases.json)
- [getmonero.org/downloads/](https://www.getmonero.org/downloads/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-getmonero-org-downloads.html)
- [getmonero.org/resources/user-guides/](https://www.getmonero.org/resources/user-guides/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-getmonero-org-user-guides.html)
- [docs.getmonero.org/interacting/monero-wallet-gui-reference/](https://docs.getmonero.org/interacting/monero-wallet-gui-reference/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-getmonero-org-gui-reference.html)
- [Flathub API: org.getmonero.Monero stats](https://flathub.org/api/v2/stats/org.getmonero.Monero) — accessed 2026-08-12 — [archived](../sources/2026-08-12-flathub-api-monero-gui-stats.json)
- [GitHub source: pages/Mining.qml](https://raw.githubusercontent.com/monero-project/monero-gui/master/pages/Mining.qml) — accessed 2026-08-12
- [GitHub source: pages/settings/SettingsLayout.qml (SOCKS5 proxy)](https://raw.githubusercontent.com/monero-project/monero-gui/master/pages/settings/SettingsLayout.qml) — accessed 2026-08-12
- [GitHub source: src/libwalletqt/Wallet.h (Qt API)](https://raw.githubusercontent.com/monero-project/monero-gui/master/src/libwalletqt/Wallet.h) — accessed 2026-08-12
- [GitHub source: wizard/WizardModeSelection.qml](https://raw.githubusercontent.com/monero-project/monero-gui/master/wizard/WizardModeSelection.qml) — accessed 2026-08-12
