# Research Contribution

This page frames what the prototype contributes as a piece of research, separate from its value as software.

## The core argument

The contribution is a **market-structure** claim, demonstrated by a working artifact:

> The trust layer of e-commerce payments — escrow, delivery verification, dispute adjudication, and settlement integrity — can be **separated** from the marketplace layer and placed on neutral, publicly verifiable infrastructure. Doing so removes any single operator's unilateral control over funds and makes the trust layer a reusable commodity rather than a service bundled with, and priced by, the storefront.

This framing is deliberately *not* a naive "blockchain is cheaper than X%" cost comparison, which is fragile to fee changes and easy to rebut. The claim is about **who holds power over the funds and the record**, not about shaving a percentage point.

## What the prototype demonstrates concretely

1. **Custody without an operator** — funds sit in `EscrowManager`, governed by an explicit on-chain state machine no party can override.
2. **Trust-minimized real-world inputs** — delivery is confirmed by a 2-of-3 oracle consensus rather than a single trusted source.
3. **Transparent, appealable adjudication** — disputes are decided by an on-chain 2-of-3 arbiter panel with evidence anchored to IPFS and an appeal round, producing a publicly auditable record.
4. **Cheap, verifiable history** — batch settlement anchors a Merkle root per period, so any transaction's inclusion can be proven without trusting the operator's books.
5. **UX foundations** — EIP-712 signed payloads, EIP-1271 smart-wallet support, and nonce-based replay protection show the path to account-abstraction-grade usability.

## Why the design choices are defensible

* The **2-of-3** thresholds (both for the oracle and for arbiters) are the minimal majorities that tolerate one faulty participant without granting any single one unilateral power — a standard fault-tolerance argument.
* **Anchoring rather than storing** (Merkle roots, IPFS hashes) reflects an honest accounting of on-chain cost: put the integrity guarantee on-chain, keep the bulk data off-chain.
* The **split-ruling** mechanism (basis points) shows the model handles real-world partial outcomes, not just binary win/lose cases.

## A useful conceptual bridge

The coordination problem the contracts solve — ensuring a multi-step, multi-party transaction completes correctly and verifiably — is analogous to the **Saga pattern** and **idempotency keys** in traditional distributed systems. The difference is that those patterns coordinate cooperating internal services, whereas the smart contract coordinates **mutually distrusting commercial parties** through a shared neutral arbiter none of them controls. Framing it this way connects the work to established distributed-systems theory rather than treating "blockchain" as a novelty.
