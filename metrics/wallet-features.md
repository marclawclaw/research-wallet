# Wallet Feature Comparison

Cross-wallet feature comparison table. All entries sourced; `[NOT FOUND]` = no credible confirmation located; `—` = not applicable.

---

## Cake Wallet (v6.4.0, July 2026)

**Sources:** https://docs.cakewallet.com/llms-full.txt (accessed 2026-08-12) — [archived](../sources/2026-08-12-docs-cakewallet-com-llms-full.txt); https://api.github.com/repos/cake-tech/cake_wallet (accessed 2026-08-12) — [archived](../sources/2026-08-12-api-github-com-cake-tech-cake-wallet.json); https://cakewallet.com (accessed 2026-08-12) — [archived](../sources/2026-08-12-cakewallet-com-home.html)

| Feature area | Feature | Cake Wallet | Notes |
|-------------|---------|-------------|-------|
| **Key management** | Seed format (Monero) | Polyseed 16-word (default); legacy 25-word; BIP39 12/24-word | Polyseed encodes creation date; BIP39 required for wallet groups |
| **Key management** | Seed format (Bitcoin) | BIP39 12-word (default); 24-word; Electrum seed | `m/84'/0'/0'` for native SegWit; `m/0'` for Electrum |
| **Key management** | Seed passphrase | Yes | Optional; all seed types |
| **Key management** | Subaddresses (Monero) | Yes | Auto-rotating after each use (toggleable); unlimited |
| **Key management** | Multiple accounts (Monero) | Yes | Unlimited accounts, independent balances |
| **Key management** | View-only wallet (watch-only) | Yes | Monero: primary address + private view key; spend key omitted |
| **Key management** | Wallet groups (multi-currency shared seed) | Yes (BIP39 only) | One BIP39 seed backs multiple currencies; Polyseed/legacy Monero excluded |
| **Key management** | Key storage location | On-device only | Privacy policy: keys never leave device, never stored by Cake Labs |
| **Signing** | Local on-device signing | Yes | All coins |
| **Signing** | Hardware wallet — Ledger | Yes | XMR, BTC, LTC, ETH, Polygon; Bluetooth + USB (Android); Bluetooth only (iOS/macOS) |
| **Signing** | Hardware wallet — Trezor | Yes | XMR (v6.2.1+), BTC, LTC, ETH, Polygon; full passphrase in v6.4.0 |
| **Signing** | Hardware wallet — BitBox | Yes (Android only) | BTC, ETH, Polygon; USB only |
| **Signing** | Hardware wallet — COLDCARD | Yes (air-gapped) | BTC; BCUR or BBQR QR format |
| **Signing** | Hardware wallet — Linux | [NOT FOUND] — not expected; pending desktop overhaul | — |
| **Signing** | Air-gapped signing (Cupcake) | Yes | Cake's companion app; QR-based |
| **Signing** | Message signing / verification | Yes | Available in advanced features |
| **UX** | Setup flow | Coin → seed type → passphrase → backup → PIN → biometrics | Streamlined; Polyseed reduces restore-height friction |
| **UX** | Receive address rotation | Yes | Monero: subaddresses; Bitcoin: new address each use |
| **UX** | Fee estimation / selection | Yes | Monero: 5 tiers (Slow to Fastest); other chains: network fee shown |
| **UX** | Transaction notes | Yes (local, on-device only) | Not recorded on-chain; local only |
| **UX** | Address book | Yes | Named recipients; QR scan import |
| **UX** | OpenAlias / Unstoppable Domains | Yes | Human-readable addresses |
| **UX** | Batch sending | Yes | Monero and Bitcoin |
| **UX** | Multi-language | Yes | 24+ languages |
| **UX** | Home screen transaction history | Yes (v6.3.2+) | Recent transactions on dashboard |
| **Backup / recovery** | Seed phrase backup | Yes | Displayed at wallet creation; manual write-down |
| **Backup / recovery** | Full app backup file | Yes | Encrypted export; requires fresh install to restore |
| **Backup / recovery** | Restore by private key | Yes | — |
| **Backup / recovery** | Restore height / date | Yes | Monero: optional; Polyseed seeds encode date automatically |
| **Backup / recovery** | Duress PIN (panic wipe) | Yes | Irreversible local wipe; no external effect |
| **Backup / recovery** | Cloud backup | No (not native) | Recommends E2E-encrypted password managers |
| **Privacy** | Monero RingCT | Yes (structural) | Mandatory Monero network feature; ring size 16 |
| **Privacy** | Monero stealth addresses | Yes (structural) | One-time per-transaction recipient addresses |
| **Privacy** | Monero Bulletproof+ | Yes (structural) | Range proofs hiding transaction amounts |
| **Privacy** | Bitcoin Silent Payments | Yes | Static, reusable, unlinkable BTC addresses |
| **Privacy** | Bitcoin Payjoin | Yes | Collaborative transactions; breaks common-input heuristic |
| **Privacy** | Litecoin MWEB | Yes | Confidential transactions (iOS, Android, macOS; not Linux) |
| **Privacy** | Zcash shielded (Sapling/Orchard) | Yes | Sender, receiver, amount hidden on-chain; Ironwood in v6.4.0 |
| **Privacy** | Zcash autoshielding | Yes | Automatic transparent → shielded sweep |
| **Privacy** | Coin control (UTXO selection) | Yes | BTC and LTC |
| **Privacy** | Built-in Tor | Yes (opt-in) | Covers node + price API traffic; experimental; slower sync |
| **Privacy** | Tor + Orbot | Yes | Documented for comprehensive coverage |
| **Privacy** | ETH/Base MEV protection | Yes | Via Blink Labs; private mempool before public broadcast |
| **Privacy** | Custom node selection | Yes | All coins; recommended for privacy |
| **Privacy** | Fiat API toggle | Yes | Disable to prevent price query IP logging |
| **Exchange / swap** | Built-in swap | Yes | Aggregates multiple providers |
| **Exchange / swap** | Decentralised swap (Chainflip) | Yes | On-chain, non-custodial |
| **Exchange / swap** | Centralised swap (ChangeNow) | Yes | Custodial during swap |
| **Exchange / swap** | Centralised swap (Trocador) | Yes | Custodial during swap; meta-aggregator |
| **Exchange / swap** | Fixed-rate option | Yes | Locks quote; slightly worse rate |
| **Exchange / swap** | USDT bridge (ETH/Polygon/Arbitrum) | Yes (v6.1.0+) | In-app cross-chain USDT bridging |
| **Exchange / swap** | KYC for swaps | [NOT FOUND] — Cake Wallet itself: none; individual providers may have thresholds | — |
| **Multi-currency** | Number of coins supported | 17 active (Wownero: view-only; Haven: removed) | — |
| **Multi-currency** | Bitcoin Lightning | Yes (via Spark protocol) | Added v6.0.1 (March 2026); iOS, Android, macOS; not Linux |
| **Open source** | Licence | MIT | — |
| **Open source** | GitHub repo | https://github.com/cake-tech/cake_wallet | Public; all platforms in one repo |
| **Open source** | Reproducible builds | [NOT FOUND] — no public process; Linux Flatpak GPG verification disabled | Issue #3513 requests this |
| **Open source** | Security audit (third-party) | [NOT FOUND] — no publicly disclosed audit | — |
| **Platform** | iOS | Yes | v6.4.0; Bluetooth HW only; Lightning yes |
| **Platform** | Android | Yes | v6.4.0; Bluetooth + USB HW; Lightning yes; BitBox yes |
| **Platform** | macOS | Yes | v6.4.0; Bluetooth HW only; Lightning yes |
| **Platform** | Linux | Yes | v6.4.0 via Flatpak; no HW wallet; no Lightning; no MWEB |
| **Platform** | Windows | No (paused) | Pre-v6.0 only; v6.x planned late 2026 |
| **Security** | PIN authentication | Yes | Device-only; not recoverable by Cake Labs |
| **Security** | Biometric authentication | Yes | Optional; device-compatible |
| **Security** | Cake 2FA (TOTP) | Yes | SHA-512; 8-digit; non-standard (incompatible with Google/Microsoft Authenticator) |
| **Security** | Duress PIN | Yes | Wipes device wallet data on entry |

---

*This table will be extended as additional wallets are researched. Last updated: 2026-08-12.*
