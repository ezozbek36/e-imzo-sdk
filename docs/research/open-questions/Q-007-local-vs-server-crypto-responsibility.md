# Q-007 — local vs server crypto responsibility

## Status

`open`

## Question

How should the repository describe the observed split between local and server responsibility for signing, timestamping, verification, and certificate-related work?

## Why it matters

This affects actor boundaries, capability inventories, and multiple RFCs that currently treat the split as descriptive and version-sensitive.

## Evidence reviewed

- `docs/validation/open-questions.md`
- `docs/phases/phase-1/canonical.md`
- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/rfc/RFC-0003-signing-and-authentication-flows.md`
- `docs/rfc/RFC-0004-verification-timestamp-certificate.md`

## Findings

- [FACT] The repository already records this split as incomplete and version-sensitive.
- [UNKNOWN] The current materials do not yet provide one settled responsibility matrix.

## Conclusion

No safe conclusion yet; keep the split descriptive rather than definitive.

## Resolution type

`unresolved`

## Repository changes needed

- `docs/phases/phase-1/canonical.md`
- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/rfc/RFC-0004-verification-timestamp-certificate.md`

## Next step

Build a version-sensitive matrix from current API docs, manuals, and demos before narrowing any boundary claims.

## Last updated

2026-03-22
