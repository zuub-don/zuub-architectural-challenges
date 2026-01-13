# Challenge 005: Event-Based Dental Claims (ADA Codes)

## Executive Summary

You are building the transaction engine for a Dental Insurance Clearinghouse. We deal with **ADA Procedures** (e.g., `D2140` Amalgam Filling). The system must process claims through a lifecycle (Submission -> Adjudication -> Payment) with **Zero Tolerance** for invalid state transitions.

## Requirements

### Functional

- **Domain Entities**:
  - `Claim`: A container for procedures.
  - `Procedure`: A line item with an ADA Code (e.g., `D0120` Periodic Oral Eval).
- **The Lifecycle**:
    1. `Submitted`: Clinic sends claim.
    2. `Adjudicated`: Logic engine checks coverage.
    3. `Approved` / `Denied`: Result of adjudication.
    4. `Paid`: Funds transferred.
- **Rules (Invariants)**:
  - A claim **cannot** be Paid unless it is Approved.
  - A claim **cannot** be Adjudicated if it's already Paid.
  - `D2391` (Resin-based Composite) cannot be billed on the same tooth surface as `D2140` (Amalgam) on the same day.

### Non-Functional

- **Auditability**: Every state change must be an immutable event.
- **Correctness**: The code structure should make invalid states *impossible* to represent (e.g., using Rust enums or FSMs).

## The Ask

1. **RFC**: Event Sourcing Diagram.
    - Define your events: `ClaimSubmitted`, `ProcedureAdded`, `ClaimAdjudicated`, `PaymentIssued`.
    - Show how you handle "Correction" events (e.g., undoing an adjudication).
2. **OpenAPI / AsyncAPI Spec**:
    - `POST /claims/{id}/submit`
    - `GET /claims/{id}/history`
3. **Implementation**:
    - A "State Machine" engine that enforces the lifecycle.
    - **Unit Tests**: Prove that `Paid -> Adjudicated` throws a compile-time or runtime error.

## "Gotchas"

- **Split Claims**: One procedure is approved, another is denied. Does the Claim status become "Partially Approved"?
- **Re-Adjudication**: A claim is paid, but then we realize the patient wasn't covered. How do you "Reverse" the flow without deleting data?
