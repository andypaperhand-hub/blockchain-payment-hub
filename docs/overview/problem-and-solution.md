# Problem & Solution

## The problem

In a typical online purchase, there is a gap in time between *paying* and *receiving the goods*. The buyer pays first and hopes the seller ships; or the seller ships first and hopes the buyer pays. Someone has to bridge that gap of trust.

Today, that someone is a chain of intermediaries: the marketplace, a payment processor, and one or more banks. They hold the money in the middle, decide when to release it, and adjudicate complaints. For providing this trust, they charge a fee on every transaction and they retain full control over the funds while they hold them.

This creates three structural issues:

1. **Concentrated control.** A single operator decides if and when funds move. Buyers and sellers must trust that decision.
2. **Opaque adjudication.** When a dispute arises, the resolution happens inside a private system. The reasoning and the record are not independently verifiable.
3. **Bundled rent.** The party that runs the storefront is also the party trusted with the money, and it prices the two together. There is no competitive market for *just the trust layer*.

## The solution

Blockchain Payment Hub moves the trust functions out of any single operator and onto a neutral, public ledger:

* **Escrow** — the buyer's payment is locked in a smart contract, not in a company's account.
* **Release condition** — funds are released automatically when delivery is confirmed by independent logistics providers, not by a marketplace employee.
* **Disputes** — disagreements are decided by a panel of arbiters voting on-chain, with every vote and piece of evidence permanently recorded.
* **Audit trail** — a tamper-proof settlement record is periodically anchored to Ethereum, so the full history can be verified by anyone.

The result is a payment rail where **no single party can unilaterally seize, withhold, or redirect funds**, and where the rules are enforced by code rather than by an administrator's discretion.

## What this prototype demonstrates

This system is a working, end-to-end research prototype. It demonstrates the full lifecycle on a live testnet:

```
checkout → escrow lock → delivery confirmation → release
                                  └→ (if contested) → dispute → on-chain ruling
```

The remaining pages explain *why* this design is defensible (Background), *how* each mechanism works (Core Concepts), and *what* the running system looks like (Architecture onward).
