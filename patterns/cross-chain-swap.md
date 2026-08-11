---
tags: [pattern, swap, exchange, privacy, cross-chain, bitcoin, monero]
applies-to: [cake-wallet]
related: [monero-stealth-address]
---

# Pattern: Cross-Chain Swap Aggregation in a Self-Custody Wallet

## What it is

A swap aggregation pattern where a self-custody wallet presents a unified "Swap" interface that routes cross-chain exchanges through a selection of third-party providers — ranging from decentralised on-chain protocols to centralised intermediaries — without requiring the user to leave the wallet app or expose their seed phrase to any external service.

## Why it matters

For Monero users specifically, cross-chain swaps (especially BTC ↔ XMR) are a key privacy workflow: acquiring XMR from Bitcoin without using a KYC exchange requires either a peer-to-peer trade or a non-custodial swap protocol. Cake Wallet is notable for providing this capability in-app, including access to Chainflip (decentralised) and ChangeNow (centralised).

## How Cake Wallet implements it

**Flow:**

1. User opens the "Swap" tab in Cake Wallet.
2. User selects source coin and destination coin (e.g. BTC → XMR).
3. Cake Wallet requests a quote from all available providers for that pair.
4. App displays quotes with estimated output amounts and provider name. User can filter for "decentralised only" or select "Best Rate".
5. User optionally selects "Fixed rate" (locks the quoted amount at a slightly worse rate) or leaves "floating rate" (market rate at execution, slippage risk).
6. User confirms. Cake Wallet sends the source funds from the user's wallet to the provider's deposit address. The provider sends the output to the user's destination address in the destination coin.
7. Trade ID is recorded locally for support tracking.

**Provider taxonomy:**

| Provider | Type | Trust model | Notes |
|----------|------|-------------|-------|
| Chainflip | Decentralised protocol | Non-custodial (liquidity pool, on-chain) | Decentralised validator network; cross-chain swaps including BTC ↔ XMR |
| Jupiter | Decentralised (Solana) | Non-custodial (on-chain DEX aggregator) | Solana-only; aggregates Solana DEXes |
| Near Intents | Decentralised (NEAR) | Non-custodial (intent-based) | NEAR ecosystem |
| ChangeNow | Centralised aggregator | Custodial during swap | Receives source funds, sends output; briefly holds funds |
| Trocador | Centralised meta-aggregator | Custodial during swap | Routes to best available exchange (itself aggregates) |
| LetsExchange | Centralised aggregator | Custodial during swap | Similar to ChangeNow |

**What "non-custodial wallet" means in this context:**

Cake Wallet itself never holds funds. However, for swaps via centralised providers (ChangeNow, Trocador, LetsExchange), the user's source funds are briefly custodied by that third party during the swap. This is a meaningful distinction for threat modelling: if ChangeNow is subject to a government order or hack during the swap window, the user's funds could be seized or lost.

For Chainflip swaps, the funds are locked in on-chain smart contracts rather than held by a company, reducing counterparty risk to the protocol's validator set and smart contract correctness.

## Privacy considerations

- **Centralised provider risk:** ChangeNow, Trocador, and LetsExchange see the source address, destination address, swap amount, and IP address of the swap request. If Tor is enabled in Cake Wallet, the IP is masked at the app layer, but the swap provider still sees the source and destination addresses.
- **Address correlation:** A BTC→XMR swap via a centralised provider creates a link between the user's BTC address and XMR address at the provider's database level (even if the chain-level link is broken). For high-assurance privacy, a BTC address → XMR address swap should use a decentralised provider (Chainflip) or a peer-to-peer route.
- **Chainflip as the privacy-preserving option:** Chainflip's on-chain protocol does not require a company database to know the source and destination. The cross-chain link still exists on the respective chains (BTC spend and XMR receipt) but is not recorded by a KYC entity.

## Implementation pattern vs true atomic swaps

True atomic cross-chain swaps use Hash Time-Locked Contracts (HTLCs) or equivalent cryptographic constructs to ensure both legs of the swap either complete atomically or are refunded — with no trusted intermediary and no custodial period. This is technically complex for Monero (due to the absence of scriptable smart contracts) and requires both parties to be online simultaneously.

Cake Wallet's swap aggregation pattern is a pragmatic alternative: it achieves the user experience of cross-chain swaps with much simpler implementation, at the cost of brief custodial exposure for centralised providers. Chainflip's inclusion represents a step toward the decentralised ideal without the complexity of HTLC-based atomic swaps.

## Limitations

- KYC thresholds: Centralised providers may impose identity verification for swaps above certain amounts. Thresholds are [NOT FOUND] in Cake Wallet's documentation.
- Disabled providers: Exolix, Swaps.xyz, StealthEX have been disabled at various points (swap-provider policy changes or technical issues).
- Swap fees: Both network fees and provider fees apply. Specific percentages are [NOT FOUND] in Cake Wallet documentation; fees are shown at time of quote.
- Speed: Swap completion time depends on blockchain confirmation times of both chains involved and provider processing time.

## Sources

- [Cake Wallet docs: Swap feature](https://docs.cakewallet.com/features/basic/swap) — accessed 2026-08-12
- [Cake Wallet docs: FAQ — Swaps and Exchanges](https://docs.cakewallet.com/faq/swaps-and-exchanges.md) — accessed 2026-08-12
- [Cake Wallet docs: LLM full export](https://docs.cakewallet.com/llms-full.txt) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-cakewallet-com-llms-full.txt)
- [Chainflip protocol](https://chainflip.io) — referenced in Cake Wallet documentation
