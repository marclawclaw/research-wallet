---
title: "Nighthawk Wallet"
tags: [zcash, wallet, mobile, light-client, privacy, shielded, community, darkfi, deprecated]
created: 2026-08-12
accessed: 2026-08-12
ecosystem: Zcash (historical); DarkFi (active development as of August 2026)
status: Zcash support ended; DarkFi testnet wallet in active development
---

# Nighthawk Wallet

Nighthawk is a community-driven privacy wallet project maintained by Nighthawk Apps (based in Sunshine Coast, Australia). It was the longest-standing independent Zcash wallet outside of ECC/Zodl, having operated continuously from 2020 to late 2024 (Android) and to late 2025 (iOS). As of 2025–2026, the Nighthawk Zcash wallet has effectively ended support for Zcash, and the team has pivoted to building a DarkFi client suite (Android, iOS, Desktop, Moonshine CLI) on the DarkFi testnet.

**Critical context for this note:** The Zcash wallet line (versions 1.x and 2.x) is the subject of this research. The new DarkFi wallet (versions 3.x, under active development as of 12 August 2026) is a distinct product on a different network; it is described in the "Current Status" section but is not the primary focus.

## Identity and Provenance

- **Developer:** Nighthawk Apps — community team; not affiliated with ECC/Zodl or Zcash Foundation
- **Operational period (Zcash):** June 2020 (v1.0.0) through November 2024 (Android v2.2.16) and November 2025 (iOS v2.5.0)
- **Current focus:** DarkFi testnet (announced 1 August 2026); Zcash Sapling wallet apps are effectively legacy
- **GitHub org:** `nighthawk-apps` — https://github.com/nighthawk-apps
- **Contact:** nighthawkwallet@protonmail.com; @NighthawkWallet on X
- **Disclosure PGP fingerprint (Zcash era):** `8c07e1261c5d9330287f4ec35aff0fd018b01972`

## Repository Overview

| Repo | Status | Stars | Forks | Last push | Licence |
|------|--------|-------|-------|-----------|---------|
| `nighthawk-wallet-android` | **Archived** — v1.x Zcash Android | 32 | 15 | 4 December 2023 | Apache-2.0 |
| `nighthawk-android-wallet` | Active — v2.x Zcash / v3.x DarkFi | 17 | 2 | 11 August 2026 | MIT |
| `nighthawk-ios-wallet` | Active — v2.x Zcash / v3.x DarkFi | 5 | 1 | 11 August 2026 | MIT |
| `zcash-ios-wallet` | Archived — v1.x Zcash iOS | 2 | 0 | 4 December 2023 | [NF] |
| `darkfi-lightwalletd` | Active — DarkFi light server (Rust) | 0 | 0 | 6 August 2026 | [NF] |

Notes:
- The v2.x Android wallet (`nighthawk-android-wallet`) was forked from ECC's `secant-android-wallet` and uses the official Zcash Android SDK (`cash.z.ecc.android.sdk`). The `main` branch has been repurposed for DarkFi v3.x; the Zcash v2.x code lives on the `v2.2.16` branch.
- All licences confirmed via GitHub API (accessed 12 August 2026).

## Licence and Open-Source Status

- **Android (v2.2.16 / Zcash era):** MIT (confirmed via GitHub API 12 August 2026)
- **iOS (v2.2.16 / Zcash era):** MIT (confirmed via GitHub API 12 August 2026)
- **Archived Android v1.x:** Apache-2.0 (confirmed via GitHub API 12 August 2026)
- Source code fully open; no obfuscated build steps documented; F-Droid builds available (see below)

## Platforms and Distribution

**Android (Zcash era, final version v2.2.16):**
- **Package ID:** `com.nighthawkapps.wallet.android`
- **Latest Zcash release:** v2.2.16 (27 November 2024)
- **Google Play:** [NOT FOUND] — Play Store listing returned 404 as of 12 August 2026; app has been removed from Google Play
- **F-Droid:** Available at `https://f-droid.org/packages/com.nighthawkapps.wallet.android/` — F-Droid page confirms the package is still indexed (page loads as of 12 August 2026)
- **Language:** Kotlin (Jetpack Compose UI)

**iOS (Zcash era, final version v2.5.0):**
- **App Store ID:** 1524708337
- **Bundle ID:** `com.nighthawkapps.wallet.ios`
- **Latest Zcash release:** v2.5.0 (released 30 November 2025)
- **App Store rating:** 4.29★ (21 ratings) — accessed 12 August 2026
- **App Store status:** Active listing but wallet shows deprecation notice: *"Nighthawk no longer supports outgoing ZEC transactions, after the recent network upgrade. To access your funds, you will need to import your wallet seed to other dedicated Zcash wallets, such as Zashi or Zingo."*
- **Language:** Swift

## Zcash Version History Highlights

The Zcash wallet versions (v1.x and v2.x) introduced these notable features (derived from CHANGELOG of `nighthawk-wallet-android` master, accessed 12 August 2026):

| Version | Date | Key additions |
|---------|------|--------------|
| v1.0.0 | June 2020 | Initial release; Sapling-only; Nighthawk-operated lightwalletd server |
| v1.0.3 | July 2020 | Custom lightwalletd server support (user-configurable in Settings) |
| v1.0.6 | August 2020 | Biometric (Face/Touch ID) support; subaddress reply-to memo convention |
| v1.0.8 | November 2020 | Deshielding (z→t transactions) enabled |
| v1.0.9 | December 2020 | ZIP 313 (reduce default fee from 10,000 to 1,000 zats) |
| v1.0.20 | July 2021 | Auto-shielding workflow; t-address tabs; balance details screen |
| v1.0.24 | August 2021 | PIN code + biometric wallet lock |
| v1.0.26 | August 2021 | ZIP 321 payment URI deep links |
| v1.0.29 | October 2021 | MoonPay ZEC purchase integration |
| v1.0.31 | November 2021 | StealthEx.io cross-chain swap integration |
| v1.0.37 | May 2022 | NU5 support (Sapling-only; Orchard not yet integrated) |
| v2.2.16 | November 2024 | Final Zcash Android release; zec.rocks default lightwalletd endpoints |

**v2.x architecture note:** The v2.x series was a complete rewrite based on ECC's `secant-android-wallet` template, using Jetpack Compose UI, the official Zcash Android SDK (`libs.zcash.sdk`), and a structured custom-server selection screen (`ChangeServerViewModel`). The default lightwalletd endpoints in v2.2.16 are the `zec.rocks` regional nodes (NA, SA, EU, AP), not Nighthawk-operated servers.

## Key Management and Seed Format

Based on source code inspection of `nighthawk-android-wallet` v2.2.16 (accessed 12 August 2026):

- **Seed format:** ZIP 32 HD key derivation from a BIP39 mnemonic. The underlying Zcash Android SDK generates a 24-word BIP39 mnemonic and derives Unified Spending Keys via ZIP 32 (`m/purpose'/coin_type'/account'`). This is the same derivation path as Zodl.
- **Wallet birthday height:** Supported — on wallet restore, users can specify the block height to begin scanning from, avoiding a full chain rescan.
- **BIP39 passphrase (25th word):** [NOT FOUND] — not exposed in UI or source code found
- **Multiple accounts:** [NOT FOUND] — the v2.x UI does not appear to support multiple accounts under one seed (unlike Zodl, which supports a base account + Keystone hardware account)
- **Viewing key export:** [NOT FOUND] — no explicit viewing key export UI found in source inspection; the underlying SDK supports IVK/FVK but Nighthawk does not surface this in the UI
- **Backup flow:** Seed words shown on wallet creation and accessible in Settings → "Backup Wallet" (screen `settingbackupwallet` confirmed in v2.2.16 source); export to password-protected PDF confirmed in v1.0.21 (July 2021) for the v1.x series

## Shielded Pool Support and Sync Infrastructure

### Pool Support (Zcash v2.x)

| Pool | Support |
|------|---------|
| Transparent (t-address) | Y — send and receive; tabs confirmed in v1.0.20+ |
| Sapling (z-address) | Y — primary shielded pool; default for Nighthawk throughout its Zcash lifecycle |
| Orchard | Y — v2.x series built on official Zcash Android SDK which gained Orchard support in SDK v2.x; however, Nighthawk README (v2.2.16) explicitly disclaims: *"This wallet currently only supports sapling shielded pool, which makes it incompatible with wallets that support custom note management and orchard pool"* |
| Ironwood (NU6.3, 2026) | N — Nighthawk ended Zcash support before Ironwood activation |
| Unified Addresses (ZIP 316) | Y — v2.x UI shows UA as the default receive format per the `wallet-features.md` existing column |

**Important:** Nighthawk's Sapling-only limitation in v2.x means shielded funds cannot be received from Orchard-default wallets (e.g. Zodl) via the shielded path unless those wallets downgrade to Sapling. This was a significant interoperability gap.

### Nighthawk Sync Infrastructure (Zcash era)

**Historical:** Nighthawk Apps operated its own lightwalletd servers from v1.0.7 (August 2020). The v1.x default server was `mainnet.lightwalletd.com` (later migrated as flagged by `OLD_NIGHTHAWK_HOST_PATTERN = ".lightwalletd.com"` in v2.x source). Nighthawk was one of the first independent Zcash projects to operate public lightwalletd infrastructure separate from ECC.

**v2.2.16 default servers:** The v2.x release switched to `zec.rocks` regional nodes (operated by the Zcash community, not Nighthawk), with five predefined endpoints confirmed in `LightWalletServer.kt` (accessed 12 August 2026):
- `zec.rocks:443` (global default)
- `na.zec.rocks:443` (North America)
- `sa.zec.rocks:443` (South America)
- `eu.zec.rocks:443` (Europe)
- `ap.zec.rocks:443` (Asia-Pacific)

**Custom server:** Y — confirmed. The `ChangeServer` screen and `ChangeServerViewModel` in v2.2.16 allow users to add a `CustomServer` (hostname + port). Source: `LightWalletServer.kt` and `AndroidChangeServer.kt` (accessed 12 August 2026).

**Number of Nighthawk-operated lightwalletd nodes:** [NOT FOUND] — no current public inventory of Nighthawk-operated nodes found. The `lightwalletd-infra-setup-doc` repo exists but its README was not accessible (404 on 12 August 2026). The v2.x wallet no longer defaults to Nighthawk-operated servers.

### Current DarkFi Sync Infrastructure

Nighthawk Apps now operates `darkfi-lightwalletd` — a dedicated light wallet server for the DarkFi network, written in Rust. Key characteristics (from nighthawkapps.com blog post, 1 August 2026):
- Serves compact DarkFi blocks over gRPC
- Runs **UnifOMR** (Oblivious Message Retrieval, scheme 0x05) — private information retrieval so the server does not learn which blocks a client needs
- Cleartext refused off-loopback; TLS required for remote deployments with certificate pinning
- Does not log detection keys or match heights

## Hardware Wallet Support

**Zcash era (v1.x and v2.x):** N — No hardware wallet integration. Nighthawk did not support Ledger, Trezor, Keystone, or any other hardware wallet in either the v1.x or v2.x Zcash series. Source: README, CHANGELOG, and source inspection (accessed 12 August 2026); no hardware wallet library dependencies found.

**DarkFi era (v3.x):** [NOT FOUND] — hardware wallet support not mentioned in the 1 August 2026 press release or DarkFi wallet README.

## Privacy Features

### Zcash era

- **Shielded by default (Sapling):** Y — shielded sending/receiving is the primary wallet mode
- **Auto-shielding:** Y — auto-shielding workflow introduced in v1.0.20 (July 2021); configured via the auto-shielding trigger (reduced to 0.01 ZEC in v1.0.37)
- **Encrypted memo field:** Y — 512-byte encrypted Zcash memo supported
- **ZIP 321 payment URIs:** Y — deep link integration added v1.0.26 (August 2021)
- **Tor support:** N — no Tor integration found in any Zcash version source or CHANGELOG
- **Custom lightwalletd server:** Y — confirmed in v1.x and v2.x (see above); allows users to point to a trusted server

**Privacy limitations acknowledged in README (v2.2.16):**
1. Wallet depends on lightwalletd server — users must trust the server for accurate data
2. Network traffic analysis can leak some privacy
3. Sapling-only pool: incompatible with Orchard-default wallets

### DarkFi era (v3.x, testnet only as of August 2026)

- **Tor by default:** Y — embedded Arti SOCKS5 on iOS; SOCKS5 Tor on Android; recommended for Desktop/CLI
- **UnifOMR sync:** Y — private information retrieval prevents server from learning which blocks a client needs
- **DarkIRC:** Y — end-to-end encrypted in-process DarkIRC chat on Android and iOS
- **Range padding and poll jitter:** Y — additional sync obfuscation described in press release

## Cross-Chain Swaps and DeFi

**Zcash v1.x era:**
- **StealthEx.io swap:** Y — added in v1.0.31 (November 2021); KYC-free cross-chain swap within the app
- **MoonPay ZEC purchase:** Y — added in v1.0.29 (October 2021); fiat-to-ZEC on-ramp
- **SideShift.ai:** Y — v1.0.5 (August 2020) listed SideShift integration for affiliate swaps

**Zcash v2.x era:**
- **Flexa branch:** A `flexa-integration` branch exists in `nighthawk-android-wallet` (not merged to the v2.2.16 tag as of 12 August 2026). Flexa NFC payments were planned but not confirmed shipped in a stable Zcash release.
- **StealthEx / MoonPay:** [NOT FOUND] — not confirmed in v2.x releases

**DarkFi era:**
- Cross-chain swap: [NOT FOUND] — not mentioned in DarkFi testnet announcement

## Audits and Security

- **Security audits:** [NOT FOUND] — no public security audit report found for either the v1.x or v2.x Zcash wallet series. Nighthawk references the Zcash Android SDK threat model (`https://zcash.readthedocs.io/en/latest/rtd_pages/wallet_threat_model.html`) but does not link to a Nighthawk-specific audit.
- **Bug bounty:** [NOT FOUND] — no formal bug bounty programme found. The README describes a responsible disclosure process (email nighthawkwallet@protonmail.com with PGP fingerprint `8c07e1261c5d9330287f4ec35aff0fd018b01972`).
- **Notable security events:** No known high-severity security events in public record as of 12 August 2026.
- **Dependency on ECC SDK:** The Nighthawk v2.x series inherits the security properties of the ECC Zcash Android SDK; any SDK-level vulnerabilities affect Nighthawk.

## Adoption Metrics

| Metric | Value | Source | Date |
|--------|-------|--------|------|
| Android GitHub stars (nighthawk-android-wallet, active) | 17 | GitHub API | 12 August 2026 |
| Android GitHub stars (nighthawk-wallet-android, archived) | 32 | GitHub API | 12 August 2026 |
| iOS GitHub stars (nighthawk-ios-wallet, active) | 5 | GitHub API | 12 August 2026 |
| iOS GitHub forks (nighthawk-ios-wallet) | 1 | GitHub API | 12 August 2026 |
| Android GitHub forks (nighthawk-android-wallet) | 2 | GitHub API | 12 August 2026 |
| Android Google Play installs | [NOT FOUND] — app removed from Play Store | — | 12 August 2026 |
| iOS App Store rating | 4.29★ (21 ratings) | iTunes lookup API (ID 1524708337) | 12 August 2026 |
| iOS App Store version | 2.5.0 (released 30 November 2025) | iTunes lookup API | 12 August 2026 |
| Android F-Droid | Listed at f-droid.org; install count [NOT FOUND] | F-Droid | 12 August 2026 |

**Assessment:** Nighthawk's adoption metrics are low compared to Zodl (51,907 Android installs; 191 ratings). The wallet has a very small user base and has effectively ended Zcash support. Its historical significance is as the longest-standing community Zcash wallet operating independently of ECC.

## Current Status (August 2026)

As of 12 August 2026:

1. **Zcash is abandoned:** Both Android (v2.2.16, November 2024) and iOS (v2.5.0, November 2025) Zcash wallets have ended. The iOS App Store listing explicitly states outgoing ZEC transactions no longer work, directing users to Zodl/Zingo.
2. **DarkFi pivot:** Nighthawk Apps announced its DarkFi wallet suite (Android, iOS, Desktop, Moonshine CLI + `darkfi-lightwalletd` server) on 1 August 2026. This is testnet-only, highly experimental, and not in production use.
3. **Repositories are active** — both `nighthawk-android-wallet` and `nighthawk-ios-wallet` had commits on 11 August 2026, indicating active DarkFi development.

The Nighthawk Zcash wallet's significance for the research-wallet project is primarily historical: it demonstrates the community lightwalletd infrastructure model and the features available in a Sapling-era Zcash light client. For current Zcash wallet comparison, Zodl is the primary reference.

## Relation to Other Notes

- [[zodl]] — primary active Zcash mobile wallet; Orchard-native, Keystone hardware wallet support
- [[ywallet]] — second active Zcash mobile wallet; multi-account, advanced features
- [[zcash-shielded-transactions]] — Zcash pool architecture (Transparent, Sapling, Orchard, Ironwood)
- [[lightwalletd-sync]] — lightwalletd trust model and sync architecture (pattern note; create if needed)

## Sources

| Source | URL | Accessed |
|--------|-----|----------|
| GitHub API: nighthawk-apps/nighthawk-android-wallet | https://api.github.com/repos/nighthawk-apps/nighthawk-android-wallet | 12 August 2026 |
| GitHub API: nighthawk-apps/nighthawk-ios-wallet | https://api.github.com/repos/nighthawk-apps/nighthawk-ios-wallet | 12 August 2026 |
| GitHub API: nighthawk-apps/nighthawk-wallet-android (archived) | https://api.github.com/repos/nighthawk-apps/nighthawk-wallet-android | 12 August 2026 |
| GitHub API: nighthawk-apps org repos | https://api.github.com/orgs/nighthawk-apps/repos?per_page=50 | 12 August 2026 |
| nighthawk-android-wallet README (v2.2.16) | https://raw.githubusercontent.com/nighthawk-apps/nighthawk-android-wallet/v2.2.16/README.md | 12 August 2026 |
| nighthawk-android-wallet LightWalletServer.kt (v2.2.16) | https://raw.githubusercontent.com/nighthawk-apps/nighthawk-android-wallet/v2.2.16/ui-lib/src/main/java/co/electriccoin/zcash/ui/screen/changeserver/model/LightWalletServer.kt | 12 August 2026 |
| nighthawk-android-wallet AndroidChangeServer.kt (v2.2.16) | https://raw.githubusercontent.com/nighthawk-apps/nighthawk-android-wallet/v2.2.16/ui-lib/src/main/java/co/electriccoin/zcash/ui/screen/changeserver/AndroidChangeServer.kt | 12 August 2026 |
| nighthawk-wallet-android CHANGELOG (archived, v1.x) | https://raw.githubusercontent.com/nighthawk-apps/nighthawk-wallet-android/master/CHANGELOG.md | 12 August 2026 |
| nighthawk-ios-wallet README (main/DarkFi) | https://raw.githubusercontent.com/nighthawk-apps/nighthawk-ios-wallet/main/README.md | 12 August 2026 |
| Apple App Store API (ID 1524708337) | https://itunes.apple.com/lookup?id=1524708337&country=us | 12 August 2026 |
| Google Play Store (404 — app removed) | https://play.google.com/store/apps/details?id=com.nighthawkapps.wallet.android | 12 August 2026 |
| Nighthawk Apps blog: DarkFi Testnet announcement | https://nighthawkapps.com/blog/nighthawk-darkfi-wallet-suite-testnet/ | 12 August 2026 |
| nighthawk-apps.github.io blog post source | https://raw.githubusercontent.com/nighthawk-apps/nighthawk-apps.github.io/master/_posts/2026-07-20-nighthawk-darkfi-wallet-suite-testnet.md | 12 August 2026 |
| nighthawk-android-wallet gradle.properties (v2.2.16) | https://raw.githubusercontent.com/nighthawk-apps/nighthawk-android-wallet/v2.2.16/gradle.properties | 12 August 2026 |
| nighthawk-android-wallet releases API | https://api.github.com/repos/nighthawk-apps/nighthawk-android-wallet/releases?per_page=20 | 12 August 2026 |
| nighthawk-ios-wallet releases API | https://api.github.com/repos/nighthawk-apps/nighthawk-ios-wallet/releases?per_page=10 | 12 August 2026 |
| F-Droid listing | https://f-droid.org/packages/com.nighthawkapps.wallet.android/ | 12 August 2026 |
