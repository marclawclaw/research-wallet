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

## Sources

- [electrum/transaction.py — PSBT implementation](https://github.com/spesmilo/electrum/blob/master/electrum/transaction.py) — accessed 2026-08-10 — source code inspection
- [Serialization of unsigned or partially signed transactions — readthedocs](https://electrum.readthedocs.io/en/latest/transactions.html) — accessed 2026-08-10
- [Cold Storage — readthedocs](https://electrum.readthedocs.io/en/latest/coldstorage.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-latest.html)
- [Hardware wallets on Linux — readthedocs](https://electrum.readthedocs.io/en/latest/hardware-linux.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-hardware-linux.html)
- [RELEASE-NOTES — Trezor external pre-signed inputs, QR size cap](https://raw.githubusercontent.com/spesmilo/electrum/master/RELEASE-NOTES) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-spesmilo-electrum-RELEASE-NOTES.txt)
