# End-to-End Transaction Flow

This page traces one purchase from checkout to release, showing which service and which contract acts at each step, and which real-time events fire.

## The happy path (delivery succeeds)

```
 1. Buyer checks out
    Frontend → Backend  POST /api/v1/orders            → order created in PostgreSQL
                         POST /api/v1/payments/request  → on-chain orderId (keccak256) prepared

 2. Buyer pays
    Frontend → wallet → EscrowManager.lockEscrow(...)   → funds locked on-chain
    Chain emits EscrowLocked
    Backend EventListener hears it → emits WS "payment.locked" (room = orderId)

 3. Seller ships, carriers scan
    Carrier webhooks → Oracle Service (/webhook/ghn, /ghtk, /viettel)
    Oracle maps to orderId; on 2-of-3 → LogisticsOracle.confirmDelivery(...)
    LogisticsOracle reaches consensus → EscrowManager.confirmDeliveryByOracle(orderId)

 4. Funds release
    EscrowManager releases to seller (minus platform fee) → emits EscrowReleased
    Backend EventListener hears it → emits WS "delivery.confirmed" / "escrow.released"

 5. Settlement (later, batched)
    Nightly Bull job builds a Merkle tree → SettlementContract.commitBatch(root, count)
```

## The disputed path

```
 At step 3–4, instead of confirming, the buyer disputes:

    Frontend → Backend  POST /api/v1/disputes
    Backend → EscrowManager.openDispute(orderId)  → escrow enters DISPUTED
              + DisputeResolution.createDispute(orderId, initiator)
    Chain emits EscrowDisputed → Backend emits WS "dispute.opened"

    OPEN (72h)   → submitEvidence(orderId, ipfsHash)  [evidence to IPFS, hash on-chain]
    VOTING (48h) → arbiters castVote(orderId, vote, sellerBasisPoints)
    RESOLVED     → first ruling stored; appeal() possible within 48h
    FINAL        → executeDisputeRuling → release / refund / partial split enforced
                   Backend emits WS "escrow.released" with the outcome
```

## The roles of each layer in one sentence each

* **Frontend** initiates actions and reflects state to the user in real time.
* **Backend** orchestrates, persists, listens to the chain, and broadcasts updates.
* **Oracle service** turns real-world delivery events into on-chain consensus.
* **Smart contracts** are the source of truth and the only thing that can move funds.
* **Settlement** compresses history into a verifiable on-chain fingerprint.

➡️ For the exact request/response shapes, see the [REST API](../api/rest-api.md); for the live events, see [WebSocket Events](../api/websocket-events.md).
