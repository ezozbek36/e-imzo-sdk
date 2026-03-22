# Q-002 — pkcs7b64 vs pkcs7_64

## Status

`open`

## Question

Are `pkcs7b64` and `pkcs7_64` safe to treat as the same payload role across local and server materials?

## Why it matters

This affects cross-layer terminology, signing flow descriptions, and any future fixture naming.

## Evidence reviewed

- `docs/validation/open-questions.md`
- `docs/phases/phase-1/canonical.md`
- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/rfc/RFC-0002-local-bridge.md`
- `docs/rfc/RFC-0003-signing-and-authentication-flows.md`

## Findings

- [FACT] The repository already records field-name drift across local and server materials.
- [UNKNOWN] Cross-version equivalence is not yet closed.

## Conclusion

No safe conclusion yet; keep the field-variant ambiguity visible.

## Resolution type

`unresolved`

## Repository changes needed

- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/rfc/RFC-0002-local-bridge.md`
- `docs/rfc/RFC-0003-signing-and-authentication-flows.md`

## Next step

Compare local signing outputs with server timestamp and verify payload roles using preserved examples.

## Last updated

2026-03-22
