---
tags: [pattern, ethereum, evm, solana, dapp-connection, walletconnect]
seen_in: [rainbow, metamask, trust-wallet, rabby, phantom]
---

# WalletConnect Integration

WalletConnect is an open protocol for connecting mobile wallets and browser extensions to decentralised applications (dApps). A wallet implements WalletConnect as a "wallet side" session handler; dApps implement it as a "dApp side" connector. Version 2 (released 2022) uses a relay server infrastructure with end-to-end encryption via the Noise protocol; neither the relay nor any third party can read transaction contents.

## Why it matters

Without WalletConnect, mobile wallets would have no standardised path to sign transactions for web-based dApps. The protocol eliminates the need for users to expose private keys to browser-injected scripts, since signing occurs inside the wallet app and only the signed transaction is returned.

## Implementations

- **[[wallets/rainbow]]:** Uses `@walletconnect/core`, `@walletconnect/react-native-compat`, and `@reown/walletkit` (WalletConnect v2 SDK). On mobile: QR code scan or deep link opens WalletConnect session. Browser extension also supports WalletConnect for mobile-extension pairing. Confirmed via `package.json` dependency inspection (2026-08-12). Rainbow also integrates Coinbase Mobile Wallet Protocol (`@coinbase/mobile-wallet-protocol-host`) as a secondary connection method.

- **[[wallets/metamask]]:** MetaMask Mobile uses WalletConnect v2. MetaMask extension also exposes `window.ethereum` provider directly in the browser, making WalletConnect secondary for desktop users but primary for mobile-to-dApp flows.

- **Trust Wallet / Rabby:** Also implement WalletConnect v2 as standard dApp connection mechanism; implementation details [NOT FOUND] in this research run.

- **[[wallets/phantom]]:** Phantom takes a different approach from EVM-first wallets. For its browser extension, Phantom injects `window.phantom.solana` (Solana wallet standard) and `window.ethereum` (EVM) directly — no WalletConnect relay needed on desktop. For mobile-to-dApp connections, Phantom uses deep links (`phantom://`) rather than WalletConnect QR codes. WalletConnect v2 support on Phantom mobile is [NOT FOUND] in archived sources (closed-source; package.json not publicly available). The Solana-native equivalent of WalletConnect for mobile apps is the [Wallet Standard](https://github.com/wallet-standard/wallet-standard), which Phantom co-developed. This means Phantom mobile dApp connections use platform-native deep links, making the flow different from the QR-code scan model used by EVM mobile wallets.

## Trade-offs

| Aspect | Detail |
|--------|--------|
| Privacy | Relay server knows that a session exists between a wallet and a dApp, but not the content (E2E encrypted). IP addresses exposed to relay. |
| Reliability | Depends on WalletConnect relay infrastructure (reown.com / WalletConnect Inc.) — centralised dependency. |
| Security | Phishing risk: users must verify the dApp's domain and session proposal before approving. Malicious QR codes or links can establish sessions to attacker-controlled dApps. |
| Version | WalletConnect v1 reached end-of-life June 2023. Only v2 should be in production use. |

## Sources

- [package.json: rainbow-me/rainbow](https://raw.githubusercontent.com/rainbow-me/rainbow/develop/package.json) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-rainbow-me-rainbow-package-json.json)
- [learn.rainbow.me: What is WalletConnect v2?](https://learn.rainbow.me/what-is-walletconnect-v2) — accessed 2026-08-12 — [archived](../sources/2026-08-12-learn-rainbow-me-walletconnect-v2.html)
