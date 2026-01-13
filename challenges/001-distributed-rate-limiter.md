# Challenge 001: Distributed Rate Limiter

## Executive Summary

We need to protect our core APIs from abuse and cascading failures. You are tasked with designing and implementing a **Distributed Rate Limiting Service** that can handle global traffic with minimal latency.

## Requirements

### Functional

- **Algorithm**: Implements a standard rate limiting algorithm (e.g., Token Bucket, Leaky Bucket, or Fixed Window). You must justify your choice.
- **Granularity**: Limit by:
  - IP Address
  - User ID (if authenticated)
  - API Key
- **Configuration**: Different limits for different routes (e.g., `POST /login` is stricter than `GET /products`).

### Non-Functional

- **Throughput**: Must handle **100,000 requests per second (RPS)**.
- **Latency**: Validation must add **< 20ms** overhead (99th percentile).
- **Consistency**: The system is distributed across 3 geographic regions. State must be synchronized (eventually or strictly? You decide and justify).
- **Reliability**: If the rate limiter service fails, the API must **fail open** (allow traffic) by default.

## The Ask

1. **RFC**: Write a design document explaining your architecture.
    - Redis vs Memcached vs Custom Store?
    - Synchronous vs Asynchronous limiting?
    - How do you handle race conditions?
2. **OpenAPI Spec**: Define the administration API for configuring limits.
    - `POST /limits`: Create a rule.
    - `GET /limits/{id}/stats`: View current usage.
3. **Implementation**: A "Key" implementation in Go/Rust/Python.
    - Mock the storage layer if needed, but the logic must be sound.

## "Gotchas" (For the candidate to solve)

- Time synchronization between servers.
- Hot keys (one IP spamming 50k RPS).
- Network partitions between regions.
