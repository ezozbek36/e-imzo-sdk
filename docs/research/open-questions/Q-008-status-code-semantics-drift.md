# Q-008 — status code semantics drift

## Status

`open`

## Question

How stable are numeric status meanings across versions, deployment profiles, and operational contexts in the current repository evidence?

## Why it matters

This affects the error inventory, mobile status modeling, and whether RFC-0006 can make stronger claims about stable semantics.

## Evidence reviewed

- `docs/validation/open-questions.md`
- `docs/validation/conformance-plan.md`
- `docs/rfc/RFC-0005-mobile-flow.md`
- `docs/rfc/RFC-0006-preliminary-error-model.md`
- `docs/inventories/errors.md`

## Findings

- [FACT] The repository already records possible drift in status meanings.
- [UNKNOWN] Cross-version and cross-deployment status semantics are not yet closed.

## Conclusion

No safe conclusion yet; keep status semantics descriptive and version-sensitive.

## Resolution type

`unresolved`

## Repository changes needed

- `docs/inventories/errors.md`
- `docs/rfc/RFC-0005-mobile-flow.md`
- `docs/rfc/RFC-0006-preliminary-error-model.md`

## Next step

Compare status meanings across mobile and error-model materials and record where semantics diverge or remain ambiguous.

## Last updated

2026-03-22
