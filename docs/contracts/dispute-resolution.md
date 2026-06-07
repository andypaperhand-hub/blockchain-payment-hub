# DisputeResolution

> `packages/contracts/contracts/modules/DisputeResolution.sol`
> Inherits: `Ownable`, `IDisputeResolution`

Decides contested orders through a 2-of-3 arbiter vote, with an evidence phase and an appeal round, and instructs the escrow contract to enforce the outcome.

## State machine

`DisputeState`: `OPEN` (evidence) → `VOTING` (arbiters vote) → `RESOLVED` (ruling stored, appeal open) → `FINAL` (irreversible).

| Window | Default |
| --- | --- |
| `evidenceWindow` | 72 hours |
| `votingWindow` | 48 hours |
| `appealWindow` | 48 hours |

The panel is `address[3] arbiters`, set at construction: `constructor(address _escrowManager, address[3] memory _arbiters)`.

## Functions

| Function | Who | What it does |
| --- | --- | --- |
| `createDispute(orderId, initiator)` | escrow flow | Opens a dispute (state `OPEN`) |
| `submitEvidence(orderId, ipfsHash)` | parties | Records an IPFS evidence pointer on-chain |
| `startVoting(orderId)` | — | Moves an open dispute into `VOTING` |
| `castVote(orderId, vote, sellerBasisPoints)` | `onlyArbiter` | An arbiter votes, proposing the seller's share |
| `appeal(orderId)` | parties | Within the appeal window, triggers a second round |
| `finalize(orderId)` | — | Closes the dispute and executes the ruling |
| `getEvidenceCount(orderId)` | view | Number of evidence items submitted |
| `setEscrowManager(address _em)` | `onlyOwner` | Configure the escrow target |

## Round-safe voting

Votes are tracked in a nested mapping keyed by **(orderId, round, arbiter)**. Keying by round avoids the problem of having to clear a nested mapping between rounds — a fresh appeal round starts with a clean slate while the prior round's record is preserved.

## Outcome enforcement

On finalization, `_resolve` / `_execute` call `EscrowManager.executeDisputeRuling(orderId, sellerBasisPoints)`. Because the ruling is expressed in basis points, outcomes range continuously from full refund (`0`) to full release (`10000`), including any split in between.

## Events

`DisputeOpened` · `EvidenceSubmitted` · `ArbiterVoted` · `DisputeResolved` · `DisputeAppealed` · `DisputeFinalized`
