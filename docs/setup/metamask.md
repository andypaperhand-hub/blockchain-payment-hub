# Configuring MetaMask

The system uses MetaMask as the browser wallet and runs on the Sepolia testnet.

## Switch to the Sepolia network

1. Open MetaMask in your browser.
2. Click the network dropdown at the top (it may say "Ethereum Mainnet").
3. Click **Show test networks**, then select **Sepolia**.

If you don't see test networks: **MetaMask → Settings → Advanced → Show test networks → ON**.

## Get free test ETH (for gas)

You need a little Sepolia ETH to pay for transaction gas:

1. Copy your wallet address from MetaMask.
2. Go to [sepoliafaucet.com](https://sepoliafaucet.com) (or another Sepolia faucet).
3. Paste your address and request funds.

## Get test mUSDT (for payments)

Payments use the test stablecoin **mUSDT**, not ETH. To obtain some:

* Call `MockERC20.mint(yourAddress, amount)` using the contract's `mint.ts` script (the deployer is the token owner), **or**
* Ask the deployer wallet (which holds 10,000 mUSDT) to transfer you some.

{% hint style="info" %}
ETH pays for **gas**; mUSDT is the **payment** token locked in escrow. You need a small amount of both to complete a purchase.
{% endhint %}
