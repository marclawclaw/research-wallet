---
title: Backpack Wallet
tags: [wallet, solana, ethereum, eclipse, xnft, browser-extension, mobile, self-custody]
access-date: 2026-08-12
---

# Backpack Wallet

Backpack is a self-custody software wallet and integrated crypto exchange built by Coral (formerly 200ms / Armada). It is best known for introducing the **xNFT standard** — executable NFTs that run sandboxed applications inside the wallet — and for subsequently pivoting toward a regulated multi-chain exchange integrated directly with the wallet UI. The wallet is open-source under GPL-3.0.

## Identity and Team

| Field | Value |
|-------|-------|
| Developer | Coral (formerly 200ms / "Armada Alliance") |
| GitHub org | coral-xyz |
| Primary repo | `coral-xyz/backpack` (1,651 stars, 947 forks, 2026-08-12) |
| Created | 24 February 2022 |
| Language | TypeScript |
| Licence | GPL-3.0 |
| Security contact | security@200ms.io |

The company was initially called 200ms, then operated under the "Armada Alliance" brand during its Solana validator epoch, and later became Coral. Backpack Exchange is the regulated trading arm.

## Supported Blockchains

The `Blockchain` enum in `packages/common/src/types.ts` (confirmed 2026-08-12) lists three chains:

```ts
export enum Blockchain {
  SOLANA   = "solana",
  ECLIPSE  = "eclipse",   // SVM-compatible L2
  ETHEREUM = "ethereum",
}
```

The iOS App Store description (v2.82.1, 2026-08-12) additionally mentions **Sui** and **Monad** as wallet-supported chains. The extension package depends on `@solana/web3.js` and `ethers`, confirming at minimum Solana and Ethereum.

Confirmed network support: Solana, Eclipse (SVM L2), Ethereum (EVM), Sui, Monad. No native Bitcoin or Monero.

## Key Management

### Seed and HD Derivation

- **Mnemonic:** BIP39 (`bip39` npm package confirmed in `packages/secure-background/src/services/evm/keyring.ts` and `svm/keyring.ts`).
- **Solana derivation:** SLIP-10 ed25519 via `ed25519-hd-key` library (`derivePath` function). Default path is `m/44'/501'/…` (standard Solana BIP44 coin type 501). Legacy Sollet path (`501'` prefix without the full `m/44'` prefix) is also supported for import. Confirmed in `packages/secure-background/src/services/svm/util.ts` (2026-08-12).
- **Ethereum derivation:** BIP44 via `ethers6` `HDNodeWallet` (`m/44'/60'/0'/0/n`). Confirmed in `packages/secure-background/src/services/evm/keyring.ts` (2026-08-12).
- **Multiple accounts:** Supported — each blockchain keyring maintains an array of derivation paths.

### Keyring Architecture

`BlockchainKeyring` (one instance per blockchain) manages three distinct keyring types:
1. **HD keyring** (mnemonic-derived, via `HdKeyringFactory`)
2. **Imported keyring** (individual private keys)
3. **Ledger keyring** (`LedgerKeyringBase`)

This architecture (`packages/secure-background/src/keyring/index.ts`) supports adding Ledger accounts alongside or instead of mnemonic-derived accounts.

## Hardware Wallet Support

- **Ledger confirmed:** `@ledgerhq/hw-transport-webhid` is listed as a direct dependency of `packages/app-extension/package.json` (v^6.28.3; confirmed 2026-08-12). `SolanaLedgerKeyringFactory` and `EthereumLedgerKeyringFactory` are both registered in `packages/secure-background/src/keyring/index.ts`.
- **Trezor:** No dependency or import found in source inspection (2026-08-12).
- The iOS App Store description states "Connect your hardware wallet for cold-storage security" — consistent with Ledger support.
- **Verdict:** Ledger confirmed (WebHID for extension); Trezor [NOT FOUND].

## xNFT — Executable NFTs

xNFTs (executable NFTs) are Backpack's signature innovation. The protocol is defined in a separate Coral repository (`coral-xyz/xnft`, 143 stars, "Executable NFT Protocol and Marketplace"). The concept:

- An xNFT is an NFT whose metadata points to a bundled React application (stored on Arweave or another decentralised storage layer).
- The wallet fetches the bundle and renders it inside an isolated sandbox within the Backpack UI.
- Applications can read wallet state (balances, public key) and request transaction signing via a sandboxed API — they cannot exfiltrate the private key.
- Developers publish xNFTs to a marketplace; users "install" them by purchasing/minting the NFT, which grants access to the app.
- The `packages/xnft-cli` package provides scaffolding tools for developers.

The xNFT paradigm aims to replace browser-based dApps for Solana with in-wallet apps that have lower phishing surface (no domain spoofing, no injected provider over arbitrary sites). As of 2026-08-12, xNFT ecosystem activity appears reduced — the README still describes Backpack as "a home for your xNFTs" but the primary product focus has shifted to the exchange.

See also: [[executable-nft]] pattern note.

## Backpack Exchange Integration

Backpack operates a regulated centralised exchange at `backpack.exchange`. Key characteristics:

- **Products:** Spot trading, margin, perpetual futures, cross-margined accounts, isolated sub-accounts, yield on collateral.
- **Regulatory status:** The exchange page footer includes "FCA" and "MAS" tokens in its trading pair list (e.g., `FCAL.US`, `MASS.US`) — these are US equity tokens, not FCA/MAS licences. Direct regulatory disclosure was not found on the accessible pages (2026-08-12). The iOS listing notes "*Access to Backpack Exchange and its trading features may not be available in your region."
- **Wallet integration:** The mobile app is a combined wallet + exchange interface. Users can trade directly from self-custodied wallet balances or deposit to the exchange sub-account.
- **Self-custody boundary:** The wallet component is non-custodial (private keys stored locally). The exchange component is custodial (exchange holds funds in sub-accounts). These are architecturally separated; the user must actively move funds between them.

## Multi-Chain and Features

| Feature | Status |
|---------|--------|
| Platforms | Browser extension (Chrome/Brave), iOS, Android |
| Solana | Y — native; SPL tokens, NFTs, staking |
| Ethereum/EVM | Y — confirmed via `ethers` dep and EVM keyring |
| Eclipse (SVM L2) | Y — SVM keyring reused |
| Sui / Monad | Y (iOS description 2026-08-12) — not confirmed in extension source |
| Bitcoin | [NOT FOUND] |
| xNFT support | Y — defining feature; xNFT-cli scaffold, xNFT marketplace |
| NFT gallery | Y — Metaplex NFTs, cNFTs, ERC-721/1155 mentioned in iOS description |
| Token swap | Y — in-wallet swap (fee details [NOT FOUND]) |
| DApp browser | Y — injected provider for extension; deep-link/in-app for mobile |
| Staking | Y — Solana delegated staking (`packages/staking` present) |
| Hardware wallet | Y (Ledger, WebHID); Trezor [NOT FOUND] |
| Multiple accounts | Y — HD derivation paths array per blockchain |
| Import private key | Y — `EthereumKeyringFactory` / `SolanaKeyringFactory` |
| Watch-only | [NOT FOUND] — no `WatchAddressKeyring` found in source |
| Transaction simulation | Y — iOS description: "Scam detection alerts you before interacting with bad sites" |
| Lock NFTs | Y — iOS description: "Lock your NFTs to protect them from malicious transactions" |
| Multisig | [NOT FOUND] |
| Coin control | N/A — not UTXO-based |
| Tor support | [NOT FOUND] |
| Air-gapped signing | [NOT FOUND] — no PSBT/QR workflow; not a BTC wallet |
| Biometric unlock | [NOT FOUND] — iOS description does not mention; likely OS keystore |
| F-Droid | [NOT FOUND] |
| Reproducible builds | [NOT FOUND] |
| WalletConnect | [NOT FOUND in source] — uses injected provider model |

## Security

### Audits
From `SECURITY.md` (2026-08-12): "At the time of writing, Backpack has **not** undergone any 3rd party security audits." Audits were described as in-progress during Beta; reports would not be published publicly for the duration of the Beta. The iOS description (v2.82.1) states "Regularly audited by leading security firms" — this contradicts the GitHub SECURITY.md, which is likely outdated (last release tag: `0.10.1-latest-4`, 25 December 2023). The discrepancy is unresolved; [NOT FOUND] for any published public audit report.

### Bug Bounty
No paid bug bounty programme at time of SECURITY.md writing. Current status [NOT FOUND].

### Licence note
The README includes a prominent warning: "This code is unaudited. Use at your own risk. This is not ready for production." This disclaimer may also be outdated given the active mobile app, but it was not removed from the README as of the latest commit.

## Development Activity

- **Latest release tag:** `0.10.1-latest-4` (25 December 2023) — the GitHub releases page has no newer formal tags, but the extension version in `package.json` is `0.10.30` (2026-08-12), and the iOS/Android apps are `v2.82.1`, indicating ongoing development published outside GitHub release tags.
- **Repository last updated:** 11 August 2026.
- **Open issues:** 533 (2026-08-12).
- **Contributors:** The GitHub contributors API returns 1 contributor under the current token scope — this is likely an API scoping issue, not a true count. The repo has 947 forks and 533 open issues, indicating a broader contributor base.

## Adoption Metrics

| Metric | Value | Source |
|--------|-------|--------|
| GitHub stars | 1,651 | GitHub API — 2026-08-12 |
| GitHub forks | 947 | GitHub API — 2026-08-12 |
| Chrome extension users | 300,000 | Chrome Web Store — 2026-08-12 |
| Chrome extension ratings | 370 (4.5★) | Chrome Web Store — 2026-08-12 |
| Google Play installs | 100,000+ (495,750 precise) | Play Store structured data — 2026-08-12 |
| Google Play reviews | ~1,947 total reviews (1,352 five-star) | Play Store — 2026-08-12 |
| iOS version | 2.82.1 | App Store API — 2026-08-12 |
| iOS rating | 4.39★ (228 ratings) | App Store API — 2026-08-12 |
| Self-reported MAU | 100,000+ | Task brief; primary source [NOT FOUND] confirmed by this research |
| xNFT repo stars | 143 | GitHub API — 2026-08-12 |

The 300,000 Chrome extension users is the most meaningful active-user signal. The combined extension + mobile install base indicates total reach well above 400,000 across platforms, likely undercounted due to Brave/Firefox extension installs not tracked by Chrome Web Store.

## Relationships and Context

- **Coral ↔ Armada Alliance:** Coral built Backpack while also running Armada, a Solana validator management platform. These are separate products under the same company entity.
- **xNFT as a platform play:** The xNFT standard was a bet that wallets could become app platforms — analogous to app stores. The bet attracted early developer attention but the ecosystem did not achieve the critical mass needed for mass adoption. Backpack's pivot to exchange reflects a commercial pragmatism.
- **Competitive position:** Among Solana wallets, Backpack is positioned as the "developer-first" wallet (xNFT, open-source, Eclipse support) compared to Phantom (mass-market) and Solflare (staking-focused). The integrated exchange is a differentiator not present in Phantom or Solflare at comparable depth.

## Sources

| Source | URL | Access date | Archived |
|--------|-----|-------------|---------|
| GitHub API: coral-xyz/backpack | https://api.github.com/repos/coral-xyz/backpack | 2026-08-12 | `../sources/2026-08-12-api-github-com-coral-xyz-backpack.json` |
| GitHub releases/latest | https://api.github.com/repos/coral-xyz/backpack/releases/latest | 2026-08-12 | `../sources/2026-08-12-api-github-com-coral-xyz-backpack-releases-latest.json` |
| README.md | https://raw.githubusercontent.com/coral-xyz/backpack/master/README.md | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-README.md` |
| SECURITY.md | https://raw.githubusercontent.com/coral-xyz/backpack/master/SECURITY.md | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-SECURITY.md` |
| LICENSE | https://raw.githubusercontent.com/coral-xyz/backpack/master/LICENSE | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-LICENSE.txt` |
| Extension package.json | https://raw.githubusercontent.com/coral-xyz/backpack/master/packages/app-extension/package.json | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-extension-package-json.txt` |
| common/types.ts (Blockchain enum) | https://raw.githubusercontent.com/coral-xyz/backpack/master/packages/common/src/types.ts | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-common-types-ts.txt` |
| secure-background keyring/index.ts | https://raw.githubusercontent.com/coral-xyz/backpack/master/packages/secure-background/src/keyring/index.ts | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-keyring-index-ts.txt` |
| secure-background keyring/ledger.ts | https://raw.githubusercontent.com/coral-xyz/backpack/master/packages/secure-background/src/keyring/ledger.ts | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-keyring-ledger-ts.txt` |
| secure-background BlockchainKeyring.ts | https://raw.githubusercontent.com/coral-xyz/backpack/master/packages/secure-background/src/keyring/BlockchainKeyring.ts | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-BlockchainKeyring-ts.txt` |
| svm/util.ts (deriveSolanaPrivateKey) | https://raw.githubusercontent.com/coral-xyz/backpack/master/packages/secure-background/src/services/svm/util.ts | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-svm-util-ts.txt` |
| svm/keyring.ts | https://raw.githubusercontent.com/coral-xyz/backpack/master/packages/secure-background/src/services/svm/keyring.ts | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-svm-keyring-full-ts.txt` |
| evm/keyring.ts | https://raw.githubusercontent.com/coral-xyz/backpack/master/packages/secure-background/src/services/evm/keyring.ts | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-evm-keyring-full-ts.txt` |
| xNFT repo API | https://api.github.com/repos/coral-xyz/xnft | 2026-08-12 | `../sources/2026-08-12-api-github-com-coral-xyz-xnft.json` |
| Apple App Store API | https://itunes.apple.com/lookup?bundleId=app.backpack.mobile | 2026-08-12 | `../sources/2026-08-12-itunes-apple-com-backpack.json` |
| Google Play Store | https://play.google.com/store/apps/details?id=app.backpack.mobile | 2026-08-12 | `../sources/2026-08-12-play-google-com-backpack.html` |
| Chrome Web Store | https://chrome.google.com/webstore/detail/backpack/aflkmfhebedbjioipglgcbcmnbpgliof | 2026-08-12 | `../sources/2026-08-12-chrome-webstore-backpack.html` |
| Backpack Exchange home | https://backpack.exchange/ | 2026-08-12 | `../sources/2026-08-12-backpack-exchange-home.html` |
