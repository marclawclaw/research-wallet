---
tags: [pattern, privacy, rpc, infura, censorship, consensys, ethereum, evm]
applies_to: [metamask]
related: [tor-by-default]
---

# Pattern: Centralised RPC Dependency (Infura)

## What it is

MetaMask routes all Ethereum JSON-RPC requests (balance checks, transaction submission, gas estimation, token detection, event log queries) through Infura by default. Infura is a blockchain infrastructure service owned by ConsenSys — the same company that makes MetaMask. Users who do not change the default configuration have their full RPC activity logged by a ConsenSys subsidiary.

## What Infura sees

For each RPC call MetaMask makes, Infura's servers receive:

- The originating IP address
- The `eth_call`, `eth_getBalance`, `eth_getLogs`, or other JSON-RPC method name
- All parameters — including wallet addresses, transaction data, contract addresses, and token amounts
- Timestamps

Infura can therefore construct a graph of which addresses a user monitors, which contracts they interact with, and the timing of all their activity.

## The 2022 Iran blocking incident

In November 2022, ConsenSys/Infura blocked API requests from IP addresses geolocated to Iran, Venezuela, and several other OFAC-sanctioned jurisdictions. MetaMask users in those countries temporarily lost access to their wallets via the default Infura endpoint. This demonstrated the censorship vector: a single company decision can revoke access for entire regions.

MetaMask responded by enabling users to manually set custom RPC endpoints, but the default remained Infura.

## Mitigation: custom RPC

MetaMask allows the default RPC to be replaced per-network with any Ethereum-compatible endpoint. Options include:

| Option | Description | Trust model |
|--------|-------------|-------------|
| Self-hosted Geth/Reth/Nethermind/Besu | Full Ethereum node on user's own hardware or server | Self-sovereign |
| Self-hosted Erigon | Archive node variant | Self-sovereign |
| Alchemy (with IP suppression) | Third-party; offers IP anonymisation in some plans | Trusted third party |
| QuickNode | Third-party RPC provider | Trusted third party |
| Ankr Public RPC | Free public RPC; less reliable; Ankr still sees IP | Trusted third party |
| Pocket Network | Decentralised node network; no single company controls | Decentralised (staking-based) |
| Chainlist | Directory of public RPCs; quality varies | Varies |

To change: Settings → Networks → [Select network] → Edit → change "New RPC URL".

## Infura metadata collection scope

Beyond RPC calls, MetaMask's default configuration also involves:

- **MetaMetrics:** Opt-in usage analytics via Segment (ConsenSys). If opted in, wallet events (page views, transaction submissions, swap initiations) are sent to ConsenSys servers.
- **Token price feeds:** MetaMask fetches ETH and token prices from ConsenSys' price API by default.
- **NFT metadata:** NFT images and metadata are fetched from ConsenSys CDN / IPFS gateways in many cases.

All of these are configurable or can be mitigated with appropriate settings; none are forced on the user after initial setup.

## Network-layer privacy

MetaMask has no built-in Tor support. Even with a custom RPC endpoint, the DNS resolution and TCP connection for the RPC endpoint are visible at the network layer to ISPs and network monitors. Users who require network-layer privacy must:

1. Run MetaMask inside Tor Browser (possible but complex; WebHID/USB hardware wallets do not work in Tor Browser)
2. Route all traffic through a system-wide VPN (shifts trust to VPN provider)
3. Self-host an RPC node on Tor (advanced)

This contrasts sharply with Monero wallets like Feather Wallet (Tor on by default) or Monero GUI (SOCKS5 proxy to .onion nodes).

## Comparison to other EVM wallets

All major browser-extension EVM wallets face the same structural challenge: the extension needs RPC access to function, and the default is always a centralised provider (Alchemy for Rainbow, Infura for MetaMask, custom for Rabby which defaults to multiple providers).

Rabby uses a multi-provider fallback (Alchemy, QuickNode, and others) rather than a single Infura dependency, distributing the surveillance surface but not eliminating it.

## RFP implications

For any wallet intended for privacy-sensitive users or jurisdictions with infrastructure censorship risk, the Infura default is a significant concern. A wallet built for the Logos Execution Zone (LEZ) environment should:

1. Either default to a self-hosted RPC node, or
2. Default to a privacy-respecting decentralised RPC network (Pocket), or
3. Build in Tor/I2P support for the RPC layer

## Sources

- [MetaMask extension README](https://raw.githubusercontent.com/MetaMask/metamask-extension/main/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-metamask-extension-README.md). Confirms Infura as default via `INFURA_PROJECT_ID` in `.metamaskrc` and reference to `getInfuraRpcUrls` helper in docs.
- [background.js: `infura-project-id` import](https://raw.githubusercontent.com/MetaMask/metamask-extension/main/app/scripts/background.js) — accessed 2026-08-12. `import '../../shared/constants/infura-project-id'` at the top of the service worker confirms Infura is baked into the build.
- ConsenSys Iran blocking: reported by multiple crypto news outlets November 2022; no single archived source in this repo — [NOT FOUND in local archive].
- [coinlaw.io/metamask-wallet-statistics/](https://coinlaw.io/metamask-wallet-statistics/) — accessed 2026-08-10 — [archived](../sources/2026-08-10-coinlaw-io-metamask-wallet-statistics.html). References ConsenSys/Infura relationship and IPO analysis.
