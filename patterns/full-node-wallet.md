---
tags: [pattern, architecture, full-node, privacy, self-sovereignty]
applies-to: [monero-gui]
related: [spv-electrum-server, monero-stealth-address]
---

# Pattern: Full-Node Wallet

## What it is

A wallet that bundles and runs a full copy of its network's blockchain locally, rather than relying on a third-party server or light-client protocol to supply transaction data. The wallet communicates with the local node via RPC rather than exposing any data to external infrastructure.

## Why it matters

The privacy and censorship-resistance model of most self-custody wallets has a structural weakness: the wallet must tell some server which addresses or transactions it is interested in. SPV wallets (Bitcoin's Electrum-style) expose address queries to Electrum servers. Remote-node Monero wallets expose the wallet's view key or address to the remote node operator.

A full-node wallet eliminates this leak: all transaction data is fetched from the node's local copy of the blockchain, which has been downloaded in full from the peer-to-peer network. The node operator is the wallet user themselves.

## How it works

1. **Node sync:** The bundled daemon downloads the full blockchain from the peer-to-peer network. For Monero, this is ~180 GB+ (uncompressed) or ~60 GB (pruned). For Bitcoin, ~600 GB+ (uncompressed). This is a one-time cost that may take days on consumer hardware.
2. **Local RPC:** The wallet sends RPC calls to the local daemon (e.g., `localhost:18081` for Monero mainnet). No external server learns which transactions the wallet is querying.
3. **Transaction broadcast:** Outgoing transactions are broadcast directly to the peer-to-peer network via the local node, not via an intermediary API.
4. **Ongoing sync:** After the initial sync, the daemon stays in sync by downloading new blocks as they arrive.

## Implementation in Sparrow Wallet (Bitcoin)

Sparrow is the only widely-used Bitcoin desktop wallet that supports **direct Bitcoin Core connection** (no Electrum server intermediary) as a first-class connectivity mode. This is functionally equivalent to a full-node wallet architecture for Bitcoin.

- **Cormorant library:** Sparrow uses its own Cormorant library to communicate with Bitcoin Core via RPC (`bitcoin-cli` / JSON-RPC). Cormorant manages descriptor wallets within Bitcoin Core rather than Electrum-protocol indexing.
- **Requirements:** Bitcoin Core v24 or later (for Taproot descriptor wallet support). Standard `bitcoin.conf` settings (RPC user/pass, or cookie authentication).
- **Privacy:** No Electrum server sees the wallet's scriptPubKeys. All transaction queries are answered by the local Bitcoin Core node, which has the full blockchain.
- **Initial sync:** Standard Bitcoin Core initial block download — ~600 GB+ uncompressed, or pruned. This is a one-time cost.
- **Taproot wallets on Bitcoin Core:** Supported from v1.7.2 (February 2023) via descriptor wallets.
- **Bootstrap mode:** [NOT FOUND] — Sparrow does not appear to offer a bootstrap mode like Monero GUI; users must wait for full node sync before wallet data is available via Bitcoin Core connection. Alternatively, they can temporarily use a public Electrum server and switch to Bitcoin Core when synced.

Sparrow also supports Electrum-protocol servers as a less privacy-preserving alternative (see [[spv-electrum-server]]).

## Implementation in Monero GUI

Monero GUI is the only Monero wallet with a full-node option. Feather Wallet, Cake Wallet, and Monerujo all connect to remote nodes.

- **Daemon bundling:** The release archive includes `monerod` alongside `monero-wallet-gui`. The GUI launches monerod as a subprocess with configurable flags (data directory, pruning, bootstrap node, etc.).
- **Bootstrap node:** While the local node is syncing, Monero GUI can use a remote bootstrap node to remain usable — a practical compromise between immediate usability and eventual sovereignty.
- **Mode selection:** At first launch, the wizard presents three modes. Advanced mode is the full-node mode. Simple mode and Simple (bootstrap) mode use remote nodes.
- **Pruned node:** Blockchain pruning is supported, reducing storage requirements to ~1/3 of the full chain while retaining full verification capability.
- **Trusted daemon:** The GUI has an explicit concept of a trusted vs untrusted daemon. When using a remote node in Simple mode, the daemon is untrusted; the wallet takes additional steps to avoid exposing sensitive data.

## Trade-offs

| Dimension | Full-node wallet | Remote-node / SPV wallet |
|-----------|-----------------|--------------------------|
| Privacy | Maximum — no third party learns wallet addresses | Degraded — remote node operator can correlate queries |
| Censorship resistance | Maximum — no intermediary to censor | Degraded — remote node can withhold transactions |
| Initial sync | Hours to days (blockchain size-dependent) | Seconds |
| Storage | ~60–600 GB (chain-dependent) | Minimal |
| Bandwidth | High ongoing (p2p block download) | Low |
| Complexity | High (daemon management) | Low |

## Related patterns

- [[spv-electrum-server]] — the dominant alternative for Bitcoin wallets; offers a middle ground via Electrum server trust
- [[monero-stealth-address]] — Monero's stealth addresses mean that even a remote Monero node cannot easily link queries to a specific user, but the node still learns the wallet's view key in some configurations

## Sources

- [getmonero.org/downloads/](https://www.getmonero.org/downloads/) — feature description of Simple vs Advanced mode — accessed 2026-08-12 — [archived](../sources/2026-08-12-getmonero-org-downloads.html)
- [GitHub: monero-project/monero-gui — wizard/WizardModeSelection.qml](https://raw.githubusercontent.com/monero-project/monero-gui/master/wizard/WizardModeSelection.qml) — accessed 2026-08-12
- [GitHub: monero-project/monero-gui — README.md](https://github.com/monero-project/monero-gui) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-monero-gui-README.md)
