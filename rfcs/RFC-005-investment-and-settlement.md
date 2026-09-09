# RFC-005: Investment & Settlement

**Status:** DRAFT
**Category:** Protocol / Financial
**Target:** KeystoneGrid v1

## 1. Summary

This RFC defines how an investor acquires an ownership interest in a Keystone Asset and how payment and ownership settlement are coordinated.

Investment is a protocol operation. A frontend confirmation or backend database update must never be treated as proof that an investment succeeded.

## 2. Core Principle

A successful investment requires an authoritative on-chain settlement.

The system must distinguish:

* investment intent;
* eligibility;
* transaction preparation;
* wallet authorization;
* transaction submission;
* transaction confirmation;
* ownership settlement.

## 3. Investment Lifecycle

The conceptual lifecycle is:

`DISCOVERY → ELIGIBILITY → REVIEW → TRANSACTION_PREPARATION → SIGNING → SUBMITTED → CONFIRMED → OWNERSHIP_UPDATED`

Failure states must also be explicit:

`REJECTED / FAILED / EXPIRED / CANCELLED`

## 4. Preconditions

Before an investment can settle, the following conditions may need to be satisfied:

* asset exists;
* asset is active;
* offering is open;
* investor is eligible;
* requested quantity is valid;
* sufficient supply exists;
* price/configuration is valid;
* payment requirements are satisfied;
* protocol authorization succeeds.

The contract must enforce all conditions that are security- or ownership-critical.

## 5. Authority Boundary

### Contract

The contract is authoritative for:

* settlement;
* ownership changes;
* supply limits;
* protocol permissions;
* applicable on-chain pricing/configuration;
* transaction validity.

### Backend

The backend provides:

* asset metadata;
* eligibility workflow;
* transaction preparation;
* indexing;
* transaction status;
* historical records;
* notifications.

### Frontend

The frontend provides:

* investment discovery;
* price and quantity display;
* eligibility UX;
* transaction preparation;
* wallet signing;
* confirmation feedback.

Neither frontend nor backend may declare an investment successful without authoritative transaction confirmation.

## 6. User Authorization

Ordinary investor transactions must be authorized by the investor's Stellar account/wallet.

Keystone should not require custody of investor private keys.

The wallet should clearly communicate:

* asset;
* quantity;
* payment;
* fees;
* destination;
* transaction intent where supported.

## 7. Settlement Atomicity

Where technically practical, payment and ownership transfer should settle atomically.

The preferred outcome is:

`Payment Success + Ownership Success`

rather than allowing:

`Payment Success + Ownership Failure`

or:

`Ownership Success + Payment Failure`

If atomic settlement is impossible for a particular architecture, the protocol must explicitly define recovery and reconciliation behavior.

## 8. Pricing

Pricing must be explicit.

The system should distinguish:

* current price;
* quoted price;
* historical price;
* indicative price;
* projected value.

Projected or estimated values must not be presented as guaranteed returns.

## 9. Transaction States

The backend may represent transactions using states such as:

`CREATED → AWAITING_SIGNATURE → SUBMITTED → CONFIRMED`

Failure states include:

* `REJECTED`;
* `FAILED`;
* `EXPIRED`;
* `CANCELLED`.

The exact state machine may be refined during implementation.

## 10. Idempotency

Investment processing must be resilient to:

* duplicate requests;
* repeated wallet submissions;
* webhook retries;
* indexer retries;
* client refreshes;
* network interruptions.

The same successful transaction must never result in duplicate ownership.

## 11. Supply

The protocol must enforce configured supply.

An investment must not create ownership beyond the asset's available supply.

## 12. Fees

Fees must be explicit.

Where applicable, users should be able to identify:

* asset price;
* protocol fee;
* issuer fee;
* network fee;
* total amount.

Fee calculation must not depend solely on frontend logic.

## 13. Refunds and Failed Settlement

A failed investment must not result in ownership.

If funds can be transferred before final ownership settlement, the protocol must define a deterministic recovery/refund mechanism.

## 14. Backend Reconciliation

The backend must reconcile:

* requested investment;
* submitted transaction;
* ledger confirmation;
* ownership event;
* payment event.

A database record must never override authoritative ledger state.

## 15. Frontend Requirements

The frontend must clearly distinguish:

* transaction preparation;
* awaiting wallet signature;
* wallet rejection;
* transaction submission;
* transaction confirmation;
* transaction failure.

Users must never see "Investment successful" merely because a wallet transaction was submitted.

## 16. Core Invariants

* Failed settlement creates no unintended ownership.
* Ownership cannot exceed supply.
* Unauthorized users cannot purchase restricted assets.
* Duplicate requests cannot produce duplicate settlement.
* Backend records cannot create ownership.
* Every settled investment is attributable to an authoritative transaction.

## 17. Security Considerations

Implementation must consider:

* price manipulation;
* stale quotes;
* replay;
* duplicate settlement;
* unauthorized purchasing;
* compromised issuer/admin accounts;
* insufficient payment;
* transaction ordering;
* partial failure;
* wallet/account changes.

## 18. Deferred Decisions

The following require implementation specifications:

* payment asset;
* Stellar Asset Contract integration;
* exact Soroban contract call sequence;
* pricing/oracle mechanism;
* escrow model;
* fees;
* refunds;
* minimum/maximum investment;
* offering windows;
* issuer funding model.

## 19. Acceptance Criteria

RFC-005 is satisfied when Keystone can define a deterministic investment lifecycle where payment, ownership, authorization, failure, and reconciliation responsibilities are unambiguous.

## 20. Final Principle

**A transaction is not successful because the user clicked "Invest"; it is successful only when the protocol settles it.**
