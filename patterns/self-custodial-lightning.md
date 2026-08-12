---
tags: [pattern, lightning, self-custodial, splicing, single-channel, bitcoin, mobile]
applies_to: [phoenix]
created: 2026-08-12
---

# Pattern: Self-Custodial Lightning via Single-Channel Splicing

This pattern describes the architecture used by Phoenix (ACINQ) to deliver fully self-custodial Lightning payments on a mobile device without requiring the user to manage channels, liquidity, or routing tables. It contrasts sharply with the [[patterns/lndhub-lightning|LNDhub custodial hub model]] used by BlueWallet.

## Problem being solved

Mobile Lightning wallets face a fundamental tension:

1. **Self-custody requires channel keys** — the user must hold signing keys for their Lightning channels.
2. **Channel management is complex** — opening, closing, balancing, and routing all require significant technical knowledge and always-on connectivity.
3. **Mobile devices are resource-constrained** — maintaining a routing table, running a full LN node, and staying online are impractical.

Early approaches either sacrificed custody (LNDhub, custodial wallets) or sacrificed usability (requiring users to manage channels, liquidity, inbound/outbound balances).

## The single-channel + splicing solution

Phoenix's approach (introduced v2.0.0, August 2023) has three components:

### 1. Single channel per user

Each Phoenix user has exactly one Lightning channel, always with ACINQ's node. This channel persists indefinitely and is resized (spliced) as needed rather than being closed and reopened. Prior to v2.0.0, Phoenix could accumulate many channels per user; splicing eliminated this fragmentation.

**Key implication:** All of a user's Lightning liquidity is in one UTXO / one channel state. This is the minimum on-chain footprint for self-custodial Lightning.

### 2. Splice-in (on-chain → Lightning)

When a user receives a Bitcoin transaction to their on-chain address (P2TR since v2.2.0), Phoenix:

1. Waits for 3 on-chain confirmations
2. Creates a splice transaction that expands the existing channel by the incoming amount
3. The splice transaction is a standard Bitcoin transaction that is indistinguishable from a regular Bitcoin spend (since v2.7.0, via taproot)
4. The user pays only mining fees — no percentage fee

**Trust model:** Splice-in is trustless once the on-chain deposit is confirmed. The user's funds are in their own swap-in wallet (controlled by Phoenix's keys) until the splice completes.

### 3. Splice-out (Lightning → on-chain)

When a user sends to a Bitcoin address, Phoenix:

1. Creates a splice transaction that reduces the channel by the payment amount and pays the on-chain address directly
2. No external swap service is involved — the operation is a standard Bitcoin transaction from the channel's UTXO
3. The user sets their own feerate and can bump fees via CPFP

**Trust model:** Splice-out is fully trustless — no third-party holds funds during the process.

## Trampoline routing

Because Phoenix runs on a mobile device and cannot maintain a Lightning routing table, it uses **trampoline routing**:

1. Phoenix sends a "trampoline payment" to ACINQ's node
2. ACINQ's node (the trampoline) computes the actual route to the recipient and forwards the payment
3. Phoenix's only peer is ACINQ's node — it cannot route via other nodes

**Consequence:** ACINQ observes all payment metadata (sender, recipient, amount). As ACINQ states in their FAQ: "The current version of Phoenix offers no advantage regarding privacy over existing, hosted, custodial wallets." Future versions plan to add blinded paths to improve this.

## Taproot channels (v2.7.0, October 2025)

From v2.7.0, all new Lightning channels use taproot:

- Channel open/close/splice transactions appear as regular Bitcoin taproot spends
- Cannot be identified as Lightning channel operations by chain analysis
- ~15% cheaper on-chain due to taproot script efficiency
- Existing channels migrate to taproot automatically on next on-chain operation

## Fee model

| Operation | Fee |
|-----------|-----|
| LN send | 0.4% + 4 sat (fixed; shown pre-payment) |
| LN receive (sufficient liquidity) | 0 (free) |
| LN receive (insufficient liquidity — splice required) | 1% + mining fees |
| On-chain receive (swap-in) | mining fees only |
| On-chain send (splice-out) | mining fees only |
| New channel creation | 1000 sat flat |
| Inbound liquidity reservation | 1% + mining fees |

The 1% fee for insufficient-liquidity receives is the key economic trade-off: users who receive large infrequent payments pay once; users who maintain adequate inbound liquidity pay nothing for receives.

## Trust model summary

| Aspect | Trust level |
|--------|-------------|
| Custody of channel keys | User holds keys — fully self-custodial |
| Channel state backup | Encrypted backup on ACINQ server; user's seed sufficient to recover |
| Routing | ACINQ node — trampoline (trust for privacy, not for funds) |
| On-chain swap trustlessness | Trustless (since v2.0.0) |
| ACINQ node dependency | High — no alternative peers; if ACINQ disappears, force-close required |
| Payment privacy | Low — ACINQ observes sender, recipient, amount |
| On-chain privacy | High (since v2.7.0 taproot) |

## Contrast with LNDhub (BlueWallet)

| Dimension | Phoenix (self-custodial splicing) | LNDhub (BlueWallet) |
|-----------|----------------------------------|---------------------|
| Custody | User holds channel keys | Operator holds funds |
| Trust for funds | Trustless | Full custodial trust |
| Trust for privacy | ACINQ sees payments | Hub operator sees all |
| Channel management | Automatic (splice-in/out) | Hidden (shared hub channels) |
| On-chain footprint | 1 UTXO per user | Shared across users |
| Self-hosting option | No (must use ACINQ) | Yes (self-host LNDhub + LND) |
| Complexity for user | None — transparent | None — transparent |
| Fee model | 0.4% send + mining fees | Depends on hub operator |

## PhoenixD — server extension of this pattern

[PhoenixD](https://github.com/ACINQ/phoenixd) applies the same single-channel + splicing model for server/daemon use cases. Developers can run a phoenixd instance (Linux/macOS/Windows WSL) with the same `lightning-kmp` engine, exposing an HTTP API for programmatic Lightning payments. This extends the pattern to merchant, API, and service use cases without introducing custodial risk.

## Sources

| Source | URL | Accessed |
|--------|-----|----------|
| Phoenix FAQ | https://phoenix.acinq.co/faq | 2026-08-12 — [archived](../sources/2026-08-12-phoenix-acinq-co-faq-full.txt) |
| Splicing blog post | https://acinq.co/blog/phoenix-splicing-update | 2026-08-12 — [archived](../sources/2026-08-12-acinq-co-blog-phoenix-splicing-update.txt) |
| v2.0.0 release notes | https://api.github.com/repos/ACINQ/phoenix/releases/tags/android-v2.0.0 | 2026-08-12 |
| v2.7.0 release notes (taproot) | https://api.github.com/repos/ACINQ/phoenix/releases/tags/android-v2.7.0 | 2026-08-12 |
| GitHub README | https://raw.githubusercontent.com/ACINQ/phoenix/master/README.md | 2026-08-12 — [archived](../sources/2026-08-12-github-com-acinq-phoenix-README.md) |
