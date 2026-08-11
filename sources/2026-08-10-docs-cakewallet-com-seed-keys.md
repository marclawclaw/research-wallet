> For the complete documentation index, see [llms.txt](https://docs.cakewallet.com/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.cakewallet.com/features/managing-your-wallet/seed-keys.md).

# Seed and keys

### Wallet keys

Your keys encode the private information in your wallet and are what allow you to spend coins and see incoming transactions. You may also use a wallet's keys to restore the wallet.

General terms specific to public-key cryptography where a private key and a public key make up a keypair. A public key is derived from the private key.

* The public key is safe to share, and gives the recipient the ability to encrypt something or verify a signature.
* The private key gives the owner the ability to sign or decrypt something, and as such, should be kept private.

### Wallet seed

Your seed is just a version of your private key written in a way that’s easier for you to write down. Your seed and keys are actually the same things, just in different forms! You can use wallet's seed to restore your wallet too.

### Viewing your wallet's seed/keys

Tap the gear icon at the top right of the home screen to open `Settings`. Then, under Wallet settings, tap `Seed & Keys`. You will be prompted for your PIN, biometric authentication, or password. On the next screen, you will see your wallet's seed and keys. Cake Wallet has 1 set of these per wallet, so you must save them all individually.

![Tap the gear icon to open Settings](https://3615068045-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F42pJTvTiQMiULx18h5vy%2Fuploads%2FbLk0TuQ6Vj9Dtkfdr7Ok%2Fseed-1.png?alt=media\&token=49eb5e95-9d9d-4d10-9d60-7abdc47b0810) ![Tap "Seed & Keys"](https://3615068045-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F42pJTvTiQMiULx18h5vy%2Fuploads%2FcyWEjz7NHP2Oxj3WgjZr%2Fseed-2.png?alt=media\&token=273a08d7-a075-41ed-b31a-e2f93e6d68b0) ![Wallet seed/keys screen](https://3615068045-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F42pJTvTiQMiULx18h5vy%2Fuploads%2F6vRRO1sH3SfAKY9uwhwM%2Fseed-3.png?alt=media\&token=2494843f-b78a-4cd6-bb64-956b4b23a9bf)

{% hint style="danger" %}
DO NOT show your seed/keys to anyone. Your funds will likely be stolen if you give out either.
{% endhint %}

Please write down your seed/keys and store them in a safe place. This will allow you to restore your wallet if you lose your device, or otherwise cannot access the wallet.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.cakewallet.com/features/managing-your-wallet/seed-keys.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
