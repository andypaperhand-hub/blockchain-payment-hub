# Old Model vs New Model

A side-by-side comparison of how the same purchase behaves under each model.

## The two models at a glance

| Dimension | Traditional marketplace | Blockchain Payment Hub |
| --- | --- | --- |
| **Who holds the money mid-transaction** | The marketplace / processor | A smart contract (no party controls it) |
| **What releases the funds** | An operator decision / internal policy | An automatic on-chain condition (delivery confirmed) |
| **Who confirms delivery** | The operator, trusting carrier data | A panel of logistics providers reaching consensus on-chain |
| **How disputes are decided** | Private support team | A 2-of-3 arbiter vote recorded on-chain, with an appeal round |
| **Auditability of the record** | Internal, private | Public, tamper-proof (anchored to Ethereum) |
| **Can the trust layer be reused by others** | No — bundled with the storefront | Yes — any marketplace can integrate the same contracts |
| **Failure mode** | Operator error / policy / outage moves or freezes funds | Funds follow the coded rules regardless of any party's wishes |

## Walking through one purchase

**Traditional**

1. Buyer pays the marketplace.
2. The marketplace holds the balance.
3. Seller ships; the marketplace marks the order delivered based on carrier status.
4. The marketplace releases the balance to the seller, minus commission.
5. If the buyer complains, the marketplace's support team decides — privately.

**Blockchain Payment Hub**

1. Buyer locks payment into the **EscrowManager** contract.
2. Funds are held *by the contract* — visible on-chain, controllable by no one.
3. Seller ships; logistics providers report delivery to the **LogisticsOracle**.
4. When 2 of 3 providers confirm, the oracle calls the contract and funds auto-release to the seller (minus a transparent platform fee).
5. If the buyer disputes, **DisputeResolution** opens an evidence window; arbiters vote on-chain; the ruling — including split refunds — is enforced automatically and permanently recorded.

## The honest caveat

The new model does not make trust *free*; it relocates it. Instead of trusting an operator, participants trust (a) the correctness of the smart contract code and (b) the honesty of the oracle providers and arbiters. The design's job is to make those trust assumptions **minimal, explicit, and verifiable** — which the following Core Concepts pages describe in detail.
