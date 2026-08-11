> For the complete documentation index, see [llms.txt](https://docs.cakewallet.com/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.cakewallet.com/features/privacy-and-security/built-in-tor.md).

# Built-in Tor

Every time your wallet talks to a node or checks an exchange rate, the server on the other end sees your IP address — a strong hint of who and where you are. Cake Wallet ships with Tor built in: flip one switch and the app routes its connections through the Tor network, hiding your IP from the services it talks to. No extra apps required.

{% hint style="info" %}
Tor hides your IP from the servers your wallet contacts. It doesn't make transactions themselves anonymous — that's what coins and features like Monero, [Silent Payments](https://docs.cakewallet.com/features/privacy-and-security/silent-payments), and [MWEB](https://docs.cakewallet.com/features/privacy-and-security/litecoin-mweb) are for. Use both layers together.
{% endhint %}

## Turn it on

Open `Settings` from the top right, then under App settings tap `Connections` and enable `Enable built-in Tor`.

![Connections settings with Enable built-in Tor](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/settings_connections_01_top_hl.png)

Cake Wallet marks the feature experimental: not every connection type supports Tor routing yet, so if you want belt-and-suspenders coverage, combine it with a VPN or run [Orbot](https://docs.cakewallet.com/features/advanced/tor-with-orbot) in whole-device mode.

## What to expect

* **Startup takes a little longer.** The app establishes a Tor connection first — you'll see a brief connecting state before your wallet syncs.
* **Syncing is drastically slower than clearnet.** Tor adds latency by design, and there's no useful estimate to give — expect syncing to take far longer overall, especially for a large rescan. If you need a fast sync, use your own clearnet node.
* **Some third-party services may not work over Tor.** Fiat on-ramp providers in particular sometimes block Tor connections. The `Fiat API` and `Swap` settings on the same Connections screen offer Tor-only modes if you'd rather those features fail closed than leak your IP.

## Onion nodes

With Tor active you can also connect to `.onion` nodes — nodes reachable only inside the Tor network, removing the exit-hop entirely.

Cake Wallet ships onion presets for two coins only:

* **Monero** — `Cake Wallet (Onion)` in the Monero node list.
* **Zcash** — `Cake Wallet (Tor)` and `Cake Wallet (Tor #2)` in the Zcash node list.

There are **no built-in onion presets for Bitcoin, Litecoin or any other coin**. On those, add an onion node yourself from [Manage nodes](https://docs.cakewallet.com/features/managing-your-wallet/manage-nodes) if you have one to point at.

Without Tor running, `.onion` nodes will not connect — if your wallet is stuck on one, switch to a clearnet node or enable Tor.

## Built-in Tor or Orbot?

* **Built-in Tor** protects Cake Wallet's own connections with zero setup — the right choice for most people.
* [**Orbot**](https://docs.cakewallet.com/features/advanced/tor-with-orbot) routes your whole device (or chosen apps) through Tor, covers connection types the built-in support doesn't reach yet, and suits users who already run Tor for everything.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.cakewallet.com/features/privacy-and-security/built-in-tor.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
