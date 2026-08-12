---
tags: [wallet, ethereum, evm, browser-extension, mobile, desktop, debank, rabby]
chain: ethereum-evm
applies_to: [rabby]
---

# Rabby Wallet

## Identity

- **Developer:** RabbyHub (a team operating under DeBank; legal entity for iOS: OPCODE LABS PTE. LTD.)
- **Category:** Self-custody, browser extension (primary); also mobile (iOS/Android) and desktop (macOS/Windows/Linux)
- **Tagline:** "The game-changing wallet for Ethereum and all EVM chains"
- **Ecosystem:** EVM-only (no native Bitcoin, Solana, or Monero)
- **License:** MIT (extension and mobile repos both use MIT; brand name and logo explicitly reserved per LICENSE)
- **Language:** TypeScript (browser extension); TypeScript/React Native (mobile)
- **GitHub org:** https://github.com/RabbyHub
- **Official site:** https://rabby.io
- **API backend:** https://api.rabby.io (Rabby-operated; separate from DeBank OpenAPI)

## Repositories

| Repo | Stars | Forks | Contributors | Latest version | Updated |
|------|-------|-------|--------------|---------------|---------|
| [RabbyHub/Rabby](https://github.com/RabbyHub/Rabby) (extension) | 1,878 | 593 | 72 | v0.94.2 (7 Aug 2026) | 12 Aug 2026 |
| [RabbyHub/rabby-mobile](https://github.com/RabbyHub/rabby-mobile) (monorepo) | 77 | 37 | [NOT FOUND] | v0.6.84 (mobile app) | 10 Aug 2026 |
| [RabbyHub/RabbyDesktop](https://github.com/RabbyHub/RabbyDesktop) | 67 | 30 | [NOT FOUND] | [NOT FOUND] | 18 Jul 2026 |

Sources: GitHub API — accessed 2026-08-12 — archived in `sources/`.

## Architecture (browser extension)

Rabby follows the same four-script browser extension architecture as MetaMask, which it explicitly acknowledges forking from:

1. **`background.js`** — handles async requests, encryption, keyring operations; stores keyrings, passwords, and preferences in Chrome local storage. Contains `walletController` (exposes methods to UI via `runtime.getBackgroundPage`) and `providerController` (handles dapp requests).
2. **`content-script`** — injected at `document_start`; injects `pageProvider.js` and relays messages between page and background via `broadcastChannel`.
3. **`pageProvider.js`** — injected into dapp context; mounts `window.ethereum`; bridges dapp calls to background.
4. **`ui`** — shared React code serving `notification.html` (dapp permission popups), `index.html` (tab view), and `popup.html` (extension icon popup).

## Key management

- **Seed format:** BIP39 12-word mnemonic (generated via `@scure/bip39` with English wordlist, 128-bit entropy = 12 words)
- **HD derivation:** BIP44, standard Ethereum path `m/44'/60'/0'/0/n`
- **Supported keyring types (confirmed via `src/constant/index.ts`):**
  - `HdKeyring` — BIP39 HD seed phrase
  - `SimpleKeyring` — imported single private key
  - `WatchKeyring` — watch-only (address only, no key)
  - `WalletConnectKeyring` — WalletConnect v2
  - `GnosisKeyring` — Safe (Gnosis) smart account
  - `CoboArgusKeyring` — Cobo Argus co-management
  - `CoinbaseKeyring` — Coinbase Smart Wallet connection
  - Hardware keyrings: Ledger, Trezor, OneKey, GridPlus (Lattice1), Keystone (QR), NGRAVE ZERO (QR), BitBox02, imKey
- **Password encryption:** keyring data stored in Chrome local storage with password-based encryption; Least Authority 2024 audit recommended migrating from AES-CBC to AES-GCM
- **Multiple accounts:** Y — multiple HD accounts per seed phrase; multiple seed phrases; multiple hardware devices; watch-only addresses; Safe addresses; all accessible from a single unified address book
- **Watch-only:** Y — `WatchAddressKeyring` type; address-only with no signing capability

## Hardware wallet support

Confirmed via `src/constant/index.ts` `createHardwareObject()` and `src/background/service/keyring/` directory inspection (2026-08-12):

| Hardware wallet | Connection | Notes |
|----------------|-----------|-------|
| Ledger | WebHID (USB) | `@ledgerhq/device-transport-kit-web-hid`; Bluetooth on mobile (`@ledgerhq/react-native-hw-transport-ble`) |
| Trezor | WebUSB | `@rabby-wallet/eth-trezor-keyring`, `@trezor/connect-webextension` |
| OneKey | USB/Bluetooth | `@onekeyfe/hd-core`, `@onekeyfe/hd-web-sdk`; BLE on mobile |
| GridPlus (Lattice1) | WebUSB | `@rabby-wallet/eth-lattice-keyring` |
| Keystone | QR code (air-gapped) | `@keystonehq/metamask-airgapped-keyring`, `@keystonehq/hw-app-eth` |
| NGRAVE ZERO | QR code (air-gapped) | listed as `NGRAVEZERO` in hardware enum |
| BitBox02 | WebUSB | `eth-bitbox02-keyring` directory |
| imKey | USB | `@imkey/web3-provider`, `eth-imkey-keyring` |

Mobile app (v0.6.84) confirmed hardware wallet keyrings: Ledger (BLE), OneKey (BLE). Trezor listed in mobile dependencies (`@trezor/connect-mobile`).

## Signing capabilities

- **EIP-191 (`personal_sign`):** Y
- **EIP-712 typed data (`eth_signTypedData_v4`):** Y — see [[eip712-typed-signing]]
- **`eth_sign` (raw hash):** Legacy; deprecated in UI
- **EIP-1559 (Type-2 transactions):** Y — confirmed for Ledger, GridPlus, Trezor, OneKey, BitBox02, Keystone, NGRAVE ZERO, private key, and HD keyring (via `SUPPORT_1559_KEYRING_TYPE` constant)

## Security features

### Pre-transaction simulation ("What will happen")

Rabby's defining feature. Before a user signs any transaction, the extension calls `wallet.openapi.preExecTx()` against the Rabby API backend (`https://api.rabby.io`) to simulate the transaction outcome. The result, stored as `balance_change`, is displayed in the signing confirmation UI via `BalanceChange.tsx` and structured action components in `src/ui/views/Approval/components/Actions/`.

The action decoder supports: token swaps, sends, NFT sends, token approvals, permit2 revocations, batch revocations, LP add/remove, contract deployment, cross-chain swaps, bridge transfers, and multi-swap. Unrecognised transactions are displayed via `ContractCall.tsx` (raw decoded calldata).

This is a server-side simulation: transaction data is sent to `api.rabby.io` for execution. This differs from MetaMask's Blockaid (on-device PPOM). The trade-off: higher simulation accuracy (full chain state), lower privacy.

### Security Engine (`@rabby-wallet/rabby-security-engine`)

A rule-based risk evaluation layer (`src/background/service/securityEngine.ts`) that applies `defaultRules` to every transaction and signing request. Rules carry a severity level (Forbidden, Danger, Warning, Safe, Closed). Users can adjust thresholds or disable rules. The engine evaluates context such as:
- Known phishing origins (domain blacklist seeded from MetaMask phishing list `nkbihfbeogaeaoehlefnkodbefgpgknn/phishing.html`)
- Contract risk (honeypot detection, unverified contracts)
- Token approval amounts (unlimited approve detection)
- Permit signature analysis (ERC-2612 permit, Permit2)

### Allowance manager

`src/ui/views/ManageApprovals/`, `ManageBatchApprovals/`, `TokenApproval/`, `NFTApproval/` — dedicated UI to list and revoke ERC-20 token approvals and NFT approvals. Batch revocation supported (`BatchRevokePermit2`).

### Address whitelist

`src/background/service/whitelist.ts` — optional send-to whitelist that, when enabled, restricts outbound transfers to approved addresses only.

### MEV protection (Broadcast Mode)

`src/ui/views/Approval/components/BroadcastMode/index.tsx` — broadcast mode selector on the signing screen. Options include: standard (public mempool), MEV-protected (private RPC endpoint). Chain-dependent; not available on all chains.

## Multi-chain support

Rabby dynamically fetches its chain list from `https://static.debank.com/supported_chains.json` (cached in Chrome local storage, refreshed every 55 minutes). As of 2026-08-12: **73 active EVM chains** in the DeBank chain list (coinlaw.io reported "141+" as of June 2026, which may reflect a different snapshot or count of testnets and custom RPC additions).

Custom chains are supported via `src/background/service/customTestnet.ts` — users can add any EVM-compatible network (chain ID, RPC URL, currency symbol, block explorer). Custom RPC endpoints can also be set for any supported mainnet chain.

**Solana:** Not supported. GitHub issue #2585 requesting Solana support remains open as of 2026-08-12 with no maintainer response.

## Built-in swap and bridge

- **Swap:** Y — built-in DEX aggregator; 0.25% fee; chains include Ethereum, Arbitrum, BSC, Base, Polygon, Avalanche, OP Mainnet, others. Revenue: $83,625 trailing 30-day fees (DefiLlama, 30 June 2026 snapshot); $4.88M annualised.
- **Bridge:** Y — powered by LI.FI aggregation stack (10+ chains, 18-bridge stack); integrated since 20 August 2024 per LI.FI announcement.
- **Perps:** Y (Desktop) — `src/ui/views/DesktopPerps/` and `DesktopSmallSwap/`; perpetual futures UI (likely aggregated via third party).
- **Lending:** Y (Desktop) — `src/ui/views/DesktopLending/`.

## Mobile app (Rabby Mobile)

Repository: `RabbyHub/rabby-mobile` (monorepo); mobile app at `apps/mobile`, v0.6.84 as of 2026-08-12.

- **Platforms:** iOS and Android (React Native)
- **Hardware wallet support (mobile):** Ledger (BLE), OneKey (BLE), Trezor (`@trezor/connect-mobile`)
- **Feature parity with extension:** partial; mobile shares core keyring packages (`@rabby-wallet/service-keyring`, `@rabby-wallet/eth-keyring-ledger`, etc.) but the UI and some desktop-only features (Perps, lending) are unavailable or differ
- **iOS developer:** OPCODE LABS PTE. LTD. (Singapore entity)
- **iOS App Store rating:** 3.4/5 from ~1,200 ratings (coinlaw.io, June 2026 snapshot)

## Desktop app (Rabby Desktop)

Repository: `RabbyHub/RabbyDesktop` (67 stars, MIT-like licence). Appears to wrap the browser extension in an Electron shell. Latest activity: 18 July 2026. Feature set appears similar to extension with added Perps and Lending views.

## NFT support

Y — `NFTView/`, `NFTApproval/`, `SendNFT/` views in extension. Displays ERC-721 and ERC-1155 tokens. NFT approval revocation supported.

## DeBank relationship and privacy implications

Rabby is developed by the DeBank team. Key privacy considerations:

1. **API backend:** `https://api.rabby.io` (Rabby-operated, separate domain from DeBank). Transaction pre-execution data (from/to addresses, calldata, value) is sent to `api.rabby.io` for simulation. This is a **server-side simulation**; transaction data leaves the browser.
2. **Chain list:** Fetched from `https://static.debank.com/supported_chains.json` (DeBank-operated domain). A UUID-based API key (`uuidv4()`) is generated per install and sent with API requests to `api.rabby.io`.
3. **Portfolio data:** Rabby integrates DeBank's portfolio/balance data to display token balances, history, and protocol positions — all queries routed via `api.rabby.io`.
4. **No privacy policy link found in source:** `src/constant/index.ts` does not reference a privacy policy URL directly; privacy terms may be in the DeBank parent policy.
5. **Gas Account:** Rabby offers a "Gas Account" feature (prepaid gas sponsorship) that requires signing in with an Ethereum address to a Rabby/DeBank service (`src/background/service/gasAccount.ts`).

## Funding and corporate background

- **$25 million Series A** at $200 million valuation, December 2021, led by Sequoia China (HongShan)
- **$36 million total funding** across 22 investors (coinlaw.io, sourced from investor list, June 2026)
- Legal entity for iOS: OPCODE LABS PTE. LTD. (Singapore)

## Security audits

From `audits/` directory in `RabbyHub/Rabby` repo (2026-08-12):

| Year | Auditor | Report name | Finding |
|------|---------|-------------|---------|
| 2024 | Least Authority | `[20241212]Least Authority - DeBank Rabby Wallet Extension Final Audit Report.pdf` | December 2024 audit; recommended migrating encryption from AES-CBC to AES-GCM |
| 2024 | SlowMist | `[20241217]Rabby Browser Extension Wallet - SlowMist Audit Report.pdf` | December 2024 audit; [NOT FOUND — report not fetched; filename confirms December 2024 audit] |
| 2025 | SlowMist | `[20250821]Rabby Browser Extension Wallet - SlowMist Audit Report.pdf` | [NOT FOUND — report not fetched; filename confirms August 2025 audit] |
| 2025 | Least Authority | `[20250903]Least Authority - Rabby Wallet Wallet Extension Final Audit Report.pdf` | Key recommendation: migrate encryption from AES-CBC to AES-GCM (per coinlaw.io, July 2026) |

Earlier audits in `2021/`, `2022/`, `2023/` subdirectories (filenames not inspected).

WalletBeat (2026) independent scorecard: **Stage 0** — Security 2/7, Privacy 0/5, Self-Sovereignty 2/6, Transparency 1/5. (Source: coinlaw.io citing walletbeat.eth.limo, June 2026.)

## Adoption metrics

| Metric | Value | Date | Source |
|--------|-------|------|--------|
| GitHub stars (extension) | 1,878 | 2026-08-12 | GitHub API |
| GitHub forks (extension) | 593 | 2026-08-12 | GitHub API |
| Chrome Web Store installs | ~800,000 | June 2026 | coinlaw.io / allcryptowallets.org |
| Google Play downloads | ~466,800 | June 2026 | coinlaw.io / allcryptowallets.org |
| iOS App Store downloads (est.) | ~90,000 | June 2026 | coinlaw.io / allcryptowallets.org |
| Total installs (aggregated) | ~1.4 million (current) / 4.2 million (self-reported 2025 milestone) | June 2026 / 2025 | coinlaw.io |
| MAU | [NOT FOUND] | — | No self-reported MAU; no credible analyst figure |
| Chrome rating | 4.0/5 | June 2026 | coinlaw.io |
| Google Play rating | 3.9/5 | June 2026 | coinlaw.io |
| iOS rating | 3.4/5 (~1,200 ratings) | June 2026 | coinlaw.io |
| Swap fees (30-day) | $83,625 | 30 Jun 2026 | DefiLlama (via coinlaw.io) |
| Swap fees (annualised) | $4.88 million | 30 Jun 2026 | DefiLlama (via coinlaw.io) |

Note on install discrepancy: 4.2 million is a self-reported all-time cumulative milestone from 2025 press coverage; 1.4 million is the mid-2026 aggregate from live store counters which do not deduct uninstalls the same way. Both figures represent different counting methodologies, not a decline.

## Key differentiators vs MetaMask

| Feature | Rabby | MetaMask |
|---------|-------|---------|
| Pre-tx simulation | Y (server-side, api.rabby.io) | Y (on-device Blockaid PPOM) |
| Security engine rule set | Y (customisable, ~100+ rules) | Y (Blockaid; not user-configurable) |
| Allowance manager (built-in) | Y (batch revoke, token + NFT) | N (requires external dapp) |
| Automatic chain switching | Y (switches to chain a dapp uses automatically) | N (prompt to add/switch) |
| Multi-address management | Y (power-user address book; supports mixing HD, HW, WC, Safe) | P (basic; no cross-type labelling) |
| Swap fee | 0.25% | 0.875% |
| Bridge (built-in) | Y (LI.FI aggregator) | P (via MetaMask Bridge; separate) |
| Snaps / extensions | N | Y |
| Open-source | Y (MIT) | N (source-available; ConsenSys proprietary) |
| Default RPC privacy | api.rabby.io + DeBank chain list (server-side sim) | Infura (ConsenSys-owned) |
| Reproducible builds | [NOT FOUND] | N |
| Solana support | N | P (via Bitcoin Snap; no Solana Snap) |

## Known limitations

- **No BIP39 passphrase (25th word):** [NOT FOUND in source] — standard BIP39 generation without optional passphrase extension in UI.
- **No Tor proxy support:** [NOT FOUND] — no SOCKS5 proxy setting in source.
- **Server-side simulation privacy risk:** transaction pre-execution data sent to `api.rabby.io`; users with strong privacy requirements should be aware.
- **Solana absent:** issue #2585 has received no maintainer response as of 2026-08-12.
- **WalletBeat Stage 0:** Low score on privacy (0/5) likely reflects the server-side simulation model and DeBank data sharing.
- **AES-CBC encryption (pre-patch):** Least Authority 2025 audit flagged the use of AES-CBC; migration to AES-GCM was recommended.
- **iOS App Store rating:** 3.4/5 is below average for wallets in this category; reasons not confirmed in source.

## Sources

- [GitHub API: RabbyHub/Rabby](https://api.github.com/repos/RabbyHub/Rabby) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-rabbyhub-rabby.json)
- [GitHub API: RabbyHub/rabby-mobile](https://api.github.com/repos/RabbyHub/rabby-mobile) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-rabbyhub-rabby-mobile.json)
- [GitHub releases/latest: v0.94.2](https://api.github.com/repos/RabbyHub/Rabby/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-rabbyhub-rabby-releases-latest.json)
- [README.md](https://raw.githubusercontent.com/RabbyHub/Rabby/master/README.md) — accessed 2026-08-12 — architecture description and MetaMask fork acknowledgement
- [LICENSE](https://raw.githubusercontent.com/RabbyHub/Rabby/master/LICENSE) — accessed 2026-08-12 — MIT, brand reserved
- [package.json](https://raw.githubusercontent.com/RabbyHub/Rabby/master/package.json) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-rabbyhub-rabby-package-json.txt) — hardware wallet deps, version, security SDK
- [src/constant/index.ts](https://raw.githubusercontent.com/RabbyHub/Rabby/master/src/constant/index.ts) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-rabbyhub-rabby-constant-index-ts.txt) — `createHardwareObject()`, `KEYRING_CLASS`, `INITIAL_OPENAPI_URL`
- [src/background/service/keyring/index.ts](https://raw.githubusercontent.com/RabbyHub/Rabby/master/src/background/service/keyring/index.ts) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-rabbyhub-rabby-keyring-index-ts.txt) — BIP39 usage, keyring SDK types
- [src/background/service/syncChain.ts](https://raw.githubusercontent.com/RabbyHub/Rabby/master/src/background/service/syncChain.ts) — accessed 2026-08-12 — chain list fetch from `static.debank.com/supported_chains.json`
- [DeBank supported_chains.json](https://static.debank.com/supported_chains.json) — accessed 2026-08-12 — 73 active EVM chains
- [rabby-mobile/README.md (monorepo)](https://raw.githubusercontent.com/RabbyHub/rabby-mobile/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-rabbyhub-rabby-mobile-README.md)
- [coinlaw.io/rabby-wallet-statistics/](https://coinlaw.io/rabby-wallet-statistics/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-coinlaw-io-rabby-wallet-statistics.html) — last updated 20 July 2026; install counts, swap fees, WalletBeat score, App Store ratings, audit dates, funding
- [AGENTS.md](https://raw.githubusercontent.com/RabbyHub/Rabby/master/AGENTS.md) — accessed 2026-08-12 — security invariants, confirms Least Authority and SlowMist audits
- [audits/2024 directory listing](https://api.github.com/repos/RabbyHub/Rabby/contents/audits/2024) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-rabbyhub-rabby-audits-2024.json) — confirms Least Authority (Dec 2024) and SlowMist (Dec 2024) reports present
- [audits/2025 directory listing](https://api.github.com/repos/RabbyHub/Rabby/contents/audits/2025) — accessed 2026-08-12 — confirms SlowMist (Aug 2025) and Least Authority (Sep 2025) reports present
