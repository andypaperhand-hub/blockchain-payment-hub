# System Overview

The running system is made of **five services** plus the smart contracts on Sepolia. This page shows how they fit together; the next page traces a single payment through them.

## The five services

```
┌───────────────────────────────────────────────────────────────┐
│                        User's Browser                          │
│            (connects a wallet via the MetaMask extension)      │
└────────────────────────────────┬──────────────────────────────┘
                                 │ HTTP / WebSocket
                                 ▼
┌───────────────────────────────────────────────────────────────┐
│  Frontend  ·  port 3000  ·  Next.js 14                         │
│  • Checkout page      • Seller dashboard     • Disputes page   │
└────────────────────────────────┬──────────────────────────────┘
                                 │ REST (/api/v1) + WebSocket
                                 ▼
┌───────────────────────────────────────────────────────────────┐
│  Backend  ·  port 3001  ·  NestJS 10                           │
│  • Orders, Payments, Disputes, Settlement, Fiat, Metrics       │
│  • Listens to on-chain events (EthersService + EventListener)  │
│  • Nightly batch settlement (Bull + Redis)                     │
│  • Pushes real-time updates (Socket.IO)                        │
│  • Swagger docs at /api/docs                                   │
└──────────┬───────────────────────────────────┬────────────────┘
           │                                   │
           ▼                                   ▼
┌────────────────────────┐        ┌───────────────────────────────┐
│  PostgreSQL 15         │        │  Oracle Service · port 3002   │
│  Orders, payments,     │        │  Receives carrier webhooks;   │
│  disputes, batches     │        │  on 2-of-3 consensus, calls   │
└────────────────────────┘        │  the on-chain oracle contract │
┌────────────────────────┐        └───────────────────────────────┘
│  Redis 7  (job queue)  │
│  Schedules settlement  │
└────────────────────────┘

All services connect to the Ethereum Sepolia testnet via Infura.
```

## What each service is responsible for

| Service | Stack | Responsibility |
| --- | --- | --- |
| **Frontend** | Next.js 14, Wagmi v2, RainbowKit | The UI buyers and sellers use; talks to the wallet and the backend |
| **Backend** | NestJS 10, TypeORM | Business logic, persistence, event listening, settlement, real-time notifications |
| **Oracle service** | NestJS (standalone) | Ingests logistics webhooks, maps them to orders, drives on-chain consensus |
| **PostgreSQL** | v15 | System of record for off-chain order/payment/dispute data |
| **Redis** | v7 | Bull job queue powering the payment state machine and nightly settlement |

## Backend module map

The backend is organized into focused NestJS modules, each owning its slice of the domain:

* `orders/` — create and track orders
* `payments/` — generate payment requests, track lock/confirm state (Bull state machine)
* `disputes/` — open disputes, attach evidence, appeal
* `settlement/` — build Merkle batches, anchor roots, verify proofs (Pinata/IPFS)
* `oracle/` — receive/forward delivery signals
* `fiat/` — fiat bridge (exchange-rate, on/off-ramp endpoints)
* `blockchain/` — `EthersService` + `EventListener` connecting to Sepolia
* `notifications/` — Socket.IO gateway broadcasting domain events
* `metrics/` — system metrics endpoint

## On-chain layer

Four core contracts plus a test token live on Sepolia: **EscrowManager**, **LogisticsOracle**, **DisputeResolution**, **SettlementContract**, and **MockERC20** (the test stablecoin). See the [Smart Contracts](../contracts/overview.md) section for addresses and references.
