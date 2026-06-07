# Running Tests

The project ships with smart-contract, unit, and end-to-end tests.

## Smart contract tests (Hardhat)

```bash
cd packages/contracts
pnpm run test
```

Expected: **18/18 tests passing**. For coverage:

```bash
pnpm run coverage
```

The contract test suite lives in `packages/contracts/test/` (e.g. `EscrowManager.test.ts`).

## Backend unit tests (Jest)

```bash
cd packages/backend
pnpm run test
```

Expected: **74/74 tests passing**.

## Backend end-to-end tests

```bash
cd packages/backend
pnpm run test:e2e
```

These spin up a real HTTP server and exercise the full request/response cycle (Supertest).

## What CI runs

On every push, GitHub Actions runs three jobs (see `.github/workflows/ci.yml`):

* **Backend Unit & E2E Tests** — `pnpm --filter @payment-hub/backend test`
* **Solidity Tests** — compile contracts, then `pnpm --filter @payment-hub/contracts test`
* **Backend Build** — `pnpm --filter @payment-hub/backend build`

➡️ See [Hosting & CI/CD](operations/hosting-and-ci.md) for deployment automation.
