> For the complete documentation index, see [llms.txt](https://docs.cakewallet.com/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.cakewallet.com/features/basic/swap.md).

# Swap

Swap converts one cryptocurrency into another right inside Cake Wallet — Bitcoin to Monero, Lightning sats to a stablecoin, anything to anything the providers support. No exchange account, no deposit-withdraw dance: pick a pair, review the rate, swipe.

{% stepper %}
{% step %}

### Pick what you're sending and receiving

Tap `Swap` on the home screen. Set **Send** to the asset and wallet the funds come from, and **Receive** to what you want back. Multi-network assets like USDC ask you to pick the network; assets with special forms — like Bitcoin's Lightning balance — appear as their own entries.

![Swap configured from Lightning sats to USDC on Ethereum](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/swap_exec_03_to-usdc_hl.png)

The receiver defaults to your own matching wallet — `Select Receiver` lets you pick another of your wallets or an external address.
{% endstep %}

{% step %}

### Review the quote

Enter an amount and Cake Wallet finds the best rate across its swap providers, showing the estimated amount you'll receive and which provider is quoting it.

![Swap quote with provider and rate](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/swap_exec_04_quote_hl.png)

Want a different provider? Tap the provider bar. `Best Rate` is the default preference; you can also stick to decentralized providers (Chainflip, Jupiter, Near Intents) or pick a specific one. Trocador acts as an aggregator that routes your trade to whichever exchange offers the best terms.

![Provider selection screen](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/swap_exec_05_provider.png)

The swap Configure screen (sliders icon) also offers a **Fixed rate** toggle: floating-rate swaps are estimates that track the market until execution, while fixed-rate swaps lock the quoted amount in exchange for a slightly worse rate.
{% endstep %}

{% step %}

### Confirm and send

The confirmation sheet shows the exact amounts, your network fee, the destination wallet, and the **Swap ID** — the reference support teams use to find your trade. Check it over, then `Swipe to send`.

![Swap confirmation with Swap ID](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/swap_exec_06_confirm_hl.png)

{% hint style="info" %}
The final amount can differ a little from the first estimate — rates move between quoting and execution on floating-rate swaps, and provider fees settle at execution time. If you need the amount of crypto you receive to be an exact amount, use `Fixed rate`.
{% endhint %}
{% endstep %}

{% step %}

### Track it

The trade appears in `History` and moves through statuses — `Waiting` → `Sending` → `Success`. Tap it for Trade Details: status, Swap ID, provider, and a tracking link on the provider's site. Most swaps complete in minutes; cross-chain pairs can take longer.

![Trade details with status and Swap ID](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/swap_exec_08_trade-details.png)

![Received funds in the destination wallet](https://raw.githubusercontent.com/cake-tech/docs-assets/main/v6.3.1/swap_exec_12_usdc-arrived.png)
{% endstep %}
{% endstepper %}

## If something goes wrong

Save your **Swap ID** (also called Trade ID — [glossary](https://docs.cakewallet.com/faq/glossary/trade-id)), note down the provider used, and contact Cake Wallet support first via the in-app chat or <support@cakewallet.com> — we'll contact the provider behind your trade if necessary and escalate for you. See [Swap support](https://docs.cakewallet.com/support/swap) for the full provider directory.

## Related

* [Send funds](https://docs.cakewallet.com/features/basic/send-funds) — the same fee and confirmation concepts apply
* [Coin control](https://docs.cakewallet.com/features/advanced/coin-control) — choose which outputs fund a swap (available from the swap Configure screen)
* [Bridge USDT between chains](https://docs.cakewallet.com/features/basic/bridge-usdt-between-chains) — moving the *same* asset between networks


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.cakewallet.com/features/basic/swap.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
