# As a Buyer

The buyer journey: connect a wallet, pay into escrow, then confirm receipt.

## Steps

1. Go to **http://localhost:3000**.
2. Click **Connect Wallet** — MetaMask opens and asks you to connect.
3. Make sure MetaMask is on the **Sepolia** network.
4. Browse to a product and click **Buy Now** — this creates an order.
5. On the checkout page you'll see the order details and a **Pay with mUSDT** button.
6. Click it. MetaMask asks you to approve **two** transactions:
   * **Approve** — lets the escrow contract spend your mUSDT.
   * **Lock Payment** — moves your mUSDT into the escrow contract.
7. Once confirmed on-chain, the seller is notified and ships the item.

## What's happening underneath

* Locking calls `EscrowManager.lockEscrow(...)`; the contract emits `EscrowLocked`.
* The backend hears that event and pushes a real-time `payment.locked` update to the page.
* Your funds are now held by the contract — not the seller, not the marketplace.

## After delivery

When the item arrives you can confirm receipt (releasing funds to the seller), or — if something is wrong — open a dispute. If you do nothing after delivery is confirmed and the confirm window passes, the payment auto-releases to the seller.

{% hint style="info" %}
You need a little Sepolia ETH for gas **and** some mUSDT to pay. See [Configuring MetaMask](../setup/metamask.md).
{% endhint %}
