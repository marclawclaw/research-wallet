---
tags: [pattern, signing, simulation, pre-execution, ethereum, evm, security]
applies_to: [rabby, metamask]
---

# Pattern: Pre-Transaction Simulation in EVM Wallets

## What it is

Pre-transaction simulation runs a transaction against the current blockchain state before the user signs it, then displays the predicted outcome — balance changes, token flows, NFT transfers, smart-contract state changes — in the confirmation UI. The user sees "what will happen" before committing.

Without simulation, the user sees only the raw transaction parameters (`to`, `value`, `data`) and must understand the ABI encoding themselves. With simulation, the wallet decodes the outcome into human-readable form: "You will send 100 USDC and receive 0.041 ETH."

## Why it matters

EVM wallets are the primary vector for approval-exploit drainer attacks: a user signs what appears to be a routine interaction but the calldata authorises a drainer contract to sweep tokens. Simulation surfaces:
- Unexpected token outflows (drainer withdrawals)
- Unlimited `approve()` calls (ERC-20 allowance exploits)
- NFT transfers the user did not intend
- Zero-value or negative-balance outcomes from swaps

## Rabby implementation — server-side simulation

**Source:** `src/ui/views/Approval/components/SignTx.tsx`, confirmed call to `wallet.openapi.preExecTx()` — accessed 2026-08-12.

### How it works

1. User triggers a transaction (e.g. dapp calls `eth_sendTransaction`).
2. Before the confirmation UI renders, `SignTx.tsx` calls `wallet.openapi.preExecTx({ tx, nonce, origin, ... })` via the Rabby backend (`https://api.rabby.io`).
3. The backend simulates the transaction against live chain state and returns a `balance_change` object containing:
   - `send_token_list` — tokens leaving the wallet
   - `receive_token_list` — tokens arriving
   - `send_nft_list` / `receive_nft_list` — NFT flows
4. `BalanceChange.tsx` renders this as a "What will happen" panel above the raw transaction details.
5. Concurrently, the Security Engine (`@rabby-wallet/rabby-security-engine`) evaluates the transaction context against `defaultRules` and displays risk badges (Forbidden / Danger / Warning / Safe).
6. The Action decoder (`src/ui/views/Approval/components/Actions/`) identifies the transaction type (Swap, Send, TokenApprove, RevokePermit2, AddLiquidity, CrossSwapToken, etc.) and displays a human-readable summary.

### Supported action types (confirmed via source directory 2026-08-12)

`AddLiquidity`, `ApproveNFT`, `ApproveNFTCollection`, `AssetOrder`, `BatchRevokePermit2`, `CancelTx`, `ContractCall` (fallback), `CrossSwapToken`, `CrossToken` (bridge), `DeployContract`, `MultiSwap`, `PushMultiSig`, `RevokeNFT`, `RevokeNFTCollection`, `RevokePermit2`, `RevokeTokenApprove`, `Send`, `SendNFT`, `Swap`, `SwapLimitPay`, `TokenApprove`, `TransferOwner`, `UnWrapToken`, `WrapToken`.

### Trade-off: server-side vs on-device simulation

| Dimension | Rabby (server-side, api.rabby.io) | MetaMask (on-device Blockaid PPOM) |
|-----------|-----------------------------------|-----------------------------------|
| Accuracy | High — full chain state at simulation time | High — PPOM uses downloaded threat intelligence; may lag |
| Privacy | Lower — tx data (addresses, calldata) leaves browser to Rabby API | Higher — simulation runs locally; no tx data sent remotely |
| Latency | Network round-trip | Near-zero (on-device) |
| Coverage | All chains Rabby supports | Ethereum mainnet + select chains |
| Customisability | Security Engine rules user-adjustable | Not user-configurable |
| Offline operation | N — requires API connectivity | Y — PPOM works offline once model is downloaded |

## MetaMask implementation — on-device Blockaid PPOM

MetaMask uses `@blockaid/ppom_release` v1.5.3 (Privacy Preserving Offline Module), confirmed via `package.json` inspection 2026-08-12. The PPOM runs locally in the browser extension. Transaction data is not sent to a remote server for analysis. Coverage is limited to chains for which Blockaid has published threat-intelligence datasets.

MetaMask does **not** display a token-balance-change simulation panel in the same style as Rabby. It shows a Blockaid risk assessment (Safe / Warning / Danger) and, for some interaction types, decoded action labels. It does not show "you will receive X token".

## Key differentiator for the RFP

For the lambda prize RFP context (power users, self-custody, security focus):

- Rabby's pre-transaction simulation is the **single most-cited differentiator** by power-user reviews. It makes the confirmation UI function as an audit surface, not just an approve/reject gate.
- The server-side model provides broad chain coverage and accurate simulation but requires trust in Rabby's API infrastructure and involves transmitting transaction data off-device.
- A wallet targeting strong privacy should implement on-device simulation (or make it opt-in), similar to MetaMask's Blockaid approach.

## Sources

- `src/ui/views/Approval/components/SignTx.tsx` — `preExecTx()` call confirmed — accessed 2026-08-12 via GitHub raw API
- `src/ui/views/Approval/components/TxComponents/BalanceChange.tsx` — balance change rendering — accessed 2026-08-12
- `src/ui/views/Approval/components/Actions/` — full action type list — accessed 2026-08-12
- `src/background/service/securityEngine.ts` — Security Engine rule evaluation — accessed 2026-08-12
- MetaMask `package.json` — `@blockaid/ppom_release` v1.5.3 — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-metamask-extension-package-json.txt)
- See also: [[rabby]] for full wallet context; [[eip712-typed-signing]] for signing mechanics
