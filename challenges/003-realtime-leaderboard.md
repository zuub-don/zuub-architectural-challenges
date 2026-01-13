# Challenge 003: Real-time Leaderboard

## Executive Summary

We are launching a massive multiplayer game. We need a **Real-time Leaderboard System** that tracks scores for millions of players and allows instant lookup of global rankings.

## Requirements

### Functional

- **Score Updates**: Receive `(user_id, score_increment)`.
- **Querying**:
  - `GetTop(k)`: Get the top `k` implementation.
  - `GetRank(user_id)`: Get the exact rank of a specific user.
  - `GetAround(user_id, k)`: Get neighbors of a user (e.g., who is just above/below me).

### Non-Functional

- **Cardinality**: 10 Million Users.
- **Velocity**: 50,000 score updates per second (write-heavy).
- **Latency**: Queries must return in **< 50ms**.
- **Periodicity**: Leaderboards reset Weekly AND Monthly.

## The Ask

1. **RFC**: Data Storage Strategy.
    - Redis Sorted Sets? (Can it handle 10M keys in one object?)
    - Database sharding?
    - Skip Lists?
2. **OpenAPI Spec**:
    - `POST /scores`
    - `GET /leaderboard/top`
    - `GET /leaderboard/user/{id}`
3. **Implementation**:
    - Highly optimized storage layer.
    - Demonstrate performance with a benchmark script.

## "Gotchas"

- Redis single-thread bottlenecks at 50k WPS.
- Handling ties in ranking.
- "Hot" leaderboard vs "Archive" leaderboards.
