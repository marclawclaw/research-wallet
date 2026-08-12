---
title: Executable NFT (xNFT)
tags: [pattern, xnft, solana, wallet-extensibility, in-wallet-apps]
access-date: 2026-08-12
---

# Executable NFT (xNFT)

An **executable NFT (xNFT)** is an NFT whose metadata payload includes a bundled application — typically a React component compiled to a browser-compatible bundle — stored on a decentralised storage layer (Arweave). When the wallet fetches the NFT, it renders the application inside a sandboxed iframe, creating an in-wallet mini-app.

## Origin

Invented by Coral (formerly 200ms) for the [[backpack]] wallet. The protocol is defined in `coral-xyz/xnft` (143 GitHub stars, 2026-08-12) and described as an "Executable NFT Protocol and Marketplace."

## How It Works

1. **Authoring:** Developer writes a React app using the xNFT SDK (`@coral-xyz/xnft`). The `xnft-cli` scaffold tool packages the app and deploys the bundle to Arweave.
2. **Minting:** Developer mints a Solana NFT (Metaplex standard). The NFT metadata URI points to the Arweave bundle rather than a static image/JSON.
3. **Distribution:** xNFT is listed in the Backpack xNFT marketplace. Users purchase/mint the NFT, which grants a copy to their wallet.
4. **Execution:** When the user opens the xNFT inside Backpack, the wallet fetches the bundle from Arweave, verifies ownership of the NFT, and renders the React app inside an isolated sandbox.
5. **Wallet API:** The sandboxed app communicates with the wallet via a restricted API — it can read the user's public key, request token balance information, and prompt the user to sign transactions. It cannot read or export private keys.

## Security Model

The xNFT execution sandbox is a key security claim:
- The app runs in an isolated context — no access to the host wallet's private key storage.
- No domain-name phishing attack surface — the app is loaded from Arweave content-addressed storage, not a spoofable domain.
- Transaction signing requires explicit user approval in the wallet UI, identical to any dApp interaction.

However, the Backpack repo README (2026-08-12) states "This code is unaudited. Use at your own risk." No public audit of the xNFT sandbox isolation has been found.

## Ecosystem Status (2026-08-12)

The xNFT ecosystem did not achieve critical mass. Backpack's product focus has shifted substantially toward Backpack Exchange (regulated CEX). The README still describes Backpack as "a home for your xNFTs" but the iOS/Android app description leads with exchange features. The `coral-xyz/xnft` repo has 143 stars — a limited developer footprint relative to the broader Solana dApp ecosystem.

## Comparison to Analogous Patterns

| Approach | Example | Execution context | Key difference |
|----------|---------|-------------------|----------------|
| xNFT | Backpack | Inside wallet (sandboxed bundle) | NFT ownership gates access; on-chain asset |
| MetaMask Snaps | MetaMask | Inside extension (WASM sandbox) | Code is permission-gated, not an NFT; extensibility only |
| DApp browser | Phantom, Solflare | External web browser / in-app WebView | Standard web domain model; phishing risk remains |
| Wallet Connect | Any | External app, WalletConnect protocol | No code execution inside wallet |

## Sources

| Source | URL | Access date | Archived |
|--------|-----|-------------|---------|
| xNFT GitHub repo API | https://api.github.com/repos/coral-xyz/xnft | 2026-08-12 | `../sources/2026-08-12-api-github-com-coral-xyz-xnft.json` |
| xnft-cli package in backpack | https://api.github.com/repos/coral-xyz/backpack/contents/packages/xnft-cli | 2026-08-12 | (inline API response, not separately archived) |
| Backpack README | https://raw.githubusercontent.com/coral-xyz/backpack/master/README.md | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-README.md` |
| Backpack SECURITY.md | https://raw.githubusercontent.com/coral-xyz/backpack/master/SECURITY.md | 2026-08-12 | `../sources/2026-08-12-github-coral-xyz-backpack-SECURITY.md` |
