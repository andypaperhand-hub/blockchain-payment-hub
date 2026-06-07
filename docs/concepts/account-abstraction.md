# Account Abstraction & Signed Payments

A recurring friction in blockchain UX is that every action normally requires the user to sign and pay gas for an on-chain transaction. This page explains how the system reduces that friction with **signed payment payloads** and replay protection.

## Signed payment payloads (EIP-712)

Rather than requiring a raw, hard-to-read transaction, a merchant can authorize a payment by signing a **structured, human-readable payload** using the EIP-712 standard. The `SignatureVerifier` contract:

* Defines a typed `PaymentPayload` structure.
* Hashes it with EIP-712 domain separation (`hashPayload`), so a signature for this contract and chain cannot be reused elsewhere.
* Verifies the merchant's signature (`_verifyMerchantSignature`) before acting on it.

It also supports **EIP-1271** (`isValidSignature`), which means the "signer" can be a *smart-contract wallet*, not only a traditional externally-owned account. This is the foundation that lets smart wallets and account-abstraction flows participate.

## Replay protection (NonceManager)

A signed message, if it could be submitted twice, would be a serious vulnerability — someone could replay an old authorization to move funds again. `NonceManager` prevents this:

* Each merchant has a `currentNonce`.
* Every signed payload consumes a nonce via `_validateAndUseNonce`.
* `isNonceUsed` lets anyone check whether a given nonce has already been spent, and the `NonceUsed` event records consumption.

Combined with a deadline on the payload, this guarantees each authorization is valid **once** and only for a limited time.

## Why this matters for the research framing

These pieces — typed signatures, smart-contract-wallet support, and nonce-based replay protection — are the building blocks of **account abstraction**: the broader effort to make blockchain accounts behave more like flexible, user-friendly accounts (sponsored gas, batched actions, recovery) rather than bare key pairs. For an e-commerce payment rail aimed at ordinary buyers, lowering this UX barrier is essential, and the prototype demonstrates the core primitives.

{% hint style="info" %}
The contracts implement the signing and replay-protection primitives directly. Full sponsored-transaction / passkey flows are noted as an extension direction in [Limitations & Future Work](../appendix/limitations-and-future-work.md).
{% endhint %}
