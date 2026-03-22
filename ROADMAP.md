# ROADMAP

## Now

- Resolve open questions Q-003 through Q-008 tracked in
  [`docs/validation/open-questions.md`](./docs/validation/open-questions.md):
  - Q-003 — one-vs-two mobile-side actors
  - Q-004 — mobile upload callback authenticity/integrity
  - Q-005 — `/backend/mobile/authenticate` GET vs POST mismatch
  - Q-006 — detached verify documentation conflict
  - Q-007 — local-vs-server cryptographic responsibility split
  - Q-008 — status-code semantics drift by version/deployment
- Review RFC-0001 through RFC-0006 for cross-RFC terminology and boundary
  consistency against the glossary and entity inventory.

## Next

- Define an initial reference boundary for `sdk/` based on stabilized spec surface.
- Assign maturity labels per RFC (`draft` / `review` / `stable`).
- Produce a cross-version capability matrix for local plugin API surface (`v4.73`, `v5.00`, `v6.3`).

## Later

- Build a minimal reference implementation from stabilized specs.
- Add a testkit/conformance suite aligned with documented behavior.
- Plan multi-language adapters based on the same conformance surface.

## Done

- Repository structure and canonical/supporting document labeling stabilized.
- Phase 1 domain model complete: actors, entities, capabilities, lifecycle,
  error surface, contradiction inventory (`docs/phases/phase-1/canonical.md`).
- RFC draft set produced: RFC-0001 through RFC-0006.
- Structured inventories produced: glossary, entities, errors (`docs/inventories/`).
- Open-questions tracker established with 8 questions, Q-001 and Q-002
  resolved editorially (`docs/validation/open-questions.md`).
- Research notes directory scaffolded (`docs/research/open-questions/`).
- Independent project positioning: `DISCLAIMER.md` and `LICENSE` added.

## Out of scope (permanently)

- Reverse engineering or decompiling any YT LLC (`yt.uz`) binaries
- Claiming official status or endorsement
- Replacing or competing with the official E-IMZO system
