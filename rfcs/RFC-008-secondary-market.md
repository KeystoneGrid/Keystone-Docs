# RFC-008: Secondary Market

**Status:** DRAFT
**Category:** Protocol / Market Infrastructure
**Target:** KeystoneGrid v1

## 1. Summary

This RFC establishes the requirements and architectural boundaries for future secondary trading of Keystone Asset ownership interests.

Tokenization alone does not create liquidity.

A secondary market requires explicit mechanisms for:

* sellers;
* buyers;
* pricing;
* eligibility;
* matching;
* settlement;
* ownership transfer.

## 2. Core Principle

Secondary trading must not bypass the ownership, compliance, and settlement rules established by KeystoneGrid.

A secondary transaction is complete only when ownership is successfully transferred through the authoritative protocol.

## 3. Candidate Market Models

Keystone may eventually support:

### Peer-to-Peer Transfers

A holder directly transfers ownership to another eligible holder.

### Order Book

Buyers and sellers submit orders that are matched.

### Request for Quote

A buyer requests a price from one or more sellers or market participants.

### Issuer-Facilitated Matching

The issuer or authorized operator facilitates matching without necessarily operating a conventional exchange.

### Automated Market Mechanism

A protocol-controlled mechanism determines prices and executes trades according to predefined rules.

No model is selected by this RFC.

## 4. Trade Lifecycle

A conceptual restricted-market lifecycle is:

`LISTED → MATCHED → ELIGIBILITY_CHECK → SIGNED → SETTLED → OWNERSHIP_UPDATED`

Failure states may include:

* rejected;
* expired;
* cancelled;
* failed;
* unavailable.

## 5. Eligibility

A buyer may require eligibility before completing a secondary purchase.

Eligibility requirements may include:

* identity verification;
* jurisdiction;
* asset-specific rules;
* investor classification;
* transaction limits.

The requirements must remain compatible with RFC-003.

## 6. Seller Requirements

A seller must own the quantity being offered.

The protocol must prevent:

* selling more than owned;
* selling already committed units;
* double-selling;
* unauthorized transfer.

## 7. Settlement

Secondary settlement should be atomic where technically practical.

The preferred outcome is:

`Payment ↔ Ownership Transfer`

rather than allowing one side to complete without the other.

## 8. Pricing

The system must distinguish:

* listing price;
* bid price;
* executed price;
* indicative price;
* historical price.

Frontend displays must not represent indicative prices as guaranteed execution prices.

## 9. Orders

If an order-book model is selected, orders should have explicit:

* owner;
* asset;
* quantity;
* price;
* expiration;
* status;
* authorization.

Stale orders must not remain executable indefinitely.

## 10. Fees

Fees must be transparent.

Potential fees include:

* protocol fee;
* issuer fee;
* marketplace fee;
* network fee.

The final fee architecture requires a separate implementation specification.

## 11. Liquidity

Keystone must not claim that tokenization guarantees liquidity.

Market depth depends on:

* buyer demand;
* seller supply;
* asset quality;
* pricing;
* regulatory restrictions;
* market participation.

## 12. Backend Responsibilities

The backend may provide:

* market discovery;
* order indexing;
* search;
* price history;
* notifications;
* analytics;
* market metadata.

Backend market records are not authoritative ownership records.

## 13. Frontend Requirements

The frontend should communicate:

* availability;
* market status;
* price;
* quantity;
* eligibility;
* fees;
* settlement state.

Users must understand whether an order is:

* open;
* matched;
* pending;
* executed;
* cancelled;
* expired.

## 14. Market Controls

Implementation should consider:

* spam orders;
* stale orders;
* duplicate orders;
* spoofing;
* unauthorized transfers;
* compromised accounts;
* price manipulation;
* failed settlement;
* emergency suspension.

## 15. Emergency Controls

Restricted assets may require emergency market controls.

Potential controls include:

* pause;
* transfer suspension;
* order cancellation;
* issuer intervention;
* protocol emergency mode.

Emergency powers must be narrowly scoped and auditable.

## 16. Core Invariants

* A seller cannot sell more than they own.
* An ineligible buyer cannot complete a restricted purchase.
* A failed trade does not mutate ownership.
* A settled trade has one authoritative ownership outcome.
* Duplicate settlement cannot occur.
* Backend market records cannot override protocol ownership.

## 17. Security Considerations

The implementation must evaluate:

* order replay;
* signature replay;
* front-running;
* stale eligibility;
* compromised market operators;
* malicious issuers;
* price manipulation;
* denial-of-service;
* settlement race conditions.

## 18. Deferred Decisions

Future specifications must define:

* market architecture;
* order model;
* matching mechanism;
* escrow;
* settlement sequence;
* fees;
* dispute handling;
* market-maker model;
* emergency procedures;
* legal/regulatory requirements.

## 19. Acceptance Criteria

RFC-008 is satisfied when Keystone has a clear framework for future secondary trading without assuming liquidity, bypassing compliance, or weakening authoritative ownership.

## 20. Final Principle

**Liquidity is a market property, not a token property.**

KeystoneGrid must earn secondary-market liquidity through useful assets, credible verification, compliant participation, transparent pricing, and reliable settlement.
