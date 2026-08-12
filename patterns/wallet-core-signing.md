---
tags: [pattern, signing, multi-chain, open-source]
seen_in: [trust-wallet]
---

# Wallet Core: Shared C++ Signing Library Pattern

A pattern in which a single open-source C++ cryptographic library handles all key derivation, address generation, and transaction signing for multiple blockchains, with platform-native language bindings generated on top. The library acts as a "black box" — it takes inputs (UTXOs, private keys, transaction parameters) and produces outputs (signed, encoded transactions) without implementing networking or UI.

## The pattern

1. All chain-specific signing and address derivation logic is centralised in one C++ codebase.
2. Language bindings (Swift, Kotlin/Java, Rust, Go, WebAssembly) are auto-generated from the C++ API, ensuring consistency across platforms.
3. Chain support is registry-driven: a machine-readable `registry.json` defines derivation paths, elliptic curves, address encodings, and explorer URLs per chain. Adding a chain requires a registry entry + C++ implementation, not per-platform code.
4. The library is published as open-source (Apache-2.0) independently of the wallet app, allowing other teams to adopt it.

## Implementations

- **[[wallets/trust-wallet]]:** The primary consumer of wallet-core. wallet-core v4.7.3 (August 2026); 167 chains in registry; 3,553 GitHub stars; Apache-2.0. The mobile app itself is not open-source; only the signing library is published. Other third-party wallets (Crypto.com Wallet, Frontier) also use wallet-core.

## Observed parameters (Trust Wallet / wallet-core, August 2026)

| Parameter | Value |
|-----------|-------|
| Language | C++ (core); Swift, Kotlin/Java, Rust, Go, WASM (bindings) |
| Chains in registry | 167 |
| Supported curves | secp256k1, ed25519, ed25519Blake2bNano, nist256p1, ed25519ExtendedCardano |
| HD standard | BIP32/BIP44 (per-chain derivation paths in registry.json) |
| Seed format | BIP39 (12-word mnemonic) |
| EVM derivation path | `m/44'/60'/0'/0/0` (standard) |
| Bitcoin segwit path | `m/84'/0'/0'/0/0` (default) |
| Bitcoin legacy path | `m/44'/0'/0'/0/0` |
| Bitcoin taproot path | `m/86'/0'/0'/0/0` |
| Solana path | `m/44'/501'/0'/0'` |
| License | Apache-2.0 |
| Latest version | 4.7.3 (7 August 2026) |

## Trade-offs

**Advantages:**
- Reduces platform-divergence bugs: iOS and Android use identical signing logic.
- Registry-driven extensibility: adding a chain does not require per-platform changes.
- Broad ecosystem adoption: the library is used beyond Trust Wallet, benefiting from more eyes on the cryptographic code.
- Open-source: allows independent audit of the signing logic (even when the app is closed).

**Disadvantages:**
- C++ cross-compilation complexity: building for iOS, Android, and WebAssembly requires intricate toolchains.
- The "black box" design means the app layer is still unaudited if not open-sourced. In Trust Wallet's case, the app is closed-source, so only the signing layer benefits from open scrutiny.
- Dependency risk: a bug in the shared library propagates to all consuming wallets simultaneously (as seen with the 2022 WASM entropy issue in the browser extension's WebAssembly build of wallet-core).

## Sources

- [wallet-core README](https://raw.githubusercontent.com/trustwallet/wallet-core/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-com-trustwallet-wallet-core-README.md)
- [wallet-core registry.json](https://raw.githubusercontent.com/trustwallet/wallet-core/master/registry.json) — accessed 2026-08-12 — [archived](../sources/2026-08-12-trustwallet-wallet-core-registry.json)
- [GitHub API: trustwallet/wallet-core](https://api.github.com/repos/trustwallet/wallet-core) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-trustwallet-wallet-core.json)
- [Trust Wallet Developer Docs (full text)](https://developer.trustwallet.com/developer/llms-full.txt) — accessed 2026-08-12 — [archived](../sources/2026-08-12-developer-trustwallet-com-llms-full.txt)
