---
tags: [pattern, seed, key-management, bitcoin, bip32, bip39]
applies_to: [electrum]
---

# Pattern: Electrum Seed Version System

Electrum's native seed format (introduced in v2.0, 2014) is a proprietary alternative to BIP39 that embeds a version number in the mnemonic phrase itself. The version number determines which key derivation scheme to use, enabling forward compatibility as the wallet evolves.

## Motivation

Early Electrum (before v2.0) used a bidirectional hash-to-mnemonic encoding that required a fixed wordlist. BIP39, introduced in 2013, adds a checksum but still requires a fixed wordlist and carries no version information. The Electrum developers identified two structural flaws in BIP39:

1. **No version number:** If a future wallet does not implement a particular BIP32 derivation method, it will silently return an empty wallet rather than reporting incompatibility.
2. **Wordlist dependency for checksum:** The BIP39 checksum requires the wordlist to compute, binding the format to specific word lists (one per language), threatening portability.

Electrum's design resolves both.

## Mechanism

The version number is extracted from the HMAC-SHA512 of the normalised seed phrase:

```python
def version_number(seed_phrase):
    normalised = prepare_seed(seed_phrase)          # strip diacritics, collapse spaces
    h = hmac_sha_512("Seed version", normalised)    # keyed hash
    s = h.encode('hex')
    length = int(s[0]) + 2                          # prefix length in nibbles
    prefix = s[0:length]
    return hex(int(prefix, 16))
```

The `prepare_seed` function normalises to Unicode NFC, removes diacritics, collapses whitespace (including CJK spacing), making the seed portable across keyboard layouts.

## Version numbers

| Hex prefix | Type | Wallet type |
|------------|------|-------------|
| `0x01` | Standard | P2PKH and Multisig P2SH |
| `0x100` | SegWit | P2WPKH (bech32) and P2WSH |
| `0x101` | 2FA | Two-factor authenticated wallets (TrustedCoin) |

New wallet creation defaults to `0x100` (SegWit/native bech32) since v3.3.0.

## Security

Electrum uses the same 2048-word BIP39 wordlist (for user familiarity) but the checksum role is replaced by the version prefix requirement. A valid seed must produce a hash that begins with a registered version prefix. This:

- Makes brute-force slightly easier (attacker can skip key stretching for non-version-matching candidates), but the net security is 135 bits equivalent (132 bits entropy + 11 bits from 2048-iteration PBKDF2 − 8 bits from prefix constraint).
- Provides unambiguous detection of seed compatibility.

## Derivation

From the version-validated seed:
1. `hmac_sha_512("Bitcoin seed", master_seed)` → master private key + chain code (standard BIP32 root).
2. Derivation paths follow BIP44 (`m/44'/0'/0'`) for P2PKH, BIP84 (`m/84'/0'/0'`) for native SegWit (P2WPKH), BIP48 for multisig P2WSH.

## BIP39 import

Electrum supports importing BIP39 seeds as a legacy option. The user selects "BIP39 seed" in the restore wizard. The version check is skipped; derivation follows whatever path the user specifies (defaulting to BIP44/BIP84). Electrum warns users that BIP39 seeds are less forward-compatible.

## Contrast with BIP39

| Property | Electrum native | BIP39 |
|----------|----------------|-------|
| Wordlist required at checksum time | No | Yes |
| Version number | Yes (embedded in hash) | No |
| Checksum | Hash prefix (version-based) | Last bits of entropy hash |
| Forward compatibility | Explicit (version field) | Implicit (requires all derivation paths forever) |
| Languages | One list, any language | One list per language |

## Sources

- [Electrum Seed Version System — readthedocs](https://electrum.readthedocs.io/en/latest/seedphrase.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-seedphrase.html)
- [Electrum FAQ — What is the seed?](https://electrum.readthedocs.io/en/latest/faq.html) — accessed 2026-08-10 — [archived](../sources/2026-08-10-electrum-readthedocs-io-faq-full.html)
