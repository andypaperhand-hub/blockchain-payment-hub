# Environment Variables

Secrets live in a `.env` file at the **repository root**. It is listed in `.gitignore` and must never be committed.

## RPC connection (Infura)

```bash
# HTTPS endpoint for Sepolia. Create a free project at https://app.infura.io
SEPOLIA_RPC_URL=https://sepolia.infura.io/v3/YOUR_INFURA_KEY

# WebSocket endpoint (same key) — used for real-time event listening
SEPOLIA_WS_URL=wss://sepolia.infura.io/ws/v3/YOUR_INFURA_KEY
```

## Wallet

```bash
# Your MetaMask private key — export via Account Details → Export Private Key
# IMPORTANT: prefix with 0x. Example: 0xabc123...
DEPLOYER_PRIVATE_KEY=0xYOUR_PRIVATE_KEY_HERE

# Your wallet address (the 0x... shown in MetaMask)
TREASURY_ADDRESS=0xYOUR_WALLET_ADDRESS_HERE
```

{% hint style="danger" %}
Never share or commit `DEPLOYER_PRIVATE_KEY`. Anyone with it controls the wallet. This is a testnet key, but treat it as a secret regardless.
{% endhint %}

## Live contract addresses (already filled in)

```bash
ESCROW_CONTRACT_ADDRESS=0xC40AB9fDD3B70a3327647ff7aD851921D85D0C4B
DISPUTE_CONTRACT_ADDRESS=0xC9dE93D7c83D73b82Aa147BeE3653a86CD1cdCEe
ORACLE_CONTRACT_ADDRESS=0xA1A34F39f2c2644941e40ef149813D22da7077e5
SETTLEMENT_CONTRACT_ADDRESS=0xe6c346D2680D2c0cCfF4a00755fd947fA77E60c9
USDT_ADDRESS=0xfAB3f938A1E198119e32b7a2Fd1F16BFAE9e07a0
```

## Off-chain services

```bash
# IPFS pinning for dispute evidence — free JWT at https://app.pinata.cloud/keys
PINATA_JWT=YOUR_PINATA_JWT_HERE

# WalletConnect — free Project ID at https://cloud.walletconnect.com
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=YOUR_PROJECT_ID_HERE
```

{% hint style="info" %}
Variables prefixed `NEXT_PUBLIC_` are read by the frontend at **build time**. If you change them, rebuild the frontend.
{% endhint %}
