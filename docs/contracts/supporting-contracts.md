# Supporting Contracts

These contracts are not user-facing on their own, but they underpin the security and testing of the system.

## NonceManager

> `packages/contracts/contracts/core/NonceManager.sol` — inherited by `EscrowManager`

Provides per-merchant replay protection for signed payment payloads.

| Member | Purpose |
| --- | --- |
| `currentNonce(merchant)` | The merchant's current nonce |
| `isNonceUsed(merchant, nonce)` | Whether a nonce has been consumed |
| `_validateAndUseNonce(merchant, nonce)` | Internal: consume a nonce, reverting on reuse |
| `NonceUsed` event | Emitted when a nonce is spent |

## SignatureVerifier

> `packages/contracts/contracts/core/SignatureVerifier.sol` — inherited by `EscrowManager`
> Uses OpenZeppelin `EIP712` with domain `("BlockchainPaymentHub", "1")`

Verifies EIP-712 typed signatures over a `PaymentPayload`, and supports smart-contract-wallet signatures via **EIP-1271** (`IERC1271.isValidSignature`).

| Member | Purpose |
| --- | --- |
| `hashPayload(payload)` | EIP-712 digest of a structured payment payload |
| `_verifyMerchantSignature(...)` | Internal: validate the merchant's signature |

Together with `NonceManager`, this enables signed, replay-safe payments and is the foundation for account-abstraction UX. See [Account Abstraction & Signed Payments](../concepts/account-abstraction.md).

## MockERC20 (mUSDT)

> `packages/contracts/contracts/mocks/MockERC20.sol` — inherits OpenZeppelin `ERC20`, `Ownable`

A configurable test stablecoin used as the payment token on testnet.

| Member | Purpose |
| --- | --- |
| `constructor(name, symbol, decimals_)` | Deploy with a chosen decimals value |
| `decimals()` | Returns the configured decimals (overridden) |
| `mint(to, amount)` | `onlyOwner` — mints test tokens (used to fund test buyers) |

{% hint style="info" %}
To get test mUSDT, the deployer can call `mint()` (see the `mint.ts` script in `packages/contracts/scripts/`) or transfer from the deployer wallet, which holds 10,000 mUSDT.
{% endhint %}
