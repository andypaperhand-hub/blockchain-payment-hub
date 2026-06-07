# LogisticsOracle

> `packages/contracts/contracts/oracle/LogisticsOracle.sol`
> Inherits: `Ownable`

Brings real-world delivery facts on-chain by aggregating confirmations from three independent logistics providers and requiring a majority before acting.

## Configuration

* `arbiters`/providers: a fixed array of **3 provider addresses**, set in the constructor `constructor(address _escrowManager, address[3] memory _providers)`.
* Required consensus: configurable via `setRequiredConsensus(uint256 n)` (default **2**).

## Functions

| Function | Who | What it does |
| --- | --- | --- |
| `confirmDelivery(orderId, trackingCode)` | `onlyProvider` | A provider records its confirmation; if the threshold is now met, triggers consensus |
| `setEscrowManager(address _em)` | `onlyOwner` | Points the oracle at the escrow contract |
| `setRequiredConsensus(uint256 n)` | `onlyOwner` | Adjusts the consensus threshold |

## Events

* `ProviderConfirmed(orderId, provider, trackingCode)` — one provider has confirmed.
* `ConsensusReached(orderId, confirmedAt)` — the threshold was met; the oracle then calls `EscrowManager.confirmDeliveryByOracle(orderId)`.

## Trust model

The oracle's honesty rests on the assumption that **a majority of the three providers will not collude or fail simultaneously**. With a 2-of-3 threshold, the system tolerates one provider being offline, late, or wrong without freezing funds and without letting any single provider release funds on its own. See [The Delivery Oracle](../concepts/delivery-oracle.md) for the rationale.
