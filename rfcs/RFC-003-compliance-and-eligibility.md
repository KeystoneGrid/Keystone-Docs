# RFC-003: Compliance & Eligibility

**Status:** DRAFT
**Category:** Compliance / Architecture
**Target:** KeystoneGrid v1

## 1. Summary

This RFC defines the compliance and eligibility boundary between KeystoneGrid's application services and its on-chain protocol.

Compliance must be explicit, auditable, privacy-conscious, and configurable per asset and jurisdiction.

## 2. Principles

Keystone must not assume that every investor or asset follows one universal compliance model.

Eligibility may depend on:

* identity verification;
* jurisdiction;
* investor category;
* asset type;
* offering restrictions;
* sanctions screening;
* accreditation;
* transaction limits.

## 3. Identity vs Eligibility

Identity verification and transaction eligibility are separate concepts.

A user may have a verified identity but still be ineligible for a particular asset.

Therefore:

`Identity ≠ Eligibility`

## 4. Eligibility States

A conceptual eligibility lifecycle is:

`UNKNOWN → PENDING → ELIGIBLE / INELIGIBLE / EXPIRED`

Eligibility records should contain:

* subject;
* asset/scope;
* status;
* evaluated timestamp;
* expiry timestamp;
* provider/reference;
* audit metadata.

## 5. Provider Abstraction

External compliance providers must be accessed through an abstraction layer.

Conceptually:

`Investor → Keystone Compliance Service → Provider(s) → Eligibility Result`

Core protocol logic must not become tightly coupled to a single compliance vendor.

## 6. Privacy

Personally identifiable information and sensitive verification documents should remain off-chain.

The blockchain should contain only the minimum state required for protocol enforcement.

Never store:

* passports;
* identity documents;
* residential addresses;
* detailed KYC records;
* private compliance notes

on-chain.

## 7. Protocol Enforcement

Where eligibility is a protocol requirement, it must be enforceable at transaction time.

The frontend may hide unavailable actions.

The backend may perform compliance checks.

Neither is sufficient if the protocol itself must prevent an invalid transaction.

## 8. Transfers

Compliance requirements apply not only to purchases but potentially to transfers.

For a restricted asset:

`Sender → Transfer Request → Recipient Eligibility → Protocol Authorization → Transfer`

## 9. Expiration

An eligibility result may expire.

Expired eligibility must not be silently treated as current eligibility.

The system should support re-evaluation without unnecessarily repeating unrelated verification processes.

## 10. Administrative Actions

Compliance administrators must be authorized.

Changes to eligibility affecting protocol behavior should be auditable.

## 11. Backend Requirements

The backend must provide:

* compliance workflow;
* provider integration;
* eligibility records;
* audit trail;
* webhook processing;
* expiry handling;
* access control;
* privacy controls.

Webhook processing must be idempotent.

## 12. Frontend Requirements

The frontend should communicate:

* verification status;
* eligibility status;
* unavailable actions;
* required next steps;
* expiration where relevant.

Sensitive compliance information must not be unnecessarily exposed.

## 13. Security Requirements

The system must:

* protect provider credentials;
* minimize sensitive logging;
* restrict administrative access;
* validate provider callbacks;
* prevent replayed webhook events;
* audit eligibility changes.

## 14. Invariants

* Ineligible accounts cannot perform restricted operations.
* Expired eligibility is not current eligibility.
* Backend records cannot bypass protocol restrictions.
* Sensitive compliance data is not required on-chain.

## 15. Deferred Decisions

Future policy documents must define:

* jurisdictions;
* accepted compliance providers;
* investor classifications;
* accreditation rules;
* sanctions requirements;
* retention periods;
* legal review requirements.

## 16. Acceptance Criteria

RFC-003 is satisfied when Keystone has a provider-independent eligibility architecture, privacy boundary, protocol enforcement model, audit model, and transfer-compliance model.

## 17. Final Principle

**Compliance information belongs primarily off-chain; compliance consequences that affect protocol safety must be enforceable on-chain.**
