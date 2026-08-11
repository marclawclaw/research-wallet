> For the complete documentation index, see [llms.txt](https://docs.cakewallet.com/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.cakewallet.com/features/advanced/duress-pin.md).

# Duress PIN

A duress PIN is a second PIN for the worst-case scenario: someone forcing you to unlock your wallet. Enter your duress PIN instead of your real one, and **Cake Wallet immediately deletes every wallet in the app** — there's nothing left on the device to hand over.

{% hint style="danger" %}
**This is a wipe, not a decoy.** Entering the duress PIN permanently deletes all wallets from this device, and the app gives no warning when it happens — that's the point. Your funds stay safe on their blockchains, but they are only recoverable with your seed phrases. **Do not set up a duress PIN until every wallet's seed is backed up somewhere outside this device.**
{% endhint %}

## Who this is for

The duress PIN protects against physical coercion — border crossings, robbery, anyone demanding you open the app. It's an advanced tool with a sharp edge: a mistyped-on-purpose PIN can't be undone. If that tradeoff doesn't fit your threat model, your regular PIN plus [Cake 2FA](https://docs.cakewallet.com/features/privacy-and-security/authentication) covers most situations.

## Set it up

{% stepper %}
{% step %}

### Back up every seed first

Confirm you have the seed words for **every** wallet on this device written down and stored safely — view them under `Settings` → `Seed & Keys`. After a duress wipe, those words are the only way back. Cake Wallet will ask you to confirm this before setup.
{% endstep %}

{% step %}

### Enable Duress PIN

Open `Settings` → App settings → `Security` and turn on `Duress PIN`. Two dialogs come first: a `Notice` explaining that this is an advanced feature which will delete all of your wallets, and then a `Confirm` dialog asking `Did you back up all your seeds?`. Only after you accept both does the PIN pad open — so decide your duress PIN before you get there.

![Security settings with Duress PIN toggle](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/settings_security_01_list_hl-1.png)
{% endstep %}

{% step %}

### Choose a distinct, memorable PIN

Pick something clearly different from your real PIN — you must never enter it by accident, but you need it available under stress. Practice the difference mentally, never on the device.
{% endstep %}
{% endstepper %}

## What happens when it's used

Entering the duress PIN at any unlock prompt does **not** unlock your wallets. Cake Wallet wipes the device first — closing the open wallet, clearing its secure storage, deleting the wallet files and database entries, and resetting its authentication data — and then drops you on the welcome/onboarding screen, exactly as if the app had just been installed. The unlock itself is reported as a failure, so there is no wallet to hand over and nothing on screen that hints a wipe just happened.

To recover afterwards, reopen or reinstall Cake Wallet and [restore each wallet from its seed](https://docs.cakewallet.com/get-started/setup/restore-existing-wallet/seed-or-keys). A [wallet group](https://docs.cakewallet.com/features/managing-your-wallet/wallet-groups) seed restores all of its member wallets, which makes recovery much faster — one more reason to prefer grouped BIP39 wallets.

{% hint style="info" %}
A duress wipe removes wallets from **this device only**. It does not touch funds on-chain, other devices, or [Cake backups](https://docs.cakewallet.com/features/managing-your-wallet/create-backup) you've exported elsewhere — an attacker who later finds a backup file would still need its password.
{% endhint %}


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.cakewallet.com/features/advanced/duress-pin.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
