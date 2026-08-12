---
tags: [wallet, ethereum, evm, browser-extension, mobile, consensys]
chain: ethereum
ecosystem: EVM
status: active
last_updated: 2026-08-12
---

# MetaMask

Self-custody EVM wallet by ConsenSys. Available as a browser extension (Chrome, Firefox, Brave, Edge) and as a mobile app (iOS, Android). The dominant Ethereum wallet by monthly active users, with roughly 30 million MAU as of late 2025 / early 2026.

## Identity

- **Developer:** ConsenSys Software Inc.
- **First released:** September 2016
- **GitHub (extension):** https://github.com/MetaMask/metamask-extension — 13,195 stars, 5,570 forks (accessed 2026-08-10)
- **GitHub (mobile):** https://github.com/MetaMask/metamask-mobile — 3,008 stars, 1,640 forks (accessed 2026-08-12)
- **License (extension):** ConsenSys proprietary licence (source-available, not open-source; GitHub API reports `NOASSERTION`, not MIT). Contribution permitted; redistribution and modification are not. Source available for inspection.
- **License (mobile):** No licence granted. Source is viewable. ConsenSys states it is "exploring many models, all with a significant open component" but has not made a final decision. The pre-existing claim of MIT licence in the `_index.md` shortlist is **incorrect** — confirmed by direct inspection of both `LICENSE` files on 2026-08-12.
- **Language:** TypeScript (both extension and mobile)
- **Latest version (extension):** v13.43.0 — released 11 August 2026
- **Latest version (mobile):** v8.6.0 — released 7 August 2026

## Adoption metrics

| Metric | Value | Date | Source |
|--------|-------|------|--------|
| MAU | 30M+ (plateau since March 2022) | December 2025 | Bitcoin Magazine, ConsenSys |
| Lifetime downloads | 143M | 2025 | coinlaw.io via DefiLlama cross-ref |
| Chrome Web Store | [NOT FOUND] — structured rating data not accessible | 2026-08-12 | — |
| GitHub stars (extension) | 13,195 | 2026-08-10 | GitHub API |
| GitHub stars (mobile) | 3,008 | 2026-08-12 | GitHub API |
| Swap fee revenue (cumulative) | $198.64M | 2026-07-30 | DefiLlama via coinlaw.io |
| Swap fee revenue (annualised) | $52.94M | 2026-07-30 | DefiLlama via coinlaw.io |
| Ethereum mainnet fee share | 70.3% ($139.55M) of cumulative | 2026-07-30 | DefiLlama |
| CoinGecko 2026 hot-wallet ranking | #2 (behind Trust Wallet) | 2026 | CoinGecko |

## Key management

### Secret Recovery Phrase (SRP)
- BIP39 mnemonic, 12 words by default; 24-word import supported
- Entropy: 128-bit (12-word) or 256-bit (24-word)
- Derivation path: BIP44, m/44'/60'/0'/0/n for Ethereum accounts; BIP44 paths for other EVM chains follow the same root with standard coin-type integers
- Bitcoin native integration (via `@metamask/bitcoin-wallet-snap`, December 2025): BIP84 path (m/84'/0'/0'/0/n) for native SegWit Bitcoin accounts
- No BIP39 passphrase ("25th word") support in the standard flow — SRP is the single secret
- SRP stored in the extension's local encrypted vault (AES-256-GCM via `@metamask/browser-passworder`), decrypted only in-memory on unlock

### Multiple accounts
- Multiple ETH accounts from a single SRP via BIP44 incremental index (n=0, 1, 2, …)
- "Import Account" for one-off private-key imports (not derived from SRP)
- "Add Hardware Wallet" for hardware-derived accounts (separate key path, signed on device)
- Smart accounts via Delegation Toolkit (ERC-4337 / EIP-7702) from external apps; not exposed in the standard wallet UI without a dapp using the Smart Accounts Kit

### Vault encryption
- Local vault: AES-256-GCM; password-derived key via scrypt (N=131072, r=8, p=1) via `@metamask/browser-passworder`
- Mobile: iOS Secure Enclave / Android Keystore used for biometric unlock; vault decryption key is wrapped and stored in platform keychain

## Signing

- **EIP-191 (personal_sign):** Personal message signing; prepends `\x19Ethereum Signed Message:\n<length>` before hashing
- **EIP-712 (eth_signTypedData_v4):** Typed structured data signing; human-readable breakdown shown in confirmation UI; can be verified on-chain. MetaMask Connect documents both methods for dapp integration.
- **EIP-7702:** Authorisation list signing for EOA smart account delegation; `@metamask/eip-7702-internal-rpc-middleware` in extension v13.x
- **EIP-5792:** Wallet capabilities / batch call support; `@metamask/eip-5792-middleware` in extension
- **eth_sign:** Disabled by default since v10.x (raw hash signing was a phishing vector); can be re-enabled in developer settings
- **Transaction simulation:** Blockaid PPOM (Privacy Preserving Offline Module) runs local transaction simulation — `@blockaid/ppom_release` v1.5.3 in extension. Detects phishing, asset drainers, approval exploits before the user signs.

## Hardware wallet support

Confirmed via `package.json` dependencies in `metamask-extension` master branch (accessed 2026-08-12):

| Device | Connection | SDK/Library |
|--------|-----------|-------------|
| Ledger (all models) | WebHID (USB), Bluetooth (via Ledger Live companion) | `@ledgerhq/hw-transport-webhid`, `@metamask/eth-ledger-bridge-keyring` |
| Trezor (Model T, Safe 3, Safe 5) | WebUSB via TrezorConnect | `@trezor/connect-web`, `@metamask/eth-trezor-keyring` |
| Lattice1 / GridPlus | USB / WiFi (GridPlus Lattice SDK) | `gridplus-sdk`, `eth-lattice-keyring` |
| QR-based hardware wallets (Keystone, others) | BC-UR v2 animated QR | `@keystonehq/bc-ur-registry-eth`, `@metamask/eth-qr-keyring` |

Hardware accounts are added as separate MetaMask accounts; signing requests are routed to the device for confirmation. Hardware accounts show alongside software accounts in the account switcher.

## Browser extension architecture

- **Manifest V3** (primary build for Chrome/Edge/Brave); Manifest V2 build maintained for Firefox (`yarn dist:mv2`)
- **Background:** Service worker (`background.js`) — handles wallet state, keyring management, network requests, Infura RPC calls. MV3 service worker lifecycle limitations are managed via `persistenceManager` and keep-alive mechanisms.
- **Content script:** Injected into every web page; injects the `window.ethereum` provider object (`eth_requestAccounts`, `eth_sendTransaction`, etc.)
- **UI:** React + Redux; popup, notification windows, and side-panel modes
- **LavaMoat:** Dependency sandboxing for supply-chain attack mitigation — `yarn build` uses `webpack:lavamoat`
- **MetaMetrics:** Opt-in analytics via Segment; SENTRY_DSN for error reporting; both are opt-in during onboarding

## Mobile architecture

- React Native (TypeScript)
- WalletConnect v2 integration for connecting to desktop dapps
- In-app DApp browser (WebView with injected `window.ethereum`)
- Biometric unlock: Touch ID / Face ID (iOS), fingerprint / face unlock (Android) — key wrapped in platform keychain, biometric gates decryption
- Push notifications via Firebase Cloud Messaging

## Multi-chain / network management

- Ethereum mainnet configured by default (Infura as RPC)
- Built-in popular network list (Arbitrum, Optimism, Base, Polygon, BNB Chain, Avalanche, Linea, etc.)
- "Add Network" via manual RPC URL entry or auto-detection from Chainlist
- EIP-1559 gas estimation on supported chains; legacy gas estimation fallback for non-EIP-1559 chains
- Network switching via dapp request (`wallet_addEthereumChain`, `wallet_switchEthereumChain`)

## UX features

- **MetaMask Swaps:** Aggregated DEX swap within the wallet UI; 0.875% fee on swap amount (unchanged since October 2020 desktop launch); routes via 1inch, 0x, Paraswap, and other aggregators
- **Perpetual futures:** Hyperliquid integration, live since 8 October 2025 — $35.46M daily perp notional vs $6.08M daily DEX aggregator volume (coinlaw.io, July 2026)
- **Token detection:** Auto-detects ERC-20 tokens via Infura and etherscan heuristics
- **NFT support:** ERC-721 and ERC-1155 display (can be toggled off)
- **Gas estimation:** EIP-1559 (base fee + priority fee); three speed presets (Low, Market, Aggressive) plus Advanced / Custom
- **Address book:** Local address book with optional ENS resolution
- **MetaMask Rewards:** Linea tokens programme — Rewards Season 1 distributed $30M in Linea tokens (October 2025 – January 2026)
- **MetaMask Card:** Metal crypto debit card (announced alongside Rewards Season 1)
- **Bitcoin integration (December 2025):** Native Bitcoin send/receive via `@metamask/bitcoin-wallet-snap`; uses BIP84 path; requires Snaps to be enabled

## MetaMask Snaps

Extensibility platform launched in 2023 allowing third-party developers to add functionality via isolated JavaScript sandboxes inside the extension.

- **What Snaps can do:** Add non-EVM blockchain support (Solana, Bitcoin, Starknet, Cosmos, etc.), custom account types (smart accounts, multi-party computation), display transaction insights, derive non-EVM keys, provide custom RPC methods, add ENS-resolver plugins
- **Security model:** Snaps run in an isolated worker environment; no access to other Snaps' data or MetaMask core storage. Any Snap that derives keys must undergo a security audit before publication to the Snaps registry.
- **Key Snaps shipped by MetaMask:** `@metamask/bitcoin-wallet-snap` (Bitcoin), `@metamask/ens-resolver-snap`, `@metamask/institutional-wallet-snap`, `@metamask/gator-permissions-snap`
- **Flask:** MetaMask Flask is an experimental developer build with unrestricted Snap access (not recommended for production use)

## Privacy

- **Default RPC:** Infura (ConsenSys-owned infrastructure). All RPC calls (balance checks, transaction history, gas estimation) go to Infura servers by default. Infura logs IP addresses and RPC call data — a significant privacy concern.
- **Custom RPC:** Any Ethereum-compatible RPC endpoint can be set per-network. Users who care about privacy should point MetaMask to their own Ethereum node or a privacy-respecting RPC (e.g. Alchemy with IP suppression, or a self-hosted Geth/Reth node).
- **MetaMetrics:** Usage analytics, opt-in during onboarding. If opted in, usage events sent to ConsenSys' Segment instance.
- **Phishing detection:** Real-time blocklist checks against known phishing domains; Blockaid PPOM adds on-device simulation without sending tx data to remote servers
- **No Tor support:** No built-in Tor integration; no SOCKS5 proxy setting. Users must route traffic through a system-level VPN or Tor Browser (which then loads MetaMask extension).
- **IP exposure:** Every dapp the user connects to via MetaMask sees their IP address (because the dapp is a normal website). Infura sees the IP and RPC queries. No network-layer privacy built in.

## Security

- **Blockaid PPOM:** Privacy Preserving Offline Module; runs transaction simulations locally within the extension (no tx data sent to remote servers). Detects asset drainers, approval exploits, phishing domains. `@blockaid/ppom_release` v1.5.3 in v13.43.0.
- **Phishing detection:** MetaMask maintains a phishing domain blocklist (`@metamask/phishing-controller`); extension intercepts navigation to known phishing sites
- **LavaMoat:** Prevents compromised npm dependencies from accessing privileged extension APIs; each package operates within a declared capability boundary
- **Security audits:** [NOT FOUND] — no comprehensive public security audit report identified at primary sources (metamask.io, GitHub) as of 2026-08-12. MetaMask has a HackerOne bug bounty programme (https://hackerone.com/metamask).
- **Phishing losses:** Crypto wallet phishing losses fell to $83.3M in 2025, down 83% from $494M in 2024 (MetaMask Security Report, January 2026) — MetaMask attributes this in part to Blockaid detection deployment
- **ConsenSys IPO:** ConsenSys planned IPO delayed to fall 2026; BlockEden April 2026 analysis reaffirmed $10B+ valuation target. Co-founder Dan Finlay departed April 2026.

## Backup and recovery

- SRP (Secret Recovery Phrase) is the sole backup mechanism — the 12-word mnemonic must be written down by the user
- No cloud backup of the SRP; no ConsenSys custody of keys
- SRP can be exported from Settings → Security & Privacy → Reveal Secret Recovery Phrase (password required)
- Private key for individual accounts can be exported similarly
- Snap-based social recovery: not built-in natively; possible via third-party Snaps implementing MPC or social recovery; no official MetaMask snap for this exists as of 2026-08-12

## Account types

- **EOA (Externally Owned Account):** Default; ECDSA secp256k1 keys derived from SRP via BIP44
- **Hardware wallet account:** EOA keys stored on hardware device; MetaMask stores only the account address and public key
- **Imported account:** Single private key (not SRP-derived)
- **Smart account:** Via MetaMask Delegation Toolkit (`@metamask/gator-permissions-snap`) or external smart account providers using ERC-4337 / EIP-7702; not a standard feature of the base wallet UI — requires dapp or Snap integration

## Limitations for RFP evaluation

- **Licence:** Source-available, not open-source. ConsenSys retains copyright; redistribution and modification are not permitted without a separate licence agreement. This is a critical issue for forks or derivative wallet products.
- **Infura dependency:** Default configuration routes all blockchain queries through ConsenSys' Infura. Privacy and censorship risk if Infura restricts access (as occurred with Iranian IP addresses in 2022). Mitigated by custom RPC.
- **No native Monero support:** MetaMask is EVM-only. Monero and Bitcoin require Snaps.
- **No Tor:** No network-layer privacy; no SOCKS5/Tor proxy support.
- **No PSBT / multisig:** Native multisig for ETH requires external smart contract (Safe or similar); MetaMask itself does not produce or manage PSBT-style partially signed transactions.
- **Complexity:** Large codebase (1.38 GB repository), 2,838 open issues; less auditable than simpler alternatives.
- **No reproducible builds:** No documented reproducible build process or multi-party build attestation.
- **Smart account maturity:** ERC-4337 support requires external Snap or dapp; not integrated into base UX.

## Sources

- [GitHub API: MetaMask/metamask-extension](https://api.github.com/repos/MetaMask/metamask-extension) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-metamask-metamask-extension.json)
- [GitHub API: MetaMask/metamask-mobile](https://api.github.com/repos/MetaMask/metamask-mobile) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-metamask-metamask-mobile.json)
- [GitHub releases/latest: metamask-extension v13.43.0](https://api.github.com/repos/MetaMask/metamask-extension/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-metamask-extension-releases-latest.json)
- [GitHub releases/latest: metamask-mobile v8.6.0](https://api.github.com/repos/MetaMask/metamask-mobile/releases/latest) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-metamask-mobile-releases-latest.json)
- [coinlaw.io/metamask-wallet-statistics/](https://coinlaw.io/metamask-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-metamask-wallet-statistics.html). Updated 2026-07-30; last update cycle June 2026. Key figures: 30M MAU, $198.64M cumulative revenue, $52.94M annualised, 0.875% swap fee, 83% phishing decline, $35.46M daily perp notional.
- [MetaMask extension README](https://raw.githubusercontent.com/MetaMask/metamask-extension/main/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-metamask-extension-README.md). Confirms Infura as default RPC; browsers supported; Manifest V3 primary build.
- [MetaMask extension LICENSE](https://raw.githubusercontent.com/MetaMask/metamask-extension/main/LICENSE) — accessed 2026-08-12. Confirms ConsenSys proprietary, not MIT.
- [MetaMask mobile LICENSE](https://raw.githubusercontent.com/MetaMask/metamask-mobile/main/LICENSE) — accessed 2026-08-12. No open-source licence granted.
- [package.json dependency inspection: metamask-extension main](https://raw.githubusercontent.com/MetaMask/metamask-extension/main/package.json) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-metamask-extension-package-json.txt). Source of evidence for: Ledger (`@ledgerhq/hw-transport-webhid`, `@metamask/eth-ledger-bridge-keyring`), Trezor (`@trezor/connect-web`, `@metamask/eth-trezor-keyring`), Lattice1 (`gridplus-sdk`, `eth-lattice-keyring`), QR hardware (`@keystonehq/bc-ur-registry-eth`, `@metamask/eth-qr-keyring`), Blockaid (`@blockaid/ppom_release`), Snaps (`@metamask/snaps-sdk`, `@metamask/snaps-controllers`), Bitcoin snap (`@metamask/bitcoin-wallet-snap`), BIP39 (`@metamask/scure-bip39`), HD keyring (`@metamask/eth-hd-keyring`), EIP-5792 (`@metamask/eip-5792-middleware`), EIP-7702 (`@metamask/eip-7702-internal-rpc-middleware`).
- [MetaMask docs: Snaps introduction](https://docs.metamask.io/snaps/learn/about-snaps/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-metamask-io-about-snaps.html)
- [MetaMask docs: Sign data (EIP-712, personal_sign)](https://docs.metamask.io/wallet/how-to/sign-data/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-metamask-io-sign-data.html)
- [MetaMask homepage](https://metamask.io) — accessed 2026-08-12 — [archived](../sources/2026-08-12-metamask-io-home.html)
- [MetaMask docs: wallet/](https://docs.metamask.io/wallet/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-metamask-io-wallet.html)
- [MetaMask docs: SRP concepts](https://docs.metamask.io/wallet/concepts/secret-recovery-phrase/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-metamask-io-srp.html)
