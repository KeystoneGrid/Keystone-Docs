# KeystoneGrid RFCs

This directory contains the architectural and protocol RFCs that define the design of KeystoneGrid.

RFCs are the primary mechanism for proposing, documenting, reviewing, and stabilizing protocol-level decisions.

## RFC Lifecycle

An RFC progresses through the following lifecycle:

```text
DRAFT
  ↓
PROPOSED
  ↓
ACCEPTED
  ↓
IMPLEMENTED
  ↓
DEPRECATED
```

### DRAFT

An idea is being developed and is not yet ready for implementation.

### PROPOSED

The design is sufficiently defined for architectural review and community feedback.

### ACCEPTED

The design has been approved by the KeystoneGrid maintainers and may be implemented.

### IMPLEMENTED

The design has been implemented and the implementation has been reviewed and merged.

### DEPRECATED

The design has been superseded or is no longer recommended.

## RFC Index

| RFC     | Title                    | Status   |
| ------- | ------------------------ | -------- |
| RFC-001 | Asset Model & Lifecycle  | PROPOSED |
| RFC-002 | Ownership Model          | PROPOSED |
| RFC-003 | Compliance & Eligibility | PROPOSED |
| RFC-004 | Asset Verification       | PROPOSED |
| RFC-005 | Investment & Settlement  | PROPOSED |
| RFC-006 | Revenue Distribution     | PROPOSED |
| RFC-007 | Asset Token Standard     | PROPOSED |
| RFC-008 | Secondary Market         | PROPOSED |

## How RFCs Relate to Implementation

RFCs define **what the protocol must guarantee and why**.

Implementation issues define **how those requirements are implemented in code**.

Contributors should use the relevant RFC as the architectural source of truth when working on implementation issues.

A typical workflow is:

```text
RFC
 ↓
Specification
 ↓
GitHub Issue
 ↓
Contributor Assignment
 ↓
Implementation
 ↓
Tests
 ↓
Pull Request
 ↓
Review
 ↓
Merge
```

Contributors should not substantially change protocol behavior through an implementation PR without first proposing an RFC amendment or new RFC where appropriate.

## RFC Requirements

A protocol RFC should clearly describe:

* Problem and motivation
* Goals and non-goals
* Terminology
* Protocol behavior
* State transitions
* Authorization requirements
* Security considerations
* Invariants
* On-chain/off-chain responsibilities
* Failure behavior
* Testing requirements
* Deferred decisions

## Contributing to an RFC

Community members are encouraged to review proposed RFCs and provide:

* Technical feedback
* Security concerns
* Alternative designs
* Implementation considerations
* Questions about protocol behavior

Implementation should begin only after the relevant design has reached an appropriate level of maturity.

## Source of Truth

For protocol behavior:

**Accepted RFCs and protocol specifications are authoritative.**

Backend services, frontend applications, indexes, and external integrations must not redefine protocol state or ownership independently of the KeystoneGrid protocol.
