---
tags: [wallet, monero, android, mobile, java, apache-2.0, self-custody]
category: self-custody software wallet
website: https://www.monerujo.app
github: https://github.com/m2049r/xmrwallet
launched: 2017
license: Apache-2.0
---

# Monerujo

Monerujo is the original Monero-native Android wallet, developed by a pseudonymous developer (m2049r) and first released in August 2017. It is Android-only, lightweight (remote node, no local blockchain download), and fully open-source (Apache-2.0). As of 12 August 2026, the repository has 688 GitHub stars, 290 forks, and 60 contributors. The latest release is v4.1.7 "Exolix" (17 June 2025). The project has had over 100 releases since inception.

Monerujo is notable for several unique features: PocketChange (output pre-splitting to eliminate change-output delays), Sidekick (Bluetooth-connected second phone as a software hardware wallet), CrAzYpass (local file encryption with a generated high-entropy password), CeasarSeed (seed offset/passphrase), Street Mode (balance hide), and Exolix KYC-free swap integration. Ledger Nano S hardware wallet support is provided via USB OTG.

---

## Adoption / usage metrics

| Metric | Value | Date | Source |
|--------|-------|------|--------|
| GitHub stars | 688 | 2026-08-12 | [GitHub API](https://api.github.com/repos/m2049r/xmrwallet) — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet.json) |
| GitHub forks | 290 | 2026-08-12 | [GitHub API](https://api.github.com/repos/m2049r/xmrwallet) — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet.json) |
| GitHub open issues | 217 | 2026-08-12 | [GitHub API](https://api.github.com/repos/m2049r/xmrwallet) — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet.json) |
| GitHub contributors | 60 | 2026-08-12 | [GitHub contributors API](https://api.github.com/repos/m2049r/xmrwallet/contributors?per_page=100) — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet-contributors.json) |
| GitHub watches/subscribers | 42 | 2026-08-12 | [GitHub API](https://api.github.com/repos/m2049r/xmrwallet) |
| Total releases (GitHub) | 100+ (API page saturated at 100) | 2026-08-12 | [GitHub releases API](https://api.github.com/repos/m2049r/xmrwallet/releases) — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet-releases.json) |
| Latest release | v4.1.7 "Exolix" | 2025-06-17 | [GitHub](https://github.com/m2049r/xmrwallet/releases/tag/v4.1.7) |
| GitHub: latest release APK downloads | 10,642 (v4.1.7) | 2026-08-12 | [GitHub releases API](https://api.github.com/repos/m2049r/xmrwallet/releases) — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet-releases.json) |
| GitHub: v4.1.6 downloads | 4,149 | 2026-08-12 | same |
| GitHub: v4.1.5 downloads | 2,402 | 2026-08-12 | same |
| Google Play installs (tier) | 100,000+ | 2026-08-12 | [Google Play](https://play.google.com/store/apps/details?id=com.m2049r.xmrwallet) — [archived](../sources/2026-08-12-play-google-com-monerujo.html) |
| Google Play rating | 3.19 / 5 | 2026-08-12 | [Google Play structured data](https://play.google.com/store/apps/details?id=com.m2049r.xmrwallet) — [archived](../sources/2026-08-12-play-google-com-monerujo.html) |
| Google Play rating count | 1,176 | 2026-08-12 | [Google Play structured data](https://play.google.com/store/apps/details?id=com.m2049r.xmrwallet) — [archived](../sources/2026-08-12-play-google-com-monerujo.html) |
| F-Droid availability | Y (official Monerujo F-Droid repo: https://f-droid.monerujo.io/fdroid/repo/) | 2026-08-12 | [README](https://github.com/m2049r/xmrwallet/blob/master/README.md) — [archived](../sources/2026-08-12-github-com-m2049r-xmrwallet-README.md) |
| F-Droid installs | [NOT FOUND] — F-Droid does not publish install counts | — | — |
| Monthly Active Users (MAU) | [NOT FOUND] — structurally unavailable; see note | — | — |
| Primary programming language | Java + C++ (Monero core, libwallet) | 2026-08-12 | [GitHub API](https://api.github.com/repos/m2049r/xmrwallet) |
| Repo created on GitHub | 2017-08-02 | — | [GitHub API](https://api.github.com/repos/m2049r/xmrwallet) |
| Last push to master | 2025-06-17 | 2026-08-12 | [GitHub API](https://api.github.com/repos/m2049r/xmrwallet) |
| Monero core version bundled | v0.18.3.4 (as of v4.1.5, November 2024) | 2024-11-01 | [v4.1.5 release notes](https://github.com/m2049r/xmrwallet/releases/tag/v4.1.5) — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet-releases.json) |

**Note on MAU:** Monero's privacy architecture (RingCT, stealth addresses, no public address reuse) makes on-chain active-address attribution structurally impossible. No Monero wallet project publishes MAU. GitHub stars (688) and Google Play tier (100,000+) are used as adoption proxies.

**Note on maintenance:** The last push to `master` was 17 June 2025 — over 13 months before this research date (12 August 2026). This suggests reduced maintenance activity relative to Cake Wallet (pushed 2026-08-11) and Monero GUI (active). The project is not archived on GitHub.

---

## Key management

- **Seed format:** 25-word Monero standard mnemonic (electrum-style). No Polyseed support (confirmed: 0 search results for "polyseed" in the codebase). No BIP39.
- **Seed offset (CeasarSeed):** Y — users can set a seed passphrase offset when restoring from seed. The `bSeedOffset` UI button is present in `GenerateFragment.java`; the passphrase is applied as a word-list offset to the 25-word seed. This is a Monero-native passphrase extension (distinct from BIP39 passphrase).
- **Multiple wallets:** Y — the wallet list screen manages multiple independent `.keys` files. Each wallet is a separate Monero wallet with its own seed.
- **Multiple accounts per wallet:** Y — Monero HD-style sub-accounts supported within a single wallet (`accounts_drawer_new` string; `WalletFragment.java` account index). Users can create and label multiple accounts.
- **Subaddresses:** Y — full subaddress support (`SubaddressFragment.java`, `SubaddressInfoAdapter`). Users can generate and label subaddresses per account.
- **View key export:** Y — view key displayed in wallet details ("Your view key can be used to monitor incoming transactions without giving permission to spend"). Source: `help.xml` and `LoginFragment.java`.
- **Watch-only wallet:** Y — users can create a watch-only wallet from address + view key (`TYPE_VIEWONLY` in `LoginFragment.java` and `GenerateFragment.java`; FAQ documents this feature). Source: [FAQ](https://raw.githubusercontent.com/m2049r/xmrwallet/master/doc/FAQ.md) — [archived](../sources/2026-08-12-github-m2049r-xmrwallet-doc-FAQ.md).
- **Key import:** Y — wallet files (`.keys` file + cache file) can be imported by copying to the Monerujo folder on the device. Users can also restore from keys manually.

---

## Signing

- **On-device signing:** Y — transaction construction and signing are performed locally on the Android device via the bundled `libwallet` / Monero C++ library. No keys leave the device (for non-hardware-wallet wallets).
- **Monero transaction construction:** handled by the embedded Monero Core v0.18.3.4 library (`libwallet2_api`), compiled for Android via NDK. The `libdevice_trezor.a` static library is also compiled in (from `app/CMakeLists.txt`).
- **Offline / air-gapped signing:** N — no air-gapped signing workflow (no animated QR, no microSD, no NFC export). This is a lightweight online wallet; Sidekick (see below) provides an offline-key variant but requires Bluetooth connectivity, not true air-gap.
- **PocketChange:** Y — a pre-splitting feature that automatically creates spare change outputs with every transaction, maintaining a pool of ~6 outputs at a selected denomination (configurable via `PocketChangeFragment`). Eliminates the typical 20-minute output lock delay on consecutive spending. Source: `strings.xml` (`pocketchange_info`); landing page — [archived](../sources/2026-08-12-monerujo-app-home.html).

---

## Hardware wallet

- **Ledger support:** Y — Ledger Nano S (and Nano S series) via USB OTG cable (`BTChipTransportAndroidHID`, `Ledger.java`). Minimum firmware v1.6.0 (`MINIMUM_LEDGER_VERSION`). USB HID protocol (same as desktop Ledger integration in Monero Core). The website states: "Works with Ledger Nano S series with a USB OTC cable." The help text refers specifically to Nano S; newer models (X, Stax, Flex) are not mentioned in docs. Source: [landing page](https://www.monerujo.app) — [archived](../sources/2026-08-12-monerujo-app-home.html); [help.xml](https://raw.githubusercontent.com/m2049r/xmrwallet/master/app/src/main/res/values/help.xml).
- **Trezor support:** Library compiled in (`device_trezor` static lib, `Wallet.Device.Trezor` enum entry), but no Trezor UI path found in `LoginFragment.java` or `GenerateFragment.java`. Trezor is in the model enum alongside Ledger and Sidekick, suggesting partial backend support. UI support not confirmed from source inspection; [NOT FOUND] in public documentation.
- **Sidekick (Bluetooth software hardware wallet):** Y — Monerujo includes a companion app ([m2049r/sidekick](https://github.com/m2049r/sidekick), 17 stars, Apache-2.0) that turns a secondary Android phone into an offline key store. The secondary phone holds the private keys and authorises each transaction over Bluetooth; the primary Monerujo phone handles networking and UI. Described as a "virtually air-gapped" solution at zero hardware cost. Source: [Sidekick README](https://raw.githubusercontent.com/m2049r/sidekick/master/README.md) — [archived](../sources/2026-08-12-github-m2049r-sidekick-README.md); `help.xml` (`help_sidekick`).
- **Kastelo / Sekura hardware wallet:** Historical — m2049r developed firmware for a dedicated Monero hardware wallet (Kastelo/Sekura, repo: `monerujo-hw`, 5 stars), last pushed 2018; this device never reached mass production and is not supported in current releases.

---

## UX

- **Architecture:** Lightweight; always requires a remote node. No full local node option.
- **Node selection:** Y — users choose from a curated list of community-trusted nodes or enter a custom node (host:port with optional username:password authentication). NodeScan automatically finds open public Monero nodes. Source: landing page; FAQ; `NodeFragment.java`.
- **Default nodes:** 3 default nodes + history of last 5 used nodes (FAQ). Updated to current nodes in v4.1.6.
- **Custom node with auth:** Y — `username:password@node.address:port` format supported. Source: FAQ.
- **Mainnet / Stagenet / Testnet:** Y — selectable via product flavours at build time; separate app IDs for each network variant.
- **QR code scanning:** Y — receive address QR with XMR/USD/EUR conversion via Kraken API; QR scanning for payments.
- **OpenAlias:** Y — human-readable Monero addresses (e.g. `monerujo.io`) resolved via DNSSEC. Source: `OpenAliasHelper.java`; `strings.xml`.
- **Street Mode:** Y — hides wallet balances and hides previous transactions from view while enabled. Source: `strings.xml` (`menu_streetmode`); landing page.
- **Background Lock:** Y — wallet continues syncing while locked (background service). Source: landing page; FAQ ("Background updating - make sure you exit the wallet to stop updating").
- **Fee estimation:** Y — handled by the bundled Monero core library; standard dynamic fee from node.
- **Coin control (sweep):** P — "Send all confirmed funds in this account!" sweep-all function present (`send_sweepall`), but no per-output manual coin control UI (unlike Feather Wallet). Partial.
- **Biometric unlock:** Y — optional fingerprint unlock (`FingerprintHelper.java`).
- **i18n / translations:** Y — Weblate translation project; supports Spanish, Portuguese (BR), Turkish, Arabic, Swedish, French, Czech, Ukranian, Tamil, and others. Source: README; v4.1.6 and v4.1.7 release notes.

---

## Backup and recovery

- **Mnemonic seed (25-word) backup:** Y — seed displayed during wallet creation and accessible later from wallet details. Clipboard copy disabled as a security measure (prevents clipboard sniffing by other apps). Source: FAQ.
- **CrAzYpass:** Y — wallet `.keys` files are encrypted on-device using a generated high-entropy random passphrase (the "Restore Password") derived from a slow-hash. This password is displayed to the user and must be recorded for backup. Protects wallet files if the device storage is accessed without the app. Source: `CrazyPassEncoder.java`; `help.xml`; FAQ.
- **ZIP backup:** Y — `ZipBackup.java` packages the `.keys` file and cache into a ZIP archive (written to user-selected URI). Used for manual off-device backup. Source: `ZipBackup.java`.
- **Manual wallet file import:** Y — users can copy existing Monero wallet files (`.keys` + cache) into the Monerujo folder. Source: FAQ.
- **Restore height:** Y — supported during restore from seed. Accepts block number or YYYY-MM-DD date. Source: `help.xml`; `RestoreHeight.java`.
- **Passphrase restore:** Y (CeasarSeed offset); no BIP39 passphrase (not applicable to 25-word Monero seed).

---

## Privacy

- **Tor:** Y — via Orbot (Guardian Project NetCipher integration). Toggle in the wallet list screen routes all node connections through Tor. Onion-address nodes are supported (`.onion` detection via `OnionHelper.java`). Requires Orbot installed separately on the device. Source: `NetCipherHelper.java`; `help.xml` (`help_tor`; `help_tor_enable`) — source-confirmed.
- **I2P support:** N — [NOT FOUND] in source or documentation.
- **Custom node for privacy:** Y — users can point to their own Monero full node, eliminating reliance on third-party nodes.
- **No telemetry:** Y — privacy policy states all data is stored locally and not collected remotely. The only external data calls are: node connections, Kraken API for XMR/USD rate, and Exolix API when using swap. Source: `privacy-policy.md` — [archived](../sources/2026-08-12-monerujo-app-home.html).
- **Stealth addresses:** Handled by Monero Core library (see `[[monero-stealth-address]]`).

---

## BTC/cross-chain swap

- **Exolix integration:** Y — current active swap provider (`ShiftService.EXOLIX`, enabled). Supports XMR → BTC, LTC, ETH, USDT, SOL (from `ShiftService` enum: `"XMR:BTC:LTC:ETH:USDT:SOL"`). KYC-free, non-custodial exchange. Sends the BTC destination address and amount to Exolix; user's IP is also transmitted. Source: `ShiftService.java`; `ExolixApiImpl.java`; `about.xml`.
- **SideShift:** Listed in `ShiftService` enum (`SIDESHIFT`) but `enabled = false` — disabled as of current codebase.
- **XMR.TO:** Listed in `ShiftService` enum (`XMRTO`) but `enabled = false` — XMR.TO shut down in 2021.
- **Custodial risk:** Exolix is a centralised exchange. Transactions are routed via Exolix's API. Source: `about.xml` privacy policy text.

---

## Open source and security

- **Licence:** Apache-2.0 (confirmed from `LICENSE` file). Note: the existing wallet-features.md Monerujo column lists "MIT" — this is incorrect and is updated in the feature table below.
- **APK signature:** SHA-256 `967ca1930c19f383a43729a059cd21727855f78cd5e0d9882f780189fe1d8cf1` (Signer DN: `CN=m2049r`). APK hash published per release. Source: v4.1.7 release notes — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet-releases-latest.json).
- **Reproducible builds:** N — no reproducible build process, no multi-signer attestation. The CI pipeline (`circleci/config.yml`) builds and tests but does not attest builds. Source: `.circleci/config.yml`.
- **Security audit:** [NOT FOUND] — no public security audit report found for Monerujo.
- **Bug bounty:** [NOT FOUND] — no public bug bounty programme found.
- **HackerOne / responsible disclosure:** [NOT FOUND] — no security contact or responsible disclosure policy found in the repo.
- **Build verification:** SHA-256 hash and `apksigner` certificate DN published with each GitHub release, enabling users to verify their download. Source: release notes.

---

## Limitations and criticisms

1. **Android-only:** No iOS, no desktop (Windows, macOS, Linux). This is a fundamental platform constraint. Cake Wallet supports iOS, Android, macOS, and Linux.
2. **No Polyseed:** Only 25-word Monero seed supported. Polyseed encodes the creation date in the seed, eliminating the need to record a restore height. Monerujo users must manually record the restore height or rescan from block 0 (can take hours).
3. **No Trezor UI:** Although `device_trezor` is compiled in and the `Wallet.Device.Trezor` enum exists, no Trezor-specific UI flow was found in source inspection. Ledger is the only confirmed hardware wallet path in the UI.
4. **No air-gapped signing:** No offline transaction signing via QR, microSD, or NFC. Sidekick provides a Bluetooth-paired offline key option but is not a true air-gap.
5. **No Polyseed, no I2P, no multisig UI, no P2Pool mining:** All features present in Feather Wallet or Monero GUI are absent.
6. **Low Play Store rating:** 3.19/5 from 1,176 ratings — lower than typical wallet ratings, likely reflecting the niche user base and historic issues.
7. **Maintenance pace:** Last GitHub push 17 June 2025 (>13 months before research date). Cake Wallet by contrast was pushed 11 August 2026. Activity has slowed significantly in 2025–2026.
8. **Monero core version lag:** Bundled Monero Core v0.18.3.4 (from November 2024 release). As of August 2026, upstream Monero is at v0.18.5.x. The bundled library version has not been updated in v4.1.6 or v4.1.7.

---

## Sources

- [GitHub API: m2049r/xmrwallet](https://api.github.com/repos/m2049r/xmrwallet) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet.json)
- [GitHub releases API](https://api.github.com/repos/m2049r/xmrwallet/releases) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet-releases.json)
- [GitHub releases API (latest)](https://api.github.com/repos/m2049r/xmrwallet/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet-releases-latest.json)
- [GitHub contributors API](https://api.github.com/repos/m2049r/xmrwallet/contributors?per_page=100) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-m2049r-xmrwallet-contributors.json)
- [README.md](https://raw.githubusercontent.com/m2049r/xmrwallet/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-com-m2049r-xmrwallet-README.md)
- [FAQ.md](https://raw.githubusercontent.com/m2049r/xmrwallet/master/doc/FAQ.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-m2049r-xmrwallet-doc-FAQ.md)
- [Monerujo website (monerujo.app)](https://www.monerujo.app) — accessed 2026-08-12 — [archived](../sources/2026-08-12-monerujo-app-home.html)
- [v4.1.5 blog post](https://www.monerujo.app/monerujo-v4-1-5.html) — accessed 2026-08-12 — [archived](../sources/2026-08-12-monerujo-app-v415-release.html)
- [Sidekick README](https://raw.githubusercontent.com/m2049r/sidekick/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-m2049r-sidekick-README.md)
- [Google Play: com.m2049r.xmrwallet](https://play.google.com/store/apps/details?id=com.m2049r.xmrwallet) — accessed 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-monerujo.html)
- Source inspection: `SettingsFragment.java`, `GenerateFragment.java`, `LoginFragment.java`, `NetCipherHelper.java`, `OnionHelper.java`, `Ledger.java`, `ShiftService.java`, `ExolixApiImpl.java`, `ZipBackup.java`, `help.xml`, `strings.xml`, `about.xml`, `app/CMakeLists.txt`, `build.gradle` — accessed 2026-08-12 via GitHub raw API
