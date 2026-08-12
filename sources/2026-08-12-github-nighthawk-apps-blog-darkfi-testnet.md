---
layout: post
title: "Nighthawk Apps releases client suite for DarkFi Testnet"
subtitle: "Press Release"
post_type: press release
date: 2026-08-01
---

Sunshine Coast, Australia.

Nighthawk Apps today announced that its updated Nighthawk wallet suite spanning Android, iOS, Desktop and the **Moonshine** CLI; together with the companion **darkfi-lightwalletd** server, is available for public testing for the **DarkFi testnet**. The release positions confidential chain interaction and privacy-preserving light-client sync with UnifOMR as the default path for everyday users, rather than as an optional afterthought.

The majority of cryptocurrency networks still expose user activity by design. Transparent chains publish every transfer in full view, and in privacy chains where transaction graph and amounts are private, the light wallet servers typically learn which block ranges a client requests and the metadata can fingerprint wallets over time. DarkFi inverts that model: transfers are completely confidential on-chain and Nighthawk’s light wallet path is built so a block server need not learn which of a user’s notes decrypt. This release is strictly for testnet and works with highly experimental code.

---

## Closing the privacy gap

On a typical transparent network, balances and payment flows are public records. A light server that answers “give me blocks 1000–2000” can infer interest in those heights even if it never sees keys. Nighthawk on DarkFi takes a different stance. DRK transfers are private by construction, so there is no transparent-versus-shielded split for users to manage. Sync prefers **UnifOMR** (scheme `0x05`): encrypted digests and private information retrieval so the server does not learn which notes match. Production wallets talk only to **darkfi-lightwalletd** and avoid direct access to a full `darkfid` node. Sync also applies range padding, poll jitter, TLS certificate pinning for remote servers and optional Tor so traffic patterns and clearnet endpoints are harder to exploit.

The sync-time benefit is practical, not only cryptographic. Traditional trial decryption downloads every compact block in the scan window and attempts to decrypt every note — work that grows with chain length and output count, so catch-up and periodic sync get slower as the network ages. UnifOMR flips that cost model: the client asks for an encrypted any-match digest over the window, then pulls only the matching heights through PIR. Wallet sync therefore tracks *relevant activity* instead of re-scanning the whole tip history, which keeps restore and incremental sync far snappier on phones and desktops than a full trial-decrypt pass over the same range.

When UnifOMR cannot complete, clients may fall back to padded trial sync. That path is still stronger than raw full-node visibility, but it is intentionally described as degraded relative to the FHE-backed default — slower and heavier precisely because it returns to scanning the window note-by-note. Moonshine is stricter: it is UnifOMR-only and has no PerfOMR or height-list fallback.

---

## darkfi-lightwalletd

**darkfi-lightwalletd** is a dedicated light wallet server for the DarkFi network. It serves compact blocks — with ZK proofs, signatures, and PoW data stripped — over gRPC, runs UnifOMR detection when built with the default `fhe-omr` feature, and relays transactions with optional OMR clues for private receive detection. Cleartext is refused off-loopback; remote deployments expect TLS, and clients pin the leaf certificate’s SHA-256 digest. The server does not keep wallet accounts or session identity, and it is designed not to log detection keys or match heights.

In short: the light wallet server exists so phones, desktops, and CLI clients download far less than a full `darkfid` node would require, without becoming an oracle for “which blocks this wallet cares about” in the UnifOMR path. Under the active UnifOMR Param2 profile, a detection key is about **38 MB** (gRPC per-key cap **48 MB**, total request budget **64 MB**, up to **16** keys), digests are served at the last BFV RNS level for size and noise reduction, and a match returns only the relevant compact blocks via SealPIR-style `FetchPirBatch` instead of streaming every height for trial decryption.

Operators should size the host for UnifOMR detection load and compact-block cache growth. Recommended infrastructure specs for **darkfi-lightwalletd**:

| Scale | CPU | RAM | Storage | Bandwidth |
|-------|-----|-----|---------|-----------|
| ~1k users | 2 cores | 4 GB | 50 GB SSD | ~50 Mbps |
| ~10k users | 8 cores | 16 GB | 200 GB SSD | ~500 Mbps |
| ~100k users | 32+ cores (detector pool) | 64+ GB | 1 TB NVMe | ~5 Gbps |

