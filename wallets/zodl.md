---
title: "Zodl (formerly Zashi) — Zcash Self-Custody Wallet"
subject: zodl
ecosystem: Zcash
tags: [zcash, shielded, orchard, mobile, ios, android, self-custody, zodl, zashi, ecc, light-client]
created: 2026-08-12
accessed: 2026-08-12
---

# Zodl (formerly Zashi)

Zodl is the primary Zcash mobile wallet for non-technical users. Built by the same team that created Zcash itself, it is the most privacy-maximalist Zcash wallet in production: shielded by default, Orchard-native, and built on top of the official Zcash Android SDK and Zcash Swift SDK. The wallet was rebranded from Zashi to Zodl in late February 2026 when the core wallet development team split from Electric Coin Company (ECC) and formed the independent Zcash Open Development Lab (ZODL).

## Identity and Provenance

- **Current name:** Zodl (branded "Zodl: Zcash Wallet" on both stores)
- **Former name:** Zashi (developed and released under ECC; renamed Zashi → Zodl February 2026)
- **Developer:** Zcash Open Development Lab (ZODL) — formerly the ECC wallet team; independent entity since early 2026. The App Store lists the artist as "The Zerocoin Electric Coin Company" (legacy registration) but the Play Store lists "Zcash Open Development Lab (ZODL)" as the author.
- **GitHub org:** `zodl-inc` — repos `zodl-inc/zodl-android` (Kotlin) and `zodl-inc/zodl-ios` (Swift)
- **Licence:** MIT (both Android and iOS repos confirmed via GitHub API, 2026-08-12)
- **Rebrand date:** Version 3.0.0 (build 1466), 25 February 2026 — CHANGELOG entry: "Rebrand to Zodl"
- **Backing:** $25M from Paradigm / a16z at time of rebrand (noted in [[_index]])

## Platforms and Distribution

- **Android:** Google Play Store — package ID `co.electriccoin.zcash` (legacy ECC ID, unchanged post-rebrand). Google Play structured data: 50,000+ installs (precise figure: 51,907), rating 4.43/5 (191 ratings). Latest: v3.9.2 (build 2370), released 2026-08-10.
- **iOS:** Apple App Store — App ID 1672392439, bundle ID `co.electriccoin.secant-mainnet`. iOS rating: 4.97/5 (118 ratings). Latest version: 3.9.2, released 2026-08-11. Initial release: 2024-03-28.
- **F-Droid:** Zodl runs its own F-Droid repository at `https://foss.zodl.com` (not f-droid.org; own repo ships the same build-time integrations as Google Play including Flexa, CMC, Crashlytics, signed with the same upload key). Direct APK also available from GitHub releases.
- **No desktop build:** Mobile only; no macOS, Windows, or Linux client.

## Zcash Privacy Architecture

Zodl is built on Zcash's layered privacy model. Understanding the wallet requires understanding the three Zcash note-commitment pools:

- **Transparent (t-addresses):** Fully public on-chain; no privacy. Zodl can send to and receive from transparent addresses but shielded-by-default routing minimises t-address exposure.
- **Sapling (z-addresses, original shielded pool):** Zero-knowledge shielded transactions. Deprecated in favour of Orchard from NU5 (2022) onward; Zodl retains Sapling support for legacy receiving.
- **Orchard (current shielded pool, NU5+):** Improved circuit design over Sapling; uses Pasta curves; the recommended default. Zodl routes all new transactions to Orchard by default.
- **Ironwood (NU6.3, July 2026):** New shielded pool activated in Zcash's Ironwood network upgrade. Zodl added Ironwood support in v3.8.0 (2026-07-27). In v3.9.0 (2026-08-08), Zodl added "Move to Ironwood" — a guided migration that moves Orchard funds into the new Ironwood pool, either as a private batch of smaller background transfers or as an immediate single transfer.
- **Unified Addresses (UAs):** ZIP 316 — a single address that encodes both shielded (Orchard or Sapling) and transparent receivers. Zodl uses UAs as the default receive address format; senders without shielded capability are routed to the transparent component while shielded senders go to the Orchard component automatically.

## Key Management

- **Seed phrase:** 24-word BIP39 mnemonic. Zodl uses the Zcash standard derivation path via ZIP 32 (`m/purpose'/coin_type'/account'`), which derives a Unified Spending Key (USK). The USK controls both Orchard and Sapling keys from a single seed.
- **Birthday height:** Supported. When restoring a wallet, Zodl allows the user to specify the block height from which scanning begins (the wallet's "birthday"), dramatically reducing sync time by skipping older blocks the wallet could not have received funds in.
- **Passphrase:** No BIP39 passphrase (25th word) extension — not found in source or documentation.
- **Viewing keys:** The underlying SDK exposes Incoming Viewing Keys (IVK) and Full Viewing Keys (FVK) but Zodl's UI does not surface these directly for the base account. The threat model confirms the wallet holds spending keys; key material never leaves the device.
- **Keystone hardware wallet:** Y — Zodl integrates with the Keystone hardware wallet (air-gapped QR-based signing). Added in v1.3 (December 2024) for Android; expanded significantly through 2025–2026 with wallet birthday height on Keystone import (v3.4), firmware version enforcement (requires Keystone firmware ≥3.0.1 for Ironwood migration signing), and Coinholder Polling governance from Keystone (v3.6). No other hardware wallets are supported. Ledger Zcash app is a separate integration not found in Zodl.
- **No other hardware wallets:** Ledger, Trezor, and other hardware wallets are not supported in Zodl.

## Shielded Transactions — Send and Receive

- **Default:** All send/receive defaults to Orchard shielded. The wallet specifically uses PCZT (Partially Created Zcash Transaction) format internally for Keystone signing.
- **Auto-shielding:** Y — Zodl has a shielding panel in the Balances screen (added in v0.3, January 2024) that prompts users to shield transparent funds. Auto-shielding via SDK is supported: "new transactions and shield transparent funds" is listed as a Keystone integration feature. v2.1 CHANGELOG: "We fixed a bug that prevented shielding when many small transparent inputs were involved." This is opt-triggered, not automatic on receipt, but the wallet actively prompts the user.
- **Transparent transactions:** Supported (send and receive). Zodl sends to ZIP 320 (TEX) transparent exchange addresses by automatically de-shielding to a fresh ephemeral transparent address, then making a second fully transparent transaction — this de-shielding/reshielding pattern is designed to prevent address linkage.
- **Memo field:** Y — Zodl fully exposes the Zcash encrypted memo field in send/receive flows. ZIP 321 payment URIs (QR-code encoded payment requests including amount + memo) are scanned and the memo is pre-filled in the send screen. CHANGELOG 2025: "Scanning zip321 now properly prefills memo."
- **ZIP 321 payment URIs:** Y — full support for ZIP 321 (payment request URIs). Added in v0.5 (mid-2024); refined through multiple releases.
- **Unified Addresses:** Y — default receive format.

## Synchronisation

- **Architecture:** Light client. Zodl does not download the full blockchain. It uses the `lightwalletd` protocol: a gRPC-based compact block server operated by Zodl or community nodes. Compact blocks contain only note commitments and nullifiers; the wallet decrypts relevant notes client-side.
- **Sync time:** First-run sync can be slow (minutes to tens of minutes depending on wallet birthday and server load) because the wallet must process all compact blocks since the birthday height to find relevant notes. The wallet displays sync progress.
- **Spend-before-Sync:** The Zcash Android SDK (on which Zodl is built) has implemented a "Spend-before-Sync" compact block processing algorithm that processes blocks non-linearly to discover spendable balance faster — still in beta as of SDK README.
- **Server selection:** Automatic by default (v3.7.0+: broadcasts transactions across multiple servers simultaneously for reliability). Advanced Settings allows custom lightwalletd server.
- **Tor protection:** Y — opt-in Tor for IP address protection. Can be enabled from Settings or prompted on the home screen. Exchange rate fetches are always routed over Tor when enabled; as of v3.7 (a pre-v3.7 note): exchange rates are no longer requested over a direct non-Tor connection even when Tor Protection is disabled. Torification uses an embedded Tor client in the SDK.

## Privacy Limitations (from threat model)

The wallet publishes a formal invariant-centric threat model (`zodl-inc/zodl-project/wallet_threat_model.md`). Key known weaknesses:

1. **Lightwalletd trust:** Zodl is a light client — users must trust the lightwalletd operator not to lie about blockchain state. A compromised lightwalletd can make users think they have funds they don't, or hide received transactions. Mitigation: ZIP 307 block header validation is planned but not yet implemented.
2. **Bandwidth side channels:** A network-level adversary can tell when a user receives a shielded transaction (memo fetch causes a bandwidth spike), correlate paired senders/receivers using the same lightwalletd instance, and determine IP address (mitigated by Tor opt-in).
3. **Dust attacks:** An address-knowing adversary can tell when a user spends funds sent by that adversary (by watching the transparent input count on-chain).
4. **Wallet fingerprinting:** Note selection differences may distinguish Zodl from other wallets to a blockchain observer.
5. **Transparent t-address leakage:** T-addresses are fully public; Zodl does not force users to shield before spending, so transparent balance is exposed if users choose transparent receive.

## Additional Features

- **Built-in swap:** Y — Zodl includes in-wallet ZEC↔other-crypto swaps (ZEC to BTC, stablecoins, etc.). Swap was introduced in v1.2 (late 2024); the Swap button moved to a prominent position in v1.4+. Slippage protection enforced on the swap provider's minimum-amount bounds. In-wallet cross-chain payments route shielded ZEC → recipient's preferred asset ("Zodl Crosspay" feature described in the App Store listing).
- **Flexa payments:** Y — Zodl integrates Flexa for NFC-based merchant payments. Flexa allows spending ZEC at participating merchants without KYC. Listed as "Pay with Flexa" / "More options" in the app. Present from v1.2 onward; Firebase Crashlytics and Flexa are listed as build-time integrations in the F-Droid note.
- **Currency conversion:** Y — USD/ZEC (and multi-fiat, added v3.7.0) via CMC (CoinMarketCap); fetched over Tor when Tor Protection enabled.
- **Address book:** Y — encrypted address book with Android auto-backup and cloud backup support.
- **Coinholder Polling (governance):** Y — private on-chain voting on Zcash governance proposals from within the wallet, compatible with both Zodl and Keystone accounts (added v3.6, 2026-06).
- **ZIP 320 (TEX addresses):** Y — supports sending to transparent exchange (TEX) addresses via auto de-shielding with fresh ephemeral transparent address.
- **Multiple accounts:** Y — account switcher added in v1.3 (2024-12-19). Supports the base Zodl account plus Keystone-connected hardware wallet accounts.
- **No multisig:** Native Zcash multisig (threshold signing) is not supported. Keystone integration provides a 1-of-1 hardware wallet signing flow, not multisig.
- **Watch-only:** Not confirmed in Zodl UI (Keystone account type is an air-gapped signer, not strictly watch-only).
- **Crash reporting:** Firebase Crashlytics — fully opt-in since Zashi 2.0 (v2.0.0, April 2025).

## Relationship to ECC and ZODL

- **Electric Coin Company (ECC):** Created Zcash in 2016; built the Zashi wallet (originally under the "Secant" codename) and the Zcash Android SDK and Swift SDK. ECC still maintains the Zcash protocol; the SDKs remain in the `zcash/` and `Electric-Coin-Co/` GitHub namespaces and are maintained separately from ZODL.
- **Zcash Open Development Lab (ZODL):** The independent organisation formed by the former ECC wallet team in February 2026. ZODL raised $25M (Paradigm + a16z) and owns the Zodl brand, the `zodl-inc` GitHub org, and the wallet store listings. The Android package ID `co.electriccoin.zcash` was retained for continuity (no user reinstall required).
- **Trust implications:** ZODL is the official successor to Zashi for end users; the Zcash SDK foundations (cryptography, sync, zero-knowledge proof generation) remain in ECC-stewarded libraries. This creates a layered trust relationship: users trust ZODL for the UI/UX/swap integrations and ECC (via SDKs) for the underlying cryptographic machinery.
- **Open-source governance:** The wallet has an open Responsible Disclosure policy, a formal threat model, and open GitHub issue trackers. No bug bounty programme with monetary rewards was found (distinct from Monero wallets which have explicit ZMR bounties).

## Adoption Metrics

- **Android:** 50,000+ installs (Google Play structured data — exact figure 51,907 per embedded JSON); rating 4.43/5 (191 ratings) — accessed 2026-08-12.
- **iOS:** App Store rating 4.97/5 (118 ratings); initial release 2024-03-28; v3.9.2 released 2026-08-11 — accessed 2026-08-12.
- **GitHub stars:** zodl-android: 95; zodl-ios: 90 — both at `zodl-inc` org, accessed 2026-08-12.
- **MAU:** [NOT FOUND] — neither ZODL nor ECC publish wallet-level MAU. Zcash does not support wallet-level attribution.
- **On-chain impact:** Zodl's Unified Address default routing is the documented driver of the Zcash shielded pool surge: shielded transactions reached 59.3% of all Zcash transactions in February 2026 (up from ~30% in early 2025; up from 8% in early 2024). The Orchard pool alone holds 4.2 million ZEC (25.4% of circulating supply) as of May 2026. This is the strongest available proxy for Zodl adoption significance — no other wallet has driven a comparable on-chain behavioural shift in the Zcash ecosystem.

## Limitations

- **Zcash-only:** No multi-chain support for self-custody. Crosspay and swaps are outbound-only (ZEC shielded → recipient's chain asset); the wallet holds only ZEC.
- **Small absolute user base:** 50,000+ Android installs is small compared to any major Bitcoin or Ethereum wallet (MetaMask: 143M lifetime downloads). The entire Zcash ecosystem is niche.
- **Sync time:** Light client sync can take several minutes on first launch. Ironwood migration (added v3.9.0) adds another sync-intensive background operation.
- **Lightwalletd dependency:** Users depend on ZODL-operated or community lightwalletd servers. Custom server selection is available in Advanced Settings but requires technical knowledge to configure.
- **No passphrase (25th word):** BIP39 passphrase extension is not supported, reducing flexibility for advanced users wanting a hidden wallet.
- **Single hardware wallet (Keystone only):** No Ledger, Trezor, or other hardware wallet support.
- **Crashlytics and CMC:** Build-time integrations with Firebase (Crashlytics, opt-in) and CoinMarketCap (exchange rates) mean the FOSS build differs from the main release. The F-Droid repo (`foss.zodl.com`) ships the Play-equivalent build including Flexa and Crashlytics, unlike the stricter f-droid.org policy. This is a transparency trade-off.
- **Zcash market dynamics:** ZEC's market cap and mainstream adoption remain limited; Zodl's user ceiling is bounded by Zcash ecosystem size.

## Relation to Other Notes

- [[zcash-shielded-transactions]] — Zcash privacy model, Orchard vs Sapling vs transparent pools
- [[_index]] — Zcash wallet discovery, ecosystem context, shortlist rationale
- [[ywallet]] — alternative Zcash wallet (power-user, desktop + mobile, multi-account)
- [[nighthawk]] — community-built Zcash mobile wallet, F-Droid distributed

## Sources

| Source | URL | Accessed | Archived |
|--------|-----|----------|---------|
| GitHub API: zodl-inc/zodl-android | https://api.github.com/repos/zodl-inc/zodl-android | 2026-08-12 | [archived](../sources/2026-08-12-api-github-com-zodl-inc-zodl-android.json) |
| GitHub API: zodl-inc/zodl-ios | https://api.github.com/repos/zodl-inc/zodl-ios | 2026-08-12 | [archived](../sources/2026-08-12-api-github-com-zodl-inc-zodl-ios.json) |
| zodl-android CHANGELOG | https://raw.githubusercontent.com/zodl-inc/zodl-android/main/CHANGELOG.md | 2026-08-12 | [archived](../sources/2026-08-12-github-zodl-inc-zodl-android-CHANGELOG.md) |
| zodl-android README | https://github.com/zodl-inc/zodl-android | 2026-08-12 | [archived](../sources/2026-08-12-github-zodl-inc-zodl-android-README.md) |
| zodl-ios README | https://github.com/zodl-inc/zodl-ios | 2026-08-12 | [archived](../sources/2026-08-12-github-zodl-inc-zodl-ios-README.md) |
| zodl-project README | https://github.com/zodl-inc/zodl-project | 2026-08-12 | [archived](../sources/2026-08-12-github-zodl-inc-zodl-project-README.md) |
| Zodl threat model | https://raw.githubusercontent.com/zodl-inc/zodl-project/master/wallet_threat_model.md | 2026-08-12 | [archived](../sources/2026-08-12-github-zodl-inc-zodl-project-threat-model.md) |
| zodl-inc GitHub org repos | https://api.github.com/orgs/zodl-inc/repos | 2026-08-12 | [archived](../sources/2026-08-12-api-github-com-orgs-zodl-inc-repos.json) |
| zodl-android releases (top 5) | https://api.github.com/repos/zodl-inc/zodl-android/releases?per_page=5 | 2026-08-12 | [archived](../sources/2026-08-12-api-github-com-zodl-inc-zodl-android-releases.json) |
| zodl-ios releases (top 5) | https://api.github.com/repos/zodl-inc/zodl-ios/releases?per_page=5 | 2026-08-12 | [archived](../sources/2026-08-12-api-github-com-zodl-inc-zodl-ios-releases.json) |
| Apple App Store (App ID 1672392439) | https://itunes.apple.com/lookup?id=1672392439 | 2026-08-12 | [archived](../sources/2026-08-12-itunes-apple-com-zodl-ios-id.json) |
| Google Play Store listing | https://play.google.com/store/apps/details?id=co.electriccoin.zcash | 2026-08-12 | [archived](../sources/2026-08-12-play-google-com-zodl-android.html) |
| Zcash Android SDK README | https://github.com/zcash/zcash-android-wallet-sdk | 2026-08-12 | [archived](../sources/2026-08-12-github-zcash-android-sdk-README.md) |
| ZIP 321: Payment Request URIs | https://zips.z.cash/zip-0321 | 2026-08-12 | [archived](../sources/2026-08-12-zips-z-cash-zip-0321.html) |
| ECC blog: Zashi is now Zodl | https://electriccoin.co/blog/zashi-is-now-zodl (URL returns 404 at time of fact-check 2026-08-12; rebrand date confirmed via CHANGELOG and app store archives) | 2026-08-12 | [archived](../sources/2026-08-12-electriccoin-co-blog-zashi-is-now-zodl.html) |
| CryptoNews: Zcash shielded pool 30% | https://cryptonews.net/news/analytics/32936591/ | 2026-08-10 | [archived](../sources/2026-08-10-crypto-news-net-zcash-shielded-pool-30pct.html) |
| Coin Bureau: Best Zcash Wallets | https://coinbureau.com/analysis/best-zcash-wallets | 2026-08-10 | [archived](../sources/2026-08-10-coinbureau-com-best-zcash-wallets.html) |
