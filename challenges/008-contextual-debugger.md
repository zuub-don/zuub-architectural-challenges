# Challenge 008: The Ultimate Debugger (Contextual AI)

## Executive Summary
You are building the "Brain" of our observability stack. We have millions of distributed traces (Spans, Logs, Metrics). Your goal is to detect **Semantic Mismatches**: transactions that technically "succeeded" (HTTP 200) but functionally failed (e.g., User wasn't charged, Inventory wasn't reserved).

## Requirements

### The "Needle" (Semantic Ambiguity)
- **Scenario**: A user buys an item.
    - `Payment Service`: Returns `200 OK` (body: `{"status": "authorized"}`).
    - `Inventory Service`: Returns `200 OK` (body: `{"status": "reserved"}`).
    - `Shipping Service`: Returns `200 OK` (body: `{"status": "pending_pickup"}`).
- **The Bug**: In 0.01% of cases, `Payment Service` returns `200 OK` but the body says `{"status": "declined_insufficient_funds"}`, yet the downstream services proceed anyway.
- **The Task**: A standard regex monitor won't catch this because the Schema changes often. You need **Contextual Awareness**.

### Functional
1.  **Ingest**: Accept OpenTelemetry Traces (JSON).
2.  **Analyze**:
    - **NLP / Embeddings**: Cluster log messages to define "Normal" vs "Anomalous".
    - **Causal Graph**: Map the dependencies. If A fails, B should not proceed.
3.  **Optimize**:
    - Use **Simulated Annealing** (or Genetic Algorithms) to tune the sensitivity thresholds of your anomaly detector.
    - *Input*: A set of labelled "Good" and "Bad" traces.
    - *Output*: Optimal hyperparameters for your detection model.

### Non-Functional
- **Scale**: Real-time analysis of 10k spans/sec.
- **Explainability**: When you flag a trace, you must explain *why* (e.g., "Payment Status 'declined' usually stops flow, but here it continued").

## The Ask
1.  **RFC**: The AI/Heuristic Architecture.
    - How do you embed log lines? (TF-IDF vs BERT vs LLM API).
    - Designing the Cost Function for the Simulated Annealing tuner.
2.  **Implementation**:
    - Build the **Parameter Tuner** (The Annealing Loop).
    - Build a mock Trace Generator that injects subtle semantic bugs.
    - Demonstrate the system finding the bugs.

## "Gotchas"
- **Drift**: "Normal" changes over time.
- **False Positives**: Alert fatigue kills usability.
- **Compute Cost**: Running BERT on every log line is too expensive. How do you sample?
