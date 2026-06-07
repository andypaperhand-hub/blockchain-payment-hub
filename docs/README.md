---
description: >-
  A decentralized escrow payment gateway for e-commerce, built on Ethereum.
  Buyers lock funds in a smart contract; funds are released only when delivery
  is confirmed; disputes are settled on-chain.
---

# Blockchain Payment Hub

**Blockchain Payment Hub** is a research prototype that replaces the traditional bank-and-processor settlement layer of e-commerce with a set of smart contracts on the Ethereum blockchain.

When a buyer pays, the money is not handed to the marketplace. It is locked inside an **escrow** smart contract. The contract releases the funds to the seller only after delivery has been independently confirmed, and if anything goes wrong, an on-chain panel of arbiters decides the outcome. No bank, processor, or marketplace operator can unilaterally move the money.

{% hint style="info" %}
This is a **dissertation / research prototype** deployed on the Ethereum **Sepolia testnet**. No real money is involved — payments use a test stablecoin (mUSDT).
{% endhint %}

## The one-paragraph pitch

> Today, an e-commerce marketplace acts as both the *storefront* and the *trusted intermediary* that holds your money until you receive your goods. These two roles are bundled together, and the marketplace charges rent for the bundle. Blockchain Payment Hub **unbundles the trust layer** from the marketplace: the escrow, the delivery confirmation, the dispute ruling, and the audit trail all live on a neutral public ledger that no single party controls. The marketplace can keep running the storefront — it just no longer needs to be trusted with the money.

## What you'll find in these docs

| Section | What it covers |
| --- | --- |
| **Background & Motivation** | *Why* this exists — the economics of payment intermediaries, written for non-technical readers |
| **Core Concepts** | The four mechanisms — escrow, oracle, disputes, settlement — explained plainly |
| **System Architecture** | The five running services and how a payment flows through them |
| **Smart Contracts** | A reference for each deployed contract, with real function signatures |
| **Getting Started → Operations** | Everything needed to install, run, use, test, and deploy the system |
| **Academic Appendix** | The research contribution, limitations, and how this maps to the paper |

## At a glance

* **Chain:** Ethereum Sepolia testnet (via Infura)
* **Smart contracts:** Solidity 0.8.28, Hardhat, OpenZeppelin v5
* **Backend:** NestJS 10, PostgreSQL 15, Redis 7, Socket.IO
* **Frontend:** Next.js 14, Wagmi v2, RainbowKit
* **Tooling:** pnpm workspaces, Turborepo, Docker Compose, GitHub Actions

➡️ New here? Start with **[Problem & Solution](overview/problem-and-solution.md)**. Want to run it? Jump to **[Prerequisites](setup/prerequisites.md)**.

**Repository:** [github.com/alexdou0703/blockchain-payment-hub](https://github.com/alexdou0703/blockchain-payment-hub)
