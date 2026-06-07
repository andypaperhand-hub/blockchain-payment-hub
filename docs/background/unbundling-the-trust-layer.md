# Unbundling the Trust Layer

## The central claim

The strongest framing for this project is **not** "blockchain is cheaper than a 2% fee." Naive cost comparisons are fragile: fees change, and a marketplace can always undercut a specific number.

The defensible claim is a **market-structure** argument:

> An e-commerce marketplace currently bundles two separable services — the **marketplace layer** (storefront, discovery, logistics coordination) and the **trust layer** (escrow, dispute resolution, settlement integrity). Because the trust layer is controlled by the same party that runs the marketplace, there is no competitive market for trust on its own. Moving the trust layer onto a neutral public ledger separates the two, so the trust layer becomes a commodity that any marketplace can plug into.

## Why separation matters

When the trust layer is unbundled and placed on neutral infrastructure:

* **No single operator has custody.** Funds sit in a smart contract whose rules are public and immutable. Neither buyer, seller, nor marketplace can override them.
* **The trust layer is portable.** A new or small marketplace can adopt the *same* escrow and dispute infrastructure that a large one uses, without having to build trust from scratch or be trusted with customer funds.
* **Adjudication is verifiable.** Every dispute vote and every settlement root is on a public ledger. The process can be audited by anyone, not just the operator.
* **Competition shifts to the storefront.** Marketplaces compete on discovery, selection, and experience — not on who you are forced to trust with your money.

## An analogy

In traditional distributed systems, problems like "did this multi-step transaction actually complete, and can we prove it?" are solved with patterns such as the **Saga pattern** and **idempotency keys** — mechanisms that coordinate state across services so that no participant has to blindly trust another. On-chain escrow and oracle attestation solve an *analogous* coordination problem, but across mutually distrusting commercial parties rather than cooperating internal services. The smart contract is the shared, neutral coordinator that none of the parties controls.

## What this is *not* claiming

To keep the argument honest, it is worth stating the boundaries:

* It is **not** claiming the marketplace adds no value — discovery and logistics coordination are genuinely valuable.
* It is **not** claiming blockchain is universally cheaper — on-chain operations have their own costs (gas, infrastructure).
* It is **not** a fix for problems that are really about *market power* elsewhere in a supply chain (for example, fragmented producers facing a few large buyers — that is a market-structure problem in a different layer, not a payment-fee problem).

The claim is narrow and specific: **the trust layer can be separated from the marketplace layer**, and doing so changes who holds power over the funds.
