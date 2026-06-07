# Troubleshooting

Common issues and fixes.

## "Empty string for network or forking URL"

Hardhat can't find `SEPOLIA_RPC_URL`. Ensure `.env` exists at the **repo root** (not in `packages/contracts`) and the value is filled in:

```bash
grep SEPOLIA_RPC_URL .env
```

## "Insufficient funds" when deploying

Your wallet lacks Sepolia ETH for gas. Get free test ETH from a Sepolia faucet (e.g. [sepoliafaucet.com](https://sepoliafaucet.com)).

## Docker: "port is already in use"

Something is already on port 3000, 3001, or 3002:

```bash
lsof -i :3001        # find the process
kill -9 PID          # stop it (use the PID from above)
```

## MetaMask shows the wrong network

Set MetaMask to **Sepolia Test Network**. If it's hidden: Settings → Advanced → Show test networks → ON.

## "nonce too high" / "replacement transaction underpriced"

MetaMask has a stuck pending transaction. **MetaMask → Settings → Advanced → Reset Account** clears local transaction history without touching your funds.

## `pnpm install` fails

Confirm you are on Node.js 20 or 22 (not 25):

```bash
node --version       # must be v20 or v22
```

## Backend won't connect to the database

Check PostgreSQL is healthy:

```bash
docker compose ps    # postgres should show "healthy"
docker compose up postgres redis   # start them if not
```
