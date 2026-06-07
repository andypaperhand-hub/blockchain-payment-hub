# SettlementContract

> `packages/contracts/contracts/l1/SettlementContract.sol`
> Inherits: `Ownable`

Anchors a tamper-proof fingerprint of batched transactions to Ethereum L1, and lets anyone prove a given transaction was included — without storing every record on-chain.

## Functions

| Function | Who | What it does |
| --- | --- | --- |
| `commitBatch(merkleRoot, txCount)` | `onlySequencer` | Stores a new batch's Merkle root and transaction count |
| `verifyTransaction(...)` | view | Verifies a Merkle proof of inclusion against a committed root |
| `getBatchCount()` | view | Returns the number of committed batches |
| `setSequencer(address _seq)` | `onlyOwner` | Sets the authorized sequencer |

Each batch is stored in a `Batch` struct and a `BatchCommitted(batchId, merkleRoot, txCount)` event is emitted on commit. The internal `_verifyMerkle` helper recomputes the root from a leaf plus its sibling path.

## Why a separate contract

The escrow contract is the source of truth for *funds*. The settlement contract is the source of truth for the *integrity of the historical record*. Anchoring one 32-byte root per batch makes large-scale reconciliation cheap and independently auditable. See [Batch Settlement & Merkle Anchoring](../concepts/batch-settlement.md) for the full explanation.

## Trust model

Only the **sequencer** (the backend's settlement worker) can commit batches, but it cannot forge inclusion: a Merkle proof either reproduces the committed root or it does not. Anyone can verify, so the sequencer is trusted to *publish* but not to be *honest about contents* — the math enforces that.
