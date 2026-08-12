---
title: "Zcash PCZT — Partially Completed Zcash Transaction"
tags: [zcash, pczt, hardware-wallet, offline-signing, cold-wallet, psbt, zip-244]
created: 2026-08-12
accessed: 2026-08-12
pattern_type: transaction_signing_workflow
---

# Zcash PCZT — Partially Completed Zcash Transaction

## What it is

PCZT (Partially Completed Zcash Transaction) is the Zcash equivalent of Bitcoin's PSBT (Partially Signed Bitcoin Transaction — BIP 174). It is a serialisation format for an unsigned or partially signed Zcash transaction that can be passed between multiple parties or devices for signing, without any party having access to the full key set or the live network.

## Problem it solves

Zcash shielded transactions require complex zero-knowledge proof generation. A hardware wallet or offline signer cannot independently produce the full transaction — it typically needs:
1. The spending key or authorisation signature
2. The zero-knowledge proof (which may require significant computation)

PCZT separates these concerns: the online wallet constructs the transaction structure and proof, then passes an unsigned PCZT to the signer. The signer adds only the authorisation (signature/binding signature), and the completed transaction is returned to the online wallet for broadcast.

## How it differs from the YWallet cold-wallet workflow

YWallet's cold-wallet workflow uses a **proprietary unsigned transaction file** (not PCZT). The file is produced by YWallet, transferred by USB OTG to an offline device running the `zcash-sync` CLI or a prior version of YWallet, signed offline, then returned for broadcast. This is a two-device software air-gap, not a standardised hardware wallet protocol.

**PCZT** is the standardised format, introduced after YWallet's original cold-wallet design. It enables interoperability between different Zcash wallets and hardware signers.

## Implementations (as of 12 August 2026)

| Wallet/Tool | PCZT support | Notes |
|-------------|-------------|-------|
| [[zkool2]] (ZKool) | Y — "Load/Save/Sign Transactions: Offline Signing, Cold Wallet, PCZT" (README) | Successor to YWallet |
| [[ywallet]] | N — uses proprietary unsigned transaction format | Deprecated; PCZT not backported |
| [[zodl]] | Y — Keystone hardware wallet integration uses QR-based shielded signing (functionally similar; exact PCZT usage [NOT FOUND]) | — |
| Zcash Foundation tooling | Under development (ZIP 244 defines transaction format) | — |

## Relationship to Keystone hardware wallet

Keystone's Zcash firmware (v3.0.1+, supported by Zodl) provides air-gapped shielded transaction signing via QR code. Whether this uses the PCZT format specifically or a proprietary format is [NOT FOUND] as of 12 August 2026.

## Related Bitcoin pattern

[[psbt-hardware-signing]] — Bitcoin's PSBT (BIP 174/370) is the direct analogue. PCZT differs in that it must also carry zero-knowledge proof material for Sapling/Orchard shielded components.

## Sources

- ZKool2 README: https://raw.githubusercontent.com/hhanh00/zkool2/main/README.md — accessed 2026-08-12 — archived `sources/2026-08-12-github-com-hhanh00-zkool2-README.md`
- YWallet README (cold wallet description): https://raw.githubusercontent.com/hhanh00/zwallet/main/README.md — accessed 2026-08-12 — archived `sources/2026-08-12-github-com-hhanh00-zwallet-README.md`
