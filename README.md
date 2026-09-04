# KeystoneGrid

> **Open infrastructure for bringing verified real-world assets onto Stellar.**

KeystoneGrid is an open-source project building the infrastructure required to represent, verify, manage, and interact with real-world assets on the Stellar network.

The project begins with **real estate** as its primary asset class, with a long-term architecture designed to support additional real-world asset categories without rebuilding the underlying protocol.

KeystoneGrid combines **on-chain asset ownership and settlement** with **off-chain asset verification, compliance workflows, metadata, indexing, and application services**.

---

## Table of Contents

* [Overview](#overview)
* [The Problem](#the-problem)
* [The KeystoneGrid Approach](#the-keystonegrid-approach)
* [Core Principles](#core-principles)
* [Architecture](#architecture)
* [Repositories](#repositories)
* [Core Components](#core-components)
* [Asset Lifecycle](#asset-lifecycle)
* [Investor Lifecycle](#investor-lifecycle)
* [Issuer Lifecycle](#issuer-lifecycle)
* [Compliance Model](#compliance-model)
* [On-Chain vs Off-Chain Responsibilities](#on-chain-vs-off-chain-responsibilities)
* [Why Stellar](#why-stellar)
* [Security Philosophy](#security-philosophy)
* [Open Source Development](#open-source-development)
* [Contribution Workflow](#contribution-workflow)
* [Issue Guidelines](#issue-guidelines)
* [Pull Request Expectations](#pull-request-expectations)
* [Development Roadmap](#development-roadmap)
* [Repository Links](#repository-links)
* [Documentation](#documentation)
* [Project Status](#project-status)
* [Security Disclosure](#security-disclosure)
* [License](#license)

---

## Overview

Traditional real-world assets are difficult to make digitally accessible.

A property may have:

* A legal owner
* Title or ownership documentation
* Valuation records
* Inspection records
* Rental or income information
* Legal restrictions
* Regulatory requirements
* Multiple stakeholders

Most of this information exists outside blockchain systems.

At the same time, blockchains are good at maintaining transparent and verifiable records of:

* Ownership
* Transfers
* Transactions
* Permissions
* Asset balances
* Settlement
* Distribution records

KeystoneGrid is designed to connect these two worlds.

### The core idea

```text
Real-World Asset
       │
       ▼
Verification & Compliance
       │
       ▼
KeystoneGrid
       │
 ┌─────┴─────┐
 │           │
 ▼           ▼
Off-chain   On-chain
services    protocol
 │           │
 └─────┬─────┘
       ▼
    Stellar
       │
       ▼
Transparent ownership,
settlement & asset activity
```

KeystoneGrid does **not** attempt to put every piece of real-world information on-chain.

Instead, it determines which information and state transitions require blockchain guarantees and which are better handled by conventional infrastructure.

---

# The Problem

Real-world asset markets face several structural problems.

## 1. High barriers to participation

Many assets require substantial capital to access.

Real estate is an obvious example.

A person may want exposure to a property but cannot afford to purchase the entire property.

---

## 2. Fragmented information

Property information can be distributed across:

* Government records
* Legal documents
* Valuation reports
* Inspection reports
* Property managers
* Financial records
* Private databases

There is often no unified digital representation of the asset.

---

## 3. Limited transparency

Investors may have difficulty independently verifying:

* What they own
* How much of an asset exists
* Who issued it
* How income is calculated
* How distributions are performed
* Whether ownership records have changed

---

## 4. Limited liquidity

Traditional ownership structures can make it difficult to transfer or exit an investment.

A digital representation does not automatically solve liquidity, but it can provide programmable infrastructure for compliant transfer and settlement.

---

## 5. Trust gaps

A blockchain can prove that an address owns a token.

It cannot, by itself, prove that the underlying real-world property actually exists or that the issuer legally owns it.

This is one of the central problems KeystoneGrid is designed to address.

---

# The KeystoneGrid Approach

KeystoneGrid separates the system into three major layers:

```text
┌──────────────────────────────────────────────┐
│                 APPLICATION                  │
│                                              │
│ Investor • Issuer • Compliance • Admin      │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│                  SERVICES                    │
│                                              │
│ API • Verification • Indexing • Metadata    │
│ Database • Notifications • Analytics        │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│                  PROTOCOL                    │
│                                              │
│ Asset Registry • Ownership • Investment     │
│ Compliance Rules • Distribution • Settlement│
└──────────────────────┬───────────────────────┘
                       │
                       ▼
                    STELLAR
```

The three layers are intentionally separated.

This allows developers to contribute to one part of the system without unnecessarily coupling the entire project.

---

# Core Principles

KeystoneGrid is being developed according to the following principles.

## 1. Blockchain should solve the right problems

Not every piece of data belongs on-chain.

On-chain state should be limited to information and operations that benefit from decentralized verification, deterministic execution, and transparent settlement.

---

## 2. The blockchain should be the source of truth for ownership

The backend may index ownership.

The frontend may display ownership.

Neither should become the authoritative source of ownership.

For example:

```text
Blockchain:
Address A → 1,000 units

Backend:
Indexed result → Address A owns 1,000 units

Frontend:
Portfolio → Address A owns 1,000 units
```

The backend and frontend should derive their state from the protocol rather than independently defining ownership.

---

## 3. Real-world verification must be explicit

Tokenizing an asset does not automatically prove that the underlying asset is legitimate.

KeystoneGrid therefore treats:

* Ownership verification
* Document verification
* Issuer verification
* Valuation
* Compliance
* Asset status

as first-class concerns.

---

## 4. Security takes priority over feature count

KeystoneGrid is financial infrastructure.

Incorrect ownership accounting or distribution logic can cause real financial loss.

Therefore:

> **Security, correctness and auditability take precedence over shipping speed.**

---

## 5. Open source should be part of the architecture

The system should be understandable and extensible by developers outside the core team.

Important decisions should therefore be documented rather than hidden in implementation details.

---

# Architecture

At a high level:

```text
                        ┌─────────────────┐
                        │     Users       │
                        │                 │
                        │ Investors       │
                        │ Issuers         │
                        │ Compliance      │
                        │ Administrators  │
                        └────────┬────────┘
                                 │
                                 ▼
                     ┌─────────────────────┐
                     │ Keystone-Frontend   │
                     │                     │
                     │ Web Application     │
                     │ Wallet Integration  │
                     │ User Experience     │
                     └──────────┬──────────┘
                                │
                                ▼
                     ┌─────────────────────┐
                     │ Keystone-Backend    │
                     │                     │
                     │ API                 │
                     │ Database            │
                     │ Indexing            │
                     │ Verification        │
                     │ Metadata            │
                     └──────────┬──────────┘
                                │
                                │
                                ▼
                     ┌─────────────────────┐
                     │ Keystone-Contract   │
                     │                     │
                     │ Soroban Contracts   │
                     │ Asset Logic         │
                     │ Ownership           │
                     │ Compliance          │
                     │ Settlement          │
                     └──────────┬──────────┘
                                │
                                ▼
                           ┌──────────┐
                           │ Stellar  │
                           └──────────┘
```

---

# Repositories

KeystoneGrid is organized into separate repositories.

| Repository                                                               | Responsibility                                                |
| ------------------------------------------------------------------------ | ------------------------------------------------------------- |
| [`Keystone-Docs`](https://github.com/KeystoneGrid/Keystone-Docs)         | Architecture, specifications and project documentation        |
| [`Keystone-Contract`](https://github.com/KeystoneGrid/Keystone-Contract) | Soroban smart contracts and on-chain protocol                 |
| [`Keystone-Backend`](https://github.com/KeystoneGrid/Keystone-Backend)   | APIs, indexing, verification, database and off-chain services |
| [`Keystone-Frontend`](https://github.com/KeystoneGrid/Keystone-Frontend) | Web application and user experience                           |

The repositories are independent but form one system.

---

# Core Components

## Asset Registry

The asset registry represents real-world assets within KeystoneGrid.

An asset may eventually contain information such as:

* Asset identifier
* Asset type
* Issuer
* Location
* Ownership reference
* Verification status
* Valuation reference
* Metadata URI
* Tokenization state
* Lifecycle status

The protocol should only store information that needs blockchain-level guarantees.

---

## Ownership

Ownership represents the on-chain relationship between an eligible participant and a tokenized asset.

The exact implementation will be defined as part of the Stellar/Soroban protocol design.

Ownership must be:

* Deterministic
* Queryable
* Testable
* Auditable
* Resistant to unauthorized modification

---

## Compliance

Certain real-world assets may require restrictions on who can acquire, hold, or transfer them.

KeystoneGrid therefore separates:

```text
Identity
   ↓
Verification
   ↓
Eligibility
   ↓
On-chain authorization
```

The blockchain should not store unnecessary sensitive identity information.

Instead, it should receive the minimum information necessary to enforce protocol rules.

---

## Revenue Distribution

Income-producing assets may generate revenue.

Examples include:

* Rent
* Lease payments
* Revenue-sharing agreements
* Other asset-specific income

The protocol may provide mechanisms for allocating and claiming eligible distributions according to ownership.

Distribution logic must be designed with particular attention to:

* Precision
* Rounding
* Partial ownership
* New investors
* Existing investors
* Repeated distributions
* Unclaimed funds
* Administrative failures

---

## Verification

Verification is the bridge between real-world information and on-chain representation.

Potential verification categories include:

### Property ownership

Does the issuer have a legitimate ownership claim?

### Legal documentation

Are relevant legal records available and verified?

### Valuation

What is the asset's independently supported value?

### Physical verification

Does the physical asset correspond to the submitted information?

### Issuer verification

Who is responsible for issuing the asset?

---

# Asset Lifecycle

A typical asset lifecycle may look like:

```text
                    DRAFT
                      │
                      ▼
                 SUBMITTED
                      │
                      ▼
                 VERIFICATION
                      │
          ┌───────────┴───────────┐
          │                       │
       REJECTED                APPROVED
                                  │
                                  ▼
                             TOKENIZED
                                  │
                                  ▼
                                ACTIVE
                                  │
                  ┌───────────────┼───────────────┐
                  │               │               │
                  ▼               ▼               ▼
              INVESTMENT      DISTRIBUTION     UPDATES
                  │               │               │
                  └───────────────┼───────────────┘
                                  │
                                  ▼
                               CLOSED
```

The exact lifecycle and state transitions will be defined by the protocol specification.

---

# Investor Lifecycle

A typical investor journey:

```text
Connect Stellar Wallet
        │
        ▼
Identity / Eligibility
        │
        ▼
Browse Verified Assets
        │
        ▼
Review Asset Information
        │
        ▼
Choose Investment
        │
        ▼
Authorize Transaction
        │
        ▼
Receive Ownership
        │
        ▼
Monitor Investment
        │
        ▼
Receive / Claim Eligible Income
        │
        ▼
Exit / Transfer
```

Not every asset will necessarily support every lifecycle stage.

Asset-specific restrictions may apply.

---

# Issuer Lifecycle

An issuer may follow:

```text
Create Account
      │
      ▼
Issuer Verification
      │
      ▼
Submit Asset
      │
      ▼
Provide Documentation
      │
      ▼
Asset Verification
      │
      ▼
Valuation
      │
      ▼
Approval
      │
      ▼
Tokenization
      │
      ▼
Funding / Investment
      │
      ▼
Asset Management
      │
      ▼
Revenue Distribution
      │
      ▼
Asset Closure
```

---

# Compliance Model

KeystoneGrid treats compliance as a system-wide concern rather than a simple boolean stored against a wallet address.

The target architecture is conceptually:

```text
User
 │
 ▼
Identity Provider
 │
 ▼
Verification
 │
 ▼
Eligibility Decision
 │
 ▼
KeystoneGrid Compliance Layer
 │
 ▼
Protocol Authorization
 │
 ▼
Allowed Operation
```

The implementation must minimize the amount of personally identifiable information exposed to public blockchain infrastructure.

Compliance requirements may differ depending on:

* Asset type
* Jurisdiction
* Issuer
* Investor category
* Transfer type
* Regulatory requirements

KeystoneGrid is open-source infrastructure and does not by itself constitute legal or regulatory advice.

Projects deploying KeystoneGrid for real financial activity are responsible for obtaining appropriate legal and regulatory guidance.

---

# On-Chain vs Off-Chain Responsibilities

One of the most important architectural decisions in KeystoneGrid is determining what belongs on-chain.

## On-chain

Potential examples:

* Asset identifiers
* Ownership
* Token balances
* Eligible operations
* Protocol state
* Distribution accounting
* Settlement
* Contract authorization
* Important state transitions
* Cryptographic references to verified information

## Off-chain

Potential examples:

* Personal identity information
* KYC documents
* Property documents
* Large metadata
* Images
* Search indexes
* Analytics
* Notifications
* Third-party verification records
* Application workflows

The guiding principle is:

> **Store proofs and state on-chain where trust minimization matters; store large or sensitive operational data off-chain.**

---

# Why Stellar

KeystoneGrid is being developed on **Stellar** and its smart-contract platform, **Soroban**.

Soroban smart contracts are written in Rust and compiled to WebAssembly. Stellar also provides a built-in Stellar Asset Contract that allows smart contracts to interact with Stellar assets.

Stellar's authorization model provides contract-level authorization mechanisms such as `require_auth` and `require_auth_for_args`, allowing contracts to enforce authorization without implementing their own signature verification and replay-protection system in ordinary cases.

Stellar also provides asset authorization controls that can be useful when regulated or permissioned assets require controlled participation.

These capabilities make Stellar a strong foundation for exploring compliant real-world asset infrastructure.

### KeystoneGrid will therefore be Stellar-native.

We are **not** attempting to reproduce an Ethereum/EVM architecture on another chain.

The protocol should use Stellar and Soroban primitives where they provide a better model.

---

# Security Philosophy

KeystoneGrid handles financial and ownership-related state.

Security therefore applies at every layer.

## Smart contracts

Must prioritize:

* Authorization
* Access control
* Arithmetic correctness
* State transition safety
* Replay protection
* Reentrancy considerations
* Storage correctness
* Upgrade/deployment safety
* Emergency mechanisms
* Comprehensive testing

Soroban provides host-managed authorization primitives that should be preferred over unnecessary custom authentication systems.

---

## Backend

Must prioritize:

* Authentication
* Authorization
* Input validation
* Secrets management
* Rate limiting
* Database security
* Audit logging
* Dependency security
* API abuse prevention

---

## Frontend

Must prioritize:

* Transaction transparency
* Correct network selection
* Wallet safety
* Clear signing information
* Error handling
* Avoiding unsafe assumptions about transaction state

A frontend should never claim a transaction succeeded merely because a wallet request was submitted.

---

# Open Source Development

KeystoneGrid is intended to be an open-source project.

Contributors are welcome to participate in:

* Smart contracts
* Backend development
* Frontend development
* Testing
* Security
* Documentation
* Developer tooling
* SDKs
* Infrastructure
* UX
* Research
* Architecture

The project is designed around a public contribution workflow.

---

# Contribution Workflow

The preferred workflow is:

```text
Issue
  │
  ▼
Discussion
  │
  ▼
Contributor expresses interest
  │
  ▼
Maintainer assigns issue
  │
  ▼
Contributor creates branch
  │
  ▼
Implementation
  │
  ▼
Tests
  │
  ▼
Pull Request
  │
  ▼
Code Review
  │
  ▼
CI Checks
  │
  ▼
Merge
```

Contributors should avoid beginning substantial architectural work without first discussing the intended approach in the relevant issue.

---

# Issue Guidelines

Issues should be specific enough that an independent contributor can understand the task.

A good issue should contain:

### Problem

What needs to change?

### Context

Why does the change matter?

### Scope

What should be implemented?

### Acceptance criteria

How will we know the work is complete?

### Testing requirements

What tests should be added or updated?

### Dependencies

Does the issue depend on another repository, issue, or architectural decision?

---

## Example

```text
Title:
feat(contract): add verified asset registration

Problem:
The protocol currently has no mechanism for registering an
asset after the off-chain verification process has completed.

Requirements:
- Accept a unique asset identifier.
- Accept the issuer address.
- Store the verification reference.
- Prevent duplicate registration.
- Require appropriate authorization.
- Emit an asset registration event.

Acceptance criteria:
- Registration succeeds for an authorized issuer.
- Duplicate assets are rejected.
- Unauthorized callers are rejected.
- Unit tests cover success and failure cases.
```

---

# Pull Request Expectations

Every pull request should:

* Clearly describe the change.
* Reference the relevant issue.
* Include appropriate tests.
* Avoid unrelated changes.
* Update documentation when necessary.
* Follow repository coding standards.
* Explain architectural decisions when relevant.

For protocol changes, contributors should also describe:

* Security implications
* Storage implications
* Authorization implications
* Backward compatibility
* Migration requirements

---

# Development Roadmap

The roadmap is intentionally divided into stages.

## Phase 0 — Foundation

* [ ] Define protocol specification
* [ ] Define asset model
* [ ] Define ownership model
* [ ] Define compliance model
* [ ] Define verification architecture
* [ ] Define repository responsibilities
* [ ] Define API boundaries
* [ ] Define security model
* [ ] Establish contributor guidelines

---

## Phase 1 — Core Protocol

* [ ] Soroban development environment
* [ ] Asset registry
* [ ] Asset lifecycle
* [ ] Ownership model
* [ ] Issuer authorization
* [ ] Investor authorization
* [ ] Core events
* [ ] Contract tests
* [ ] Testnet deployment

---

## Phase 2 — Backend Infrastructure

* [ ] Database schema
* [ ] API foundation
* [ ] Stellar event indexing
* [ ] Asset metadata service
* [ ] Verification workflow
* [ ] Issuer workflow
* [ ] Investor APIs
* [ ] Transaction indexing
* [ ] Audit logging

---

## Phase 3 — Frontend

* [ ] Wallet connection
* [ ] Asset discovery
* [ ] Asset details
* [ ] Verification information
* [ ] Investment flow
* [ ] Portfolio
* [ ] Distribution history
* [ ] Issuer dashboard
* [ ] Compliance dashboard

---

## Phase 4 — Production Hardening

* [ ] Security review
* [ ] Contract audit
* [ ] Backend security review
* [ ] Integration testing
* [ ] Failure recovery
* [ ] Monitoring
* [ ] Deployment automation
* [ ] Production documentation

---

## Phase 5 — Ecosystem

Potential future work includes:

* SDKs
* Developer APIs
* Asset integrations
* Third-party applications
* Additional asset classes
* Advanced compliance primitives
* Secondary-market infrastructure
* Property data integrations
* Valuation integrations
* Institutional integrations

---

# Repository Links

### Documentation

[Keystone-Docs](https://github.com/KeystoneGrid/Keystone-Docs)

### Smart Contracts

[Keystone-Contract](https://github.com/KeystoneGrid/Keystone-Contract)

### Backend

[Keystone-Backend](https://github.com/KeystoneGrid/Keystone-Backend)

### Frontend

[Keystone-Frontend](https://github.com/KeystoneGrid/Keystone-Frontend)

### Organization

[KeystoneGrid on GitHub](https://github.com/KeystoneGrid)

---

# Documentation

The documentation repository is intended to become the canonical source for:

* Architecture
* Protocol specifications
* API specifications
* Data models
* Security decisions
* RFCs
* Contributor documentation
* Deployment documentation
* Integration guides
* Developer guides

As the project evolves, major architectural decisions should be documented here.

---

# Project Status

KeystoneGrid is currently under active development.

The architecture is being established and the original Ethereum-based BOTEstate implementation is being redesigned for Stellar/Soroban.

The current implementation should therefore be considered **experimental and not production-ready** unless a specific release is explicitly marked otherwise.

Do not use the protocol with real financial assets or funds without independently evaluating the implementation, security, legal, and regulatory risks.

---

# Security Disclosure

Security vulnerabilities should **not** be disclosed through public GitHub issues.

Please follow the security disclosure process described in:

[`SECURITY.md`](SECURITY.md)

If the repository does not yet contain a security policy, one should be added before production deployment.

---

# License

KeystoneGrid is an open-source project.

The applicable license and contribution terms are defined in the repository's license files.

If you intend to build commercial infrastructure on top of KeystoneGrid, review the applicable license and project policies before deployment.

---

## Building the Future of Open RWA Infrastructure

KeystoneGrid is being built with a simple principle:

> **Real-world assets should be represented by infrastructure that is transparent, verifiable, programmable, and accessible to developers.**

The goal is not merely to put assets on a blockchain.

The goal is to build the infrastructure that connects **real-world ownership and economic activity with open financial technology**.
