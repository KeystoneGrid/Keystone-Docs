# RFC-002: Ownership Model

**Status:** DRAFT
**Category:** Protocol / Core Architecture
**Target:** KeystoneGrid v1

## 1. Summary

This RFC defines what an investor owns when participating in a Keystone Asset and establishes ownership as authoritative protocol state rather than an application database record.

The ownership model must support fractional participation, compliance-aware transfers, deterministic accounting, wallet-based control, and future secondary-market functionality.

## 2. Ownership Principle

KeystoneGrid must make ownership independently verifiable from Stellar.

The backend may index and enrich ownership information but must never become the authoritative source of:

* ownership;
* balances;
* supply;
* transfers;
* settlement outcomes; or
* distribution entitlement where the protocol defines it.

## 3. Ownership Layers

Keystone distinguishes:

1. **Underlying Asset** — the real-world asset.
2. **Keystone Asset** — the protocol representation of that real-world asset.
3. **Digital Ownership Interest** — the units representing an investor's economic/protocol interest.
4. **Holder** — the Stellar account controlling those units.

Holding a digital ownership interest does not automatically mean that the holder possesses unrestricted legal title to the underlying physical asset.

The legal rights associated with an interest must be explicitly defined by the asset's legal structure and offering documentation.

## 4. Fractional Ownership

A Keystone Asset may be divided into a fixed number of ownership units.

Each asset should define:

* total supply;
* unit precision;
* initial issuance;
* ownership representation;
* transfer rules;
* eligibility requirements.

Ownership percentage should be calculated from:

`holder_units / total_outstanding_units`

rather than stored as an independent authoritative value.

## 5. Ownership Lifecycle

Ownership may be created through:

* authorized initial issuance;
* approved investment settlement; or
* another explicitly authorized protocol mechanism.

Ownership may be destroyed through:

* authorized redemption;
* retirement;
* cancellation; or
* another protocol-defined mechanism.

Every ownership mutation must be attributable to an on-chain operation.

## 6. Wallet-Based Control

The investor's Stellar account controls their ownership interest.

Keystone must not require private-key custody for ordinary investor operations.

Application accounts may provide profiles, preferences, notifications, and compliance information, but they do not replace the wallet as the authority over protocol ownership.

## 7. Transfers

A transfer is valid only when:

1. the sender owns the units;
2. the asset permits transfers;
3. the recipient satisfies applicable eligibility requirements;
4. all protocol restrictions are satisfied; and
5. the transaction is successfully authorized and committed.

Frontend restrictions are never sufficient to enforce ownership rules.

## 8. Compliance-Aware Ownership

A valid primary acquisition does not imply unrestricted future transferability.

Depending on the asset, a recipient may need to satisfy:

* KYC requirements;
* jurisdiction restrictions;
* accreditation requirements;
* asset-specific eligibility;
* sanctions screening; or
* other applicable rules.

The exact compliance model is defined by RFC-003.

## 9. Ownership and Distributions

Where an asset distributes revenue or other economic benefits, entitlement must be derived from the protocol-defined ownership basis.

The ownership model does not prescribe the complete distribution mechanism. That is defined by RFC-006.

## 10. Backend Responsibilities

The backend may index:

* holder;
* asset;
* quantity;
* transaction;
* ledger;
* ownership events;
* historical balances.

The backend must treat these records as derived data.

Reconciliation must be possible against Stellar.

## 11. Frontend Requirements

The frontend should display:

* asset;
* units held;
* total supply;
* ownership percentage;
* wallet/account;
* transfer eligibility;
* relevant restrictions.

Users should be able to distinguish current on-chain ownership from cached/indexed application data.

## 12. Core Invariants

The protocol must guarantee:

* no negative balances;
* no unauthorized issuance;
* no unauthorized transfer;
* no transfer above available balance;
* supply limits are respected;
* backend records cannot create ownership;
* every ownership mutation has an authoritative transaction.

## 13. Security Considerations

Implementation must consider:

* unauthorized minting;
* transfer bypass;
* compromised administrative accounts;
* stale compliance state;
* replayed transactions;
* duplicate settlement;
* wallet/account changes;
* lost wallet access;
* emergency asset suspension.

## 14. Deferred Decisions

The following remain implementation decisions:

* Stellar-issued asset versus Soroban-managed balance;
* exact token standard;
* decimals;
* permissioning mechanism;
* transfer authorization mechanism;
* clawback/forced-transfer policy;
* legal ownership structure;
* secondary-market integration.

These decisions must be resolved before production contract implementation.

## 15. Acceptance Criteria

RFC-002 is considered satisfied when:

* ownership is clearly separated from legal title;
* fractional ownership is defined;
* wallet authority is established;
* transfer requirements are defined;
* backend authority is explicitly limited;
* distribution ownership basis is established;
* token implementation choices are documented.

## 16. Final Principle

**If ownership cannot be independently verified from the protocol, KeystoneGrid does not have authoritative ownership.**
