# EscrowManager

> `packages/contracts/contracts/core/EscrowManager.sol`
> Inherits: `NonceManager`, `SignatureVerifier`, `ReentrancyGuard`, `Ownable`

The custody contract. It holds escrowed ERC-20 funds and enforces the legal transitions between escrow states. It is the only contract that moves money.

## State machine

`EscrowState`: `CREATED` · `LOCKED` · `RELEASED` · `AUTO_RELEASED` · `DISPUTED` · `REFUNDED` · `PARTIAL_REFUND`

Each escrow is stored in an `Escrow` struct keyed by `orderId` (`bytes32`), holding the merchant, customer, token, amount, `platformFeeRate` (basis points), current state, and the timestamps `lockedAt` / `deliveredAt` / `autoReleaseAt`, plus an `oracleConfirmed` flag.

## Configuration

| Variable | Default | Meaning |
| --- | --- | --- |
| `defaultPlatformFeeRate` | `250` (2.5%) | Fee taken on release, in basis points |
| `deliveryWindowSeconds` | 3 days | Expected delivery time |
| `confirmWindowSeconds` | 7 days | Buyer's confirm/dispute window |
| `disputeWindowSeconds` | 7 days | Window to open a dispute |

## Key functions

| Function | Who | What it does |
| --- | --- | --- |
| `lockEscrow(...)` | buyer flow | Pulls the ERC-20 amount in and locks it; sets state `LOCKED` |
| `confirmDelivery(orderId)` | buyer | Buyer confirms receipt; releases to seller |
| `confirmDeliveryByOracle(orderId)` | `onlyOracle` | Oracle confirms delivery after 2-of-3 consensus |
| `triggerAutoRelease(orderId)` | anyone | After the confirm window with delivery confirmed, releases to seller |
| `openDispute(orderId)` | buyer/seller | Moves escrow to `DISPUTED` |
| `executeDisputeRuling(orderId, sellerBasisPoints)` | `onlyDisputeContract` | Enforces the arbiters' ruling (full/partial/refund) |
| `getEscrowParties(orderId)` | view | Returns `(customer, merchant)` |

Internal helpers `_releaseToSeller`, `_refundToCustomer`, and `_partialRelease` perform the actual transfers and fee split.

## Admin functions (`onlyOwner`)

`addAcceptedToken` · `removeAcceptedToken` · `setDisputeContract` · `setOracleAggregator` · `setPlatformTreasury` · `setFeeRate`

## Events

`EscrowCreated` · `EscrowLocked` · `EscrowReleased` · `EscrowAutoReleased` · `EscrowDisputed` · `EscrowRefunded` · `EscrowPartialRefund` · `DeliveryConfirmed`

The backend's event listener subscribes to these and rebroadcasts them to the UI over WebSocket.

## Safety properties

* **Reentrancy:** state-changing external calls are guarded by `ReentrancyGuard`.
* **Access control:** privileged transitions are gated by `onlyOracle` / `onlyDisputeContract` / `onlyOwner`.
* **State guards:** `escrowExists` and `inState` modifiers prevent illegal transitions.
* **Replay protection:** signed payment payloads consume a nonce (see [Supporting Contracts](supporting-contracts.md)).
