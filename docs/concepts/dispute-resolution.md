# Dispute Resolution

Even with escrow and a delivery oracle, things go wrong: the wrong item arrives, it is damaged, or the buyer claims non-receipt. The dispute mechanism decides these cases **on-chain**, transparently, with no operator able to override the outcome.

## The arbiter panel

Disputes are decided by a fixed panel of **three arbiters** (set at deploy time). A ruling requires a **2-of-3 majority**. No single arbiter can decide an outcome alone.

## The dispute lifecycle

A dispute moves through four states, each with its own time window:

```
OPEN  ──►  VOTING  ──►  RESOLVED  ──►  FINAL
(evidence) (arbiters)  (appeal      (irreversible,
 72h        48h         window 48h)  funds disbursed)
```

| State | Window | What happens |
| --- | --- | --- |
| `OPEN` | 72h | Parties submit evidence (IPFS hashes) |
| `VOTING` | 48h | Arbiters cast votes, each proposing a split |
| `RESOLVED` | 48h | First ruling is stored; an appeal may be filed |
| `FINAL` | — | Outcome is irreversible and funds are disbursed |

## Evidence is anchored, not stored on-chain

Storing files on a blockchain is expensive and impractical. Instead, evidence (photos, tracking screenshots, messages) is uploaded to **IPFS**, and only the content hash is recorded on-chain via `submitEvidence(orderId, ipfsHash)`. This gives a tamper-evident pointer: anyone can later fetch the evidence and verify it matches the hash that was recorded at the time.

## Rulings can be nuanced

A vote is not just "buyer wins" or "seller wins." Arbiters vote with a `sellerBasisPoints` value — the share of the escrow that should go to the seller. This allows **split rulings**: for example, a partially damaged item might resolve to 60% to the seller and 40% refunded to the buyer. The escrow contract enforces the split through its `executeDisputeRuling()` and partial-refund logic.

## The appeal round

After an initial ruling (`RESOLVED`), a party may `appeal()` within the appeal window, triggering a second voting round. To make this robust, votes are keyed by **(orderId, round, arbiter)**, so a fresh round starts cleanly without the previous round's votes contaminating it. Once the dispute reaches `FINAL`, the outcome is irreversible.

➡️ See the [DisputeResolution contract reference](../contracts/dispute-resolution.md) for exact functions and events.
