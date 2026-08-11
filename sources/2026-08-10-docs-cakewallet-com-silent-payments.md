> For the complete documentation index, see [llms.txt](https://docs.cakewallet.com/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.cakewallet.com/features/privacy-and-security/silent-payments.md).

# Silent Payments

Say goodbye to "send me a new address." A Silent Payment address is a single Bitcoin address — starting with `sp1` — that you can post anywhere: your website, social media bio, a donation page. Every payment sent to it lands at a fresh, unique address on-chain that only you can find and spend.

## Why you'd want this

A normal Bitcoin address is public the moment you share it: anyone who sees it can look up every payment it ever receives. The usual fix is handing out a fresh address for every payment — fine for one-off transfers, painful for donations, tips, or anyone who pays you repeatedly.

Silent Payments remove the tradeoff. The sender's wallet uses your `sp1` address to *derive* a unique one-time address that only you can detect. On-chain, the payment looks like any ordinary Taproot transaction — observers can't even tell a Silent Payment happened, and unlike older reusable-address schemes (BIP47), there's no extra "notification" transaction leaking metadata.

Cake Wallet was the first wallet with with **full Silent Payments support** — both sending and receiving.

## Sending to a Silent Payment address

Nothing to learn: paste an `sp1...` address into the [Send screen](https://docs.cakewallet.com/features/basic/send-funds) like any other address. Your wallet handles the rest, and the recipient's privacy comes free.

## Receiving with Silent Payments

{% stepper %}
{% step %}

### Find your sp1 address

Tap `Receive`, tap the address type under the QR code, and choose `Silent Payments`. This address never changes — that's the point. Post it anywhere.

![Silent Payments receive address](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/fund_btc-sp_carrot-cake_receive_hl.png)
{% endstep %}

{% step %}

### Turn on scanning

Because payments to you are hidden on-chain, your wallet has to scan for them. Open `Settings` → `Privacy` → `Silent Payments` and enable `Set Silent Payments always scanning`.

![Silent Payments settings](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/silentpayments_scan_01_menu_hl.png)
{% endstep %}

{% step %}

### Let it catch up

While scanning, a small amber dot shows next to the chain chips on your home screen. If you know when your first Silent Payment arrived, use `Silent Payments Scanning` to scan from a specific date or block height instead of the whole chain — it's much faster.

![Scanning options: from height, from date, one block](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/silentpayments_scan_03_scanning-page.png)
{% endstep %}
{% endstepper %}

## The honest tradeoffs

Privacy this good has one cost: **your wallet does the finding**. Nobody else can detect your payments, so no server can do it for you.

* **Scanning takes time.** The wallet checks candidate transactions block by block. Scanning from your wallet's creation date (rather than from the beginning) keeps this manageable, and `Scan one block` lets you check a specific block instantly.
* **Scanning uses battery and data** while active. If you receive Silent Payments rarely, you can leave always-scanning off and enable it when you're expecting one — payments are never lost, just not visible until you scan.
* **Scanning needs a tweak-capable node, and Cake Wallet will not switch yours.** If the node you have selected can't serve the data scanning needs, the scan runs against the default Cake Wallet server instead — see the note below.

{% hint style="warning" %}
**Which server does your scanning?** Silent Payments scanning needs an Electrum server that serves the silent-payments `tweaks` index — in practice an `electrs` build with tweak support. When a scan starts, Cake Wallet asks the node you currently have selected whether it can serve that data:

* **It can** — the scan runs against your own node.
* **It cannot** — the scan runs against Cake Wallet's own server, `electrs.cakewallet.com:50001`, instead.

Two things worth knowing about that fallback as the app behaves today. Your node setting is **not** changed — the rest of the wallet keeps using your selected node.

So if you run your own node specifically for privacy, confirm it is a tweak-capable `electrs` build and that your wallet is connected to it. Otherwise your Silent Payments scanning traffic goes to Cake Wallet's infrastructure to ensure Silent Payments continue to work.
{% endhint %}

## Learn more

For a deeper dive into how Silent Payments work under the hood, see [silentpayments.xyz](https://silentpayments.xyz).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.cakewallet.com/features/privacy-and-security/silent-payments.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
