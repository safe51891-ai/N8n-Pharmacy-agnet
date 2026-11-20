# Process map: pharmacy document intake

## Current state (manual)
```mermaid
flowchart LR
  A[Fax / e-fax arrives] --> B[Intake specialist opens PDF]
  B --> C{What is it?}
  C --> D[Re-key patient, prescriber, insurance into system]
  D --> E[Look up NPI manually]
  E --> F{Anything missing?}
  F -- yes --> G[Call / fax prescriber office]
  F -- no --> H[Route to BV, PA or care team by email/Slack]
  G --> B
```
Pain points to confirm while shadowing: re-keying errors, NPI lookups, documents sitting unread, missing-info loops, no measurement of cycle time.

## Future state (automated first pass)
```mermaid
flowchart LR
  A[Fax / upload] --> W[Webhook]
  W --> O[Docling OCR + layout]
  O --> L[LLM classify + extract]
  L --> V[Code checks: NPI, DOB, ICD-10, required fields, confidence]
  V -->|clean Rx / card| BV[Benefits verification queue]
  V -->|clean PA| PA[Prior authorization queue]
  V -->|missing / invalid| MI[Outreach queue + drafted request]
  V -->|patient letter| CC[Customer care ticket]
  V -->|low confidence / unknown| MR[Manual review]
  V --> K[(KPI log, no PHI)]
```
Removed steps: manual classification, re-keying, NPI lookup, writing the outreach request. Humans keep: judgement calls, clinical questions, payer calls.
