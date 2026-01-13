# Zuub Architectural Challenges

This repository contains "Synthetic Product Requirement Documents" (PRDs) designed to test senior engineering candidates on System Design, Data Modeling, and AI-Assisted Implementation.

## The Challenges

Candidates are expected to pick one challenge and deliver:

1. **RFC**: A technical design document.
2. **OpenAPI Spec**: A rigorous API contract.
3. **Key Implementation**: A working proof-of-concept.

### [001: Distributed Rate Limiter](challenges/001-distributed-rate-limiter.md)

Design a high-performance distributed rate limiter handling 100k RPS.

### [002: Webhook Delivery Service](challenges/002-webhook-delivery.md)

Build a resilient event delivery system with exponential backoff and observability.

### [003: Real-time Leaderboard](challenges/003-realtime-leaderboard.md)

Architect a real-time leaderboard for 10M users with high write velocity.

### [004: Pulsating UI State](challenges/004-pulsating-ui-state.md)

Build a dashboard that accurately tracks "chaotic" jobs (Sync/Async mix) with robust state reconciliation.

### [005: Event-Based Correctness](challenges/005-dental-claims-correctness.md)

Design a "Correct by Construction" event sourcing system for Dental Claims (ADA Codes).

### [006: Supply Chain Parser](challenges/006-supply-chain-parser.md)

Reliably extract BOL numbers from messy logistics documents (No 2FA/Auth codes).

### [007: The Billion Record Challenge](challenges/007-mpi-processing.md)

High-performance ingestion of 1GB+ CSV data. Target speed: >3M records/sec.
