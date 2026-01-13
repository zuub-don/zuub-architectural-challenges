# Contributing to Zuub Architectural Challenges

We encourage the team to add new "Synthetic PRDs" to this repository to keep our interview process fresh and covering new domains.

## How to Add a New Challenge

1.  **Create a File**: Create a new markdown file in `challenges/` following the naming convention: `NNN-topic-name.md` (e.g., `004-distributed-counter.md`).
2.  **Follow the Format**: Use the structure below to ensure consistency.
3.  **Update README**: Add your new challenge to the main `README.md` list.
4.  **Push**: Commit and push to `main`.

## Challenge Template

```markdown
# Challenge NNN: [Title]

## Executive Summary
[Brief 1-2 sentence description of the system to be built. e.g. "We need a distributed key-value store optimized for read-heavy workloads."]

## Requirements

### Functional
- **Core Capability 1**: ...
- **Core Capability 2**: ...
- [Ambiguous Requirement]: Leave one requirement slightly open-ended to test clarification questions.

### Non-Functional
- **Scale**: [e.g. 100k RPS, 1TB Storage]
- **Latency**: [e.g. p99 < 50ms]
- **Availability**: [e.g. 99.99%]

## The Ask
1.  **RFC**: Architecture design (Diagrams, Data Model, Trade-offs).
2.  **OpenAPI Spec**: Define the API surface.
3.  **Implementation**: A Proof-of-Concept (POC) focusing on the hardest part (e.g., the locking mechanism, or the sharding logic).

## "Gotchas"
- [Edge Case 1]
- [Edge Case 2]
```
