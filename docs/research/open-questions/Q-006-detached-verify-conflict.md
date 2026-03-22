# Q-006 — detached verify conflict

## Status

`open`

## Question

Which detached verification endpoint shape is actually supported in the current repository evidence?

## Why it matters

This affects the verification flow description and whether the repository can state one detached-verify pattern without hiding a source conflict.

## Evidence reviewed

- `docs/validation/open-questions.md`
- `docs/validation/conformance-plan.md`
- `docs/rfc/RFC-0003-signing-and-authentication-flows.md`
- `docs/rfc/RFC-0004-verification-timestamp-certificate.md`

## Findings

- [FACT] The repository already records a detached verification documentation conflict.
- [CONFLICT] Manual text and example material do not yet yield one stable detached-verify call shape.

## Conclusion

No safe conclusion yet; keep the detached-verify conflict visible.

## Resolution type

`unresolved`

## Repository changes needed

- `docs/rfc/RFC-0003-signing-and-authentication-flows.md`
- `docs/rfc/RFC-0004-verification-timestamp-certificate.md`

## Next step

Preserve the conflicting detached-verify endpoint examples side by side with source references.

## Last updated

2026-03-22
