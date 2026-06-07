# First-Time Setup

Do these steps **once** on a new machine.

## Step 1 — Clone the repository

```bash
git clone https://github.com/alexdou0703/blockchain-payment-hub.git
cd blockchain-payment-hub
```

## Step 2 — Install dependencies

This installs packages for every service in the monorepo at once:

```bash
pnpm install
```

{% hint style="info" %}
The first install may take 2–3 minutes.
{% endhint %}

## Step 3 — Create your environment file

```bash
cp .env.example .env
```

Then fill in your secrets — see [Environment Variables](environment-variables.md) for what each one means.

## Step 4 — Configure MetaMask

Point your wallet at the Sepolia testnet and fund it with free test ETH — see [Configuring MetaMask](metamask.md).

Once these four steps are done, continue to [Running the System](../running/with-docker.md).
