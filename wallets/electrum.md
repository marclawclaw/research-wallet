---
tags: [wallet, bitcoin, desktop, spv, lightning]
category: self-custody software wallet
website: https://electrum.org
github: https://github.com/spesmilo/electrum
launched: 2011
license: MIT
---

# Electrum

Electrum is the longest-running Bitcoin software wallet, launched in November 2011 by Thomas Voegtlin. It is a lightweight (SPV) client that connects to a network of Electrum servers rather than downloading the full blockchain, achieving instant startup times while keeping private keys local. As of August 2026 it supports desktop (Windows, macOS, Linux) and Android, includes a built-in Lightning Network implementation with trampoline routing via ACINQ, and integrates with all major hardware wallets.

## Adoption / usage metrics

| Metric | Value | Date | Source |
|--------|-------|------|--------|
| GitHub stars | 8,561 | 2026-08-10 | [GitHub API](https://api.github.com/repos/spesmilo/electrum) — [archived](../sources/2026-08-10-github-api-spesmilo-electrum.json) |
| GitHub forks | 3,463 | 2026-08-10 | [GitHub API](https://api.github.com/repos/spesmilo/electrum) — [archived](../sources/2026-08-10-github-api-spesmilo-electrum.json) |
| GitHub contributors | ~350 | 2026-08-10 | GitHub API pagination headers (`rel="last"` = page 350 at 1-per-page) — [GitHub API](https://api.github.com/repos/spesmilo/electrum/contributors?per_page=1) |
| Open issues | 1,238 | 2026-08-10 | [GitHub API](https://api.github.com/repos/spesmilo/electrum) — [archived](../sources/2026-08-10-github-api-spesmilo-electrum.json) |
| Subscribers (watchers) | 405 | 2026-08-10 | [GitHub API](https://api.github.com/repos/spesmilo/electrum) — [archived](../sources/2026-08-10-github-api-spesmilo-electrum.json) |
| Android installs (Google Play) | 1M+ | 2026-08-10 | [_index.md](../_index.md) citing AppBrain (credential required); Play Store badge on electrum.org |
| MAU (Monthly Active Users) | [NOT FOUND] | — | — |
| Latest release | 4.8.1 | 2026-08-10 | [RELEASE-NOTES](https://raw.githubusercontent.com/spesmilo/electrum/master/RELEASE-NOTES) — [archived](../sources/2026-08-10-github-com-spesmilo-electrum-RELEASE-NOTES.txt) |
| Release prior to 4.8.1 | 4.8.0 (July 8, 2026) | 2026-08-10 | [RELEASE-NOTES](https://raw.githubusercontent.com/spesmilo/electrum/master/RELEASE-NOTES) — [archived](../sources/2026-08-10-github-com-spesmilo-electrum-RELEASE-NOTES.txt) |
| Repo created | 2012-08-02 | 2026-08-10 | [GitHub API](https://api.github.com/repos/spesmilo/electrum) — [archived](../sources/2026-08-10-github-api-spesmilo-electrum.json) |
| Last commit | 2026-08-10 | 2026-08-10 | [GitHub API](https://api.github.com/repos/spesmilo/electrum) — [archived](../sources/2026-08-10-github-api-spesmilo-electrum.json) |
| F-Droid availability | Yes | 2026-08-10 | [electrum.org download page](https://electrum.org/#download) — [archived](../sources/2026-08-10-electrum-org-download.html) |
| Reproducible builds | Yes — signed by 5 independent builders | 2026-08-10 | [electrum.org download page](https://electrum.org/#download) — [archived](../sources/2026-08-10-electrum-org-download.html) |

## How it works

### User perspective

1. **Download and verify.** The user downloads the binary from electrum.org, verifies the GPG signature against one of five independent signers (ThomasV, SomberNight, Emzy, felixb_f321x, svanstaa), and installs it.
2. **Create or restore wallet.** On first launch a wizard offers: new seed (Electrum native format or BIP39 import), restore from existing seed, import watch-only master public key, or import individual private keys.
3. **Seed backup.** The wallet displays a 12-word mnemonic (Electrum native format) that the user writes down. The seed is the sole recovery artefact for standard wallets.
4. **Connect to servers.** Electrum automatically connects to ~10 public Electrum servers. Users can manually specify a server (including .onion servers for Tor), or point to their own self-hosted Electrum server (Fulcrum, ElectrumX, etc.).
5. **Transact.** To send, the user fills in an address, amount, and fee rate. Electrum supports coin control (UTXO selection), RBF by default, CPFP, and transaction batching. Hardware wallets prompt on-device confirmation at signing time.
6. **Lightning.** The user can open a Lightning channel from the wallet's on-chain balance and send/receive Lightning payments. Trampoline routing via ACINQ's node means the user does not need to maintain a full network graph.

### System perspective

Electrum implements Simplified Payment Verification (SPV) as described in Satoshi Nakamoto's original paper. Rather than downloading the full blockchain (~650 GB as of 2026), it:

1. Maintains a chain of block headers (compact, ~80 bytes each).
2. Subscribes to its own scriptPubKey hashes on the main server to receive transaction notifications.
3. For each transaction notification, requests a Merkle inclusion proof from the server, then verifies it against the locally held header chain.
4. Polls all ~10 connected servers for fee estimates; uses the median when auto-connect is enabled.

This architecture trusts the server not to lie by omission (it could withhold transactions), and fully trusts the server for unconfirmed transactions. Confirmed transactions are SPV-verified. Private keys never leave the client.

The Electrum protocol is JSON-RPC over SSL/TLS (or plain TCP) — not HTTP. Servers are identified by certificate pinning (TOFU for self-signed, CA chain for CA-signed).

For Lightning, Electrum uses a trampoline routing design: the client constructs a payment onion whose first hop is a hardcoded ACINQ trampoline node (`node.acinq.co`), which then finds the path to the final recipient. This means Electrum does not need to download the Lightning gossip graph.

## Key features

- **Key management:** BIP32 HD wallet. Electrum's native seed format (v2.0+) encodes a version number in an HMAC-SHA512 prefix (`Seed version` || normalised mnemonic phrase) — see [[patterns/spv-electrum-server]] for detail. BIP39 import is supported as a legacy option. Seed has 132 bits of entropy (12 words from a 2048-word list). Key derivation follows BIP44/BIP84 paths for legacy and SegWit respectively. Master public key can be exported to create a watch-only wallet. Individual private keys can be imported (sweep or non-sweep) into dedicated import wallets. AES-256-CBC encryption of private keys with user password; ECIES asymmetric encryption of the wallet file on disk (enabled by default since v2.8). Password is never stored; wallet file can be saved without decrypting private keys.

- **Signing:** Full PSBT (BIP174) support — Electrum can create, partially sign, combine, and broadcast PSBTs. Native multisig m-of-n co-signing via file transfer, QR code, or CosignerPool plugin. Air-gapped signing workflow: create unsigned transaction on online (watching-only) wallet → export to file/QR → sign on offline machine → import and broadcast. Hardware wallets sign in-session when connected (see Hardware wallet support below). ECDSA low-R grinding applied to signatures (matches Bitcoin Core behaviour, improves fee predictability). Transactions default to nVersion=2.

- **UX:** Qt GUI on desktop (PyQt6); QML GUI on Android. Coin control (UTXO selection and freezing at both address and UTXO level — freeze individual UTXOs added in v3.3.5). Fee estimation uses dynamic rates polled from multiple servers. RBF (Replace-By-Fee) enabled by default for all transactions except Lightning channel open/close; right-click "Increase Fee" to bump, "Cancel (double-spend)" to cancel. CPFP (Child Pays for Parent) via right-click on unconfirmed transactions. Transaction batching: new payments can be added to an unconfirmed RBF transaction. Bulk payment (multi-output transaction) supported via CSV import or comma-separated address/amount entries. Locktime randomisation (since v3.3.3, privacy improvement). Screenshot protection on Windows (prevents Microsoft Recall/screen capture; configurable). Biometric authentication on Android (since v4.5.x). Gap limit configurable (default 20 consecutive unused addresses).

- **Backup/recovery:** The seed phrase (12 words) is the sole backup for deterministic wallets. On recovery, the user selects "I already have a seed" in the wizard and re-enters the phrase. Wallet file (`default_wallet` in datadir) is a JSON database encrypted with ECIES; it can be backed up separately but is not required for recovery from seed. Import wallets (non-seed key stores) require manual backup of the wallet file since they cannot be recovered from seed. Upgrade path from Electrum 1.x (old hash-based seed) handled automatically.

- **Privacy:** Electrum's SPV model requires the main server to know your address set (the server receives scriptPubKey hash subscriptions). This is acknowledged as a privacy trade-off. Mitigations: (a) Tor support — route all connections through a SOCKS5 proxy to .onion Electrum servers, configurable via GUI or CLI flags (`-p socks5:localhost:9050 -s <onion>:50001:t`); (b) manual server selection — connect to a self-hosted Electrum server for full privacy; (c) coin control — freeze specific UTXOs to avoid linking; (d) dust attack mitigation — option to avoid spending flagged coins (added in RELEASE-NOTES ~v4.x); (e) CoinJoin — **no built-in CoinJoin**; users must use external tools (e.g., JoinMarket, Wasabi via PSBT export). Locktime randomisation reduces heuristic clustering.

- **Lightning:** Built-in Lightning Network using trampoline routing. ACINQ (`node.acinq.co`) is the sole hardcoded mainnet trampoline node. Features: open/close channels from on-chain balance; send/receive LN payments; LNURL (LUD-16 lightning address support, LUD-17 payment identifiers added in v4.8.0); Nostr Wallet Connect (NWC) plugin; submarine swaps (on-chain ↔ Lightning); reverse swaps to external addresses (added in v4.5.x); watchtower support (self-host or third-party). Anchor outputs required for newly opened channels (since v4.8.0). LN forwarding is explicitly experimental and warned against in mainnet (v4.8.1). Multi-path payments (MPP) supported.

- **Hardware wallet support:** Trezor (all models; Python library `trezor[hidapi]`), Ledger (Nano S/X/S Plus/Stax; `hidapi ledger-bitcoin`), KeepKey (bundled fork of python-keepkey since v4.6.0), Digital Bitbox (original BitBox01; `hidapi`), BitBox02 (`bitbox02`), Archos Safe-T mini (`safet`), Coldcard (Mk1–Mk4; `ckcc-protocol`), Jade (`pyserial cbor2`). Hardware wallets sign transactions on-device; the private key never enters Electrum's process memory. For air-gapped Coldcard (QR/NFC/SD card workflows), PSBT is used — see [[patterns/psbt-hardware-signing]]. On Linux, udev rules must be configured per-device.

- **Multisig:** Native m-of-n multisig wallet creation via wizard. Cosigners exchange master public keys (xpubs); Electrum derives shared addresses. Partially signed transactions are exchanged by: (a) file (USB stick); (b) QR code; (c) CosignerPool server plugin (encrypted with cosigner's master public key). Any cosigner combination of m-of-n can sign in any order. Outputs use P2SH (legacy) or P2WSH (native SegWit) address types. TrustedCoin plugin provides 2-of-3 two-factor authentication (TrustedCoin holds one key, signs transactions via their API after OTP verification; billing is per-transaction).

## Architecture decisions

- **SPV / Electrum server over full node:** Electrum chose SPV to achieve instant startup and minimal storage/bandwidth — critical for desktop and mobile users who cannot run a full node. The trade-off is that the Electrum server can lie by omission (not reporting relevant transactions) and is trusted for unconfirmed transactions. This is explicitly documented in the FAQ. Users who want full-node security can run their own ElectrumX or Fulcrum server and point the client at it.

- **Native seed format over BIP39:** Electrum explicitly rejected BIP39 because it lacks a version number (making forward compatibility fragile) and requires a fixed wordlist for checksum computation. The Electrum seed version system embeds a version prefix in the HMAC-SHA512 of the mnemonic, allowing future derivation schemes to be identified without guessing — see [[patterns/electrum-seed-version]].

- **Trampoline Lightning over full graph sync:** Rather than maintaining a complete Lightning gossip graph on each client, Electrum delegates route-finding to ACINQ's trampoline node. This reduces storage and bandwidth significantly but introduces a single trusted routing intermediary for Lightning payments.

- **Python + Qt GUI:** The entire stack is Python (≥3.10), making it portable and auditable but contributing to the perception of complexity for new users. The Android app uses QML (Qt for Python) rather than a native Android stack.

- **Plugin architecture:** Hardware wallet support, TrustedCoin 2FA, CosignerPool, Nostr Wallet Connect (NWC), PSBT-over-Nostr (psbt_nostr), watchtower, swap server, audio modem (QR code via sound), and revealer (steganographic seed backup) are all implemented as plugins.

## Differentiators

Electrum is the **oldest surviving Bitcoin wallet** (2011) and has the highest GitHub star count (8,561) of any Bitcoin-only software wallet, reflecting its position as the long-tenured reference implementation for power-user desktop Bitcoin.

- **Vs [[bluewallet]]:** BlueWallet is mobile-first (iOS + Android), React Native stack, with a more approachable UX for new users. Electrum is desktop-first with a Qt UI that exposes more complexity. Electrum has deeper UTXO control, richer CLI/RPC interface, and more hardware wallet integrations. BlueWallet's Lightning is custodial-by-default (LNDhub); Electrum's is non-custodial trampoline.

- **Vs [[sparrow-wallet]]:** Sparrow is a desktop-only Java wallet optimised for UTXO privacy (CoinJoin via PayJoin/Whirlpool, Tor built-in, full-node connection). Electrum covers more platforms (including Android) and has built-in Lightning. Sparrow has no Lightning. Electrum lacks built-in CoinJoin. Both support PSBT and hardware wallets but Sparrow has a richer UTXO management UI.

- **Vs [[phoenix]]:** Phoenix is Lightning-native mobile (ACINQ), self-custodial with a single-channel trampoline model. Electrum is primarily on-chain with Lightning as an add-on. Phoenix has simpler Lightning UX; Electrum has more on-chain features and desktop presence.

## Limitations and criticisms

- **2018–2019 phishing attacks via malicious server error messages.** Older versions of Electrum (before v3.3.2/v3.3.3) allowed servers to return arbitrary HTML-formatted error messages that were rendered in the GUI. Attackers ran malicious Electrum servers and displayed pop-up messages telling users their wallet needed to be "updated" — directing them to a phishing download site. Millions of dollars were reportedly stolen. Patches in v3.3.2 (December 2018) rendered error messages as plain text; v3.3.3 (January 2019) stopped exposing server error messages entirely. This is the most serious user-impact security incident in Electrum's history.

- **SPV trust model — server can lie by omission.** The Electrum server knows which addresses belong to the client (through scriptPubKey hash subscriptions). It can withhold information about transactions affecting those addresses. This is documented behaviour: "The server can lie by omission." Users who need stronger guarantees must self-host an Electrum server connected to their own full node.

- **No Taproot (P2TR) wallet creation.** As of 4.8.1, Electrum supports the code-level primitives for Taproot (address detection, `WITVER1_P2TR` output type, `is_taproot_address()` helper, `taproot_output_script()` in `bitcoin.py`) and can *send to* Taproot addresses, but does not expose P2TR as a wallet creation option — `WIF_SCRIPT_TYPES` does not include `p2tr`. This is a notable gap relative to wallets like Sparrow that support Taproot receive addresses. The RELEASE-NOTES note BitBox02 support for "send-to-taproot" (#7693) but no receive-side P2TR wallet type.

- **No built-in CoinJoin.** Electrum has coin control and UTXO freezing, but no integrated CoinJoin (Whirlpool, JoinMarket, or PayJoin). Users seeking on-chain privacy mixing must use external tools.

- **UX complexity for new users.** Multiple independent reviewers (TechRadar's Mayank Sharma, Money's Marco Monroy Robles) have noted that Electrum is not suited for beginners — the wallet exposes too many technical concepts without adequate guidance. Wikipedia (accessed 2026-08-10) cites Sharma noting "the wallet is not designed for inexperienced users."

- **AES-256-CBC encryption susceptible to dictionary attacks.** Wikipedia (citing Holmes & Buchanan, 2023, *Forensic Science International: Digital Investigation*) notes that while the wallet file is encrypted with PBKDF2, private keys are encrypted with AES-256-CBC with the user's password, making them potentially vulnerable to offline brute-force/dictionary attack if an attacker obtains the encrypted wallet file.

- **Lightning considered experimental for forwarding on mainnet.** v4.8.1 adds a warning when LN forwarding is enabled on mainnet: "you really should not be doing that, yet." The Lightning implementation is production-grade for sending/receiving but is not recommended for node operators.

- **Single trampoline dependency.** Mainnet Lightning routing depends entirely on ACINQ (`node.acinq.co`). If ACINQ's trampoline node is unavailable or becomes adversarial, Lightning payments fail. There is no fallback trampoline node on mainnet.

## Sources

- [electrum.org home](https://electrum.org) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-org-home.html)
- [electrum.org download page](https://electrum.org/#download) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-org-download.html)
- [Electrum FAQ — readthedocs](https://electrum.readthedocs.io/en/latest/faq.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-faq-full.html)
- [Electrum Seed Version System — readthedocs](https://electrum.readthedocs.io/en/latest/seedphrase.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-seedphrase.html)
- [Multisig Wallets — readthedocs](https://electrum.readthedocs.io/en/latest/multisig.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-multisig-doc.html)
- [Cold Storage — readthedocs](https://electrum.readthedocs.io/en/latest/coldstorage.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-latest.html)
- [Hardware wallets on Linux — readthedocs](https://electrum.readthedocs.io/en/latest/hardware-linux.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-hardware-linux.html)
- [Using Electrum Through Tor — readthedocs](https://electrum.readthedocs.io/en/latest/tor.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-tor.html)
- [Simple Payment Verification — readthedocs](https://electrum.readthedocs.io/en/latest/spv.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-spv.html)
- [spesmilo/electrum — GitHub API](https://api.github.com/repos/spesmilo/electrum) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-api-spesmilo-electrum.json)
- [RELEASE-NOTES — GitHub](https://raw.githubusercontent.com/spesmilo/electrum/master/RELEASE-NOTES) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-spesmilo-electrum-RELEASE-NOTES.txt)
- [README.md — GitHub](https://raw.githubusercontent.com/spesmilo/electrum/master/README.md) — accessed 2026-08-10 — [archived](../sources/2026-08-10-github-com-spesmilo-electrum-README.md)
- [electrum/bitcoin.py — script types](https://github.com/spesmilo/electrum/blob/master/electrum/bitcoin.py) — accessed 2026-08-10 — source code inspection
- [electrum/trampoline.py — ACINQ node](https://github.com/spesmilo/electrum/blob/master/electrum/trampoline.py) — accessed 2026-08-10 — source code inspection
- [electrum/plugins/ — plugin list](https://api.github.com/repos/spesmilo/electrum/contents/electrum/plugins) — accessed 2026-08-10 — GitHub API
- [Electrum (software) — Wikipedia](https://en.wikipedia.org/wiki/Electrum_(software)) — accessed 2026-08-10 — [archived](../sources/2026-08-10-en-wikipedia-org-electrum-software.html)
- [Holmes & Buchanan 2023 — AES-CBC encryption finding](https://doi.org/10.1016/j.fsidi.2022.301486) — cited in Wikipedia — accessed 2026-08-10
