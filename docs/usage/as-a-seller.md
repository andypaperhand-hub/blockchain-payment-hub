# As a Seller

The seller manages incoming orders and ships goods from the dashboard.

## Steps

1. Go to **http://localhost:3000/seller/dashboard**.
2. Connect your seller wallet via MetaMask.
3. You'll see incoming orders and their payment status.
4. When you ship an item, mark it as shipped — this notifies the logistics oracle.
5. Track delivery confirmations in real time.

## Getting paid

Once 2 of 3 logistics providers confirm delivery, the oracle calls the escrow contract and your funds release automatically (minus the transparent platform fee). You do not need to chase the marketplace for payout — the contract enforces it.

If a buyer opens a dispute, the order moves into the dispute flow; the arbiter panel decides the split. See [Raising a Dispute](raising-a-dispute.md).
