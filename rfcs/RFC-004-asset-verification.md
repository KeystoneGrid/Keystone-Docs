# RFC-004: Asset Verification

**Status:** DRAFT
**Category:** Verification / Architecture
**Target:** KeystoneGrid v1

## 1. Summary

This RFC defines how KeystoneGrid establishes confidence that a proposed real-world asset satisfies the requirements for registration and tokenization.

Tokenization itself is never considered proof of asset existence, ownership, valuation, or legal status.

## 2. Verification Principle

An asset claim must be distinguishable from verified evidence.

Keystone therefore separates:

* asset submission;
* review;
* verification;
* approval;
* registration;
* tokenization.

## 3. Verification Categories

Depending on asset class, verification may include:

* issuer identity;
* asset existence;
* legal ownership;
* title;
* valuation;
* physical inspection;
* legal status;
* financial records;
* supporting documentation.

Not every asset requires every category.

## 4. Verification Record

A verification record should include:

* asset;
* verification type;
* status;
* verifier/provider;
* evidence reference;
* creation time;
* verification time;
* expiration;
* audit information.

## 5. Verification Lifecycle

`NOT_STARTED → PENDING → VERIFIED / FAILED → EXPIRED`

A failed verification means that the defined verification requirement was not satisfied. It does not independently establish fraud.

## 6. Evidence

Evidence should generally remain off-chain.

Protected storage may contain:

* legal documents;
* valuation reports;
* inspection reports;
* ownership documents;
* financial records.

Where appropriate, Keystone may store a cryptographic commitment/hash so that document integrity can later be demonstrated.

## 7. Verification Authority

Verification decisions must be attributable to an authorized:

* Keystone verifier;
* approved third party;
* compliance/legal service;
* specialized verification provider.

The system must not represent unreviewed issuer claims as verified facts.

## 8. Asset Approval

An asset should not become protocol-active solely because an issuer submitted metadata.

Approval requires satisfying the asset-specific verification policy.

## 9. On-Chain Representation

The protocol may store compact verification state or commitments where required.

It must not store private evidence documents.

## 10. Expiration

Verification may become stale.

Where a verification has an expiry policy, an expired verification must not be displayed as current.

## 11. Backend Requirements

The backend is responsible for:

* verification workflows;
* evidence management;
* verifier management;
* review queues;
* expiry tracking;
* audit records;
* verification-provider integration.

## 12. Frontend Requirements

Users should clearly see:

* what has been verified;
* what remains under review;
* when verification occurred;
* whether verification has expired;
* what verification does and does not establish.

## 13. Security

The system must protect verification evidence and prevent unauthorized modification of verification records.

Administrative actions affecting verification must be auditable.

## 14. Invariants

* Verification has a defined scope.
* Verification cannot silently change.
* Expired verification is not current verification.
* Tokenization does not independently prove the underlying asset.
* Backend verification does not create ownership.

## 15. Deferred Decisions

Future policy specifications must define:

* verification thresholds;
* verifier qualifications;
* asset-specific requirements;
* evidence retention;
* jurisdictional requirements;
* re-verification frequency.

## 16. Acceptance Criteria

RFC-004 is satisfied when Keystone can distinguish submitted assets from verified assets and maintain an auditable verification lifecycle without putting sensitive evidence on-chain.

## 17. Final Principle

**KeystoneGrid should never ask the blockchain to prove something that only real-world evidence can establish.**
