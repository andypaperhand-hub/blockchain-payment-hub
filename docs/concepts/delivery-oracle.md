# The Delivery Oracle

A smart contract cannot see the physical world. It does not know whether a parcel actually arrived. The **oracle** is the bridge that brings that real-world fact on-chain in a trust-minimized way.

## The problem the oracle solves

If a single carrier could tell the escrow contract "delivered," then that one carrier (or anyone who compromised it) could trigger the release of funds. That reintroduces a single point of trust — exactly what the system is trying to remove.

## The solution: multi-provider consensus

The **LogisticsOracle** contract accepts delivery confirmations from **three independent logistics providers** (in the Vietnamese context: GHN, GHTK, and Viettel Post). It only declares delivery confirmed once a **threshold** of providers agree.

```
GHN     ──confirmDelivery(orderId, trackingCode)──┐
GHTK    ──confirmDelivery(orderId, trackingCode)──┤──► 2 of 3 reached?
Viettel ──confirmDelivery(orderId, trackingCode)──┘        │
                                                           ▼
                                              ConsensusReached → calls EscrowManager
```

* Each provider is a known address; only registered providers may submit confirmations (`onlyProvider`).
* The required number of confirmations is configurable (`setRequiredConsensus`, default **2 of 3**).
* When consensus is reached, the oracle emits `ConsensusReached` and calls `confirmDeliveryByOracle()` on the escrow contract.

## Why 2-of-3 and not 1-of-1 or 3-of-3

* **1-of-1** would mean trusting a single carrier completely.
* **3-of-3** would mean any single carrier failing to report (an outage, a missed scan) would permanently freeze the funds.
* **2-of-3** tolerates one provider being unavailable or wrong, while still requiring genuine agreement. It is the smallest majority that survives a single faulty participant.

## In testing vs production

In a real deployment, the carriers' systems would call the oracle's webhooks automatically as parcels are scanned. In this prototype you can **simulate** those confirmations manually for testing — see [Simulating a Delivery](../usage/simulating-delivery.md). The on-chain logic is identical either way.
