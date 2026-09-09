# RFC-007: Asset Token Standard

**Status:** DRAFT
**Category:** Protocol / Tokenization
**Target:** KeystoneGrid v1

## 1. Summary

This RFC defines the behavioral requirements for the digital representation of a Keystone Asset.

It intentionally does not prematurely lock KeystoneGrid to one implementation mechanism.

The implementation must be selected based on protocol requirements rather than familiarity with a particular token standard.

## 2. Tokenization Principle

Tokenization represents a protocol-defined economic or ownership interest.

It does not independently establish:

* legal title;
* asset existence;
* valuation;
* regulatory status.

Those claims depend on the verification and legal framework defined elsewhere.

## 3. Required Properties

The canonical asset representation must support, where applicable:

* unique asset association;
* deterministic ownership;
* bounded supply;
* controlled issuance;
* defined precision;
* authorization;
* transfer rules;
* compliance restrictions;
* lifecycle integration;
* observable events;
* distribution compatibility.

## 4. Candidate Models

Keystone may evaluate:

### Model A — Stellar-Issued Asset

Use Stellar's native issued-asset infrastructure as the ownership/payment representation.

### Model B — Soroban-Managed Ownership

Represent balances and ownership directly inside Soroban contract storage.

### Model C — Soroban + Stellar Asset Contract

Use Soroban protocol logic together with Stellar Asset Contract functionality.

### Model D — Hybrid Architecture

Use different Stellar-native mechanisms for different asset or protocol requirements.

No model is accepted solely because it is technically possible.

## 5. Selection Criteria

The final mechanism must be evaluated against:

* compliance requirements;
* transfer restrictions;
* fractional ownership;
* wallet compatibility;
* asset lifecycle;
* distribution accounting;
* secondary markets;
* interoperability;
* upgradeability;
* security;
* operational complexity;
* auditability.

## 6. Supply

Supply must be explicit and bounded.

Unauthorized issuance must be impossible.

Where additional issuance is permitted, the conditions must be protocol-defined and auditable.

## 7. Precision

The asset representation must define its smallest ownership unit.

For example:

`1 Asset Unit = 0.000001 ownership interest`

The exact precision must be determined per implementation.

## 8. Issuance

Issuance must require appropriate authorization.

The protocol must distinguish:

* asset registration;
* token creation;
* token issuance;
* investor acquisition.

These are separate lifecycle operations.

## 9. Transferability

Transferability must be explicitly defined.

An asset may be:

* freely transferable;
* eligibility-restricted;
* issuer-controlled;
* temporarily frozen;
* permanently non-transferable.

The selected behavior must be enforced by the protocol where necessary.

## 10. Compliance

If an asset is compliance-restricted, token transfers must respect the applicable eligibility rules.

A UI restriction is not sufficient.

## 11. Lifecycle Integration

Token behavior must correspond to the asset lifecycle.

For example:

* unregistered → no valid ownership representation;
* registered → representation may be prepared;
* tokenized → issuance exists;
* active → normal protocol operations permitted;
* suspended → operations restricted according to policy;
* retired → no unauthorized new issuance.

## 12. Metadata

Large metadata should remain off-chain.

The protocol representation should contain or reference a stable asset identity.

Where useful, Keystone may use cryptographic commitments to establish metadata integrity.

## 13. Wallet Interoperability

Keystone should prefer mechanisms that work naturally with Stellar-compatible wallets and ecosystem tooling.

Custom mechanisms must provide a clear reason for their additional complexity.

## 14. Events

Economically meaningful operations should be observable.

Depending on implementation, this includes:

* issuance;
* transfer;
* redemption;
* suspension;
* activation;
* retirement.

## 15. Core Invariants

* Unauthorized issuance is impossible.
* Supply limits cannot be bypassed.
* Ownership mutations are attributable to protocol operations.
* Retired assets cannot be silently reissued.
* Transfer restrictions cannot be bypassed through the frontend.
* Token state remains consistent with protocol asset state.

## 16. Security Considerations

The final implementation must consider:

* issuer compromise;
* administrative key compromise;
* unauthorized minting;
* unauthorized transfer;
* frozen accounts;
* recovery mechanisms;
* upgrade authority;
* contract bugs;
* interoperability assumptions.

## 17. Decision Process

Before production implementation, maintainers should produce a technical decision record comparing the candidate mechanisms.

The selected mechanism must be justified against the actual Keystone requirements.

## 18. Deferred Decisions

The following remain open:

* canonical representation;
* precision;
* issuance authority;
* transfer architecture;
* compliance enforcement;
* metadata commitment;
* upgrade model;
* recovery model.

## 19. Acceptance Criteria

RFC-007 is satisfied when Keystone has selected and documented one canonical asset representation that satisfies the ownership, compliance, transfer, distribution, and interoperability requirements established by the preceding RFCs.

## 20. Final Principle

**KeystoneGrid should choose the token mechanism that best satisfies its protocol requirements—not force the protocol to fit a token standard.**
