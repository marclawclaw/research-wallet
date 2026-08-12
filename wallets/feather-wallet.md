---
tags: [wallet, monero, desktop, privacy, tor, airgap, bsd-3-clause, cpp, qt]
category: self-custody software wallet
website: https://featherwallet.org
github: https://github.com/feather-wallet/feather
launched: 2020
license: BSD-3-Clause
---

# Feather Wallet

Feather Wallet is a free, open-source, privacy-hardened Monero desktop wallet for Linux (including Tails and Whonix), Windows, and macOS. It is written in C++ using the Qt framework, is non-custodial, and connects exclusively to remote Monero nodes (it does not bundle a full node daemon). The project was started in 2020 by dsc and tobtoht, and is entirely funded through the Monero Community Crowdfunding System (CCS) and donations.

The wallet's principal differentiators within the Monero ecosystem are: Tor bundled and enabled by default (no external Tor install required), native support for air-gapped / offline transaction signing via animated QR codes or file transfer, and explicit compatibility with privacy-hardened operating systems — Tails, Qubes OS, and Whonix. It targets desktop power users seeking a feature parity with the Monero CLI without the complexity of CLI operation.

The GitHub repository (feather-wallet/feather) shows 573 stars, 80 forks, and ~20 contributors as of 12 August 2026. Primary developer is tobtoht (1,772 commits out of ~1,800 total). Latest release is v2.8.1 (14 April 2025).

---

## Adoption / usage metrics

| Metric | Value | Date | Source |
|--------|-------|------|--------|
| GitHub stars | 573 | 2026-08-12 | [GitHub API](https://api.github.com/repos/feather-wallet/feather) — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather.json) |
| GitHub forks | 80 | 2026-08-12 | [GitHub API](https://api.github.com/repos/feather-wallet/feather) — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather.json) |
| GitHub watchers (subscribers) | 17 | 2026-08-12 | [GitHub API](https://api.github.com/repos/feather-wallet/feather) — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather.json) |
| GitHub open issues | 79 | 2026-08-12 | [GitHub API](https://api.github.com/repos/feather-wallet/feather) — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather.json) |
| GitHub contributors | ~20 (API page limit) | 2026-08-12 | [GitHub contributors API](https://api.github.com/repos/feather-wallet/feather/contributors) |
| Primary developer commits | tobtoht: 1,772 | 2026-08-12 | GitHub contributors API |
| Latest release | v2.8.1 | 2025-04-14 | [GitHub releases](https://github.com/feather-wallet/feather/releases/tag/2.8.1) — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather-releases-latest.json) |
| Release cadence | 4 releases in ~2 years (2.6.7 Jun 2024, 2.6.8 Sep 2024, 2.7.0 Oct 2024, 2.8.1 Apr 2025) | 2026-08-12 | [GitHub releases API](https://api.github.com/repos/feather-wallet/feather/releases) |
| v2.8.1 AppImage downloads (x64, primary) | 1,643 | 2026-08-12 | [GitHub releases asset API](../sources/2026-08-12-api-github-com-feather-wallet-feather-releases-latest.json) |
| v2.8.1 ARM64 AppImage downloads | 2,135 | 2026-08-12 | [GitHub releases asset API](../sources/2026-08-12-api-github-com-feather-wallet-feather-releases-latest.json) |
| v2.8.1 ARM32 AppImage downloads | 1,237 | 2026-08-12 | [GitHub releases asset API](../sources/2026-08-12-api-github-com-feather-wallet-feather-releases-latest.json) |
| Monthly Active Users (MAU) | [NOT FOUND] — structurally unavailable; see note | — | — |
| Primary language | C++ | 2026-08-12 | [GitHub API](https://api.github.com/repos/feather-wallet/feather) |
| Repo created on GitHub | 2022-01-25 (moved from private hosting) | — | [GitHub API](https://api.github.com/repos/feather-wallet/feather) |
| Last push to master | 2026-07-30 | — | [GitHub API](https://api.github.com/repos/feather-wallet/feather) |

**Note on Monero MAU:** Monero's privacy architecture (RingCT, stealth addresses, no public address reuse) makes on-chain active-address attribution structurally impossible. No Monero wallet project publishes MAU. GitHub stars (573) are used as the primary adoption proxy. Download counts above are for a single release and substantially undercount all-time usage, as earlier releases are not tracked after they are superseded.

**Note on stars relative to peers:** Feather's 573 stars places it third among Monero wallets after Monero GUI (2,285) and Cake Wallet (1,838), and ahead of Monerujo (688) — though Monerujo's star count is higher. Feather's comparatively lower count reflects its desktop-only, power-user focus and smaller addressable audience relative to mobile wallets.

---

## How it works

### User perspective

1. **Install:** Download AppImage (Linux), installer or standalone .exe (Windows), or .dmg bundle (macOS) from featherwallet.org/download. All release artefacts include a GPG-detached `.asc` signature. Verify with the release signing key (fingerprint `8185 E158 A333 30C7 FD61 BC0D 1F76 E155 CEFB A71C`).
2. **Wallet creation:** On first launch, the wizard offers: create a new wallet, restore from seed, restore from keys, or create a hardware wallet. New wallets default to Polyseed (16-word). The wizard also prompts for node selection.
3. **Node connection:** Feather connects to a remote Monero daemon (monerod) via the standard Monero RPC protocol. It does not run a local node. Default nodes are curated community nodes (including onion-only nodes). User can add any node.
4. **Tor startup:** At launch, Feather checks for a local Tor daemon on port 9050. If absent, it extracts and runs the bundled Tor binary on port 19450. On Tails/Whonix, all traffic routes through the system Tor regardless.
5. **Receive:** User navigates to the Receive tab, selects or generates a subaddress, and shares the address or QR code. Feather auto-generates fresh subaddresses.
6. **Send:** User enters an address (or scans QR, or selects a contact), enters amount, selects fee priority, and confirms. Feather constructs and signs the transaction locally using the wallet's spend key and broadcasts to the selected node.
7. **Air-gapped / offline flow:** Optionally, the spend key is kept on an offline device running Feather in offline mode. The online device holds a view-only wallet; it constructs unsigned transactions, exports them as animated QR codes or files. The offline device signs them; the online device broadcasts.

### System perspective (transaction flow)

1. Keys are derived from the Polyseed (or legacy 25-word seed) on the local device. Spend key and view key are held in memory; the wallet file is encrypted with the user's password.
2. Feather uses the upstream `wallet2` library from the Monero project for all cryptographic operations.
3. On sync, the wallet downloads block headers from the connected node and scans outputs using the view key. Only outputs addressed to the wallet's subaddresses are spendable.
4. When sending, the wallet selects unspent outputs, constructs a ring of 16 decoys per input (current Monero ring size), computes Bulletproof+ range proofs, and signs using the spend key.
5. The signed transaction is sent to the connected node for relay to the Monero P2P network.
6. All network traffic (node RPC calls, Tor circuit establishment, optional update checks) routes through the Tor SOCKS5 proxy according to the configured mode.

---

## Key features

### Key management

- **Seed formats supported:**
  - **Polyseed (16-word):** Default for all new wallets. Wallet creation date embedded in the seed; no restore height required. Uses BIP-39 wordlists (shorter, more common words than Monero's legacy lists). Passphrase-protected. Developed by tevador (Monero contributor).
  - **14-word seed (legacy):** Supported for restoring wallets created prior to v2.0.0 (the former `monero-seed` library format). Feather will support 14-word restores indefinitely.
  - **25-word Monero seed (legacy):** Full restore support. Feather can also display the wallet's Polyseed as a 25-word seed (Wallet → Seed → Show 25 word seed) for users needing compatibility with other Monero software.
  - Converting 25-word to Polyseed is not possible (one-way KDF). Converting Polyseed to 25-word is possible via Wallet → Seed.
- **Seed passphrase:** Supported for Polyseed. Optional additional entropy appended during key derivation.
- **Subaddresses:** Full support — generate, label, and manage subaddresses per account. Fresh subaddresses are generated for each receive operation.
- **Multiple accounts:** Multiple sub-accounts per wallet, each with independent transaction history and balance.
- **View-key export:** View-only wallet creation is fully supported. Primary address + secret view key; spend key is not exposed.
- **Key derivation:** Entirely on-device. Feather does not transmit seed or keys to any server.

### Privacy architecture

**Tor-by-default (see also [[tor-by-default]]):**

Feather is unique in the Monero wallet ecosystem for bundling a Tor binary within the application package. On launch, Feather detects whether a local Tor daemon is running (port 9050). If not, it runs the bundled Tor on port 19450. Three configurable traffic modes:

1. **Never over Tor** — all traffic goes direct.
2. **Switch to Tor after initial sync (default)** — synchronisation (which transfers significant data) runs direct; subsequent transactions and queries route through Tor. The rationale is that a remote node does not learn much about wallet identity during sync (it only sees block requests, not addresses), while Tor provides IP-layer anonymity for transaction broadcasts and fee queries.
3. **Always over Tor** — all traffic, including sync, routes through Tor (slower).

On Tails, Whonix, or when launched via `torsocks`, all traffic is routed through the system Tor regardless of application settings. Traffic to a local node bypasses Tor.

Feather also supports I2P (documented in the official guides) for connecting to nodes accessible via I2P.

**Onion services:** featherwallet.org and docs.featherwallet.org are accessible via Tor onion addresses (`featherdvtpi7ckdbkb2yxjfwx3oyvr3xjz3oo4rszylfzjdg6pbm3id.onion`) and I2P (`rwzulgcql2y3n6os2jhmhg6un2m33rylazfnzhf56likav47aylq.b32.i2p`).

**Monero-native privacy (structural):**

All Monero transactions use RingCT (confidential amounts via Pedersen commitments), ring signatures (16 decoys per input), stealth addresses (per-transaction one-time recipient addresses), and Bulletproof+ range proofs. These are network-level mandatory features — Feather inherits them from the `wallet2` library.

### Signing

- **Local on-device signing:** All transactions are signed on the device holding the spend key.
- **Hardware wallet signing:** Feather supports Ledger (Nano S, Nano S+, Nano X, Stax, Flex) and Trezor (Model T, Safe 3, Safe 5) for signing Monero transactions. The hardware device holds the spend key; Feather sends unsigned transaction data to the device and receives the signature back. Passphrase entry on-device is supported by both families.
- **Offline / air-gapped signing (see also [[offline-airgap-signing]]):** Feather implements the full air-gap workflow:
  1. **Online device** holds a view-only wallet (primary address + secret view key; no spend key).
  2. **Offline device** runs Feather in offline mode (Settings → Network → Offline; disables all network connections), holds the full wallet including spend key.
  3. **Sending flow:** Online device constructs an unsigned transaction, exports it as an animated QR code (using the Uniform Resources / UR standard) or as a file. Offline device scans or imports, signs, exports the signed transaction. Online device scans or imports and broadcasts.
  4. **Key image synchronisation:** Required on first send from an airgapped wallet. The workflow is wizard-guided.
  5. **Webcam QR scanning:** Feather includes a built-in webcam QR scanner for both sides of the airgap. Supports animated QR codes (UR format) for large data chunks. Supports multiple attached cameras; switchable in-app.
  6. **File transfer alternative:** For users without webcams on one or both sides, file transfer via USB flash drive or SD card is also supported.

### UX

- **Coin control (output management):** Full output management:
  - Sweep single output to self.
  - Sweep multiple selected outputs to self.
  - Sweep all outputs.
  - Output splitting.
  - Manual input selection (select specific outputs as transaction inputs).
  - Coin labeling (annotate individual outputs with user-defined labels).
  - Output blackballing (exclude specific outputs from selection — available via CLI tools).
- **Transaction management:**
  - Multi-destination transactions (send to multiple recipients in one transaction).
  - Transaction pusher (broadcast a raw transaction hex, without needing a connected wallet).
  - Manual transaction import.
  - Transaction rebroadcasting (re-submit a pending transaction).
  - Multibroadcasting (broadcast to multiple nodes simultaneously).
  - Subtract fee from amount option.
  - Automatic network fee adjustment.
  - Manual fee-tier selection.
- **Payment proofs:** Full suite of Monero proof types:
  - SpendProof — proves a transaction was sent from a specific wallet.
  - OutProof — proves a specific transaction output belongs to a recipient.
  - InProof — proves receipt of funds.
  - ReserveProof — proves ownership of at least a stated amount.
  - Formatted transaction proofs (structured proof display for third-party verification).
  - Sign and verify arbitrary messages.
- **Fiat tools:** Crypto/fiat calculator; historical fiat price chart; fiat balance display in status bar.
- **Contacts:** Address book with name labels; QR scanning for new contacts.
- **Dark mode / theming:** Supported.
- **Wallet cache viewer:** View and inspect the wallet's cached transaction and output data.
- **Stagenet/testnet mode:** Switch to Monero stagenet or testnet for development/testing.
- **Built-in updater:** Auto-checks for new releases; downloads and verifies the update.
- **CSV export:** Export transaction history as CSV.
- **Auto-open passwordless wallets:** Convenience option for users with multiple wallets and no password.
- **Open multiple wallets in single instance:** Not supported (one wallet per Feather instance).
- **Feather does not support:** Mining (solo, pool, or P2Pool — marked `✖†` meaning "planned but not implemented"), multisig (marked `✖*` meaning "planned but not yet available"), language localisation beyond English (marked `✖*` as planned).

### Backup / recovery

- **Seed phrase:** Primary recovery mechanism. Displayed during wallet creation; user must write it down. Polyseed encodes creation date; 25-word seeds require storing the restore height separately.
- **Wallet file:** Feather saves a `.wallet` file (encrypted with the user's password) to the config directory. On Tails, this is saved to the persistent volume. The wallet file can be backed up manually.
- **Recovery from seed:** Feather can restore from Polyseed (16-word), legacy 14-word, or standard 25-word Monero seeds. The restore height (for 25-word seeds) or creation date (embedded in Polyseed) is used to start sync at the appropriate block height, avoiding a full chain scan.
- **Recovery from keys:** Restore from primary address + secret spend key (and optionally view key + restore height).
- **Seed entropy from dice:** Feather supports additional seed entropy derived from physical dice rolls (documented in the Advanced guides), allowing users to add entropy beyond the OS random number generator.
- **Damaged Polyseed recovery:** Guide available at docs.featherwallet.org/guides/damaged-seed for recovering from a partially damaged or partially lost Polyseed.

### OS compatibility

| Platform | Notes |
|----------|-------|
| Linux (glibc ≥ 2.31) | x64 standalone executable and AppImage; ARM32 AppImage; ARM64 AppImage; RISC-V 64-bit AppImage |
| Tails OS | Dedicated AppImage build. Designed to work with Tails' amnesic environment; wallet files stored in the persistent volume. Tor integration uses the system Tor on Tails. Full installation guide in docs. |
| Whonix | All traffic routes through the system Tor (Whonix Gateway) regardless of Feather's proxy settings. |
| Qubes OS | Compatible; used as an isolated qube. |
| Windows (10 and higher) | Installer and standalone .exe. Minimum Windows 10. |
| macOS (12 and higher) | Application bundle. Intel and Apple Silicon. Minimum macOS 12 (Monterey). |
| Flatpak | Available for Linux x64 via Flatpak (installed from the release .zip, not yet on Flathub). |
| Raspberry Pi | ARM64 and ARM32 AppImages tested on Raspberry Pi. Installation guide in docs. |

### Hardware wallet support

Feather supports Ledger and Trezor hardware wallets for Monero signing. The hardware device stores the spend key and performs signing on the secure element.

| Device | Secure Element | Passphrase | Open-Source Firmware |
|--------|---------------|-----------|----------------------|
| Ledger Nano S | Yes (ST31H320) | Yes | No |
| Ledger Nano X | Yes (ST33J2M0) | Yes | No |
| Ledger Nano S+ | Yes (ST33K1M5) | Yes | No |
| Ledger Stax | Yes (ST33K1M5) | Yes | No |
| Ledger Flex | Yes (ST33K1M5) | Yes | No |
| Trezor Model T | No | Yes | Yes (reproducible builds) |
| Trezor Safe 3 | Yes (OPTIGATM Trust M V3) | Yes | Yes (reproducible builds) |
| Trezor Safe 5 | Yes (OPTIGATM Trust M V3) | Yes | Yes (reproducible builds) |

Note: No cold-storage-only or air-gap-only hardware wallet (e.g. Coldcard, Passport) is supported, as those devices are Bitcoin-only or do not implement the Monero Ledger/Trezor protocol. Feather's offline signing feature provides an equivalent capability using a second computer.

### Open-source and reproducible builds

- **Licence:** BSD-3-Clause. Free and open-source.
- **Release signing:** All releases are GPG-signed by the release key (fingerprint `8185 E158 A333 30C7 FD61 BC0D 1F76 E155 CEFB A71C`). Detached `.asc` files ship alongside every binary.
- **Bootstrappable / reproducible builds:** Feather uses GNU Guix as the functional package manager for release builds, achieving bootstrappable builds (the entire build environment can be built from source). Multiple contributors independently reproduce and attest to release builds; attestations are published in the `feather-wallet/feather-sigs` repository on GitHub.
- **Dependency pinning:** The Guix manifest pins all dependencies. Build environment is minimal — only required packages included.
- **Security policy:** Documented in `SECURITY.md`. Bug bounty programme exists for vulnerabilities causing fund loss. Scope: latest tagged release only. Bounty range: USD 100–1,500 in XMR.
- **Security audit:** A Quarkslab audit (2022) is referenced in the existing vault index. The original audit report URL is not publicly accessible as of 2026-08-12 (Quarkslab's site returned 404 and Page Not Found). Feather's own documentation does not link to a published audit report. This is noted as `[NOT FOUND via primary source]`; the audit is referenced in community sources but the report itself was not located. Feather's SECURITY.md makes no mention of the audit.
- **Funding:** All development funded via Monero CCS. tobtoht has submitted multiple quarterly CCS proposals (2021 Q1, 2021, Nov 2021, Jul 2022, Jul 2023) and these are publicly visible on the Monero CCS website.

---

## Architecture decisions

- **Remote-node only (no bundled daemon):** Feather does not bundle or require a local `monerod`. This keeps the binary small (~23–30 MB), fast to start, and suitable for live OS environments like Tails. The trade-off is dependence on a third-party node for sync and transaction relay. Feather mitigates this through Tor (hiding the user's IP from the node operator) and through allowing the user to specify their own node or a community-run onion node.
- **`wallet2` library:** Feather uses the upstream Monero `wallet2` library (the same library used by Monero GUI and the CLI). This means Feather inherits all upstream cryptographic improvements and protocol updates immediately on release.
- **Polyseed as default:** Feather adopted Polyseed as its default seed format before Cake Wallet did, serving as a "testing grounds" for experimental features. Polyseed was developed by tevador (a Monero contributor) and Feather was the first wallet to ship it in production.
- **Air-gapped signing over hardware wallets:** The offline signing architecture enables full cold-storage using commodity hardware (any spare laptop) rather than requiring a dedicated hardware wallet device. The animated QR / UR standard eliminates the need for USB data transfer across the airgap. This design mirrors the conceptual model of a hardware wallet but uses software on an isolated device.
- **Tor as a first-class dependency:** Unlike other Monero wallets which treat Tor as an optional proxy setting, Feather bundles Tor and integrates it into the startup process. This means Tor is available immediately on first launch with no user action.
- **Qt / C++ architecture:** The choice of C++/Qt (rather than Electron or Flutter) results in a lean, native binary with no JavaScript runtime overhead. Qt provides cross-platform UI with a native look-and-feel on each OS.

---

## Differentiators

Compared with other Monero wallets:

- **vs [[cake-wallet]]:** Cake Wallet is mobile-first (iOS, Android, macOS, Linux) with multi-currency support and built-in swap aggregation. Feather is desktop-only (no mobile) and XMR-only, but has deeper privacy tooling (Tor by default, air-gapped signing, Tails/Qubes/Whonix compatibility, output blackballing), and reproducible builds with multi-signer attestation.
- **vs [[monero-gui]]:** Monero GUI is the official reference implementation with a bundled full node (monerod). It supports mining, P2Pool, and the full CLI feature set. Feather has Tor by default, Polyseed by default, and richer coin-control UX, but lacks mining and P2Pool. Both use the `wallet2` library. Monero GUI supports no Polyseed.
- **vs [[monerujo]]:** Monerujo is Android-only; Feather is desktop-only. Monerujo has no Tor-by-default or offline signing. Feature sets are complementary, not overlapping platforms.

Feather's key differentiators within the Monero ecosystem:

1. **Tor bundled and on by default** — only Monero wallet where Tor requires zero configuration for basic privacy.
2. **Air-gapped offline signing** — animated QR / UR protocol for coinbase-level security using commodity hardware.
3. **Tails, Qubes, Whonix OS-level integration** — dedicated AppImage for Tails; explicit Whonix/Qubes support.
4. **Polyseed default** — no restore height management; first Monero wallet to ship Polyseed in production.
5. **Feature parity with CLI** — Feather ships every Monero proof type, full coin control, multibroadcasting, and wallet cache inspection.
6. **Reproducible builds with multi-signer attestation** — Guix-based bootstrappable builds; feather-sigs repo.

---

## Limitations and criticisms

### Desktop-only / no mobile

Feather has no iOS or Android app and does not appear to have one planned. Users needing mobile access to Monero must use Cake Wallet or Monerujo. This significantly limits Feather's total addressable user population relative to mobile-first wallets.

### Remote node trust model

Feather requires a remote Monero node. The node operator can see:
- The user's IP address (mitigated by Tor routing).
- Timing and volume of sync requests.
- Transaction broadcasts (the node relays the transaction to the network).

Feather does not mitigate all node-operator inference risks. A determined node operator can attempt to link synchronisation patterns to transaction timing. Using a self-operated node (or an onion-accessible community node) reduces but does not eliminate this risk.

### No built-in swap or multi-currency

Feather is XMR-only. Users who want to move between XMR and BTC or other currencies must use an external exchange. Cake Wallet includes a built-in swap aggregator.

### No multisig (yet)

Multisig is marked as planned (`✖*`) in Feather's own feature comparison. Monero multisig is supported by the `monero-wallet-cli` and `wallet2` library, but Feather has not yet implemented a GUI for it.

### No P2Pool mining (yet)

Mining (including P2Pool) is marked as planned (`✖†`). Monero GUI ships built-in P2Pool. Users wanting XMR mining must use a separate miner or Monero GUI.

### Community size

With 573 GitHub stars and a single primary developer (tobtoht holds ~98% of commits), Feather has a smaller community and a higher bus-factor risk than Cake Wallet or the Monero Project's own GUI. The project has operated continuously since 2020 via CCS funding, which requires periodic community renewal.

### Quarkslab audit report not publicly accessible

A Quarkslab security audit from 2022 is referenced in the community. However, the Quarkslab website returned a 404 for the report URL as of 2026-08-12, and Feather's own documentation does not link to a published report. The audit's scope, findings, and remediation status are not verifiable from primary sources as of the research date.

---

## Sources

- [Feather Wallet homepage](https://featherwallet.org) — accessed 2026-08-12 — [archived](../sources/2026-08-12-featherwallet-org-home.html)
- [Feather Wallet download page](https://featherwallet.org/download/) — accessed 2026-08-12 — [archived](../sources/2026-08-12-featherwallet-org-download.html)
- [GitHub API: feather-wallet/feather](https://api.github.com/repos/feather-wallet/feather) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather.json)
- [GitHub releases: v2.8.1](https://github.com/feather-wallet/feather/releases/tag/2.8.1) — accessed 2026-08-12 — [archived](../sources/2026-08-12-api-github-com-feather-wallet-feather-releases-latest.json)
- [README.md](https://raw.githubusercontent.com/feather-wallet/feather/master/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-feather-wallet-readme-md.md)
- [SECURITY.md](https://raw.githubusercontent.com/feather-wallet/feather/master/SECURITY.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-feather-wallet-security-md.md)
- [Guix reproducible build README](https://raw.githubusercontent.com/feather-wallet/feather/master/contrib/guix/README.md) — accessed 2026-08-12 — [archived](../sources/2026-08-12-github-feather-wallet-guix-readme.md)
- [Docs: Feature comparison](https://docs.featherwallet.org/guides/features) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-features.html)
- [Docs: Tor support](https://docs.featherwallet.org/guides/tor-support) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-tor-support.html)
- [Docs: Seed scheme](https://docs.featherwallet.org/guides/seed-scheme) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-seed-scheme.html)
- [Docs: Offline transaction signing](https://docs.featherwallet.org/guides/offline-tx-signing) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-offline-signing.html)
- [Docs: Hardware wallet support](https://docs.featherwallet.org/guides/hardware-wallet-support) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-hardware-wallets.html)
- [Docs: About Feather Wallet](https://docs.featherwallet.org/guides/about) — accessed 2026-08-12 — [archived](../sources/2026-08-12-docs-featherwallet-org-about.html)
- [Docs: Installing Feather on Tails](https://docs.featherwallet.org/guides/tails) — accessed 2026-08-12
- [feather-wallet/feather-sigs (release attestations)](https://github.com/feather-wallet/feather-sigs) — accessed 2026-08-12
- [Quarkslab audit report URL](https://blog.quarkslab.com/audit-of-feather-wallet.html) — returned 404 on 2026-08-12; report not publicly accessible from primary source
