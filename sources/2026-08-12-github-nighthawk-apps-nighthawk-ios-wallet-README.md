# Nighthawk Wallet (DarkFi Edition) — iOS

Privacy-preserving wallet (work-in-progress) by [Nighthawk Apps](https://nighthawkapps.com). This tree ships as a **native iOS app** on the DarkFi network (DRK). The app integrates a native DarkFi wallet API via **UniFFI** (`rust/darkfi-mobile-ffi` → generated Swift + `DarkfiWalletHandle`) for chain sync, broadcast, and chat.

## Contents

- [Download](#download)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Build](#build)
- [Wallet & recovery phrase](#wallet--recovery-phrase-22-words)
- [Chat (DarkIRC)](#chat-darkirc--eventgraph)
- [Architecture](#architecture)
- [DAO Hub](#dao-hub)
- [Privacy & security](#privacy--security)
- [Verification](#verification)
- [Known issues](#known-issues)
- [Contributing & support](#contributing--support)
- [Disclosure policy](#disclosure-policy)
- [Disclaimers](#disclaimers)

---

## Download

<a href="https://apps.apple.com/us/app/nighthawk-wallet/id1524708337" style="display: inline-block; overflow: hidden; border-radius: 13px; width: 250px; height: 83px;"><img src="https://tools.applemediaservices.com/api/badges/download-on-the-app-store/black/en-US" alt="Download Nighthawk on the App Store" style="border-radius: 13px; width: 250px; height: 83px;"></a>

---

## Repository layout

Path dependencies and sibling Nighthawk repos use these **directory names**:

```text
parent/
  darkfi/                 # optional upstream clone; app vendors into third_party/darkfi
  darkfi-lightwalletd/    # gRPC lightwalletd (local sync target)
  darkfi-mobile-ffi/      # optional sibling symlink of rust/darkfi-mobile-ffi for desktop/other clients
  nighthawk-ios-wallet/   # this repo
  nighthawk-android-wallet/
  nighthawk-desktop/
  moonshine/
```

Vendored DarkFi lives at `third_party/darkfi/` (gitignored). Other clients can consume the
UniFFI crate from `rust/darkfi-mobile-ffi`, or via a sibling checkout named `darkfi-mobile-ffi`.

## Quick start

From the repository root (first-time or after native code changes):

```bash
# 1) Pin upstream DarkFi into third_party/darkfi
./scripts/vendor-darkfi.sh

# 2) Rust iOS targets (once per machine)
rustup target add aarch64-apple-ios aarch64-apple-ios-sim

# 3) UniFFI wallet native lib (REQUIRED — XCFramework .a binaries are gitignored)
./scripts/build-darkfi-mobile-ffi-ios.sh
# Faster simulator-only (NOT for TestFlight/device Archive):
#   SIM_ONLY=1 ./scripts/build-darkfi-mobile-ffi-ios.sh

# 4) Open Xcode
open stealth.xcodeproj
# Scheme: stealth-testnet (default) or stealth-mainnet
# Destination: simulator or device → ⌘B / ⌘R
```

> **TestFlight / Archive:** `DarkfiCore.xcframework` static libraries (`*.a`) are **not** in git (see `.gitignore`). A clean clone cannot link until you run the full `./scripts/build-darkfi-mobile-ffi-ios.sh` (device + simulator). Never Archive with `SIM_ONLY=1`. Re-run after Rust/UDL changes so UniFFI Swift checksums match the binary.

**Physical device from Terminal** (run in Terminal.app so codesign can access Keychain):

```bash
./scripts/deploy-ios-device.sh
# Optional: SCHEME=stealth-mainnet DEVICE_ID=<udid> ./scripts/deploy-ios-device.sh
```

**Chat:** open the **Chat** tab — DarkIRC runs **in-process** via UniFFI. **Tor is on by default** (embedded Arti SOCKS); the splash screen shows “Tor bootstrapping…” while Arti comes up. First DAG sync can take several minutes.

