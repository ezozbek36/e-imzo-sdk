# Q-005 — mobile authenticate method mismatch

## Status

`open`

## Question

Which HTTP method is actually supported for `/backend/mobile/authenticate` in the current repository evidence?

## Why it matters

This affects the documented mobile finalization flow and whether RFC-0005 can present one method without hiding a source conflict.

## Evidence reviewed

- `docs/validation/open-questions.md`
- `docs/validation/conformance-plan.md`
- `docs/rfc/RFC-0005-mobile-flow.md`

## Findings

- [FACT] The repository already records a method mismatch for this route.
- [CONFLICT] Current narrative and example materials do not align cleanly.

## Conclusion

No safe conclusion yet; keep the method conflict visible.

## Resolution type

`unresolved`

## Repository changes needed

- `docs/rfc/RFC-0005-mobile-flow.md`

## Next step

Preserve the conflicting method examples side by side with source references.

## Last updated

2026-03-22
