# RFC-001: Asset Model & Lifecycle

**Status:** DRAFT
**Category:** Protocol / Architecture
**Authors:** KeystoneGrid Core Team
**Created:** 2026-09-06
**Target:** KeystoneGrid v1

---

## 1. Summary

This RFC defines the conceptual model and lifecycle of a **KeystoneGrid Asset**.

It establishes:

* what constitutes an asset
* how an asset is identified
* the relationship between an asset and its issuer
* how assets move through verification and approval
* when an asset may be tokenized
* which state belongs on-chain
* which state belongs off-chain
* how asset suspension and retirement work
* how the backend, frontend, and Soroban protocol interact around an asset

This RFC is foundational to the KeystoneGrid architecture.

Future contract, backend, and frontend specifications should reference this document rather than independently redefining asset behavior.

---

# 2. Motivation

Real-world assets are complex.

A property, for example, may have:

* a legal owner
* a physical location
* valuation information
* legal documentation
* inspection records
* photographs
* operating information
* financial information
* regulatory requirements
* an issuer responsible for bringing it to the platform

A blockchain cannot independently establish all of these facts.

At the same time, simply storing asset information in a centralized database does not provide the ownership and settlement guarantees KeystoneGrid is intended to provide.

KeystoneGrid therefore separates the system into two layers:

```text
REAL-WORLD / APPLICATION LAYER
        │
        │ verification
        │ metadata
        │ documents
        │ compliance
        ▼
┌──────────────────────────┐
│      Keystone Backend    │
└────────────┬─────────────┘
             │
             │ protocol state
             ▼
┌──────────────────────────┐
│    Keystone Soroban      │
│        Contract          │
└────────────┬─────────────┘
             │
             ▼
          STELLAR
```

The purpose of this RFC is to define the boundary between these layers.

---

# 3. Design Principles

The Asset Model follows these principles.

## 3.1 Blockchain Is the Authority for On-Chain State

The blockchain is authoritative for protocol state explicitly enforced by the Soroban contracts.

This includes things such as:

* tokenized ownership
* on-chain balances
* protocol authorization
* asset registration
* investment state where implemented on-chain
* settlement state
* distribution accounting where implemented on-chain

The backend may index this information but must not redefine it.

---

## 3.2 Verification Is Explicit

An asset being registered in a database does not mean that the underlying real-world asset has been verified.

The system must distinguish:

```text
Registered
     ≠
Verified
     ≠
Approved
     ≠
Tokenized
     ≠
Active
```

---

## 3.3 Asset Metadata Is Not Automatically Protocol State

Not every piece of information associated with an asset belongs on-chain.

Large or frequently changing information should generally remain off-chain.

Examples include:

* photographs
* long descriptions
* legal documents
* valuation reports
* inspection reports
* application notes

The protocol should store only the information necessary to enforce protocol rules and establish stable references.

---

## 3.4 Lifecycle Transitions Must Be Explicit

Assets should not move between important states implicitly.

Every meaningful state transition should have:

* an authorized actor
* a defined precondition
* a resulting state
* an audit trail

Where a transition affects protocol state, it should be enforced by the Soroban contract.

---

## 3.5 Asset Identity Must Be Stable

An asset should have a stable protocol identifier that remains associated with the asset throughout its lifecycle.

Changing descriptive metadata must not silently create a new asset.

---

# 4. Definition of an Asset

A **Keystone Asset** is a real-world economic asset represented within KeystoneGrid through a combination of:

1. real-world identity
2. issuer information
3. verification information
4. application metadata
5. protocol registration
6. on-chain ownership representation

For the initial version of KeystoneGrid, real estate may be the first supported asset category.

However, the core asset model should not unnecessarily hard-code the protocol around real estate.

The protocol should be capable of supporting additional asset classes in the future.

---

# 5. Asset Identity

Each asset requires a unique identifier.

Conceptually:

```text
Asset
 ├── asset_id
 ├── asset_type
 ├── issuer
 ├── metadata_reference
 └── protocol_state
```

The `asset_id` must be stable and unique within the Keystone protocol.

The backend may maintain its own database identifier, but that identifier must not replace the protocol asset identifier.

---

# 6. Asset Types

Assets should have an explicit asset type.

Initial examples may include:

```text
REAL_ESTATE
```

Future types could include:

```text
REAL_ESTATE
INVOICE
EQUIPMENT
COMMODITY
REVENUE_STREAM
SECURITY
OTHER
```

Additional asset classes must be introduced through documented protocol changes.

The frontend must not assume that every asset behaves identically.

---

# 7. Issuer

Every tokenized asset must have an associated issuer or responsible entity.

Conceptually:

```text
Issuer
   │
   └── Asset
```

An issuer is responsible for providing information and evidence relating to the asset.

However:

> Being listed as an issuer does not automatically mean that the issuer is trustworthy or legally verified.

Issuer verification is therefore a separate concern.

---

# 8. Asset Metadata

Asset metadata contains information describing the real-world asset.

Potential metadata:

```text
name
description
asset_type
location
jurisdiction
valuation
currency
images
documents
financial_information
risk_information
```

Not all metadata should be stored on-chain.

---

# 9. On-Chain vs Off-Chain Data

The system should explicitly classify data according to its authority.

| Data                     | Primary Authority |
| ------------------------ | ----------------- |
| Asset protocol ID        | Soroban           |
| Tokenized ownership      | Stellar/Soroban   |
| On-chain balance         | Stellar/Soroban   |
| Protocol asset status    | Soroban           |
| Contract authorization   | Soroban           |
| Transaction execution    | Stellar           |
| Asset description        | Backend           |
| Property photographs     | Backend/storage   |
| Legal documents          | Backend/storage   |
| Verification evidence    | Backend           |
| Compliance records       | Backend/provider  |
| Valuation documentation  | Backend/provider  |
| Indexed blockchain state | Backend cache     |

The backend must clearly mark blockchain-derived values as indexed or derived data.

---

# 10. Asset Lifecycle

The complete asset lifecycle is divided into two conceptual layers.

## Application Lifecycle

```text
DRAFT
  ↓
SUBMITTED
  ↓
UNDER_REVIEW
  ↓
VERIFICATION
  ↓
APPROVED
```

## Protocol Lifecycle

```text
APPROVED
  ↓
REGISTERED
  ↓
TOKENIZED
  ↓
ACTIVE
  ↓
SUSPENDED
  ↓
ACTIVE
  ↓
RETIRED
```

These are conceptual states.

The exact implementation of each state must be defined by the relevant technical specifications.

---

# 11. Application States

Application states are primarily backend workflow states.

## DRAFT

The issuer has started an application but has not submitted it.

The asset is not publicly investable.

---

## SUBMITTED

The issuer has submitted the application for review.

The asset is not yet verified.

---

## UNDER_REVIEW

Authorized reviewers are evaluating the application.

---

## VERIFICATION

The asset and/or issuer is undergoing verification.

Possible verification areas include:

* ownership
* legal documentation
* valuation
* physical existence
* inspection
* issuer identity

---

## APPROVED

The asset has passed the required application and verification workflow.

Approval does not necessarily mean the asset is already tokenized.

---

# 12. Protocol States

Protocol states represent states that affect blockchain behavior.

## REGISTERED

The asset has been formally registered with the Keystone protocol.

Registration establishes the protocol identity of the asset.

---

## TOKENIZED

The asset's ownership/investment representation has been created according to the protocol.

The exact tokenization mechanism will be defined in a separate RFC.

---

## ACTIVE

The asset is available for the protocol operations permitted by its configuration.

For example, an active asset may allow:

* ownership transfers
* investment
* distributions
* other supported operations

The exact permitted actions depend on the asset configuration.

---

## SUSPENDED

An asset may be suspended when continued protocol activity should temporarily stop.

Potential reasons include:

* verification concerns
* legal concerns
* issuer issues
* compliance issues
* security incidents
* operational incidents

Suspension rules must be enforced at the protocol layer where they affect on-chain actions.

---

## RETIRED

A retired asset is permanently removed from normal active protocol operation.

Retirement should be treated as a terminal lifecycle state unless a future RFC explicitly defines a recovery mechanism.

---

# 13. State Machine

The conceptual state machine is:

```text
                         ┌──────────────┐
                         │    DRAFT     │
                         └──────┬───────┘
                                │ submit
                                ▼
                         ┌──────────────┐
                         │   SUBMITTED  │
                         └──────┬───────┘
                                │
                                ▼
                         ┌──────────────┐
                         │ UNDER_REVIEW │
                         └──────┬───────┘
                                │
                                ▼
                         ┌──────────────┐
                         │ VERIFICATION │
                         └──────┬───────┘
                                │
                         verification
                           succeeds
                                │
                                ▼
                         ┌──────────────┐
                         │   APPROVED   │
                         └──────┬───────┘
                                │
                                ▼
                         ┌──────────────┐
                         │  REGISTERED  │
                         └──────┬───────┘
                                │
                                ▼
                         ┌──────────────┐
                         │  TOKENIZED   │
                         └──────┬───────┘
                                │
                                ▼
                         ┌──────────────┐
                         │    ACTIVE    │
                         └──────┬───────┘
                                │
                    ┌───────────┴───────────┐
                    │                       │
                    ▼                       ▼
             ┌──────────────┐       ┌──────────────┐
             │  SUSPENDED   │       │    RETIRED   │
             └──────┬───────┘       └──────────────┘
                    │
                    │ restore
                    ▼
               ┌──────────┐
               │  ACTIVE  │
               └──────────┘
```

---

# 14. State Authority

Not every state should be stored in the same place.

| State        | Authority                              |
| ------------ | -------------------------------------- |
| DRAFT        | Backend                                |
| SUBMITTED    | Backend                                |
| UNDER_REVIEW | Backend                                |
| VERIFICATION | Backend                                |
| APPROVED     | Backend + protocol authorization input |
| REGISTERED   | Soroban                                |
| TOKENIZED    | Soroban                                |
| ACTIVE       | Soroban                                |
| SUSPENDED    | Soroban                                |
| RETIRED      | Soroban                                |

The important principle is that once a state becomes protocol-critical, it should no longer depend solely on an application database.

---

# 15. Verification and Protocol Registration

An asset should not become tokenized merely because a backend record has:

```text
verification_status = VERIFIED
```

A controlled protocol transition must occur.

Conceptually:

```text
Backend Verification
        │
        ▼
Approval
        │
        ▼
Authorized Registration
        │
        ▼
Soroban
        │
        ▼
Registered Asset
```

The exact authorization mechanism will be specified separately.

---

# 16. Verification Expiration

Some verification results may have an expiration period.

Examples:

* valuation
* inspection
* compliance status
* legal documents

Therefore verification should support:

```text
verified_at
expires_at
status
```

An expired verification does not necessarily mean that the asset is fraudulent.

It means the verification requirement must be reassessed according to platform policy.

---

# 17. Asset Suspension

Suspension is different from retirement.

### Suspension

Temporary restriction.

```text
ACTIVE
  ↓
SUSPENDED
  ↓
ACTIVE
```

### Retirement

Permanent lifecycle conclusion.

```text
ACTIVE
  ↓
RETIRED
```

Suspension and retirement should not be implemented as arbitrary backend flags when they affect on-chain behavior.

---

# 18. Asset Metadata Updates

Metadata may change during the asset lifecycle.

For example:

* property description
* photographs
* financial information
* documents
* valuation

Changes should be tracked.

Where metadata affects a protocol-critical rule, the update must be reflected in the protocol through an authorized transaction.

---

# 19. Metadata Integrity

The backend may store detailed metadata off-chain.

Where appropriate, the protocol may store a cryptographic commitment/reference to important metadata.

Conceptually:

```text
Metadata
    │
    ▼
Canonical Representation
    │
    ▼
Hash / Commitment
    │
    ▼
Soroban
```

This allows the system to detect whether referenced metadata has changed.

The exact commitment mechanism will be specified separately.

---

# 20. Asset Ownership

Ownership is one of the most important distinctions in KeystoneGrid.

The database may display:

```text
Investor A → 25%
```

but that information should ultimately derive from authoritative protocol state.

The backend must not independently create an ownership record and declare it authoritative.

A future ownership RFC will define:

* ownership representation
* token standard
* fractional ownership
* transfers
* restrictions
* eligibility
* secondary transfers

---

# 21. Asset Economics

This RFC does not define:

* investment pricing
* yield calculations
* revenue distribution
* fees
* payment currencies
* secondary-market pricing

These should be defined in separate RFCs.

The Asset Model only establishes that economic configuration may be associated with an asset.

---

# 22. Asset Documents

Documents associated with an asset remain off-chain by default.

Examples:

* title documents
* valuation reports
* inspection reports
* legal agreements
* certificates

The protocol should not attempt to store large documents directly.

Instead, the backend should maintain:

```text
Document
 ├── identifier
 ├── storage reference
 ├── content hash
 ├── document type
 ├── verification state
 └── timestamps
```

---

# 23. Asset Discovery

Only assets that satisfy the required publication criteria should appear as investable assets.

For example:

```text
DRAFT
        → not public

SUBMITTED
        → not investable

VERIFICATION
        → not investable

APPROVED
        → potentially publishable

TOKENIZED
        → potentially investable

ACTIVE
        → eligible for supported operations
```

The frontend must not infer investability from the presence of an asset record alone.

---

# 24. Backend Requirements

The backend implementing this RFC must provide:

### Asset management

* asset creation
* asset metadata
* lifecycle tracking
* issuer association
* verification association

### Verification

* verification records
* evidence references
* verification status
* expiration tracking

### Blockchain integration

* protocol asset ID mapping
* contract event indexing
* state synchronization
* transaction tracking
* reconciliation

### Auditability

* lifecycle history
* administrative actions
* verification changes
* protocol transaction references

---

# 25. Frontend Requirements

The frontend must:

* clearly display asset state
* distinguish verification from approval
* distinguish approval from tokenization
* distinguish tokenization from active status
* display relevant verification information
* display asset metadata
* clearly communicate transaction state
* never present cached backend state as unquestionable truth

---

# 26. Contract Requirements

The Soroban implementation derived from this RFC must provide a mechanism for:

* unique asset identification
* issuer association
* protocol registration
* lifecycle state
* authorized state transitions
* suspension
* retirement
* appropriate event emission

The contract should not attempt to store the entire off-chain asset record.

---

# 27. Required Events

The final contract specification should define events for important lifecycle transitions.

Conceptually:

```text
asset_registered
asset_tokenized
asset_activated
asset_suspended
asset_resumed
asset_retired
```

Events should contain enough information for the indexer to reconstruct protocol state.

Exact event topics and payloads belong in the contract specification.

---

# 28. Failure Scenarios

The implementation must consider failure between the backend and blockchain.

Example:

```text
Backend approves asset
        ↓
Blockchain registration attempted
        ↓
Transaction fails
```

The backend must not mark the asset as successfully registered merely because the transaction was submitted.

Correct flow:

```text
Submitted
   ↓
Pending
   ↓
Confirmed
   ↓
Registered
```

or:

```text
Submitted
   ↓
Failed
   ↓
Registration not completed
```

---

# 29. Security Considerations

Asset lifecycle transitions are security-sensitive.

Particular attention must be given to:

* unauthorized registration
* unauthorized suspension
* unauthorized retirement
* issuer impersonation
* verification manipulation
* metadata substitution
* stale verification
* duplicate registration
* replayed transactions
* compromised administrator accounts

The contract must enforce critical protocol invariants independently of the backend.

---

# 30. Protocol Invariants

The following invariants should hold.

### Invariant 1

An asset must have a unique protocol identifier.

### Invariant 2

An asset cannot be simultaneously in two protocol states.

### Invariant 3

Only authorized actors can perform protocol lifecycle transitions.

### Invariant 4

A retired asset cannot silently become active again.

### Invariant 5

Backend database state cannot override authoritative blockchain state.

### Invariant 6

An unregistered asset cannot have valid protocol ownership.

### Invariant 7

Failed blockchain transactions must not be represented as successful protocol operations.

---

# 31. Open Questions

The following decisions are intentionally deferred.

## 31.1 What exactly constitutes tokenization?

Possible models include:

* one token per asset
* fractional token representation
* asset-specific token contracts
* Stellar native assets
* Soroban-managed ownership

This requires a separate RFC.

---

## 31.2 Where should the asset identifier live?

Possible approaches include:

* contract-generated ID
* deterministic ID
* issuer-provided ID with validation
* hash-derived identifier

This should be decided during contract design.

---

## 31.3 How should metadata commitments work?

Options include:

* metadata hash
* content-addressed storage
* URI/reference
* Merkle commitment

A separate technical specification should resolve this.

---

## 31.4 What verification threshold is required?

Different asset classes may require different verification requirements.

The protocol should avoid assuming that one checklist applies universally.

---

# 32. Future Extensibility

The model should support future asset categories without rewriting the entire protocol.

For example:

```text
Keystone Asset
      │
      ├── Real Estate
      ├── Invoice
      ├── Equipment
      ├── Commodity
      └── Other RWA
```

Asset-specific rules should be layered on top of the core asset model where possible.

---

# 33. Implementation Dependency Graph

This RFC becomes the dependency for several future specifications.

```text
                 RFC-001
          Asset Model & Lifecycle
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
     Ownership   Verification  Compliance
        │           │           │
        └───────┬───┴───────────┘
                ▼
          Tokenization
                │
                ▼
          Investment
                │
                ▼
          Distribution
                │
                ▼
        Secondary Market
```

---

# 34. Acceptance Criteria

RFC-001 is considered ready for implementation when the team can answer the following questions without ambiguity:

* What is an Asset?
* How is an Asset uniquely identified?
* Who can create an Asset?
* What makes an Asset verified?
* What makes an Asset approved?
* When can an Asset be registered on-chain?
* What states exist on-chain?
* What states remain off-chain?
* Who can transition each state?
* What happens when a transaction fails?
* What happens when verification expires?
* What happens when an asset is suspended?
* What happens when an asset is retired?
* Which data is authoritative?
* Which data is merely indexed?
* What events must the contract emit?
* What information must the backend expose?
* What must the frontend display?

If these questions cannot be answered, implementation should not begin.

---

# 35. Proposed Status

**DRAFT**

This RFC is not yet an implementation specification.

Changes should be proposed through discussion and, for substantial architectural changes, a new revision or related RFC.

---

# 36. Final Principle

An asset in KeystoneGrid is not simply a token.

It is the connection between:

```text
REAL-WORLD ASSET
        │
        ▼
VERIFICATION
        │
        ▼
APPLICATION
        │
        ▼
PROTOCOL REGISTRATION
        │
        ▼
ON-CHAIN OWNERSHIP
        │
        ▼
ECONOMIC ACTIVITY
```

KeystoneGrid should never confuse the existence of a blockchain token with proof of the underlying real-world asset.

The protocol establishes verifiable digital ownership and enforceable on-chain rules.

The application layer establishes and manages the real-world evidence, verification processes, metadata, and operational workflows surrounding that asset.

> **KeystoneGrid does not tokenize first and ask questions later. The asset must have a defined identity, lifecycle, authority model, and verification boundary before it becomes protocol state.**
