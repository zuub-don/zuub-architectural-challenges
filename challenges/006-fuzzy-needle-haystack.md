# Challenge 006: Fuzzy Needle in a Haystack (2FA Parser)

## Executive Summary

You are building an **Intelligent Extractor** for an email processing pipeline. The goal is to extract strictly defined "Actionable Tokens" (specifically **6-digit 2FA/OTP codes**) from messy, unstructured, and potentially adversarial HTML/Text bodies.

## Requirements

### Functional

- **Input**: A raw string (email body, SMS dump, or OCR text).
- **Target**: Find the *True* Authentication Code.
- **The Noise**: Inputs will contain:
  - Phone numbers (e.g., `800-555-0199`).
  - Ticket IDs (e.g., `Ticket #948201`).
  - Copyright dates (`2026`).
  - Recoy codes (e.g., "If you didn't request this, ignore code 000000").
- **Output**: The 6-digit string, or `null`.

### Non-Functional

- **Precision**: > 99.99%. It is better to fail (return `null`) than to extract the wrong number (e.g., extracting the support phone number as the OTP).
- **Privacy**: The system must NOT log or store the PII surrounding the code.
- **Performance**: < 10ms per 100KB of text.

## The Ask

1. **RFC**: The Extraction Strategy.
    - Regex vs DOM Parsing vs NLP/LLM? (Argue for the most *robust* and *deterministic* approach).
    - How do you handle "Context"? (e.g., look for "Code", "Verification", "OTP" near the number).
2. **OpenAPI Spec**:
    - `POST /extract`: Accepts `{ text: string, context_hint: string }`.
    - Returns `{ token: string | null, confidence: number }`.
3. **Implementation**:
    - A Python/Go/Rust parsing engine.
    - **Test Suite**: A `tests.json` file with 50+ examples of "Haystacks" (some valid, some adversarial) to prove your extractor works.

## "Gotchas"

- **Adversarial**: "Your verification code is NOT 123456 call us at..."
- **Formatting**: `1 2 3 4 5 6` (spaced out), `123-456` (dashed).
- **HTML Hell**: `<span>123</span><b>456</b>` (split across tags).
