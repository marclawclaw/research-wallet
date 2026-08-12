---
title: "YWallet"
tags: [zcash, wallet, mobile, desktop, light-client, warp-sync, privacy, shielded]
created: 2026-08-12
accessed: 2026-08-12
ecosystem: Zcash
status: active
---

# YWallet

## Overview

YWallet is an independent Zcash (and formerly Ycash) light-client wallet developed by Hanh Huynh Huu (@hhanh00). It is notable for:

- **Warp Sync**: a proprietary fast block-scanning algorithm that processes roughly 10,000 blocks per second on a OnePlus 7T (Snapdragon 855+), making it the fastest shielded wallet in the Zcash ecosystem at time of publication
- **Multi-pool shielded coverage**: Transparent + Sapling + Orchard pools, Unified Addresses by default
- **Cold-wallet signing**: unsigned transaction workflow for air-gapped spending, without requiring a hardware wallet
- **Breadth of platform support**: Android, iOS, and desktop (Windows, macOS, Linux) from a single Flutter/Dart codebase

The wallet is no longer under active feature development. In May 2026 (v1.14.3), the author published a **deprecation notice** in the README and the app itself, redirecting users to his successor project **[[zkool2]]** (`https://github.com/hhanh00/zkool2`). YWallet will still receive security fixes and protocol updates (e.g. NU6.2 support was added in v1.15.0, 3 June 2026) but no new features.

**ZKool2 / Zkool** (`hhanh00/zkool2`, 35 stars, Rust) is a ground-up rewrite, Zcash-only (drops Ycash), and adds FROST threshold multisig, PCZT offline signing, per-account sync state, Tor proxy support, and flexible seed formats (12/15/18/21/24 words). As of 12 August 2026 it is under active development (latest release: zkool-v6.26.1, 2 August 2026).

## Licence & Platforms

| Property | YWallet (`zwallet`) | ZKool2 (`zkool2`) |
|----------|--------------------|--------------------|
| Licence | MIT | MIT |
| Language | Dart (Flutter) / Rust (zcash-sync FFI) | Rust |
| Platforms | Android 7.0+, iOS, Windows, macOS, Linux (AppImage, Flatpak, .msix, .dmg) | Android, Linux (AppImage, .deb, Flatpak, .iso), macOS (.dmg), Windows (.exe, .zip) |
| Latest stable version | v1.15.3 (4 June 2026) | zkool-v6.26.1 (2 August 2026) |
| Repo created | 2 July 2021 | 1 April 2025 |
| Status | **Deprecated — security/protocol fixes only** | Active development |

Desktop binaries shipped for YWallet v1.15.3 include: `Ywallet-latest-x86_64.AppImage`, `ywallet-universal.dmg`, `ywallet.flatpak`, `YWallet.msix`, `ywallet.zip` (source archive), and `zwallet.tgz`.

## Key Management

### YWallet (`zwallet`)

- **Seed format**: ZIP 32 compliant — imports standard Zcash mnemonic (seed phrase) **or** raw secret key (zcashd standard format). The README states "Import seed phrase (ZIP 32 compliant) or secret key (Zcashd standard)".
- **Multiple accounts per seed**: Yes — the wallet supports multiple accounts, each with a different seed, secret key, or viewing key. Account indices can be set at restore.
- **Watch-only accounts**: Yes — via Full Viewing Key (FVK); the account has full transaction visibility but cannot spend.
- **Viewing key export**: The wallet exposes viewing key export ("Seed & Keys" screen in docs). FVK is used for watch-only account import.
- **BIP39**: BIP39 24-word mnemonic (confirmed; iTunes App Store description: "Back up your wallet with 24 word phrase (BIP-39 compatible)"). The ZIP 32 derivation path is used. YWallet also supports sweeping funds from seed phrases at non-zero account indices (v1.13.3 fix).
- **Passphrase (25th word)**: [NOT FOUND] — not mentioned in README, CHANGELOG, or documentation.
- **PCZT** (Partially Completed Zcash Transaction — the Zcash PSBT-equivalent): **Not supported in YWallet**. The cold-wallet workflow uses a proprietary unsigned transaction file format transferred by USB OTG. PCZT is supported in the successor ZKool2.
- **Backup**: Seed phrase backup (standard). Desktop also supports database encryption at rest.

### ZKool2 (successor)

- **Seed format**: 12, 15, 18, 21, or 24 word mnemonic, with optional BIP39 passphrase and configurable account index. Also imports Unified Viewing Key, Sapling extended keys, xpub/xprv (BIP32/Electrum), BIP44 transparent, and raw private keys.
- **PCZT**: Yes — "Load/Save/Sign Transactions: Offline Signing, Cold Wallet, PCZT" confirmed in README feature table.
- **Multisig**: FROST threshold multisig — "Distributed Key Gen, FROST Signatures" confirmed in README.

## Shielded Pool Support

| Pool | YWallet | ZKool2 |
|------|---------|--------|
| Transparent (t-address) | Y | Y |
| Sapling | Y | Y |
| Orchard | Y | Y |
| Ironwood (NU6.3) | [NOT FOUND] — v1.15.0 added NU6.2 support (v1.15.3 fixed a circuit version detection bug); Ironwood migration support [NOT FOUND] | [NOT FOUND] — not mentioned in zkool2 README |
| Unified Addresses (ZIP 316) | Y — "Snap (diversified) addresses" confirmed | Y — "internal address derivation for change (Zashi, ZIP 316)" confirmed |
| ZIP 321 payment URIs | Y | Y — "Single/Multiple Recipients" confirmed in README |
| Memo (512-byte encrypted) | Y — "Memo" feature listed | Y — "Text, Binary, Full text search" |
| Auto-shielding | Y — "Automatic shielding above configurable threshold"; "One touch transparent account shielding" | N — explicitly listed as not implemented |

Note: YWallet shipped NU6.1 support in v1.14.0 (26 November 2025) and NU6.2 support in v1.15.0 (3 June 2026). The issue tracker records an open issue requesting Ironwood (NU6.3/NU7) support (issue #271, open as of 12 August 2026), indicating it has not yet been merged into v1.15.3.

## Warp Sync

[[zcash-warp-sync]]

Warp Sync is YWallet's proprietary algorithm for accelerating block scanning in a Zcash light client. Standard Zcash light-client scanning (per ZIP 307) requires downloading each block's compact transactions and testing each note commitment against the wallet's keys — an O(n) scan of commitment tree witnesses that is computationally expensive at scale.

Warp Sync, developed by @hhanh00 in the `zcash-sync` Rust library (submodule of `zwallet`), replaces the incremental Merkle witness update step with a batch witness computation algorithm. Rather than updating a witness for every note at every block, it accumulates leaf commitments and computes witnesses in bulk using a custom "Warp Merkle Tree" structure. The author claims this processes approximately **10,000 blocks per second** on a 2019 flagship device (OnePlus 7T, Snapdragon 855+).

The `zcash-sync` library (`hhanh00/zcash-sync`, 13 GitHub stars, Rust, last pushed 4 June 2026) is the standalone scanning engine. ZKool2 uses an "Improved Warp" synchronisation as its core algorithm with per-account sync state — each account can be individually enabled or disabled for sync.

YWallet does not operate its own lightwalletd server. Users connect to any standard Zcash lightwalletd gRPC endpoint; the default is the ECC/Zcash Foundation public server, but the URL is user-configurable in Settings.

## Hardware Wallet Support

YWallet **does not support hardware wallets** in its current version. Ledger integration was previously under development but was **removed** in v1.12.x (commit "remove ledger code", PR #204, approximately March 2025).

The cold-wallet workflow is instead a **software air-gap**:
1. A watch-only account is created in YWallet using an FVK
2. The spending wallet (separate device or offline computer) runs the companion `zcash-sync` CLI or a prior version of YWallet
3. YWallet creates an unsigned transaction file
4. The file is transferred via USB OTG or file copy to the offline device for signing
5. The signed transaction is returned to YWallet for broadcast

**ZKool2** supports hardware wallets via the PCZT workflow — transactions can be exported, signed offline, and re-imported. No specific hardware wallet integrations (Ledger, Keystone, Trezor) are confirmed in the zkool2 README or issue tracker as of 12 August 2026.

## Privacy Features

| Feature | YWallet | ZKool2 |
|---------|---------|--------|
| Tor support | N — not mentioned in README or documentation | Y — "TOR proxy and Onion services for all connections to Zcash servers" confirmed in README |
| No data upload | Y — README states "No data upload; all information recoverable from seed phrase or secret key" | [NOT FOUND] — likely similar by design |
| Custom lightwalletd URL | Y — user-configurable in Settings | Y — "Full Nodes & Light Nodes" supported |
| On-chain contacts (encrypted memo) | Y — contacts are stored in on-chain encrypted memos; "will never be lost" | N — "No address book" explicitly stated |
| Coin control (note selection) | Y — display and select individual notes; exclude from spending | Y — confirmed in README feature table |
| Database encryption | Y — database encryption supported on desktop | Y — "Database: Encrypted on disk" confirmed |

## Cross-chain Swaps

YWallet previously included a built-in swap feature, but it was **removed** in v1.14.1 (27 November 2025) along with the governance voting feature (commit "remove swaps and voting", PR #248). The documentation site (`ywallet.app/transacting/swaps/`) returns 404 as of 12 August 2026.

ZKool2 explicitly lists **no third-party swap integration** as a known limitation in its README.

## Audits & Security

| Item | Finding |
|------|---------|
| Security audits | [NOT FOUND] — no public security audit of YWallet or ZKool2 found as of 12 August 2026 |
| Bug bounty | [NOT FOUND] — no bug bounty programme found |
| Security documentation | YWallet docs (`ywallet.app/security`) describe PIN/fingerprint lock, database encryption, and cold wallet as security mitigations. No formal threat model published. |
| Notable security events | [NOT FOUND] |
| Responsible disclosure | [NOT FOUND] — no SECURITY.md in `zwallet` repo; zkool2 not checked for SECURITY.md as of 12 August 2026 |

The wallet is a solo-maintained open-source project with no organisational security infrastructure (no audits, no bug bounty, no formal threat model). The deprecation notice directs users to ZKool2, where the codebase is a ground-up rewrite in Rust (memory-safe language).

## Adoption Metrics

### YWallet (`zwallet` repo)

| Metric | Value | Source | Date |
|--------|-------|--------|------|
| GitHub stars (`zwallet`) | 62 | GitHub API | 2026-08-12 |
| GitHub forks (`zwallet`) | 43 | GitHub API | 2026-08-12 |
| GitHub stars (`zkool2`) | 35 | GitHub API | 2026-08-12 |
| GitHub forks (`zkool2`) | 18 | GitHub API | 2026-08-12 |
| iOS rating (all versions) | 4.28★ (25 ratings) | iTunes API (ID 1583859229) | 2026-08-12 |
| iOS current version | v1.15.3 | iTunes API | 2026-08-12 |
| iOS seller | Ycash Foundation | iTunes API | 2026-08-12 |
| Android rating | 4.65★ (85 ratings) | Google Play structured data | 2026-08-12 |
| Android installs (Google Play tier) | 10,000+ (precise: 12,213) | Google Play structured data | 2026-08-12 |
| Android package ID | `me.hanh.ywallet` | Google Play | 2026-08-12 |
| v1.15.3 APK (F-Droid) downloads | 235 | GitHub release assets | 2026-08-12 |
| v1.15.3 AppImage (Linux) downloads | 293 | GitHub release assets | 2026-08-12 |
| v1.15.3 macOS (.dmg) downloads | 301 | GitHub release assets | 2026-08-12 |
| v1.15.3 Windows (.msix) downloads | 28 | GitHub release assets | 2026-08-12 |
| v1.15.3 zip downloads | 887 | GitHub release assets | 2026-08-12 |
| v1.14.0 APK downloads (major release) | 1,068 | GitHub release assets | 2026-08-12 |
| v1.14.0 AppImage (Linux) downloads | 772 | GitHub release assets | 2026-08-12 |
| v1.14.0 macOS (.dmg) downloads | 849 | GitHub release assets | 2026-08-12 |
| MAU | [NOT FOUND] | — | — |

Note: The task brief cited App Store URL `https://apps.apple.com/us/app/ywallet/id1552532506`. The iTunes lookup for that ID returned 0 results. The app was located via search as ID 1583859229 (bundle ID: `me.hanh.ywallet.ios`, seller: Ycash Foundation). The earlier ID 1552532506 may correspond to a prior version or a different region listing that has since been removed.

## Sources

| Source | URL | Accessed | Archived |
|--------|-----|----------|---------|
| `zwallet` GitHub API | https://api.github.com/repos/hhanh00/zwallet | 2026-08-12 | `sources/2026-08-12-api-github-com-hhanh00-zwallet.json` |
| `zkool2` GitHub API | https://api.github.com/repos/hhanh00/zkool2 | 2026-08-12 | `sources/2026-08-12-api-github-com-hhanh00-zkool2.json` |
| `zwallet` releases API | https://api.github.com/repos/hhanh00/zwallet/releases?per_page=10 | 2026-08-12 | `sources/2026-08-12-api-github-com-hhanh00-zwallet-releases.json` |
| `zkool2` releases API | https://api.github.com/repos/hhanh00/zkool2/releases?per_page=10 | 2026-08-12 | `sources/2026-08-12-api-github-com-hhanh00-zkool2-releases.json` |
| `zwallet` README | https://raw.githubusercontent.com/hhanh00/zwallet/main/README.md | 2026-08-12 | `sources/2026-08-12-github-com-hhanh00-zwallet-README.md` |
| `zwallet` CHANGELOG | https://raw.githubusercontent.com/hhanh00/zwallet/main/CHANGELOG.md | 2026-08-12 | `sources/2026-08-12-github-com-hhanh00-zwallet-CHANGELOG.md` |
| `zwallet` LICENSE | https://raw.githubusercontent.com/hhanh00/zwallet/main/LICENSE | 2026-08-12 | `sources/2026-08-12-github-com-hhanh00-zwallet-LICENSE.txt` |
| `zkool2` README | https://raw.githubusercontent.com/hhanh00/zkool2/main/README.md | 2026-08-12 | `sources/2026-08-12-github-com-hhanh00-zkool2-README.md` |
| `zcash-sync` GitHub API | https://api.github.com/repos/hhanh00/zcash-sync | 2026-08-12 | `sources/2026-08-12-api-github-com-hhanh00-zcash-sync.json` |
| `zcash-sync` README | https://raw.githubusercontent.com/hhanh00/zcash-sync/main/README.md | 2026-08-12 | `sources/2026-08-12-github-com-hhanh00-zcash-sync-README.md` |
| YWallet website | https://ywallet.app | 2026-08-12 | `sources/2026-08-12-ywallet-app-home.html` |
| YWallet security docs | https://ywallet.app/security | 2026-08-12 | `sources/2026-08-12-ywallet-app-security.html` |
| ZKool2 homepage | https://hhanh00.github.io/zkool2/ | 2026-08-12 | `sources/2026-08-12-hhanh00-github-io-zkool2-home.html` |
| iTunes App Store (ID 1583859229) | https://itunes.apple.com/lookup?id=1583859229 | 2026-08-12 | `sources/2026-08-12-itunes-apple-com-ywallet-id-1583859229.json` |
| Google Play Store | https://play.google.com/store/apps/details?id=me.hanh.ywallet | 2026-08-12 | `sources/2026-08-12-play-google-com-ywallet-mehanh.html` |
| ZIP 307 (light client protocol) | https://zips.z.cash/zip-0307 | 2026-08-12 | `sources/2026-08-12-zips-z-cash-zip-0307.html` |
| `zwallet` GitHub issues | https://api.github.com/repos/hhanh00/zwallet/issues?state=all&per_page=20 | 2026-08-12 | `sources/2026-08-12-api-github-com-hhanh00-zwallet-issues.json` |

## Related notes

- [[zcash-warp-sync]] — Warp Sync algorithm detail
- [[zcash-pczt-hardware]] — PCZT offline signing (ZKool2)
- [[zodl]] — Zodl/Zashi (ECC-maintained Zcash wallet, contrasting approach)
- [[nighthawk]] — Nighthawk (another Zcash light client)
- [[zcash-shielded-transactions]] — Pattern note on Zcash shielded pool architecture
- [[psbt-hardware-signing]] — PSBT/PCZT pattern (Bitcoin analogue)
