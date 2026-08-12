---
title: "Zcash Warp Sync"
tags: [zcash, light-client, sync, warp-sync, merkle-tree, performance]
created: 2026-08-12
accessed: 2026-08-12
pattern_type: synchronisation_algorithm
---

# Zcash Warp Sync

## What it is

Warp Sync is a proprietary block-scanning algorithm developed by Hanh Huynh Huu (@hhanh00) for the `zcash-sync` Rust library, used as the sync engine for [[ywallet]] and [[zkool2]]. It dramatically accelerates witness computation for shielded notes in a Zcash light client.

## Problem it solves

Standard Zcash light-client scanning (per [[ZIP 307]]) requires:

1. Downloading compact blocks from a lightwalletd gRPC server
2. Scanning each transaction's output commitments against the wallet's keys (trial decryption)
3. Maintaining and updating a Merkle witness for each received note at every new block

Step 3 is the bottleneck at scale. The incremental witness update must touch every note for every block, resulting in O(N × B) operations where N = notes held and B = blocks scanned. For large accounts with thousands of notes and hundreds of thousands of blocks, this becomes prohibitively slow.

## How Warp Sync addresses it

Warp Sync replaces the per-block incremental witness update with a **batch Merkle computation** using a custom "Warp Merkle Tree" data structure. Key insights:

- Leaf commitments (one per output note in each block) are accumulated in bulk
- The Merkle tree root and witness paths are computed in batch at the end of a range of blocks, rather than updated one block at a time
- The algorithm exploits the static structure of already-processed blocks: previously committed leaves do not change, so their sub-tree hashes can be cached

The result is approximately **10,000 blocks per second** on a mid-range Android phone (OnePlus 7T, Snapdragon 855+), compared to typical light-client sync speeds of hundreds of blocks per second.

## Implementation

- Library: `hhanh00/zcash-sync` (Rust, MIT licence, 13 GitHub stars as of 2026-08-12)
- Used by: `hhanh00/zwallet` (YWallet) via Flutter–Rust FFI bridge; `hhanh00/zkool2` (ZKool) uses an "Improved Warp" variant with per-account sync state
- The Warp Sync algorithm handles both Sapling and Orchard commitment trees

## ZKool2 enhancement

ZKool2 ("Improved Warp") extends the algorithm with **per-account synchronisation state**: each account can be enabled or disabled for sync independently, allowing users to "park" old accounts without degrading sync speed for active accounts.

## Relationship to ZIP 307

ZIP 307 ("Light Client Protocol for Payment Detection") defines the gRPC interface between lightwalletd servers and light clients (compact block format, streaming). Warp Sync is a client-side implementation optimisation on top of ZIP 307 — it uses the same compact block stream but processes witnesses more efficiently. Warp Sync is not a Zcash protocol ZIP; it is an implementation detail internal to the `zcash-sync` library.

## Sources

- YWallet README: https://raw.githubusercontent.com/hhanh00/zwallet/main/README.md — accessed 2026-08-12 — archived `sources/2026-08-12-github-com-hhanh00-zwallet-README.md`
- `zcash-sync` GitHub API: https://api.github.com/repos/hhanh00/zcash-sync — accessed 2026-08-12 — archived `sources/2026-08-12-api-github-com-hhanh00-zcash-sync.json`
- ZKool2 README: https://raw.githubusercontent.com/hhanh00/zkool2/main/README.md — accessed 2026-08-12 — archived `sources/2026-08-12-github-com-hhanh00-zkool2-README.md`
- ZIP 307 (Light Client Protocol): https://zips.z.cash/zip-0307 — accessed 2026-08-12 — archived `sources/2026-08-12-zips-z-cash-zip-0307.html`
