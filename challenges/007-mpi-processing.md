# Challenge 007: The Billion Record Challenge (MPI Processing)

## Executive Summary
We need to ingest an entire nation's Master Patient Index (MPI) daily. The dataset contains **1 Billion Records** (roughly 50GB-100GB).
Your task: Build a processor that can ingest and validate **1GB of data in under 10 seconds** on a standard laptop.

## Requirements

### The Data
- **Format**: CSV (Comma Separated Values).
- **Schema**: order matters.
    1.  `NPI` (10-digit integer)
    2.  `FirstName` (String)
    3.  `LastName` (String)
    4.  `TaxonomyCode` (Alpha-numeric, 10 chars)
    5.  `State` (2 chars)
    6.  `Zip` (5 digits)

### Functional
- **Parse**: Read the file.
- **Validate**:
    - NPI must be 10 digits.
    - Zip must be valid integer.
- **Aggregate**: Count total records by **State** (e.g., `CA: 104,201`).

### Non-Functional
- **Speed**: > 3 Million Records / Second. (Target: 1GB < 10s).
- **Memory**: Constant memory usage (Process stream, do not load 1GB into RAM).
- **CPU**: Maximize core usage (Parallelism).

## The Ask
1.  **RFC**: Optimization Strategy.
    - `mmap` vs `Scanner` vs `Reader`.
    - Garbage Collection tuning (in Go/Java).
    - SIMD / Vectorization opportunities?
2.  **Implementation**:
    - Write the processor in a systems language (Go/Rust/C++).
    - Include a `generate` script to create the 1GB test file.
    - Include a `benchmark` script.

## "Gotchas"
- String allocation overhead (allocating 1B strings = GC Death).
- False Sharing in multicore counters.
- IO bottlenecks vs CPU bottlenecks.
