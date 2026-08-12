---
tags: [wallet, bitcoin, lightning, mobile, ios, android, react-native, lndhub, multisig, psbt]
ecosystem: bitcoin
type: mobile
license: MIT
status: active
version: 8.0.1 (21 July 2026)
---

# BlueWallet

Open-source Bitcoin and Lightning mobile wallet for iOS and Android. Built with React Native on the [[patterns/spv-electrum-server|SPV via Electrum Server Protocol]] pattern. MIT licence. Founded by Nuno Coelho, Marcos Rodriguez, and Igor Korsakov (Overtorment). As of 12 August 2026: 3,268 GitHub stars, 1,045 forks.

## Identity snapshot

| Field | Value |
|-------|-------|
| Repository | https://github.com/BlueWallet/BlueWallet |
| Language | TypeScript (React Native) |
| Licence | MIT |
| Latest release | v8.0.1 — 21 July 2026 |
| Previous major | v8.0.0 — 2 June 2026 |
| Created | 14 January 2018 |
| Stars (2026-08-12) | 3,268 |
| Forks (2026-08-12) | 1,045 |
| Open issues | 415 |
| Watchers | 90 (subscribers) |
| Contributors (top-100 page) | ~100 confirmed; full count not paginated by API |
| Platforms | iOS, Android |
| F-Droid | Y |

## Architecture

BlueWallet is a **thin (SPV-style) client** that connects to Electrum servers rather than syncing the full blockchain. It does not implement Nakamoto-style Merkle proof verification natively — by default it trusts BlueWallet-operated Electrum servers (`electrum1.bluewallet.io`, SSL 443). Users can override this to any self-hosted or public Electrum server (ElectrumX, Fulcrum). This is a trust trade-off documented in the project FAQ.

See [[patterns/spv-electrum-server]] for the protocol detail and trust model.

## Key management

- **HD derivation:** BIP32 (full hierarchical deterministic derivation)
- **Seed format:** BIP39 (12- or 24-word mnemonic) — primary for all new wallets
- **Electrum-native seed:** supported on import (Electrum standard / segwit prefixes)
- **Passphrase (BIP39 extension):** Y — user-configurable at wallet creation or import
- **SLIP39 (Shamir):** Y — `slip39-wallets.ts` in source tree; two-share 1-of-1 and 2-of-2 configurations
- **Multiple wallets:** Y — unlimited wallet slots, each independently managed; wallet search available
- **Private keys:** never leave the device; keys are encrypted at rest using AES (CBC mode via `@noble/ciphers`) with an OpenSSL-compatible key derivation (MD5-based EVP_BytesToKey, retained for backward-compatibility with existing encrypted backups)

## Wallet types supported

Source: `/class/wallets/` directory (2026-08-12):

| Class | Script type | Derivation |
|-------|-------------|------------|
| `hd-legacy-p2pkh-wallet` | P2PKH (Legacy) | m/44'/0'/0' |
| `hd-segwit-p2sh-wallet` | P2SH-P2WPKH (Wrapped SegWit) | m/49'/0'/0' |
| `hd-segwit-bech32-wallet` | P2WPKH (Native SegWit, bech32) | m/84'/0'/0' |
| `hd-taproot-wallet` | P2TR (Taproot, BIP86) | m/86'/0'/0' |
| `multisig-hd-wallet` | P2WSH, P2SH-P2WSH, P2SH | m/48'/0'/0'/2' (native), m/48'/0'/0'/1' (wrapped) |
| `lightning-custodian-wallet` | LNDhub (off-chain) | — |
| `lightning-ark-wallet` | Ark protocol (off-chain) | — |
| `watch-only-wallet` | Any (xpub/ypub/zpub/descriptor) | — |
| `legacy-wallet` | P2PKH (single-key WIF) | — |
| `segwit-bech32-wallet` | P2WPKH (single-key) | — |
| `taproot-wallet` | P2TR (single-key) | — |
| `hd-aezeed-wallet` | aezeed (LND seed format) | — |

**Taproot note:** BlueWallet added native Taproot wallet creation (`HDTaprootWallet`, BIP86) — the `[NF]` from the discovery index was stale. Taproot receive is fully supported.

## Signing

- **Local signing:** Y — all signing performed on-device in JavaScript using `bitcoinjs-lib` and `@noble/secp256k1` (crypto-js replaced by `@noble` libraries in v8.0.1 release cycle)
- **PSBT support:** Y — `bitcoinjs-lib` `Psbt` class used throughout. Multisig and hardware wallet flows use PSBT natively. See [[patterns/psbt-hardware-signing]]
- **Air-gapped signing:** Y — PSBT files can be exported / imported; BC-UR v2 animated QR scanning added in v8.0.1 (OneKey, Keystone support)
- **BBQr:** supported via `blue_modules/bbqr` (compact QR encoding for PSBTs)

## UX

- **Setup flow:** simple — create wallet (BIP39 seed generated, shown once for backup) or import existing; multiple wallet types selectable
- **Coin control (UTXO selection):** Y — `Coin control: select and manage coins` listed in official docs; `fetchUtxo()` / `getUtxo()` expose per-UTXO data to the UI
- **Fee estimation:** Y — dynamic; queries connected Electrum server
- **RBF (Replace-By-Fee):** Y — `defaultRBFSequence = 0x80000000` (BIP68 minimum); RBF for watch-only bech32 wallets added in v8.0.1
- **CPFP (Child-Pays-For-Parent):** Y — listed in official feature docs
- **Payment batching (Send to many):** Y
- **BIP47 (Payment Codes / PayNym):** Y — `@spsina/bip47` library, enable per-wallet
- **Address highlighting:** first and last segments highlighted on send screen (UX change v8.0.0)
- **Human-readable transaction data:** added v8.0.1
- **Localisation:** 60+ languages via Transifex; 19 new languages added in v8.0.1

## Backup and recovery

- **Seed backup:** BIP39 mnemonic displayed on creation; user must manually record
- **Encrypted iCloud / Google Drive backup:** Y — wallet data encrypted before upload (documented in official support pages under "Backup and export a wallet")
- **Import from other wallets:** Y — supports xpub, ypub, zpub, descriptor, WIF, Electrum seed, aezeed, SLIP39, hex/base64 private keys (hex/base64 added v8.0.0), Unchained JSON as multisig cosigner (added v8.0.1)
- **Custom derivation path on import:** Y

## Lightning Network

**Critical context:** BlueWallet operated a custodial LNDhub instance at `ln.bluewallet.io` that allowed users to create Lightning wallets without running their own node. This service was **discontinued on 31 May 2023**. The `LightningCustodianWallet` class and LNDhub support remain in the codebase, but Lightning now requires users to either:

1. **Self-host LNDhub** (https://github.com/BlueWallet/LndHub) pointed at their own LND node, or
2. **Use a third-party LNDhub provider** (e.g. Alby, Start9, Umbrel offer LNDhub-compatible endpoints), or
3. **Use the Ark protocol wallet** (`lightning-ark-wallet`) via Arkade — first integrated in v7.2.2 (November 2025); v8.0.0 upgraded the Ark SDK integration, providing a non-channel-based Lightning-adjacent payment layer

**Self-custodial status:** Lightning via LNDhub is **not self-custodial by default** — users are trusting whatever LNDhub server they connect to. Only self-hosting makes it self-custodial. The Ark wallet integration uses the Arkade delegator at `delegate.arkade.money` by default — partially trust-minimised (delegated signing) but not fully trustless.

### LNDhub architecture

LNDhub acts as a shared Lightning node with per-user account balances. The user sends on-chain bitcoin to a top-up address; the hub credits their account. Outbound payments are made by the hub on behalf of the user (deducting their account balance). Inbound payments are received by the hub and credited to the user's account. The secret is stored as `<username>:<password>@<hub-url>`.

See [[patterns/lndhub-lightning]] for protocol detail.

### Ark (Arkade) integration

- First integrated in v7.2.2 (24 November 2025); v8.0.0 upgraded the Ark SDK integration (`@arkade-os/sdk` and `@arkade-os/boltz-swap`)
- Uses Boltz submarine swaps for on-chain ↔ off-chain conversion
- Delegated signing via `https://delegate.arkade.money` (mainnet)
- LNURL payment supported; push notifications via Arkade payment push service

## Hardware wallet support

| Device | Interface | Notes |
|--------|-----------|-------|
| Ledger | BLE (Bluetooth Low Energy) | iOS and Android; `P (Ledger via BLE)` in feature table |
| Coldcard | File (PSBT export/import) | Watch-only vault + PSBT signing flow; documented in official guides |
| Cobo Vault | QR code | Documented in official guides |
| Keystone | BC-UR v2 QR | Added v8.0.1 |
| OneKey | BC-UR v2 QR | Added v8.0.1 |
| Unchained cosigner | JSON import | Added v8.0.1 |

**PSBT approach:** BlueWallet uses PSBT as the standard wire format for all hardware wallet interactions. Files are exported/imported via share sheet (iOS) or file system (Android). For QR-based devices, BC-UR v2 animated QR is used. See [[patterns/psbt-hardware-signing]].

## Watch-only vaults

- **Mechanism:** user imports xpub, ypub, zpub, or output descriptor — wallet generates all addresses and monitors balance/history via Electrum server; private keys never present
- **Use case:** mobile view of a cold-storage or hardware wallet; can initiate unsigned PSBT for hardware device to sign
- **RBF on watch-only:** enabled for HD bech32 watch-only wallets as of v8.0.1
- **Multisig watch-only:** Y — the Multisig Vault type supports watch-only coordinator mode

## Multisig

- **Formats:** P2WSH (native segwit m/48'/0'/0'/2'), P2SH-P2WSH (wrapped, m/48'/0'/0'/1'), P2SH (legacy, m/45')
- **m-of-n:** arbitrary — `MultisigHDWallet` supports configurable m and n
- **Cosigner import:** xpub, Electrum seed (standard/segwit prefixes), Unchained JSON (v8.0.1), hardware wallet export
- **PSBT workflow:** coordinator creates unsigned PSBT; cosigners sign via hardware wallet or another BlueWallet instance; final cosigner broadcasts
- **Air-gap:** PSBT file or BC-UR v2 QR (v8.0.1) for fully air-gapped Coldcard/Keystone/OneKey workflows

## Privacy

- **Tor support:** not found in source inspection — no Tor/SOCKS proxy code detected in `BlueElectrum.ts` or `constants.ts`. The wallet uses TCP sockets via `react-native-tcp-socket` to connect to Electrum servers; no onion routing layer apparent. **Assessment: [NOT FOUND] — Tor not confirmed in current codebase.**
- **Custom Electrum server:** Y — user can override default server (`electrum1.bluewallet.io`) with any Electrum endpoint; self-hosting provides full privacy for on-chain activity
- **Address reuse:** HD wallet generates fresh addresses per transaction
- **BIP47 (payment codes):** Y — sender and receiver share a payment code; unique addresses derived per payer, preventing address linking
- **CoinJoin:** N — not supported
- **Plausible deniability:** Y — documented in settings under "Plausible Deniability - Duress"; dual-password wallet encryption allows a decoy wallet
- **Push notification redaction:** Y — option to redact push notification content added v8.0.1

## Open-source / security posture

- **Licence:** MIT — full source available
- **Bug tracking:** BugSnag (commercial crash/error tracking, integrated)
- **Dependency auditing:** Snyk (paid subscription per FAQ); all dependency versions pinned
- **Reproducible builds:** the APK release on GitHub includes an `.apk.sig` file (detached signature); process documented but not independently verified as fully reproducible to byte level — **[NOT FOUND]** for formal reproducible-build attestation with multiple independent signers
- **Security audit:** no publicly disclosed third-party security audit found — **[NOT FOUND]**
- **CVEs:** no known CVEs specific to BlueWallet found — **[NOT FOUND]**
- **Responsible disclosure:** `bluewallet@bluewallet.io`

## Adoption metrics

- **GitHub stars (2026-08-12):** 3,268
- **GitHub forks (2026-08-12):** 1,045
- **MAU:** [NOT FOUND] — no self-reported or analyst-sourced MAU figure found
- **Android installs:** [NOT FOUND] — Google Play install badge not scraped; AppBrain requires credential access
- **iOS installs:** [NOT FOUND]
- **Release cadence:** approximately 2–4 major releases per year; v8.0.1 (July 2026), v8.0.0 (June 2026), v7.2.6 (February 2026), v7.2.3 (December 2025)

## v8.0.0 / v8.0.1 highlights

**v8.0.0 (2 June 2026):**
- React Native 85 upgrade
- Ark SDK upgrade (lightning-ark-wallet first introduced in v7.2.2, November 2025; v8.0.0 upgraded the Ark SDK)
- Redesigned transaction detail screen
- Hex/base64 private key import
- Realm database migration
- Greatly improved startup time

**v8.0.1 (21 July 2026):**
- iOS 26 Liquid Glass design
- BC-UR v2 air-gap scanning (OneKey, Keystone)
- Unchained JSON multisig cosigner import
- RBF for single-sig watch-only bech32 wallets
- Push notification content redaction
- 19 new languages (Filipino, Armenian, Akan, Sesotho, + 15 others)
- Replaced crypto-js with `@noble/ciphers` + `@noble/hashes`
- Human-readable data on transaction status screen
- Renewed Settings UI

## Limitations and criticisms

1. **Lightning self-custody gap:** BlueWallet's own LNDhub was shut down on 31 May 2023. Lightning is only self-custodial if the user self-hosts LNDhub or uses Ark. Most users connecting to third-party hubs are custodial — a significant departure from the "self-custody" framing. The UI does not clearly distinguish custodial vs self-custodial Lightning configurations.

2. **No Tor support (as-found):** Unlike Electrum or Sparrow, no Tor onion routing layer detected; users who want privacy beyond server selection must rely on system-level VPN/proxy.

3. **Server trust for on-chain:** Default connection to `electrum1.bluewallet.io` means BlueWallet the company can observe all address subscriptions (IP, UTXOs, balance). No Merkle proof verification as of current inspection (FAQ confirms this).

4. **Encryption strength:** AES-CBC with MD5-based key derivation (retained for backward-compatibility). MD5 is cryptographically weak for key stretching; this is a documented trade-off, not an exploitable bug, but is inferior to Argon2 or scrypt-based KDFs used by more security-focused wallets.

5. **iOS vs Android parity:** BC-UR v2 and Ark features appear cross-platform, but Ledger BLE is listed as available on both. Feature parity details beyond this are [NOT FOUND] from source inspection alone.

6. **No security audit:** no publicly disclosed independent audit; dependency security via Snyk only.

## Related patterns

- [[patterns/psbt-hardware-signing]] — PSBT-based signing flow; BlueWallet uses same approach
- [[patterns/spv-electrum-server]] — BlueWallet's on-chain backend
- [[patterns/lndhub-lightning]] — LNDhub custodial Lightning hub pattern

## Sources

| Source | URL | Accessed | Archived |
|--------|-----|----------|---------|
| GitHub API — repo metadata | https://api.github.com/repos/bluewallet/bluewallet | 2026-08-12 | [archived](../sources/2026-08-12-api-github-com-repos-bluewallet-bluewallet.json) |
| GitHub API — latest release (v8.0.1) | https://api.github.com/repos/bluewallet/bluewallet/releases/latest | 2026-08-12 | [archived](../sources/2026-08-12-api-github-com-repos-bluewallet-bluewallet-releases-latest.json) |
| GitHub API — v8.0.0 release | https://api.github.com/repos/bluewallet/bluewallet/releases/tags/v8.0.0 | 2026-08-12 | [archived](../sources/2026-08-12-api-github-com-repos-bluewallet-bluewallet-releases-v8.0.0.json) |
| BlueWallet official site | https://bluewallet.io | 2026-08-12 | [archived](../sources/2026-08-12-bluewallet-io-home.html) |
| BlueWallet README | https://github.com/BlueWallet/BlueWallet/blob/master/README.md | 2026-08-12 | [archived](../sources/2026-08-12-github-com-bluewallet-bluewallet-README.md) |
| BlueWallet LNDhub page | https://bluewallet.io/lndhub/ | 2026-08-12 | [archived](../sources/2026-08-12-bluewallet-io-lndhub.html) |
| BlueWallet — Sunsetting LNDhub.io announcement | https://bluewallet.io/sunsetting-lndhub/ | 2026-08-12 | [archived](../sources/2026-08-12-bluewallet-io-sunsetting-lndhub.html) |
| BlueWallet FAQ | https://github.com/BlueWallet/BlueWallet/blob/master/FAQ.md | 2026-08-12 | source inspection |
| Wallet class source — wallets directory | https://github.com/BlueWallet/BlueWallet/tree/master/class/wallets | 2026-08-12 | source inspection |
| BlueElectrum.ts — Electrum default server | https://github.com/BlueWallet/BlueWallet/blob/master/blue_modules/BlueElectrum.ts | 2026-08-12 | source inspection |
| hd-taproot-wallet.ts | https://github.com/BlueWallet/BlueWallet/blob/master/class/wallets/hd-taproot-wallet.ts | 2026-08-12 | source inspection |
| multisig-hd-wallet.ts | https://github.com/BlueWallet/BlueWallet/blob/master/class/wallets/multisig-hd-wallet.ts | 2026-08-12 | source inspection |
| lightning-custodian-wallet.ts | https://github.com/BlueWallet/BlueWallet/blob/master/class/wallets/lightning-custodian-wallet.ts | 2026-08-12 | source inspection |
| lightning-ark-wallet.ts | https://github.com/BlueWallet/BlueWallet/blob/master/class/wallets/lightning-ark-wallet.ts | 2026-08-12 | source inspection |
| encryption.ts | https://github.com/BlueWallet/BlueWallet/blob/master/blue_modules/encryption.ts | 2026-08-12 | source inspection |
| abstract-hd-electrum-wallet.ts (PSBT, RBF, BIP47) | https://github.com/BlueWallet/BlueWallet/blob/master/class/wallets/abstract-hd-electrum-wallet.ts | 2026-08-12 | source inspection |
