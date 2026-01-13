# Challenge 002: Webhook Delivery Service

## Executive Summary

We are building a platform that sends event notifications to third-party endpoints (e.g., Stripe style webhooks). We need a **Webhook Delivery Service** that guarantees at-least-once delivery with robust retry logic.

## Requirements

### Functional

- **Ingestion**: Accept events from internal services (JSON payload, `event_type`, `destination_url`).
- **Delivery**: Attempt to POST the payload to the destination.
- **Retries**:
  - If delivery fails (non-2xx response), retry.
  - Implement **Exponential Backoff** (e.g., 1s, 5s, 1m, 1h).
  - Max retries: 5.
- **Security**: Sign payloads using HMAC-SHA256 so receivers can verify authenticity.

### Non-Functional

- **Reliability**: Events must persist even if the delivery workers crash.
- **Scale**: Handle **5,000 events/sec**.
- **Observability**: Users must be able to see the delivery attempt logs (Success/Fail, status code, timestamp) via API.

## The Ask

1. **RFC**: Architecture design.
    - Queue selection (Kafka vs SQS vs Redis)?
    - Worker pool management.
    - How to handle "Poison Pills" (events that always crash the worker).
2. **OpenAPI Spec**:
    - `POST /events`: Internal ingestion.
    - `GET /attempts?event_id=...`: Customer-facing logs.
3. **Implementation**:
    - A simulation of the dispatcher loop.
    - A mock "Destination" server that randomly fails 50% of the time to test retries.

## "Gotchas"

- Thundering herd issues when a major destination goes down.
- Head-of-line blocking in queues.
- Ensuring delivery order (is it strict? does it matter?).
