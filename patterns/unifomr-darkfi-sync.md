---
title: "UnifOMR and Private Information Retrieval for Wallet Sync"
pattern: unifomr-darkfi-sync
tags: [darkfi, privacy, pir, unifomr, light-client, sync, oblivious-message-retrieval, fhe, bfv]
created: 2026-08-12
accessed: 2026-08-12
---

# UnifOMR and Private Information Retrieval for Wallet Sync

UnifOMR (Unified Oblivious Message Retrieval, scheme `0x05`) is a cryptographic protocol used by Nighthawk Apps' DarkFi client suite to enable private light-wallet synchronisation. Unlike traditional trial decryption (where a light client downloads all compact blocks and attempts to decrypt each note), UnifOMR allows a client to retrieve only the blocks containing notes addressed to it — without the server learning which blocks those are.

## Problem: Light Client Sync Leaks Block Interests

Traditional light clients (e.g. Zcash lightwalletd trial decryption) download all compact blocks in a scan window and trial-decrypt every note with the wallet's Incoming Viewing Key (IVK). This approach has two weaknesses:

1. **Server learns block heights:** Even though the server cannot decrypt notes, it knows exactly which block range the client requested. A long-running client reveals its wallet activity timeline through repeated requests.
2. **Scales linearly with chain growth:** As the chain grows, initial sync and periodic catch-up become progressively slower because the client must process every block in the scan window.

## UnifOMR Solution: Encrypted Digests + Private Information Retrieval

UnifOMR flips the cost model:

1. **Detection key upload:** The client submits an encrypted detection key to the server (under BFV fully homomorphic encryption). In Nighthawk's DarkFi implementation, a detection key is approximately **38 MB** (gRPC per-key cap 48 MB; total request budget 64 MB; up to 16 keys simultaneously).
2. **Server computes digest:** The server performs homomorphic computation over its compact block store to generate an encrypted any-match digest — without learning which notes matched.
3. **Digests served at last BFV RNS level:** Reducing output size and noise.
4. **Client retrieves matching blocks via PIR:** Only the matching compact block heights are returned via a SealPIR-style `FetchPirBatch` call, rather than streaming every height for trial decryption.

The server does not log detection keys, does not record which blocks matched, and does not maintain wallet session identity.

## Comparison: Trial Decryption vs UnifOMR

| Property | Trial decryption (Zcash lightwalletd) | UnifOMR (Nighthawk DarkFi) |
|----------|--------------------------------------|---------------------------|
| Server learns block heights? | Y — explicitly requested | N — PIR hides which blocks matched |
| Bandwidth scales with chain length? | Y — downloads all blocks in window | N — downloads only matched blocks |
| Client computation | Trial-decrypt all notes | Decrypt matched blocks only |
| Server computation | None | Homomorphic evaluation (expensive) |
| Fallback | N/A | Padded trial sync (still better than raw) |

## Degraded Fallback: Padded Trial Sync

When UnifOMR cannot complete (network conditions, server failure, timeout), Nighthawk clients may fall back to padded trial sync. This path still provides stronger privacy than raw trial decryption because:
- Range padding is applied (so the server sees padded block requests, not exact ranges)
- Poll jitter is added to obfuscate timing

**Moonshine CLI is stricter:** UnifOMR-only, no PerfOMR or fallback. Moonshine refuses to operate in degraded mode.

## Nighthawk DarkFi Infrastructure

Nighthawk Apps operates `darkfi-lightwalletd` — the reference light wallet server for the DarkFi network — implementing the UnifOMR protocol. Key operational characteristics (from Nighthawk Apps press release, 1 August 2026):

- gRPC transport; cleartext refused off-loopback
- TLS required for remote deployments with client-pinned leaf certificate SHA-256 digest
- No wallet account storage; no session identity
- UnifOMR enabled via `fhe-omr` build feature (default)

**Recommended infrastructure sizing for darkfi-lightwalletd:**

| Scale | CPU | RAM | Storage | Bandwidth |
|-------|-----|-----|---------|-----------|
| ~1k users | 2 cores | 4 GB | 50 GB SSD | ~50 Mbps |
| ~10k users | 8 cores | 16 GB | 200 GB SSD | ~500 Mbps |
| ~100k users | 32+ cores (detector pool) | 64+ GB | 1 TB NVMe | ~5 Gbps |

## Relation to Zcash Lightwalletd Trust Model

The Zcash lightwalletd protocol (used by Zodl, YWallet, and Nighthawk's Zcash-era wallet) does NOT use UnifOMR. Trial decryption is the standard sync mechanism in the Zcash ecosystem. The privacy limitations of Zcash lightwalletd are documented in the [[zcash-shielded-transactions]] pattern note.

UnifOMR represents a next-generation approach — technically feasible in 2026 but deployed first by Nighthawk on the DarkFi network rather than on Zcash mainnet.

## Relation to Other Notes

- [[nighthawk]] — the wallet implementing UnifOMR via DarkFi
- [[zcash-shielded-transactions]] — Zcash pool architecture and trial-decryption-based sync

## Sources

| Source | URL | Accessed |
|--------|-----|----------|
| Nighthawk Apps DarkFi testnet press release | https://nighthawkapps.com/blog/nighthawk-darkfi-wallet-suite-testnet/ | 12 August 2026 |
| nighthawk-apps.github.io blog post source | https://raw.githubusercontent.com/nighthawk-apps/nighthawk-apps.github.io/master/_posts/2026-07-20-nighthawk-darkfi-wallet-suite-testnet.md | 12 August 2026 |
| nighthawk-ios-wallet README (DarkFi edition) | https://raw.githubusercontent.com/nighthawk-apps/nighthawk-ios-wallet/main/README.md | 12 August 2026 |
