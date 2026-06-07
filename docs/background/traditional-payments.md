# How Traditional E-Commerce Payments Work

To understand what this project changes, it helps to see clearly what happens today.

## The money's journey in a normal purchase

When you buy something on a large marketplace, the money typically travels like this:

```
You (buyer)
   │  card / e-wallet
   ▼
Payment processor  ── takes a processing fee
   │
   ▼
Acquiring bank / marketplace-held balance  ── holds the funds
   │  (held until conditions are met)
   ▼
Seller's account  ── often minus a marketplace commission
```

Two distinct services are happening here, even though one company often provides both:

1. **The storefront** — listing products, search, checkout UX, ratings. This is what the buyer interacts with.
2. **The trust layer** — holding the money in the middle, deciding when to release it, and resolving complaints. This is what makes the buyer comfortable paying a stranger.

## Where the cost comes from

The fees are not only for "moving money." A large part pays for the **trust layer**: the marketplace guarantees that if your item never arrives, you can get your money back. That guarantee requires the operator to hold funds, run a support and dispute team, and absorb fraud.

This is a real and valuable service. The point of this project is **not** that the service is worthless — it is that the service is *bundled* with the storefront and *controlled* by one party, and that this arrangement has costs beyond the headline fee.

## The hidden properties of the current model

| Property | How it behaves today |
| --- | --- |
| **Custody of funds** | Held by the operator; the buyer/seller cannot independently verify or move them |
| **Release decision** | Made by the operator according to its internal policy |
| **Dispute record** | Private; not independently auditable |
| **Switching** | You cannot take "just the trust layer" to a cheaper competitor — it comes with the storefront |

The next page explains the core academic argument: that these functions can — and arguably should — be **unbundled**.
