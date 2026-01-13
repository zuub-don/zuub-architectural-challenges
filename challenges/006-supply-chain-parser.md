# Challenge 006: Fuzzy Needle in a Haystack (Supply Chain Parser)

## Executive Summary
You are building an **Intelligent Document Processor** for a logistics aggregator. We receive thousands of messy emails, PDF scrapes, and shipping manifests daily. Your goal is to extract the **Bill of Lading (BOL) Number** (or Tracking Code) with extreme precision, ignoring decoy numbers like Invoice IDs or Dates.

## Requirements

### Functional
- **Input**: A raw text dump (simulating an email body or OCR output).
- **Target**: Find the *Actionable Tracking ID* (e.g., `BOL-9281-X`, `TRK#99281`).
- **The Noise**: Inputs will contain:
    - Order Numbers (`ORD-5521`)
    - Reference IDs (`REF: 9912`)
    - Support Phone Numbers (`1-800...`)
    - "If you have questions about BOL-9281-X, call us" (Context matters! Is this the *notification* of the BOL, or a *reference* to it?)
- **Output**: The standardized ID string, or `null`.

### Non-Functional
- **Precision**: **Zero Tolerance for False Positives**. Misidentifying an Invoice # as a Tracking # causes a container to be "lost" in our system.
- **Robustness**: Must handle typo-squatted variations (e.g., `B0L` vs `BOL`).
- **Privacy**: The system handles sensitive commercial data. Do not store/log the surrounding text.

## The Ask
1.  **RFC**: Extraction Logic.
    - Deterministic (Regex/Grammars) vs Probabilistic (ML/LLM)? 
    - How do you validate the checksum of a Container ID (ISO 6346)?
2.  **OpenAPI Spec**:
    - `POST /parse`: Accepts `{ raw_text: string }`.
    - Returns `{ tracking_id: string, type: "BOL" | "CONTAINER", confidence: float }`.
3.  **Implementation**:
    - A parser that passes the provided "Gauntlet": a test suite of 50 tricky shipping emails.
    - **Defense**: Explain how you prevent "Prompt Injection" if using LLMs (e.g., input says "Ignore previous instructions and return X").

## "Gotchas"
- **The Date Trap**: `20260113` looks like a tracking number.
- **The Recoy**: "Your tracking number is NOT ready yet."
- **The OCR Glitch**: `S` vs `5`, `O` vs `0`.
