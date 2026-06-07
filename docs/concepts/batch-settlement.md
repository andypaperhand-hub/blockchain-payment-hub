# Batch Settlement & Merkle Anchoring

Every individual escrow already lives on-chain. So why is there a separate settlement contract? Because the system also needs a **compact, tamper-proof audit trail** of large numbers of transactions that can be verified cheaply and independently — including by parties doing accounting and reconciliation off-chain.

## The idea: anchor a fingerprint, not every record

Periodically (a nightly batch job), the backend collects the transactions from that period and builds a **Merkle tree** from them. The single **Merkle root** — a 32-byte fingerprint of the entire batch — is committed to the **SettlementContract** on Ethereum L1 via `commitBatch(merkleRoot, txCount)`.

```
tx1 tx2 tx3 tx4 ... txN
   \  /     \  /
   hash     hash        ← pairwise hashing up the tree
     \       /
      \     /
     Merkle root  ──commitBatch()──►  stored on Ethereum (L1)
```

## Why this is powerful

* **Cheap.** Committing one 32-byte root costs the same regardless of whether the batch has 10 or 10,000 transactions.
* **Tamper-proof.** If anyone later alters a single transaction record off-chain, its hash changes, the recomputed root no longer matches the root anchored on-chain, and the tampering is detected.
* **Independently verifiable.** Anyone can prove a specific transaction was included in a committed batch using a **Merkle proof**, via `verifyTransaction(...)` — without revealing or downloading the entire batch.

## How verification works

To prove transaction *T* was part of batch *B*:

1. Provide *T*'s leaf hash plus the small set of sibling hashes along the path to the root (the Merkle proof).
2. The contract recomputes the root from those hashes (`_verifyMerkle`).
3. If the recomputed root equals the root stored for batch *B*, inclusion is proven.

This is the same construction used by blockchains themselves and by certificate-transparency logs — a well-understood, auditable primitive.

## Who can commit

Only the designated **sequencer** address may commit batches (`onlySequencer`), and `getBatchCount()` exposes how many batches have been anchored. This separates the *production* of batches (the backend) from the *trust* in their integrity (the on-chain root that anyone can check).

➡️ See the [SettlementContract reference](../contracts/settlement-contract.md).
