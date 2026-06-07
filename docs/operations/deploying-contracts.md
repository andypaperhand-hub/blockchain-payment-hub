# Deploying Contracts

The contracts in this repo are already deployed to Sepolia (see [Contracts Overview](../contracts/overview.md)). This page covers redeploying — for example, to your own addresses.

## Deploy to Sepolia

From the contracts package:

```bash
cd packages/contracts
pnpm hardhat run scripts/deploy.ts --network sepolia
```

The deploy script writes the resulting addresses to `packages/shared/constants/addresses.json` so the backend and frontend can pick them up.

## Useful scripts

The `packages/contracts/scripts/` folder includes:

| Script | Purpose |
| --- | --- |
| `deploy.ts` | Deploys all contracts and records addresses |
| `mint.ts` | Mints test mUSDT to an address (token owner only) |
| `measure-gas.ts` | Measures gas usage of key operations |
| `args-*.js` | Constructor-argument files for Etherscan verification |

## Pre-deployment security checks

Before claiming security properties (for the paper or a demo), the project's plan calls for:

* **Static analysis** with Slither.
* **Symbolic execution** with Mythril.
* A manual review checklist covering reentrancy guards, access control on every sensitive function, signature replay protection (nonce + deadline), front-running resistance, and gas limits in batch operations.

{% hint style="warning" %}
Deployment requires a funded wallet. If you hit an "insufficient funds" error, top up with Sepolia ETH from a faucet.
{% endhint %}
