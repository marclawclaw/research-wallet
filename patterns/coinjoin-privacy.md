---
tags: [pattern, privacy, coinjoin, bitcoin, mixing, payjoin, whirlpool]
applies_to: [sparrow-wallet, electrum]
related: [psbt-hardware-signing, spv-electrum-server]
---

# Pattern: CoinJoin and Collaborative Transaction Privacy

## What it is

CoinJoin is a Bitcoin transaction construction technique in which multiple parties combine their inputs and outputs into a single transaction, making it harder for blockchain analysts to determine which input maps to which output. It was described by Greg Maxwell in 2013 and has been implemented in several forms, each with distinct trust models, anonymity set sizes, and architectural approaches.

## Why it matters

Every Bitcoin transaction is permanently public on the blockchain. Without privacy techniques, it is possible for an adversary (exchange, chain analytics firm, state actor) to:
- Trace funds from a known address (e.g. exchange deposit) across transactions to identify clusters
- Infer wallet balances and spending patterns
- Link pseudonymous addresses to real-world identities via transaction graph analysis

CoinJoin breaks these heuristics by creating transactions where the mapping from sender to recipient is ambiguous.

## CoinJoin variants

### Whirlpool (Samourai)

A centralised-coordinator CoinJoin protocol developed by Samourai Wallet:
- Fixed pool denominations (0.001 BTC, 0.01 BTC, 0.05 BTC, 0.5 BTC on mainnet)
- Equal-output coinjoin: all outputs in a round are the same denomination, maximising entropy
- Remix for free: once a UTXO has been mixed into a postmix pool, it can be remixed indefinitely at no extra cost (only mining fees)
- Coordinator: Samourai's Soroban network (encrypted Tor-routed communication layer)

**Status as of 2026:** DEFUNCT. On 24 April 2024, US authorities (DOJ/FBI) arrested Samourai Wallet founders Keonne Rodriguez and William Hill on charges of operating an unlicensed money transmitter and money laundering conspiracy. Samourai's servers and domains were seized. The Soroban coordinator is offline; no functional Whirlpool coordinator is available on mainnet. Sparrow removed the Whirlpool client in v1.9.0 (24 April 2024).

Community efforts to create an alternative decentralised coordinator (replacing Soroban) are ongoing as of 2026-08-12, but no production-ready replacement is in Sparrow.

### JoinMarket

A decentralised CoinJoin market where "makers" offer liquidity for CoinJoin and "takers" pay a small fee to mix. No central coordinator — participants discover each other via an IRC-based orderbook. Not integrated in Sparrow.

### PayJoin (BIP78)

A two-party "fake" CoinJoin where the sender and receiver each contribute at least one input. Breaks the common-input-ownership heuristic and the change-detection heuristic:
- Only 2 parties (not N); provides limited anonymity set increase
- Requires the receiver to run a BIP78-compatible endpoint (e.g. BTCPay Server, JoinMarket as a receiver)
- Does not provide mixing in the Whirlpool sense; primarily breaks change-output detection

**Status in Sparrow:** PayJoin (BIP78) is supported — Sparrow can initiate a PayJoin request. Payjoin receiver error handling improved in v2.3.0. PayJoin v2 (BIP77, async/serverless) is [NOT FOUND] in Sparrow v2.5.3.

### Stonewallx2 (Samourai)

A two-person collaborative transaction that simulates a CoinJoin by having two parties each spend from their own inputs, creating ambiguity about which outputs belong to whom. Used via Samourai's Soroban network. Removed from Sparrow with Whirlpool in v1.9.0.

## Sparrow Wallet CoinJoin implementation — historical and current

**Timeline:**
- v1.5.0 (Sep 2021): Whirlpool integration added (testnet then mainnet)
- v1.5.3 (Dec 2021): Stonewallx2 and Stowaway (collaborative PayJoin via PayNyms) added
- v1.8.5 (Apr 2024): Final Whirlpool client update (Samourai Whirlpool 1.0.6 via Soroban)
- v1.9.0 (24 Apr 2024): All Whirlpool and Soroban features removed following Samourai arrests

**Current state (v2.5.3):**
- Whirlpool: Not available. No Samourai coordinator on mainnet.
- PayJoin (BIP78): Available — Sparrow initiates PayJoin requests from the Send tab when the recipient provides a BIP78 endpoint.
- Stonewallx2 / Stowaway: Not available.
- PostMix accounts: Viewable (for existing mixed UTXOs) but no new mixing possible.

**Privacy alternatives currently available in Sparrow:**
- PayNym (BIP47): Stealth payment codes — prevents address reuse and reduces address clustering, but not mixing
- Silent Payments (BIP352): New payment protocol where each sender derives a unique address from a static payment code; prevents address reuse and clustering without notification transactions; sending from v2.3.0, receiving from v2.5.0
- Tor routing: Obscures network-level IP leakage
- Coin control: Manual UTXO selection to avoid unintentional clustering
- Full node / private Electrum server: Prevents server-side address leakage

## Anonymity set and privacy comparison

| Technique | Anonymity set | Coordinator trust | Status in Sparrow v2.5.3 |
|-----------|--------------|-------------------|--------------------------|
| Whirlpool (Samourai) | High (100+ peers per pool) | Centralised (Samourai, now defunct) | Removed (v1.9.0) |
| PayJoin (BIP78) | Very low (2 parties) | None (P2P with receiver) | Y |
| Stonewallx2 | Low (2 parties, simulated) | Centralised (Soroban, now defunct) | Removed (v1.9.0) |
| JoinMarket | Medium–high (market-dependent) | Decentralised | Not in Sparrow |
| Silent Payments (BIP352) | Not a mixing protocol — prevents address reuse | None | Y (send v2.3.0, receive v2.5.0) |
| PayNym (BIP47) | Not a mixing protocol — stealth addresses | paynym.rs | Y |

## Sources

- [GitHub release v1.5.0 — Whirlpool mainnet](https://api.github.com/repos/sparrowwallet/sparrow/releases/tags/1.5.0) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-sparrowwallet-sparrow-releases.json)
- [GitHub release v1.9.0 — Whirlpool removal](https://api.github.com/repos/sparrowwallet/sparrow/releases/tags/1.9.0) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-repos-sparrowwallet-sparrow-releases.json)
- [sparrowwallet.com/docs/spending-privately.html](https://sparrowwallet.com/docs/spending-privately.html) — accessed 2026-08-12 — [archived](../sources/2026-08-12-sparrowwallet-com-docs.html)
- Samourai Wallet arrest: US DOJ press release, 24 April 2024 — [NOT FOUND in local archive; public record]
