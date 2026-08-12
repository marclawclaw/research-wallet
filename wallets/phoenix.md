---
tags: [wallet, bitcoin, lightning, mobile, ios, android, self-custodial, acinq, splicing]
applies_to: [bitcoin, lightning-network]
created: 2026-08-12
---

# Phoenix Wallet

Phoenix is a self-custodial Bitcoin Lightning wallet for iOS and Android, developed by [ACINQ](https://acinq.co) — the Lightning Network infrastructure company behind the Eclair routing node implementation. It is the reference implementation of a consumer-grade, non-custodial Lightning wallet using the single-channel + splicing model.

## Identity

| Field | Value |
|-------|-------|
| Developer | ACINQ (Paris, France) |
| Platforms | iOS (16.0+), Android (8.0+) |
| Language | Kotlin (Kotlin Multiplatform via `lightning-kmp`) |
| License | Apache-2.0 |
| GitHub | https://github.com/ACINQ/phoenix |
| GitHub stars | 857 (2026-08-12) |
| GitHub forks | 143 |
| Contributors | 38 |
| Created | 20 June 2019 |
| Latest release | android-v2.8.1 (18 June 2026) |
| App ID (Android) | `fr.acinq.phoenix.mainnet` |
| Bundle ID (iOS) | `co.acinq.phoenix` |

## Architecture

Phoenix's defining characteristic is its **single-channel model** enabled by **splicing** (introduced in v2.0.0, August 2023). Each user has exactly one Lightning channel with ACINQ's node. Funds are added to the channel via splice-in (a Bitcoin transaction that expands the channel) and removed via splice-out (a Bitcoin transaction that directly pays an on-chain address from the channel without any intermediate swap service).

Prior to v2.0.0, Phoenix managed multiple channels per user (legacy behaviour, now migrated away). The v2.0.0 rewrite also migrated from Eclair (Scala, Android-only) to `lightning-kmp` (Kotlin Multiplatform, iOS + Android).

**Taproot channels** (introduced v2.7.0, October 2025): Lightning channels are now created as taproot channels, reducing on-chain fees by ~15% and making channel operations indistinguishable from regular Bitcoin transactions, improving on-chain privacy.

Lightning routing uses **trampoline routing** via ACINQ's node — Phoenix does not discover routes independently but delegates route computation to its channel peer (ACINQ). This is a deliberate UX trade-off to avoid requiring Phoenix to maintain a full routing table on a mobile device.

## Key management

- **Seed format:** 12-word BIP39 mnemonic (English), generated on-device at wallet creation
- **Derivation:** BIP32 HD; BIP84 derivation path (`m/84'/0'/0'`) for the on-chain (swap-in/final) wallet; Lightning keys derived separately within `lightning-kmp`
- **Passphrase (BIP39 extension):** [NOT FOUND] — FAQ does not mention BIP39 passphrase support; no evidence in release notes
- **Seed backup:** Manual backup only — user copies the 12-word phrase; ACINQ does not store the seed
- **Channel state backup:** Encrypted channel state is backed up to ACINQ's server (peer storage / SCB mechanism); the user's seed is sufficient to recover funds via force-close
- **Cross-platform restore:** A seed created on Android can be restored on iOS and vice versa (payments history does not transfer between platforms)

## Lightning features

| Feature | Support |
|---------|---------|
| Self-custodial Lightning | Y — user holds channel keys; no custodial intermediary |
| Trampoline routing | Y — via ACINQ node (only peer Phoenix connects to) |
| Single-channel model | Y — one channel per user (since v2.0.0) |
| Splice-in (on-chain → LN) | Y — trustless since v2.0.0 |
| Splice-out (LN → on-chain) | Y — trustless, user sets feerate, can bump via CPFP |
| Taproot channels | Y — new channels always taproot since v2.7.0 (Oct 2025); existing channels migrate to taproot on next on-chain operation |
| BOLT11 invoices | Y |
| BOLT12 offers | Y — static reusable invoices (v2.3.1, July 2024); multiple offers with custom descriptions (v2.6.0, May 2025) |
| LNURL-pay | Y |
| LNURL-withdraw | Y (fixed bug v2.6.1) |
| BIP353 DNS addresses | Y — paying BIP353 addresses (v2.3.3, July 2024) |
| NFC payments | Y — NFC read + broadcast (v2.6.1, June 2025); iOS tag emulation limited to EEA users (Apple restriction) |
| Lightning addresses in contacts | Y (v2.6.0) |
| Spending PIN (separate from lock PIN) | Y (v2.6.1) |
| Multiple wallets (single app instance) | Y — manage multiple wallets, each with name/photo/PIN (v2.7.0, Oct 2025) |
| Background payments (Android) | Y — via Firebase Cloud Messaging (FCM) wake-up notifications |
| Background payments (iOS) | Y — via iOS Notification Center + ACINQ's push service |

**Minimum receive amount:** No minimum, but mining fees apply when no channel exists. Payments are rejected if fee exceeds user's configured maximum (default: 5000 sat or 50% of amount).

**Inbound liquidity:** Phoenix tracks inbound liquidity within the single channel. Users can request liquidity from ACINQ in advance (paid service) to avoid on-chain fees during receiving spikes.

## On-chain capabilities

Phoenix is explicitly a **Lightning-first wallet** — there is no standalone on-chain balance. All funds are in the Lightning channel. On-chain send/receive is supported transparently via splice-out and splice-in respectively.

| Feature | Support |
|---------|---------|
| On-chain receive (BTC address) | Y — P2TR (taproot) address by default since v2.2.0 (Feb 2024); P2WSH legacy available via Settings |
| On-chain receive (address rotation) | Y — since v2.2.0 |
| On-chain send | Y — splice-out; trustless; user sets feerate; CPFP bump supported |
| On-chain address format | P2TR (taproot) default; P2WSH fallback for legacy service compatibility |
| Confirmation requirement (swap-in) | 3 confirmations before funds usable |
| Force-close recovery | Y — BIP84 path (native segwit p2wpkh) compatible with Electrum |

**Fee structure for on-chain operations:**
- On-chain deposits (swap-in): mining fees only (no percentage fee)
- On-chain sends (splice-out): mining fees (user-chosen feerate)
- Receiving LN payment with insufficient liquidity: 1% + mining fees
- Requesting inbound liquidity in advance: 1% + mining fees
- New channel creation fee: 1000 sat flat
- Sending via Lightning: 0.4% + 4 sat (fixed, shown before payment)
- Receiving via Lightning (sufficient liquidity): 0 (no fee)

## Backup and recovery

- **On-chain recovery:** Standard BIP39 + BIP84 path — any BIP84-compatible wallet (e.g. Electrum) can recover on-chain funds from the seed
- **Channel funds recovery:** If channels are force-closed, funds settle on-chain after a 720-block delay (~5 days). Visible in Settings > Wallet Info > Final Wallet
- **Payments history (iOS):** Automatically backed up to iCloud (unless disabled)
- **Payments history (Android):** Manual export/import of encrypted payments database (v2.6.0+); encrypted file survives uninstall; import at restore time

## Privacy

Phoenix's privacy posture is acknowledged as weak by ACINQ themselves:

> "The current version of Phoenix offers no advantage regarding privacy over existing, hosted, custodial wallets. We (ACINQ) know the final destination and amount of payments."
> — Phoenix FAQ, accessed 2026-08-12

This is a structural consequence of trampoline routing: because ACINQ's node routes all Phoenix payments, it observes sender, recipient, and amount. ACINQ has stated that future versions will improve privacy via blinded paths and other mechanisms.

**On-chain privacy:** Since v2.7.0, taproot channels make channel open/close/splice transactions indistinguishable from regular Bitcoin transactions, improving on-chain privacy.

**Tor:** Phoenix removed its embedded Tor library in v2.5.0 (March 2025) due to performance issues (especially with background payments). Tor access now requires a third-party Tor Proxy VPN app (e.g. Orbot) installed separately. The FAQ notes that enabling Tor may *increase* payment failure rates.

**Custom Electrum server:** Users can configure Phoenix to use their own Electrum server to monitor the blockchain and channels, reducing third-party dependency.

## PhoenixD (server/daemon)

ACINQ released **phoenixd** in March 2024 as a server/headless equivalent of Phoenix wallet for developers and node operators. It uses the same `lightning-kmp` engine and single-channel model. Latest version: v0.9.0 (13 July 2026). Supports Linux x86/ARM, macOS x86/ARM, Windows (WSL). Kotlin Multiplatform. Apache-2.0. GitHub: https://github.com/ACINQ/phoenixd (180 stars, 20 forks, 2026-08-12).

PhoenixD v0.9.0 adds Electrum blockchain client support and swap-in support. Includes an HTTP API.

## US market exit (May 2024)

ACINQ removed Phoenix from the US App Store (iOS) in May 2024, citing regulatory uncertainty in the United States. The blog post was titled along the lines of "Phoenix Wallet is no longer available in the United States on iOS" (URL: `https://acinq.co/blog/phoenix-self-custody-us-ios` — site is a JavaScript SPA; page not directly archiveable via curl).

**Impact:** US users on iOS lost access to the App Store listing. Android remained available on Google Play globally. US users could still side-load the APK from GitHub. This decision was driven by perceived regulatory risk — specifically, concerns that providing self-custodial Lightning wallets in the US might expose ACINQ to classification as a money services business (MSB).

## Open-source and security

- **Licence:** Apache-2.0
- **GPG release signing:** Yes — key E04E48E72C205463 (from v2.5.0; prior releases used key 7A73FE77DE2C4027). Signing key available at `https://acinq.co/pgp/drouinf2.asc`. SHA256SUMS.asc published per release.
- **Reproducible builds:** [NOT FOUND] — no public reproducible build process documented in repository or release notes
- **Security audit:** [NOT FOUND] — no public audit report found as of 2026-08-12
- **Bug bounty / disclosure:** [NOT FOUND] — no public programme found; contact is phoenix@acinq.co

## Adoption metrics

| Metric | Value | Source | Date |
|--------|-------|--------|------|
| GitHub stars | 857 | GitHub API | 2026-08-12 |
| GitHub forks | 143 | GitHub API | 2026-08-12 |
| Contributors | 38 | GitHub API | 2026-08-12 |
| Google Play installs | 100,000+ | Google Play Store | 2026-08-12 |
| Google Play rating | 4.27 / 5.0 | Google Play Store | 2026-08-12 |
| Google Play reviews | 867 | Google Play Store | 2026-08-12 |
| APK downloads (last 10 GitHub releases) | 22,351 | GitHub releases API | 2026-08-12 |
| iOS installs | [NOT FOUND] | App Store — no public data | — |
| MAU | [NOT FOUND] | No self-reported figure | — |

Note: The 100,000+ Google Play install bracket is the display label; the raw figure in the Play Store data is 164,112 (likely cumulative installs, not current MAU).

## Dependencies

| Component | Description |
|-----------|-------------|
| `lightning-kmp` | ACINQ's Kotlin Multiplatform Lightning implementation (shared between iOS + Android) — https://github.com/ACINQ/lightning-kmp |
| ACINQ's Lightning node | Phoenix must connect to ACINQ's node; no option to connect to arbitrary nodes |
| Eclair | ACINQ's server-side Lightning node (the peer Phoenix connects to) — https://github.com/ACINQ/eclair |
| Firebase Cloud Messaging | Used for wake-up notifications on Android (background payment receipt) |
| Electrum (optional) | User can configure own Electrum server for blockchain monitoring |
| mempool.space | Default feerate estimation source (can fall back to manual picker if unavailable) |

## Limitations

1. **ACINQ dependency:** Phoenix can only connect to ACINQ's node — no option to use arbitrary peers or a self-hosted Lightning node. If ACINQ disappears, users must force-close channels.
2. **Payment privacy:** ACINQ observes all payment metadata (sender, recipient, amount) due to trampoline routing.
3. **No hardware wallet support:** No Ledger, Trezor, or any hardware wallet integration.
4. **No multisig:** Single-key only.
5. **No coin control or UTXO management:** Phoenix abstracts all on-chain UTXO management; users have no access to individual UTXOs.
6. **No watch-only mode:** Cannot import an xpub or watch an address.
7. **No Taproot receive for non-Phoenix wallets:** On-chain address is P2TR; some older services reject it (workaround: switch to P2WSH in settings).
8. **US iOS market exit:** Not available on US App Store since May 2024.
9. **GrapheneOS:** Requires sandboxed Google Play Services for push notifications to work reliably.
10. **Tor integration removed:** As of v2.5.0, Tor requires a separate VPN app; enabling it may increase payment failures.
11. **No reproducible builds:** No documented reproducible build process.

## Sources

| Source | URL | Accessed |
|--------|-----|----------|
| GitHub API — ACINQ/phoenix | https://api.github.com/repos/ACINQ/phoenix | 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenix.json) |
| GitHub releases (latest 10) | https://api.github.com/repos/ACINQ/phoenix/releases?per_page=10 | 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenix-releases.json) |
| GitHub releases (latest 20) | https://api.github.com/repos/ACINQ/phoenix/releases?per_page=20 | 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenix-releases-20.json) |
| GitHub releases (latest) | https://api.github.com/repos/ACINQ/phoenix/releases/latest | 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenix-releases-latest.json) |
| GitHub README | https://raw.githubusercontent.com/ACINQ/phoenix/master/README.md | 2026-08-12 — [archived](../sources/2026-08-12-github-com-acinq-phoenix-README.md) |
| Phoenix FAQ (full text) | https://phoenix.acinq.co/faq | 2026-08-12 — [archived](../sources/2026-08-12-phoenix-acinq-co-faq-full.txt) |
| Phoenix homepage | https://phoenix.acinq.co | 2026-08-12 — [archived](../sources/2026-08-12-phoenix-acinq-co-home.html) |
| Splicing blog post (text extract) | https://acinq.co/blog/phoenix-splicing-update | 2026-08-12 — [archived](../sources/2026-08-12-acinq-co-blog-phoenix-splicing-update.txt) |
| PhoenixD GitHub API | https://api.github.com/repos/ACINQ/phoenixd | 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenixd.json) |
| PhoenixD README | https://raw.githubusercontent.com/ACINQ/phoenixd/master/README.md | 2026-08-12 — [archived](../sources/2026-08-12-github-com-acinq-phoenixd-README.md) |
| PhoenixD latest release | https://api.github.com/repos/ACINQ/phoenixd/releases/latest | 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-phoenixd-releases-latest.json) |
| Google Play Store listing | https://play.google.com/store/apps/details?id=fr.acinq.phoenix.mainnet | 2026-08-12 — [archived](../sources/2026-08-12-play-google-com-phoenix-mainnet.html) |
| lightning-kmp GitHub API | https://api.github.com/repos/ACINQ/lightning-kmp | 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-lightning-kmp.json) |
| Eclair GitHub API | https://api.github.com/repos/ACINQ/eclair | 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-acinq-eclair.json) |
| ACINQ blog: Phoenix self-custody US iOS withdrawal | https://acinq.co/blog/phoenix-self-custody-us-ios | 2026-08-12 — [archived (JavaScript SPA shell — page not archiveable; no Wayback Machine snapshot available)](../sources/2026-08-12-acinq-co-blog-us-exit.html) |

## Related notes

- [[patterns/self-custodial-lightning]] — Phoenix's single-channel + splicing model contrasted with LNDhub
- [[patterns/lndhub-lightning]] — BlueWallet's custodial hub model (contrasts with Phoenix)
- [[wallets/bluewallet]] — uses trampoline routing via ACINQ (same ACINQ node); custodial default
- [[wallets/electrum]] — desktop Lightning wallet with different trampoline implementation
