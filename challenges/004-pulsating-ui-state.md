# Challenge 004: Pulsating UI State (Job Monitor)

## Executive Summary
We have a legacy backend where jobs are "chaotic invariants": they switch between synchronous and asynchronous execution, pause, restart, and sometimes fail silently. We need a **Job Monitor Dashboard** that reflects the **True State** of these jobs with absolute fidelity.

## Requirements

### Functional
- **Hybrid Lifecycle**: A job might start synchronously (API waits), then switch to async (API returns "Accepted", job continues in background), then pause, then resume.
- **The "Pulse"**: The UI must interpret "Heartbeats".
    - If a job sends a heartbeat every 1s, and we miss 3, the UI must flag it as `UNHEALTHY` immediately.
    - If it resumes, it goes back to `RUNNING`.
- **Chaos**: Events may arrive out of order (e.g., "Finished" arrives before "Started"). The UI must resolve this logically.

### Non-Functional
- **Concurrency**: Monitor 50,000 active jobs.
- **Accuracy**: The UI state must match the Backend Truth within 50ms.
- **Efficiency**: No polling. Use Push (WebSockets/SSE).

## The Ask
1.  **RFC**: State Management Strategy.
    - How do you handle out-of-order events? (Vector Clocks? State Machines? CRDTs?)
    - How does the UI handle "Reconnection Reconciliation" after a network drop?
2.  **AsyncAPI / OpenAPI Spec**: Define the event protocol.
    - `JobStarted`, `JobProgress`, `JobPaused`, `JobHeartbeat`, `JobFinished`.
3.  **Implementation**:
    - **Backend**: A chaos generator that spawns jobs with random behaviors (Sync/Async, sleep, fail, lag).
    - **Frontend**: A dashboard that visualizes these jobs. (Use React/Vue/Svelte or vanilla JS).
    - **Demonstrate**: A video showing the UI handling a network disconnect and recovering the state perfectly.

## "Gotchas"
- The "Ghost Spinner": UI shows loading, but the job died 5 minutes ago.
- The "False Failure": UI says failed because of lag, but job is actually fine.
- "Flickering": Job state toggles rapidly due to race conditions.
