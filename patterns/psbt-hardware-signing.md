---
tags: [pattern, psbt, hardware-wallet, signing, bitcoin, air-gap]
applies_to: [electrum, sparrow-wallet]
---

# Pattern: PSBT-based Hardware Wallet Signing

PSBT (Partially Signed Bitcoin Transaction, BIP174) is the standard wire format for passing an unsigned or partially signed Bitcoin transaction between different signers, tools, or devices. Electrum implements full PSBT support for hardware wallet signing, multisig co-signing, and air-gapped signing workflows.

## What PSBT solves

Before PSBT, wallets used proprietary formats to pass unsigned transactions to hardware wallets or co-signers. PSBT:

1. Embeds all information a signer needs to validate inputs (UTXOs, derivation paths, redeem scripts).
2. Supports incremental signing — multiple parties can add their signature without a central coordinator.
3. Is hardware-wallet agnostic — any PSBT-capable hardware wallet can sign any PSBT-capable software wallet's transactions.

## Electrum PSBT implementation

Electrum's `transaction.py` implements `PartialTransaction` and `PSBTSection` classes. Key behaviours:

- **Detection:** A base64 or raw binary blob beginning with `cHNidP` (base64) or the PSBT magic bytes is automatically parsed as a PSBT.
- **Creation:** `PartialTransaction.from_raw_psbt(raw)` constructs an in-memory PSBT object from raw bytes.
- **Validation:** `PSBTInputConsistencyFailure` is raised if PSBT inputs are inconsistent (e.g., UTXO data doesn't match claimed outpoint).
- **Signing:** Hardware wallet plugins (Trezor, Ledger, Coldcard, etc.) consume the `PartialTransaction` object and add their partial signature. The `check_sighash` method validates sighash types, including Taproot sighash semantics (which commit to all input amounts).

## Hardware wallet signing flow (connected)

```
User fills in Send tab → Electrum builds PartialTransaction (PSBT)
    → Hardware wallet plugin serialises PSBT → sends to device over USB/HID
    → Device displays address and amount for user confirmation
    → User presses confirm on device
    → Device returns signed PSBT
    → Electrum finalises and broadcasts
```

Supported devices in Electrum v4.8.1: Trezor, Ledger, KeepKey, Digital Bitbox (BitBox01), BitBox02, Archos Safe-T, Coldcard, Jade.

## Air-gapped signing flow (Coldcard / offline machine)

```
Online watching-only wallet → creates unsigned transaction → exports as:
    (a) File (.psbt) on USB stick
    (b) QR code (animated for large transactions)
    (c) Audio modem (experimental plugin)

Offline signer (Coldcard via SD card, or Electrum on air-gapped machine):
    → reads PSBT
    → verifies outputs and amounts (device screen or offline GUI)
    → signs → returns signed PSBT via same channel

Online wallet:
    → imports signed PSBT
    → broadcasts
```

Electrum caps the maximum transaction size for QR-code/base43 transport at 30 KB (enforced in v4.8.1, `#10798`).

## Multisig PSBT flow

For m-of-n multisig, each cosigner sequentially adds their partial signature to the PSBT:

```
Cosigner 1 creates unsigned PSBT → adds signature → exports to cosigner 2
Cosigner 2 verifies, adds signature → n-1 signatures collected
Final cosigner signs → PSBT is complete → any party can broadcast
```

Electrum's CosignerPool plugin automates the exchange over an encrypted relay server: the partially signed PSBT is encrypted to the next cosigner's master public key before upload.

## Privacy note

PSBT includes full UTXO data (previous transaction outputs), which means all parties in the signing flow see all input amounts. For Taproot inputs, the sighash commits to all input amounts by protocol design.

## Supported PSBT features in Electrum

| Feature | Supported |
|---------|-----------|
| PSBT v0 (BIP174) | Yes |
| PSBT v2 (BIP370) | [NOT FOUND] — not confirmed in source inspection |
| Finalisation and broadcast | Yes |
| Trezor pre-signed external inputs | Yes (added v4.5.x, `#8324`) |
| Nostr relay transport (psbt_nostr plugin) | Yes |

## BlueWallet PSBT implementation

BlueWallet uses `bitcoinjs-lib`'s `Psbt` class (TypeScript) for all PSBT construction and signing. Key characteristics:

- **Creation:** `MultisigHDWallet` and `AbstractHDElectrumWallet` both construct `Psbt` objects for transaction signing
- **Hardware wallet flow:** PSBT exported as file (share sheet on iOS/Android) or BC-UR v2 animated QR (added v8.0.1 for OneKey and Keystone); signed PSBT returned via same channel
- **Air-gapped:** Coldcard supported via PSBT file import/export; Cobo Vault via QR; BC-UR v2 for Keystone/OneKey
- **Multisig coordinator:** BlueWallet acts as PSBT coordinator — creates unsigned PSBT, collects partial signatures from cosigners (hardware wallets or other BlueWallet instances), finalises and broadcasts
- **Unchained cosigner import:** v8.0.1 added import of Unchained JSON as a multisig cosigner, allowing interoperability with Unchained Capital's multisig platform
- **Crypto library:** v8.0.1 replaced crypto-js with `@noble/secp256k1` and `@noble/ciphers` throughout — improved auditability

**Hardware devices confirmed supported (as of v8.0.1, 2026-08-12):**
- Ledger (BLE)
- Coldcard (PSBT file)
- Cobo Vault (QR)
- Keystone (BC-UR v2 QR)
- OneKey (BC-UR v2 QR)

Sources: source inspection of `multisig-hd-wallet.ts`, `abstract-hd-electrum-wallet.ts`, v8.0.1 release notes — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-bluewallet-bluewallet-releases-latest.json)

## Sources

- [electrum/transaction.py — PSBT implementation](https://github.com/spesmilo/electrum/blob/master/electrum/transaction.py) — accessed 2026-08-10 — source code inspection
- [Serialization of unsigned or partially signed transactions — readthedocs](https://electrum.readthedocs.io/en/latest/transactions.html) — accessed 2026-08-10
- [Cold Storage — readthedocs](https://electrum.readthedocs.io/en/latest/coldstorage.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-latest.html)
- [Hardware wallets on Linux — readthedocs](https://electrum.readthedocs.io/en/latest/hardware-linux.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-hardware-linux.html)
- [RELEASE-NOTES — Trezor external pre-signed inputs, QR size cap](https://raw.githubusercontent.com/spesmilo/electrum/master/RELEASE-NOTES) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-spesmilo-electrum-RELEASE-NOTES.txt)
