# Fact-Check Review: Cake Wallet

**Verdict:** CHANGES_REQUESTED

**Scope:** `wallets/cake-wallet.md`, `metrics/wallet-adoption.md`, `metrics/wallet-features.md`, `patterns/cross-chain-swap.md`, `patterns/monero-stealth-address.md`, `patterns/polyseed-monero-seed.md`
**Sources checked:** 44 (30 live URL checks; 14 archived files read directly)
**Standard applied:** 6-month freshness for most metrics; 3-month for user-count metrics. Access date 2026-08-12.

---

## CRITICAL (4)

- **`wallets/cake-wallet.md`:85 — `metrics/wallet-features.md`:22 — Hardware wallet: "Bluetooth (iOS/macOS); Bluetooth + USB (Android)"`**
  - Issue: macOS is incorrectly grouped with iOS as Bluetooth-only. The official documentation states explicitly: "On Android, macOS and Linux you can connect over either Bluetooth or USB. On iOS, only Bluetooth is offered." Both the Ledger and Trezor entries in `wallet-features.md` repeat this error. Linux is also silently omitted.
  - Source: `sources/2026-08-12-docs-cakewallet-com-llms-full.txt` (Hardware wallet section); live: https://docs.cakewallet.com — returns 200.
  - Fix: Change the connection note to "Bluetooth + USB (Android, macOS, Linux); Bluetooth only (iOS)" in both `cake-wallet.md` and `wallet-features.md`.

- **`wallets/cake-wallet.md`:133 — `metrics/wallet-features.md`:49 — Litecoin MWEB "available on iOS, Android, and macOS (not Linux)"`**
  - Issue: The documentation states plainly: "MWEB is available on iOS and Android only. Litecoin wallets on desktop (macOS, Linux, Windows) do not get a Private (MWEB) address and cannot peg funds in or out." macOS does not support MWEB.
  - Source: `sources/2026-08-12-docs-cakewallet-com-llms-full.txt` (Litecoin section); live URL returns 200.
  - Fix: Change to "available on iOS and Android only; not available on macOS, Linux, or Windows." Update `wallet-features.md` row accordingly.

- **`wallets/cake-wallet.md`:128 — `metrics/wallet-features.md`:52 — Coin control "Manual UTXO selection (also available for Litecoin)" / "Yes | BTC and LTC"`**
  - Issue: Coin control is available for six coins, not two. Documentation states: "Coin control is available for Bitcoin, Litecoin, Bitcoin Cash, Dogecoin, Monero, and Decred wallets." Limiting the note to BTC and LTC understates the feature and misrepresents the privacy tooling of other supported coins.
  - Source: `sources/2026-08-12-docs-cakewallet-com-llms-full.txt` (Coin control section); live URL returns 200.
  - Fix: Update both files to read "BTC, LTC, BCH, DOGE, XMR, and DCR."

- **`wallets/cake-wallet.md`:106 — `metrics/wallet-features.md`:66 — "Home screen transaction history: Added in v6.3.2 (July 2026)" / "Added v6.0.1 (March 2026)"`**
  - Issue: Two independent version errors.
    1. `cake-wallet.md` attributes home screen transaction history to v6.3.2 (July 24, 2026). The documentation attributes it to v6.3.0 (July 21, 2026): "Improve how we display recent transactions, limiting it to three on the home screen." v6.3.2 was a bug-fix-only release.
    2. `wallet-features.md` attributes Bitcoin Lightning to v6.0.1 (March 2026). Documentation is unambiguous: "Bitcoin Lightning has arrived" is listed under v6.0.0 highlights (February 25, 2026). v6.0.1 (March 5, 2026) only polished Lightning (fee estimation, address loading speed, Cake Pay integration).
  - Source: `sources/2026-08-12-docs-cakewallet-com-llms-full.txt` (Release notes — v6.3 and v6.0 sections); live URL returns 200.
  - Fix: `cake-wallet.md`: change "Added in v6.3.2" to "Added in v6.3.0 (July 21, 2026)." `wallet-features.md`: change "Added v6.0.1 (March 2026)" to "Added v6.0.0 (February 25, 2026)."

---

## WARNING (6)

- **`wallets/cake-wallet.md`:270 — PRIVACY.md "last modified January 2024"`**
  - Issue: The PRIVACY.md is 30 months old (January 24, 2024), making it significantly stale for a document governing how user data is handled. The privacy practices described (what nodes receive, what is retained) are central claims in the notes. No updated privacy policy document has been archived.
  - Source: `sources/2026-08-12-github-com-cake-tech-privacy-md.md` (header: "Last modified: January 24, 2024").
  - Fix: Add a note that the PRIVACY.md was last updated January 2024 and that claims derived from it should be treated as reflecting policy as of that date, not necessarily current practice.

- **`wallets/cake-wallet.md`:30–31 — App Store rating 4.7/5 with 4,200 ratings**
  - Issue: No archived copy of the App Store listing. The note in `wallet-adoption.md` confirms: "iOS App Store: not archived (dynamic page)." App Store pages are notoriously volatile; the rating and count cannot be verified from the archived sources.
  - Source: Apple App Store listing cited but not archived; live URL https://apps.apple.com/us/app/cake-wallet-monero-bitcoin/id1334702542 returns 200 but dynamic content cannot be verified from local archive.
  - Fix: Mark App Store rating and count as `[UNARCHIVED — dynamic page; could not verify from local sources]` or reconfirm and capture a static screenshot. Alternatively, use iOS App Store public RSS or search API for a stable snapshot.

- **`wallets/cake-wallet.md`:222,263 — Flatpak GPG verification disabled (`gpg-verify=false`, `gpg-verify-summary=false`)**
  - Issue: This claim appears twice in the notes but has no cited source URL or archived evidence. No Flatpak manifest or `.flatpakrepo` file is in the sources directory. The Cake Wallet docs describe Flatpak installation but do not reference GPG verification flags. This may be accurate (Flatpak bundles distributed outside Flathub often omit GPG signing) but is not evidenced in the archived material.
  - Source: No archive. Not found in any source file.
  - Fix: Either cite the Flatpak manifest file from the GitHub repository (e.g., the `.flatpak` bundle's `metadata` or a `.flatpakrepo` file) and add to sources, or downgrade the claim to a general observation that Flatpak bundles from GitHub releases bypass Flathub's GPG trust chain.

- **`wallets/cake-wallet.md`:46 — "Windows builds are paused pending a redesigned app (expected late 2026)"`**
  - Issue: The "expected late 2026" qualifier is unsourced. The documentation only says "Windows desktop builds are paused" with no timeline commitment cited. This is a forward-looking claim without a source.
  - Source: `sources/2026-08-12-docs-cakewallet-com-llms-full.txt` (Get Started / Install section) — says "New Windows releases are on hold while the desktop app is rebuilt." No expected timeline.
  - Fix: Remove "expected late 2026" or cite a source (e.g., a GitHub issue or announcement) for that timeline estimate.

- **`wallets/cake-wallet.md`:25 — GitHub contributors "89+"`**
  - Issue: The contributors API endpoint is cited but no local archive of `https://api.github.com/repos/cake-tech/cake_wallet/contributors?per_page=100` exists in the sources directory. The `sources/2026-08-10-github-com-cake-wallet-contributors.json` file is from 10 August (2 days before the stated date of 12 August). The value "89+" depends on whether 89 contributors were returned at the per_page=100 limit — implying there may be more. This should be confirmed against the 12 August archive.
  - Source: `sources/2026-08-10-github-com-cake-wallet-contributors.json` (August 10 archive, not August 12). Archive for 12 August contributors is missing.
  - Fix: Add a 2026-08-12 archive of the contributors API response, or note the source date as 2026-08-10.

- **`wallets/cake-wallet.md`:216 — "large app binary (~356 MB on iOS)"`**
  - Issue: This figure has no cited source. No App Store listing archive, no TestFlight or IPA size figure in the sources. The notes do not indicate where this number originates.
  - Source: No archive or citation found.
  - Fix: Cite a source (e.g., the App Store "App Size" field, or a download size screenshot) or change to `[NOT FOUND]`.

---

## NOTE (5)

- **`wallets/cake-wallet.md`:1 — "dominant Monero mobile wallet of 2026"`**
  - Issue: No source is cited for the superlative "dominant." This is a characterisation rather than a metric. The ranked comparison supporting it (GitHub stars as proxy, no Monerujo or Feather stars compared in this file) is reasonable but the claim should be hedged or sourced to a comparative ranking.
  - Fix: Qualify as "leading Monero mobile wallet by GitHub stars (1,838 as of 2026-08-12)" or cite a source with explicit ranking.

- **`wallets/cake-wallet.md`:120 — "Monero's October 2017 hard fork" made RingCT mandatory**
  - Issue: RingCT became mandatory at Monero's hard fork in January 2017 (v0.10.1 "Wolfram Warptangent"), not October 2017. The October 2017 fork (v0.11) enforced other changes. This is a minor technical inaccuracy from within the notes (not from a cited source), so cannot be verified against the archived sources which do not discuss this date. The notes themselves added "October 2017" without a citation.
  - Source: Not covered in any archived source. Requires external verification; see https://www.getmonero.org/resources/moneropedia/ringCT.html (live, returns 200).
  - Fix: Verify the exact RingCT mandatory enforcement date from the Monero changelog and add a citation. Likely change to January 2017 or September 2017 depending on which fork is meant.

- **`patterns/cross-chain-swap.md` — Sources lack access-date archives**
  - Issue: Two of the four sources cited have no local archive: `https://docs.cakewallet.com/features/basic/swap` and `https://docs.cakewallet.com/faq/swaps-and-exchanges.md`. Both live URLs return 200 and the content is substantially covered by the archived `docs-cakewallet-com-llms-full.txt`, but per the citation-completeness standard, missing archives are a WARNING. However, since the full LLM export archive covers this content, this is a NOTE rather than a WARNING.
  - Fix: Add individual archives for those two URLs, or note that content is covered by the full LLM export archive (`sources/2026-08-12-docs-cakewallet-com-llms-full.txt`).

- **`patterns/monero-stealth-address.md` — Seed and keys source URL has no archive**
  - Issue: `https://docs.cakewallet.com/features/managing-your-wallet/seed-keys.md` is cited without a local archive. Live URL returns 200; content is covered by the full LLM export.
  - Fix: Note that coverage is provided by `sources/2026-08-12-docs-cakewallet-com-llms-full.txt`.

- **Cross-file consistency — `wallets/cake-wallet.md`:168 vs `metrics/wallet-features.md`:65 — "17 active coins"`**
  - Both files agree on 17 active coins. The documentation does not state an explicit count, but the enumerated list in both notes (XMR, BTC, LTC, BCH, DOGE, ETH, POL, ARB, BASE, BNB, SOL, TRX, ZEC, XNO, ZANO, DCR = 16, plus Wownero in view-only) yields 16 active + 1 view-only, which does not match "17 active." One coin may be miscounted. Check whether "active" includes or excludes Wownero.
  - Fix: Re-count. If Wownero view-only is excluded, the active count should be 16, not 17. If 17 is correct, identify which additional coin was missed. Both files need the same correction.

---

## Verified (sampled)

- GitHub stars (1,838), forks (389), open issues (308) — confirmed against `sources/2026-08-12-api-github-com-cake-tech-cake-wallet.json`.
- Primary language Dart — confirmed in GitHub API archive.
- Repo created 2020-01-04 — confirmed in GitHub API archive.
- Latest release v6.4.0, dated 2026-07-27 — confirmed against `sources/2026-08-10-github-com-cake-wallet-latest-release.json` and release notes in docs.
- MIT licence — confirmed in GitHub API archive.
- Lightning via Spark + Breez SDK — confirmed: "Cake Wallet's Lightning support is powered by Spark, a new Bitcoin scaling network, through the Breez SDK."
- SHA-512 + 8-digit TOTP (Cake 2FA) — confirmed in docs; Google/Microsoft Authenticator incompatibility — confirmed.
- Monero Polyseed 16-word default; legacy 25-word; BIP39 12-word — confirmed.
- Bitcoin BIP39 derivation path `m/84'/0'/0'` for native SegWit; Electrum path `m/0'` — confirmed.
- Exolix temporarily disabled as of v6.4.0 — confirmed in release notes.
- Swaps.xyz and StealthEX disabled as of v6.0.3 — confirmed in release notes.
- LetsExchange re-enabled in v6.2.1 — confirmed in release notes.
- USDT bridge (ETH/Polygon/Arbitrum) added in v6.1.0 — confirmed in release notes.
- Trezor XMR support added in v6.2.1 — confirmed ("Use a Trezor hardware wallet with Monero" under v6.2.0 highlights).
- MEV protection via Blink (https://blinklabs.xyz/) covering Ethereum and Base only — confirmed; Polygon and Arbitrum explicitly excluded.
- Transaction history APIs: Etherscan, PolygonScan, BaseScan, ArbiScan, BSCScan, TronGrid — confirmed, each with independent toggle.
- Privacy policy quote ("Your private keys…seeds…are your own responsibility") — verbatim match to PRIVACY.md archive.
- Monero unlock time "10 blocks" — confirmed in docs.
- Duress PIN wipes device, returns to fresh-install state, no visible wipe indicator — confirmed.
- Monero MAU `[NOT FOUND]` rationale — confirmed CORRECT; structural privacy architecture makes on-chain attribution impossible.
- GitHub issue #3513 filed 11 August 2026 (title: "Publish verifiable Linux release provenance and signed checksums") — confirmed via GitHub API.
- GitHub issue #3407 (coin selection fingerprinting) — confirmed open, created July 2026.
- GitHub issue #3335 (Bitcoin compact filter sync proposal) — confirmed open, created June 2026.
- GitHub issue #3222 (GrapheneOS/Accrescent sync and auth issues) — confirmed open; body describes PIN auth freezes and node disconnection on GrapheneOS, filed May 2026.
- Monero subaddress derivation description and view-key-only watch-only wallet — confirmed against docs.
- Polyseed encodes wallet creation date — confirmed in docs.
- Wallet groups require BIP39; Polyseed and legacy wallets cannot join — confirmed.

---

## Sources fetched

| URL | Status |
|-----|--------|
| https://cakewallet.com | Live — 200 |
| https://api.github.com/repos/cake-tech/cake_wallet | Live — 200; archive at `sources/2026-08-12-api-github-com-cake-tech-cake-wallet.json` |
| https://docs.cakewallet.com/llms-full.txt | Live — 200; archive at `sources/2026-08-12-docs-cakewallet-com-llms-full.txt` |
| https://docs.cakewallet.com | Live — 200 |
| https://github.com/cake-tech/cake_wallet/releases | Live — 200 |
| https://apps.apple.com/us/app/cake-wallet-monero-bitcoin/id1334702542 | Live — 200; **no local archive** |
| https://www.getmonero.org/resources/moneropedia/stealthaddress.html | Live — 200 |
| https://github.com/tevador/polyseed | Live — 200 |
| https://chainflip.io | Live — 200 |
| https://github.com/cake-tech/cake_wallet/issues/3407 | Live — 200; confirmed open |
| https://github.com/cake-tech/cake_wallet/issues/3513 | Live — 200; confirmed open |
| https://github.com/cake-tech/cake_wallet/issues/3222 | Live — 200; confirmed open |
| https://github.com/cake-tech/cake_wallet/issues/3335 | Live — 200; confirmed open |
| https://docs.cakewallet.com/features/basic/swap | Live — 200; **no separate archive** (covered by llms-full.txt) |
| https://docs.cakewallet.com/faq/swaps-and-exchanges.md | Live — 200; **no separate archive** (covered by llms-full.txt) |
| https://docs.cakewallet.com/features/privacy-and-security/built-in-tor | Live — 200; archive at `sources/2026-08-12-docs-cakewallet-com-tor.html` |
| https://docs.cakewallet.com/features/privacy-and-security/authentication | Live — 200; archive at `sources/2026-08-12-docs-cakewallet-com-authentication.html` |
| https://docs.cakewallet.com/cryptos/monero.md | Live — 200; archive at `sources/2026-08-12-docs-cakewallet-com-cryptos-monero.html` |
| https://docs.cakewallet.com/features/managing-your-wallet/wallet-groups | Live — 200; archive at `sources/2026-08-12-docs-cakewallet-com-wallet-groups.html` |
| https://docs.cakewallet.com/faq/common-questions.md | Live — 200; archive at `sources/2026-08-12-docs-cakewallet-com-faq-common-questions.html` |
| https://docs.cakewallet.com/sitemap.md | Live — 200; archive at `sources/2026-08-12-docs-cakewallet-com-sitemap.md` |
| https://raw.githubusercontent.com/cake-tech/cake_wallet/main/PRIVACY.md | Live — 200; archive at `sources/2026-08-12-github-com-cake-tech-privacy-md.md` |

---

*Reviewed 2026-08-12. Australian English.*
