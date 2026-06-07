# On-Chain Escrow

Escrow is the heart of the system. It is the mechanism that lets a buyer pay before receiving goods without having to trust the seller — and without handing custody to the marketplace.

## What escrow means here

An escrow is money held by a neutral third party until agreed conditions are met. In the traditional model, that "neutral third party" is the marketplace. Here, it is the **EscrowManager** smart contract. Once funds are locked, the contract — not any person or company — decides where they go, strictly according to its code.

## The lifecycle of an escrow

Each order's escrow moves through a small set of states. The contract enforces which transitions are legal.

```
        lockEscrow()
   ──────────────────────►  LOCKED
                              │
        delivery confirmed    │   buyer / oracle / timeout
   ┌──────────────────────────┼───────────────────────────┐
   ▼                          ▼                            ▼
RELEASED                AUTO_RELEASED                   DISPUTED
(seller paid)        (timeout, seller paid)               │
                                                          ▼
                                          REFUNDED / PARTIAL_REFUND
                                            (per arbiter ruling)
```

The on-chain `EscrowState` enum captures these states: `CREATED`, `LOCKED`, `RELEASED`, `AUTO_RELEASED`, `DISPUTED`, `REFUNDED`, and `PARTIAL_REFUND`.

## How funds are released

There are three legitimate ways for a locked escrow to pay out:

1. **Delivery confirmed.** The buyer confirms receipt, or the logistics oracle confirms delivery on the buyer's behalf. Funds go to the seller.
2. **Auto-release on timeout.** If delivery is confirmed but the buyer neither confirms nor disputes within the confirmation window, anyone can trigger `triggerAutoRelease()` and the funds release to the seller. This protects sellers from buyers who simply go silent.
3. **Dispute ruling.** If a dispute is opened, the outcome is decided by the dispute contract and can be a full release, a full refund, or a **split** (partial refund) defined in basis points.

## The time windows

The escrow is governed by configurable time windows (defaults shown):

| Window | Default | Purpose |
| --- | --- | --- |
| `deliveryWindowSeconds` | 3 days | Expected time for delivery |
| `confirmWindowSeconds` | 7 days | Time the buyer has to confirm or dispute after delivery |
| `disputeWindowSeconds` | 7 days | Time within which a dispute may be opened |

These windows make the "silent buyer" and "silent seller" cases resolvable without human intervention.

## The platform fee

The contract supports a transparent platform fee, stored per escrow as `platformFeeRate` in **basis points** (the default is `250`, i.e. 2.5%). On release, the fee is split off to the platform treasury and the remainder goes to the seller. Crucially, the fee rate is **on-chain and visible** — it is not a hidden markup.

➡️ See the [EscrowManager contract reference](../contracts/escrow-manager.md) for the exact functions and events.
