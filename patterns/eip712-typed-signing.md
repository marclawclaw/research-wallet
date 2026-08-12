---
tags: [pattern, signing, eip-712, eip-191, ethereum, evm, structured-data]
applies_to: [metamask, rabby, rainbow, trust-wallet]
---

# Pattern: EIP-712 Typed Data Signing in EVM Wallets

## What it is

EIP-712 defines a standard for signing structured, human-readable Ethereum messages using `eth_signTypedData_v4`. Unlike raw byte-string signing (`personal_sign` / EIP-191) or the now-deprecated `eth_sign` (which signs arbitrary hashes), EIP-712 encodes the message as a typed struct with a domain separator, making the wallet display the decoded fields in a human-readable confirmation dialog.

## Why it matters

Before EIP-712, phishing attacks could present a raw hex hash to the user, who would have no way to know what they were signing. EIP-712 allows the wallet to decode and display the fields — e.g. "You are signing: {to: 0xABC…, amount: 100 USDC, nonce: 5, deadline: 2026-09-01}" — so the user can make an informed decision.

EIP-712 signatures can also be verified on-chain (a smart contract can recover the signing address from the signature using `ecrecover` applied to the EIP-712 hash), enabling permit patterns (ERC-2612), meta-transactions (ERC-2771), and order-book protocols (OpenSea Seaport, 0x).

## EIP-712 message structure

```
domain_separator = hashStruct(
    "EIP712Domain", {
        name, version, chainId, verifyingContract, salt (optional)
    }
)

message_hash = hashStruct(primaryType, message)

signed_hash = keccak256("\x19\x01" || domain_separator || message_hash)
```

The `\x19\x01` prefix distinguishes EIP-712 signed hashes from raw transaction hashes (which begin with `\x19`), preventing signature replay attacks.

## MetaMask implementation

MetaMask supports EIP-712 via `eth_signTypedData_v4` (the stable, current version; v1 and v3 are legacy).

- **Method name:** `eth_signTypedData_v4`
- **UI:** The confirmation dialog decodes and displays all top-level fields of the typed struct in human-readable form. Array and nested struct fields are also decoded.
- **Domain separator validation:** MetaMask validates `chainId` in the domain separator against the currently connected network to prevent cross-chain replay attacks. If `chainId` mismatches the active network, MetaMask shows a warning.
- **Hardware wallets:** Ledger and Trezor both support EIP-712 signing natively in recent firmware. MetaMask routes `eth_signTypedData_v4` to the hardware device for confirmation on-device when a hardware account is active.
- **eth_sign (raw hash signing):** Disabled by default since MetaMask v10.x. Can be re-enabled in developer settings. Disabled because it signs an arbitrary 32-byte hash with no context, making it the primary mechanism for signature phishing.

## EIP-191 (personal_sign)

`personal_sign` (EIP-191) prepends `\x19Ethereum Signed Message:\n<length>` to the message before hashing and signing. This prevents the signed hash from being a valid transaction hash (a transaction always starts with a different prefix). Used primarily for authentication (Sign-In with Ethereum / SIWE, EIP-4361).

MetaMask supports both:
- `personal_sign` → EIP-191, shown as "MetaMask Message Signature Request"
- `eth_signTypedData_v4` → EIP-712, shown as "MetaMask Typed Signature Request" with decoded struct fields

## Blockaid protection on signed messages

MetaMask's Blockaid PPOM (Privacy Preserving Offline Module) applies to signing requests as well as transactions. For EIP-712 `permit` signatures (authorising a spender), Blockaid can detect if the `spender` in the permit matches a known drainer address or if the `amount` is set to `uint256.max` (unlimited approval). The analysis runs locally on-device.

## Comparison: EIP-712 vs permit2

Uniswap's Permit2 contract generalises EIP-712 signatures across all ERC-20 tokens (not just those that implement ERC-2612 natively). A single `approve(Permit2, uint256.max)` to the Permit2 contract allows subsequent EIP-712 permit signatures to authorise specific amounts to specific dapps — without additional on-chain approval transactions. MetaMask displays Permit2 signatures using the standard EIP-712 dialog. Blockaid detects malicious Permit2 usages.

## Rabby implementation

Rabby supports `eth_signTypedData_v4` (EIP-712) and `personal_sign` (EIP-191), confirmed via the `SignTypedData.tsx` and `SignText.tsx` components in `src/ui/views/Approval/components/` — accessed 2026-08-12.

Rabby additionally applies its **Security Engine** (`@rabby-wallet/rabby-security-engine`) to EIP-712 signing requests. This evaluates:
- `permit` signatures (ERC-2612): detects unlimited allowance (`amount = uint256.max`), known drainer spender addresses
- `Permit2` signatures (Uniswap Permit2): dedicated `RevokePermit2` and `BatchRevokePermit2` action components
- Order-book signatures (OpenSea Seaport, 0x): decoded via `TypedDataActions/` directory

The `SignTypedDataExplain/` component provides a decoded human-readable breakdown of the EIP-712 struct fields, similar to MetaMask's typed data dialog. The typed-data action decoder is a separate subsystem from the transaction pre-execution simulator.

Unlike MetaMask's Blockaid analysis (on-device), Rabby's Security Engine for signing requests runs locally against `defaultRules` from `@rabby-wallet/rabby-security-engine` — no server round-trip required for signing analysis specifically (the pre-execution API call is for transactions, not typed-data signing).

## Privacy note

EIP-712 domain separator typically includes `chainId` and `verifyingContract`. If a dapp uses a non-standard domain or omits `chainId`, MetaMask shows a warning but still allows signing. Replay across chains is a risk if `chainId` is omitted and the same contract is deployed on multiple chains at the same address.

## Sources

- [MetaMask docs: Sign data](https://docs.metamask.io/wallet/how-to/sign-data/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-metamask-io-sign-data.html). Confirms `eth_signTypedData_v4` and `personal_sign` as the two supported signing methods; includes EIP-712 struct example and hardware wallet note.
- [EIP-712 specification](https://eips.ethereum.org/EIPS/eip-712) — Ethereum Improvement Proposals
- [EIP-191 specification](https://eips.ethereum.org/EIPS/eip-191) — Ethereum Improvement Proposals
- MetaMask extension package.json — `@blockaid/ppom_release` v1.5.3 confirms Blockaid integration — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-metamask-extension-package-json.txt)
