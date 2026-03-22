# Q-001 — challenge vs challange

## Status

`open`

## Question

Are `challenge` and `challange` safe to treat as the same field across the current repository evidence?

## Why it matters

This affects terminology, payload examples, and whether RFCs can normalize a single canonical field name without hiding source drift.

## Evidence reviewed

- `docs/validation/open-questions.md`
- `docs/phases/phase-1/canonical.md`
- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/rfc/RFC-0005-mobile-flow.md`

## Findings

- [FACT] The repository already records spelling drift across source materials.
- [UNKNOWN] Repository-wide equivalence is not yet closed.

## Conclusion

No safe conclusion yet; keep the spelling conflict visible.

## Resolution type

`unresolved`

## Repository changes needed

- `docs/inventories/glossary.md`
- `docs/rfc/RFC-0005-mobile-flow.md`
- `docs/rfc/RFC-0001-terminology-and-architecture.md`

## Next step

Compare representative auth and mobile payload examples by version and record whether the field roles actually align.

## Last updated

2026-03-22
