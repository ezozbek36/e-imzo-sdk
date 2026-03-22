# Q-001 — challenge vs challange

## Status

`resolved-editorially`

## Question

Are `challenge` and `challange` safe to treat as the same field across the current repository evidence?

## Why it matters

This affects terminology, payload examples, and whether RFCs can normalize a single canonical field name without hiding source drift.

## Evidence reviewed

- `docs/validation/open-questions.md`
- `docs/validation/conformance-plan.md`
- `docs/phases/phase-1/canonical.md`
- `docs/inventories/entities.md`
- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/rfc/RFC-0003-signing-and-authentication-flows.md`
- `docs/rfc/RFC-0005-mobile-flow.md`
- `vendor/e-imzo-resources/e-imzo-doc/README.md`
- `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/index.php`
- `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/eimzoidcard/index.html`

## Comparison artifact

| surface / layer              | source                                                                          | observed literal field/key | observed role                                                                 | classification |
| ---------------------------- | --------------------------------------------------------------------------------| -------------------------- | ----------------------------------------------------------------------------- | -------------- |
| Desktop auth challenge issue | `vendor/e-imzo-resources/e-imzo-doc/README.md` (`/frontend/challenge`)          | `challenge`                | Random signing input returned for later PKCS#7 creation and backend auth.     | [FACT]         |
| Desktop auth demo            | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/index.php`              | `data.challenge`           | Browser callback consumes the same challenge value before local signing.      | [FACT]         |
| Mobile auth init response    | `vendor/e-imzo-resources/e-imzo-doc/README.md` (`/frontend/mobile/auth`)        | `challange`                | Returned signing input alongside `siteId` and `documentId`.                   | [FACT]         |
| Mobile auth prose            | `vendor/e-imzo-resources/e-imzo-doc/README.md` (`/frontend/mobile/auth`)        | `challenge`                | Prose describes the same returned signing input the user must sign on mobile. | [FACT]         |
| Mobile auth demo             | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/eimzoidcard/index.html` | `response.challange`       | Mobile demo reads the returned value and passes it into QR/deeplink creation. | [FACT]         |

## Findings

- [FACT] Desktop/server challenge issuance materials use the literal JSON field name `challenge`.
- [FACT] Reviewed mobile auth-init materials use the literal response field name `challange`, and the mobile demo reads `response.challange`.
- [FACT] The same mobile manual section that shows response field `challange` describes that value in prose as `challenge`.
- [INFERENCE] Across the reviewed desktop and mobile auth-init sources, `challenge` and `challange` refer to the same signing-input role rather than two distinct payload concepts.
- [FACT] Current repository evidence does not justify treating one literal wire field name as authoritative across all challenge-oriented payloads.
- [UNKNOWN] The repository still lacks version-tagged runtime fixtures proving whether every mobile/versioned payload uses the same spelling/shape outside the reviewed manual and demo materials.

## Conclusion

The question can be closed editorially.

The repository can safely treat `challenge` as the canonical editorial term for the signing-input concept and `challange` as an observed source spelling variant for that same role in reviewed mobile auth-init materials.

The repository should not treat `challenge` and `challange` as one uniform literal wire field name. Concrete payload examples should preserve the source spelling that was actually observed.

## Resolution type

`resolved-editorially`

## Repository changes applied

- `docs/inventories/glossary.md`
- `docs/inventories/entities.md`
- `docs/rfc/RFC-0005-mobile-flow.md`
- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/validation/open-questions.md`

## Next step

No additional work is required for editorial closure.

If later RFCs need a wire-level compatibility claim, collect version-tagged fixtures to confirm whether mobile payload spelling and shape remain stable outside the reviewed examples.

## Last updated

2026-03-22
