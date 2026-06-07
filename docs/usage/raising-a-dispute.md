# Raising a Dispute

If an order goes wrong — item not received, wrong item, damaged — either party can open a dispute, and the on-chain arbiter panel decides the outcome.

## Steps

1. Go to **http://localhost:3000/disputes**.
2. Click **Raise Dispute** and select the order.
3. Describe the issue and submit — this opens a dispute on-chain via `DisputeResolution`.
4. During the **evidence window (72h)**, parties attach evidence; files go to IPFS and only the hash is stored on-chain.
5. In the **voting window (48h)**, the three arbiters each cast a vote proposing the seller's share.
6. When 2 of 3 agree, the ruling is stored (`RESOLVED`). A party may **appeal** within 48h, triggering a second round.
7. On finalization (`FINAL`), the escrow contract enforces the outcome — full release, full refund, or a split.

## What the outcome can be

Because votes carry a `sellerBasisPoints` value, the result is not binary. A ruling can send any proportion to the seller and refund the rest to the buyer — e.g. 70/30 for a partially defective item. The split is executed by the escrow contract and permanently recorded.

➡️ For the mechanism in depth, see [Dispute Resolution](../concepts/dispute-resolution.md).
