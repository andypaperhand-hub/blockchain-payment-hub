# Hosting & CI/CD

This page summarizes how the live test deployment is hosted and how changes ship.

## Hosting topology

| Component | Hosted on |
| --- | --- |
| Backend (NestJS) | Railway |
| Frontend (Next.js) | Vercel |
| Database / queue | PostgreSQL + Redis (managed alongside the backend) |
| Contracts | Ethereum Sepolia testnet (via Infura) |

A legacy "test in 5 minutes" mode uses **Cloudflare quick tunnels** to expose locally running services for quick sharing.

## Environment variables in production

* On the **backend host** (Railway): the `SEPOLIA_*` URLs, contract addresses, `PINATA_JWT`, etc.
* On the **frontend host** (Vercel): the `NEXT_PUBLIC_*` variables. Because these are baked at build time, changing them requires a rebuild/redeploy of the frontend.

## Continuous integration

GitHub Actions (`.github/workflows/ci.yml`) runs on each push:

1. **Backend Unit & E2E Tests**
2. **Solidity Tests** (compile + Hardhat tests)
3. **Backend Build**

Auto-deploy is wired so that merges to `main` redeploy the hosted services.

## Redeploying manually

* **Backend (Railway):** redeploy the latest `main` commit from the Railway dashboard or CLI; view logs there.
* **Frontend (Vercel):** trigger a redeploy from the Vercel dashboard (rebuilds with current `NEXT_PUBLIC_*` values).

{% hint style="info" %}
For the authoritative, environment-specific commands, see `DEPLOYMENT.md` and `HOSTING.md` in the repository root.
{% endhint %}
