---
tags: [pattern, spv, bitcoin, lightweight-client, server-protocol]
applies_to: [electrum]
---

# Pattern: SPV via Electrum Server Protocol

Electrum implements a specific variant of SPV (Simplified Payment Verification) that differs from the Nakamoto SPV described in the Bitcoin whitepaper. Rather than connecting peer-to-peer with full nodes, it uses a dedicated client-server protocol (JSON-RPC over SSL/TLS or plain TCP) with specialised indexing servers.

## Core mechanism

1. **Block headers only.** The client downloads and validates the chain of block headers (80 bytes each), maintaining the longest proof-of-work chain. Full blocks are never downloaded.

2. **Address subscription.** The client sends the server the SHA-256 hashes of its scriptPubKeys (not the raw addresses or public keys). The server subscribes the client to notifications when transactions affecting those hashes are confirmed or appear in mempool.

3. **Merkle proof verification.** For each confirmed transaction, the client requests a Merkle inclusion proof. It verifies the proof against the locally held header chain, confirming the transaction was included in a valid block without downloading the full block.

4. **Multi-server redundancy.** The client connects to ~10 servers simultaneously. All servers supply block headers (useful for detecting chain splits and lagging servers). One server is designated "main" — used for address subscriptions, broadcasting transactions, and (optionally) fee estimates.

5. **Fee estimation.** All connected servers are polled for fee estimates. With auto-connect enabled, the client uses the median value. Sanity limits (low/high) are applied client-side.

## Trust model

| Action | Trusted party | Attack risk |
|--------|---------------|-------------|
| Confirmed transaction inclusion | SPV-verified (Merkle proof) | Low — would require rewriting the chain |
| Unconfirmed transaction reporting | Main server | Medium — server can fabricate mempool entries |
| Lie by omission (withholding txs) | Main server | Medium — client cannot detect if server hides transactions |
| Fee estimates | Median of all servers | Low — requires majority collusion |
| IP address visibility | All connected servers | High — server sees client IP; mitigated by Tor |
| Address set privacy | Main server | High — server infers all addresses belong to same wallet |

## Privacy implications

The address-subscription model reveals to the server that all subscribed scriptPubKey hashes likely belong to the same entity. This is a known and documented trade-off.

Mitigations available in Electrum:
- Route connections through Tor + .onion Electrum servers
- Self-host an Electrum server (ElectrumX, Fulcrum) backed by a personal full node
- Use manual server pinning to a trusted server

## Electrum server protocol

The protocol is versioned JSON-RPC. Key methods include:
- `blockchain.scripthash.subscribe` — subscribe to a scriptPubKey hash
- `blockchain.scripthash.get_history` — fetch transaction history
- `blockchain.transaction.get_merkle` — get Merkle proof for a transaction
- `blockchain.headers.subscribe` — subscribe to new block headers
- `mempool.get_fee_histogram` — get fee histogram for dynamic fee estimation

The client uses SSL/TLS for all connections (plain TCP was removed as a client-selectable option in v3.1). Both CA-signed and self-signed certificates are accepted; self-signed certs use TOFU (Trust On First Use) pinning.

## Comparison to alternatives

| Approach | Storage | Trust | Privacy | Startup |
|----------|---------|-------|---------|---------|
| Full node (Bitcoin Core) | ~650 GB (2026) | Trustless | Best | Hours–days |
| SPV via Electrum server | ~few MB headers | Server-trusting | Moderate | Instant |
| SPV peer-to-peer (bloom filters) | ~few MB | Partial (IP leakage) | Poor | Minutes |
| Web wallet / custodial | None (client) | Full server trust | Poor | Instant |

## BlueWallet SPV implementation

BlueWallet connects to Electrum servers via `react-native-tcp-socket` (TCP/SSL). Key differences from Electrum desktop:

- **Default server:** `electrum1.bluewallet.io` (SSL 443) — BlueWallet-operated; trusted by default
- **Fallback servers:** `electrum.acinq.co` (SSL 50002)
- **User override:** Y — Settings → Electrum Server; any ElectrumX/Fulcrum endpoint accepted
- **No Merkle proof verification:** confirmed in FAQ — "The idea is that by default BW doesn't use public electrum servers, only ones hosted by BlueWallet, so they are kinda trusted." This is a weaker security model than Electrum desktop, which verifies Merkle proofs against locally held headers
- **No Tor:** no Tor/SOCKS proxy support detected in source inspection (2026-08-12); network-level privacy requires system VPN or routing
- **Server history:** previous Electrum servers stored locally (`ELECTRUM_SERVER_HISTORY`), allowing reconnection preference

Sources: `BlueElectrum.ts` source inspection — accessed 2026-08-12; FAQ.md — accessed 2026-08-12

## Sparrow Wallet Electrum server implementation

Sparrow connects to Electrum-compatible servers (ElectrumX, Fulcrum, Electrs, Electrs-Esplora, EPS, BWT) but explicitly does **not** implement SPV — it does not download and verify block headers or Merkle proofs. Instead, it fully delegates transaction data provision to the configured server.

Key differences from Electrum desktop and BlueWallet:
- **No Merkle proof verification.** Sparrow trusts the configured server to return correct transaction data. This is deliberate: the project encourages users to use a self-hosted server or Bitcoin Core to eliminate this trust.
- **No peer-to-peer block header download.** Sparrow's lightweight architecture relies entirely on server-side indexing.
- **Bitcoin Core (direct RPC):** Sparrow supports connecting directly to Bitcoin Core via the **Cormorant** library (a custom descriptor-wallet RPC adapter), bypassing Electrum-protocol servers entirely. Requires Bitcoin Core v24+ for Taproot wallet support. This is the highest-trust-minimising mode.
- **Public servers:** A curated list of pre-configured public servers is provided for beginners. The Sparrow documentation frames this as the starting point of a "privacy journey" toward self-hosted infrastructure.
- **SSL for all connections:** All Electrum server connections use SSL/TLS. The server SSL certificate is TOFU-pinned (stored in `certs/` in the Sparrow home directory).
- **Tor integration:** When a SOCKS5 proxy is configured or the server address is `.onion`, Sparrow routes all server communication through Tor. Local network addresses (192.168.*.*, 172.16.*.*, 10.*.*.*) bypass the proxy.
- **Electrum RPC batching:** Enabled for mempool-electrs servers from v2.1.0.

Supported server implementations (all over SSL):

| Server | Notes |
|--------|-------|
| Fulcrum | Recommended by Sparrow for performance |
| ElectrumX | Full support |
| Electrs | Full support |
| Electrs-Esplora | Full support |
| EPS (Electrum Personal Server) | Full support |
| BWT (Bitcoin Wallet Tracker) | Full support |
| Bitcoin Core (direct RPC) | Via Cormorant; highest privacy |

Sources: [sparrowwallet.com/features/](https://sparrowwallet.com/features/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-features.html); [sparrowwallet.com/docs/faq.html](https://sparrowwallet.com/docs/faq.html) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-docs-faq.html)

## Known CVEs

- **CVE-2012-2459** (severity: low): SPV verification could accept blocks with left-sibling hash duplicates in the Merkle tree, a known Bitcoin protocol flaw. Fixed in Electrum v4.8.0 (July 2026), release note: "fix CVE-2012-2459: reject left-sibling duplicates (#10568)".

## Sources

- [Simple Payment Verification — readthedocs](https://electrum.readthedocs.io/en/latest/spv.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-spv.html)
- [Electrum FAQ — Does Electrum trust servers?](https://electrum.readthedocs.io/en/latest/faq.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-faq-full.html)
- [Electrum protocol specification — readthedocs](https://electrum.readthedocs.io/en/latest/protocol.html) — accessed 2026-08-10
- [RELEASE-NOTES v4.8.0 — CVE-2012-2459 fix](https://raw.githubusercontent.com/spesmilo/electrum/master/RELEASE-NOTES) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-spesmilo-electrum-RELEASE-NOTES.txt)
