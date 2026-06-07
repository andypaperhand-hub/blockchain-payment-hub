# Contracts Overview

All contracts are written in **Solidity 0.8.28**, built with **Hardhat**, and use **OpenZeppelin v5** for battle-tested primitives (`Ownable`, `ReentrancyGuard`, `ERC20`, `EIP712`).

## Deployed addresses (Sepolia)

These contracts are live on the Ethereum **Sepolia testnet** and can be inspected on [sepolia.etherscan.io](https://sepolia.etherscan.io).

| Contract | Purpose | Address |
| --- | --- | --- |
| **EscrowManager** | Locks buyer funds; releases to seller on delivery | `0xC40AB9fDD3B70a3327647ff7aD851921D85D0C4B` |
| **LogisticsOracle** | Aggregates delivery confirmations from 3 providers | `0xA1A34F39f2c2644941e40ef149813D22da7077e5` |
| **DisputeResolution** | 2-of-3 arbiter voting for disputed orders | `0xC9dE93D7c83D73b82Aa147BeE3653a86CD1cdCEe` |
| **SettlementContract** | Anchors batch settlement Merkle roots to L1 | `0xe6c346D2680D2c0cCfF4a00755fd947fA77E60c9` |
| **MockERC20 (mUSDT)** | Test stablecoin used for payments (not real money) | `0xfAB3f938A1E198119e32b7a2Fd1F16BFAE9e07a0` |

**Deployer / Treasury wallet:** `0x160F267cEF249Ced05c30e32172E9420E8Ad1EC8`

{% hint style="warning" %}
These are **testnet** contracts. mUSDT has no real-world value.
{% endhint %}

## How the contracts relate

```
        ┌──────────────────┐
        │  EscrowManager   │  holds funds, enforces state machine
        └───┬──────────┬───┘
   onlyOracle│          │onlyDisputeContract
            ▼          ▼
  ┌────────────────┐  ┌─────────────────────┐
  │ LogisticsOracle│  │  DisputeResolution  │
  │ (2-of-3 carriers)│ (2-of-3 arbiters)    │
  └────────────────┘  └─────────────────────┘

  SettlementContract  ── independent; anchors Merkle roots (sequencer-only)
  MockERC20 (mUSDT)   ── the ERC-20 token escrowed
  NonceManager / SignatureVerifier ── inherited by EscrowManager (signed payments)
```

The `EscrowManager` is the only contract that custodies funds. It grants two privileged callers: the oracle (to confirm delivery) and the dispute contract (to execute rulings). Each is enforced by a modifier (`onlyOracle`, `onlyDisputeContract`).

## Source layout

```
packages/contracts/contracts/
├── core/
│   ├── EscrowManager.sol
│   ├── NonceManager.sol
│   └── SignatureVerifier.sol
├── modules/
│   └── DisputeResolution.sol
├── oracle/
│   └── LogisticsOracle.sol
├── l1/
│   └── SettlementContract.sol
├── mocks/
│   └── MockERC20.sol
└── interfaces/
    ├── IEscrow.sol
    └── IDisputeResolution.sol
```
