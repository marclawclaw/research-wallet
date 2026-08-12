---
tags: [pattern, privacy, tor, network, transport]
applies-to: [feather-wallet, cake-wallet]
related: [monero-stealth-address, full-node-wallet]
---

# Pattern: Tor-by-Default — Bundled Tor as a First-Class Transport

## What it is

"Tor-by-default" is a wallet architecture pattern in which the Tor anonymity network is bundled within the wallet application binary and activated automatically at startup, requiring no external Tor installation or manual proxy configuration from the user. The goal is to provide IP-layer anonymity for wallet-to-node communication (and transaction broadcasting) out of the box, without placing the configuration burden on privacy-conscious users.

## Why it matters

When a Monero (or other cryptocurrency) wallet connects to a remote node, the node operator can see the connecting IP address. Even if on-chain transactions are unlinkable (as Monero's are by design), the node operator may be able to correlate:

- **Sync timing** with transaction broadcast timing, linking a specific IP to a specific transaction.
- **Viewing key inference:** For Monero, the node learns which block ranges the wallet is scanning — potentially narrowing the wallet's age or approximate balance range.
- **Address observation (for transparent chains):** For Bitcoin or Ethereum, the node can directly associate the wallet's addresses with the connecting IP.

Bundling Tor and enabling it by default eliminates the IP linkage without requiring the user to install Tor separately, configure a SOCKS5 proxy, or understand the Tor network.

## How Feather Wallet implements this

Feather Wallet is the primary reference implementation of the tor-by-default pattern in the Monero wallet ecosystem.

**Startup detection and bundled binary:**

On launch, Feather checks for a running Tor daemon on `localhost:9050` (the standard Tor port). If a local Tor daemon is not found, Feather extracts and starts the bundled Tor binary on port `19450`. The bundled binary is the official Tor binary, versioned and updated with each Feather release (v2.8.1 ships Tor v0.4.8.16).

**Three traffic modes (user-configurable):**

| Mode | Sync traffic | Transaction broadcast / queries |
|------|--------------|---------------------------------|
| Never over Tor | Direct | Direct |
| Switch to Tor after sync (default) | Direct | Via Tor |
| Always over Tor | Via Tor | Via Tor |

The default mode ("switch to Tor after sync") reflects a deliberate privacy/convenience trade-off: synchronisation is data-intensive and slow over Tor, while the node does not learn much about wallet identity from sync requests alone. Post-sync, sensitive operations (transaction broadcast, fee queries) route through Tor.

**OS-level override:**

On Tails OS, Whonix, or when Feather is launched via `torsocks`, all network traffic is routed through the system Tor, overriding the in-app mode setting. Local node traffic is never routed through Tor.

**Onion service accessibility:**

The featherwallet.org website, documentation, and download page are available at `.onion` and `.b32.i2p` (I2P) mirrors, enabling fully anonymous discovery and download.

## How Cake Wallet implements this

Cake Wallet offers optional built-in Tor (single toggle in Settings → Connections), but it is **not enabled by default**. The documentation warns of significantly slower sync. Cake Wallet requires the user to actively enable Tor; it does not bundle Tor in the same automatic manner as Feather.

On Tails or via `torsocks`, Cake Wallet traffic would route through the system Tor, but this is not officially documented as a supported configuration.

## Trade-offs

| Dimension | Tor-by-default | Opt-in Tor | No Tor |
|-----------|----------------|-----------|--------|
| IP anonymity | Strong (default) | Weak (requires user action) | None |
| Setup friction | Zero | Low | Zero |
| Sync speed | Slower (if "always Tor") or mixed (if "switch after sync") | Full speed until enabled | Full speed |
| Binary size | Larger (+Tor binary, ~4–6 MB) | Unchanged (system Tor used) | Unchanged |
| Trust in Tor network | Required | Required when enabled | N/A |
| Exit node vulnerability (applies to clear node connections) | Mitigated for post-sync ops | Mitigated when enabled | Not mitigated |
| Protection against adversarial nodes | Partial (Tor hides IP; node still sees timing and request patterns) | Same | None |

## Limitations

**Tor does not provide transaction-level anonymity on its own.** Feather combines Tor with Monero's on-chain privacy (RingCT, stealth addresses, ring signatures), which together address both IP-layer and on-chain-layer privacy. Using Tor with a transparent chain (e.g. Bitcoin) provides IP anonymity only; on-chain linkability remains.

**The "switch after sync" default leaves sync traffic unprotected.** While the Feather developers argue that node operators learn little from sync requests, a motivated adversary who can observe both the network and the node may still be able to correlate sync patterns.

**Tor circuit reuse risk.** If the Tor circuits used for sync and for transaction broadcast are the same, circuit correlation remains possible. Feather's architecture of separating sync (direct) from post-sync ops (Tor) avoids this specific risk in the default mode.

## Sources

- [Docs: Tor support — Feather Wallet](https://docs.featherwallet.org/guides/tor-support) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-tor-support.html)
- [Feather Wallet v2.8.1 changelog — Tor updated to v0.4.8.16](https://github.com/feather-wallet/feather/releases/tag/2.8.1) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather-releases-latest.json)
- [Feather Wallet homepage: Clearnet/Onion/I2P mirrors](https://featherwallet.org) — accessed 2026-08-12 — [archived](../sources/2026-08-12-featherwallet-org-home.html)
- [Docs: Cake Wallet built-in Tor](https://docs.cakewallet.com/features/privacy-and-security/built-in-tor) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-tor.html)
