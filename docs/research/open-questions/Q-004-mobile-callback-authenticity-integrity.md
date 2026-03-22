# Q-004 — mobile callback authenticity and integrity

## Status

`open`

## Question

What, if anything, do the current repository materials show about authenticity and integrity checks for the mobile upload callback?

## Why it matters

This affects trust-boundary language, backend integration guidance, and whether the mobile callback can be treated as a stronger signal than the sources support.

## Evidence reviewed

- `docs/validation/open-questions.md`
- `docs/phases/phase-1/canonical.md`
- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/rfc/RFC-0005-mobile-flow.md`

## Findings

- [FACT] The repository already records that the callback protection model is unclear.
- [UNKNOWN] The current evidence is not enough to state a stable callback-authenticity contract.

## Conclusion

No safe conclusion yet; keep callback-protection claims limited.

## Resolution type

`unresolved`

## Repository changes needed

- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/rfc/RFC-0005-mobile-flow.md`

## Next step

Collect explicit callback-protection statements or working examples from current repository materials.

## Last updated

2026-03-22
