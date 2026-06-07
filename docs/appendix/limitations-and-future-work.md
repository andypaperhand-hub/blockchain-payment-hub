# Limitations & Future Work

Stating limitations honestly strengthens the argument. This page is deliberately candid about what the prototype does and does not establish.

## Limitations

* **Testnet only.** Everything runs on Sepolia with a mock stablecoin. It does not demonstrate mainnet economics, real gas costs at scale, or regulatory compliance.
* **Trust is relocated, not eliminated.** Participants must still trust (a) the correctness of the contract code, (b) the honesty of a majority of oracle providers, and (c) the honesty of a majority of arbiters. The design minimizes and makes these assumptions explicit, but does not remove them.
* **Oracle and arbiter selection.** The prototype uses fixed provider and arbiter sets. How these are chosen, rotated, and held accountable in a real deployment is an open governance question, not a solved one.
* **Account abstraction is partial.** The signing and replay-protection primitives are implemented; full sponsored-gas / passkey onboarding flows are not.
* **Not a cost-superiority proof.** The work argues about market structure and control, not that the on-chain approach is universally cheaper than incumbents.

## Scope boundaries (what this is *not* about)

It is worth separating this work from adjacent problems it does **not** claim to solve:

* Problems rooted in **market power elsewhere in a supply chain** — for example, fragmented producers facing a few large buyers — are market-structure issues in a *different* layer. They are not payment-fee problems, and an escrow payment rail does not address them. Framing such problems as payment problems would be academically vulnerable.

## Future work

* **Mainnet / L2 cost study.** Deploy to a low-fee L2 and measure real per-transaction economics under load.
* **Decentralized oracle and arbiter sourcing.** Stake-based or reputation-based selection with slashing for dishonest behavior.
* **Full account abstraction (EIP-4337 / EIP-7702).** Sponsored gas and passkey onboarding so buyers never touch a seed phrase or hold ETH for gas.
* **Empirical grounding.** Anchor the motivation to concrete data (marketplace fee structures, seller margins, domestic payment-gateway comparisons) to quantify the rent being unbundled.
* **Formal verification** of the escrow state machine's safety and liveness properties.
