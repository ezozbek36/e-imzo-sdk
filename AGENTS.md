# AGENTS.md

## Project

This repository supports a clean-room documentation and specification effort for E-IMZO integrations.

The project goal is to build a **clean-room abstraction over observed behavior** and then use that specification as the foundation for future multi-language SDK implementations.

This repository is currently a **research/specification workspace**, not the final SDK implementation repository.

---

## Primary goals

1. Collect and normalize evidence about E-IMZO integration behavior.
2. Produce a descriptive specification before building abstractions.
3. Separate observed behavior from inferred behavior.
4. Build RFC-style documents that can later support:
   - reference implementations,
   - conformance tests,
   - multi-language SDKs.
5. Avoid undocumented claims and avoid copying accidental design flaws into the future abstraction.

---

## Non-goals

Agents working in this repository must **not** do the following unless explicitly requested:

- Do not perform reverse engineering.
- Do not decompile binaries.
- Do not invent hidden protocol details.
- Do not claim guarantees not supported by repository evidence.
- Do not write marketing copy instead of technical analysis.
- Do not prematurely implement production SDKs.
- Do not collapse descriptive spec and proposed abstraction into one layer.
- Do not treat examples as universal truth without checking broader evidence.
- Do not assume cryptographic correctness from naming alone.

---

## Project stance

This project is:

- **clean-room**
- **descriptive-first**
- **evidence-driven**
- **specification-first**
- **implementation-later**
- **multi-language by design**
- **AI-assisted, but test-validated**

This project is not:

- a reverse-engineering effort
- an unofficial clone of existing SDK code
- a speculative redesign disconnected from observed behavior

---

## Source hierarchy

When analyzing repository contents, treat sources with different confidence levels.

### Highest-confidence sources

Use these first for observable surface area:

1. Generated API documentation
2. Runnable demos / integration examples
3. Main integration manuals
4. Structured operational notes / troubleshooting notes

### Interpretation rule

- Generated API docs are strong evidence for names and surfaced operations.
- Demos are strong evidence for real integration flows.
- Manuals are useful but may be incomplete or ambiguous.
- Knowledge-base notes are useful for operational behavior and edge cases, but may be informal.

Agents must explicitly distinguish:

- **fact**
- **inference**
- **unknown**
- **conflict**

---

## Required analytical model

Every substantial output must separate the following layers:

### 1. Observed behavior

What is directly evidenced by repository materials.

### 2. Normalized terminology

A cleaned-up vocabulary that maps aliases and inconsistent naming to canonical terms.

### 3. Proposed abstraction

A future-facing internal model for SDK/spec design.
This must be clearly labeled as proposed and must not be confused with source truth.

### 4. Unknowns / unresolved questions

Details that remain underspecified or contradictory.

Do not merge these layers.

---

## Output conventions

Unless explicitly instructed otherwise, agents should produce outputs in markdown with:

- clear section headers
- tables for terms, entities, actors, operations, and errors
- explicit labels:
  - `[FACT]`
  - `[INFERENCE]`
  - `[UNKNOWN]`
  - `[CONFLICT]`

For RFC drafts, use:

- `[OBSERVED]`
- `[NORMALIZED]`
- `[OPEN QUESTION]`
- `[PROPOSED ABSTRACTION]`

---

## Repository workstreams

Agents may contribute to one of the following workstreams:

### A. Source digestion

Examples:

- summarize source files
- extract operations
- extract entities
- identify contradictions
- build terminology maps

### B. Phase 1 outputs

Examples:

- domain model
- actor model
- capability inventory
- state/lifecycle notes
- error surface inventory

### C. Phase 2 outputs

Examples:

- RFC-0001 Terminology and Architecture
- RFC-0002 Local Bridge / Local Crypto Service
- RFC-0003 Signing and Authentication Flows
- RFC-0004 Verification / Timestamp / Certificate Operations
- RFC-0005 Mobile Flow
- RFC-0006 Preliminary Error Model

### D. Validation scaffolding

Examples:

- conformance test ideas
- fixture inventories
- edge-case lists
- compatibility matrix drafts

### E. Future implementation preparation

Only after spec maturity:

- reference API shape proposals
- language-specific mapping notes
- SDK package boundaries

---

## Rules for claims

Agents must follow these claim rules strictly:

1. Do not state a claim as fact unless supported by repository evidence.
2. If a point is inferred from multiple sources, label it as inference.
3. If sources disagree, surface the conflict explicitly.
4. If details are missing, say unknown.
5. Do not fill gaps with prior assumptions about PKCS#7, X.509, timestamping, trust stores, mobile flows, or certificate validation.
6. Do not assume local and remote responsibilities unless clearly evidenced.

---

## Rules for terminology

Agents should normalize terminology carefully.

- Preserve original source names when quoting or mapping operations.
- Introduce canonical names only when useful.
- Keep an alias map.
- Do not silently rename protocol concepts.
- Do not over-normalize if two terms might represent different concepts.

Example:
If sources use multiple names for the same idea, represent:

- canonical term
- source aliases
- evidence
- confidence

---

## Rules for RFC drafting

When drafting RFCs:

1. Start descriptive, not normative.
2. Include a clear purpose and non-goals.
3. State the evidence base.
4. Separate observed behavior from proposed abstraction.
5. List open questions explicitly.
6. Include “risk of incorrect assumptions”.
7. Do not fake protocol precision where sources are vague.
8. Prefer small RFCs over a single giant document.

---

## Rules for AI-assisted work

AI agents are allowed and encouraged to help with:

- extracting structure from messy docs
- mapping terminology
- drafting RFC skeletons
- clustering errors and edge cases
- producing comparison tables
- generating checklists
- identifying ambiguities
- translating descriptive findings into future implementation notes

AI agents must not be treated as authoritative for:

- cryptographic correctness
- undocumented protocol guarantees
- legal/compliance conclusions
- security guarantees
- exact runtime behavior without evidence

All AI-generated outputs should be reviewable against repository evidence.

---

## Rules for code generation

Unless explicitly requested, do not generate SDK implementation code.

If code generation is requested later:

- start with interfaces/types, not full business logic
- prefer reference implementation over many ports at once
- require conformance fixtures
- require clear mapping back to spec documents

Specification precedes implementation.

---

## Preferred working sequence

Agents should generally follow this order:

1. inventory sources
2. extract observed operations
3. build terminology map
4. define actors and boundaries
5. define entities
6. define capabilities
7. define lifecycles / state transitions
8. inventory errors and ambiguities
9. draft RFCs
10. propose validation/conformance plan
11. only then discuss SDK surface

Do not skip directly to SDK design.

---

## Pull request / patch expectations

When contributing changes:

- keep changes scoped
- avoid mixing refactors with new analysis
- explain what evidence the change is based on
- note whether a statement is fact, inference, or open question
- prefer additive documentation over destructive rewriting unless cleanup is requested

---

## Default review checklist

Before finalizing any output, agents should verify:

- Is every nontrivial statement evidence-backed?
- Are facts and inferences clearly separated?
- Are unknowns called out explicitly?
- Are conflicts surfaced instead of silently resolved?
- Is descriptive behavior separated from proposed abstraction?
- Does the output avoid reverse-engineering language?
- Does it avoid premature implementation detail?
- Would a future SDK author be able to use this safely?

---

## Mission statement

Document what is observed.
Name what is ambiguous.
Normalize only with discipline.
Design abstractions only after the descriptive layer is stable.
