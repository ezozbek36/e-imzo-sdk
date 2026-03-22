# Q-002 — pkcs7b64 vs pkcs7_64

## Status

`resolved-editorially`

## Question

Are `pkcs7b64` and `pkcs7_64` safe to treat as the same payload role across local and server materials?

## Why it matters

This affects cross-layer terminology, signing flow descriptions, and any future fixture naming.

## Evidence reviewed

- `docs/validation/open-questions.md`
- `docs/validation/conformance-plan.md`
- `docs/phases/phase-1/canonical.md`
- `docs/inventories/glossary.md`
- `docs/inventories/entities.md`
- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/rfc/RFC-0002-local-bridge.md`
- `docs/rfc/RFC-0003-signing-and-authentication-flows.md`
- `docs/rfc/RFC-0004-verification-timestamp-certificate.md`
- `vendor/e-imzo-resources/apidoc/API.v-4.73.md`
- `vendor/e-imzo-resources/apidoc/API.v-5.00.md`
- `vendor/e-imzo-resources/e-imzo-doc/README.md`
- `vendor/e-imzo-resources/e-imzo-doc/client/java/README.md`
- `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/e-imzo-client.js`
- `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/cabinet.php`
- `vendor/e-imzo-resources/knowledge-base/migration-guide.md`

## Comparison artifact

| surface / layer                 | source                                                                    | observed literal field/key | observed role                                                                                       | classification |
| ------------------------------- | ------------------------------------------------------------------------- | -------------------------- | --------------------------------------------------------------------------------------------------- | -------------- |
| Local browser-wrapper example   | `vendor/e-imzo-resources/e-imzo-doc/README.md`                            | `data.pkcs7_64`            | Local `create_pkcs7` success field carrying base64 PKCS#7 produced by the local runtime.            | [FACT]         |
| Local demo wrapper              | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/e-imzo-client.js` | `data.pkcs7_64`            | Local `create_pkcs7` callback value forwarded into later auth/timestamp flows.                      | [FACT]         |
| Local API snapshots v4.73/v5.00 | `vendor/e-imzo-resources/apidoc/API.v-4.73.md`, `API.v-5.00.md`           | `pkcs7_64`                 | Local `pkcs7` plugin argument name for existing PKCS#7 material in verify/append/timestamp helpers. | [FACT]         |
| Server timestamp response       | `vendor/e-imzo-resources/e-imzo-doc/README.md`                            | `pkcs7b64`                 | Server REST field for the timestamp-attached PKCS#7 returned by `/frontend/timestamp/pkcs7`.        | [FACT]         |
| Server timestamp demo           | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/cabinet.php`      | `data.pkcs7b64`            | Demo reads server timestamp response field and passes it into later verify/archive logic.           | [FACT]         |
| Server verify/auth Java client  | `vendor/e-imzo-resources/e-imzo-doc/client/java/README.md`                | `pkcs7b64`                 | Java client variable name for base64 PKCS#7 submitted to `/backend/auth` and verify endpoints.      | [FACT]         |

## Findings

- [FACT] Reviewed local API docs/examples use the literal spelling `pkcs7_64` for base64 PKCS#7 material on the local side.
- [FACT] Reviewed server REST docs/examples use the literal spelling `pkcs7b64` for base64 PKCS#7 material on the server side.
- [FACT] The reviewed Java server client uses `pkcs7b64` not only for timestamped results, but also for PKCS#7 values submitted into `/backend/auth` and `/backend/pkcs7/verify/*`.
- [FACT] The migration guidance moves timestamping and verification from older local `pkcs7.*` helpers toward server REST endpoints, which increases the chance that one flow crosses both spellings.
- [INFERENCE] Across the reviewed materials, `pkcs7_64` and `pkcs7b64` are safe to treat as source-variant field names for the broader editorial concept “base64 PKCS#7 payload.”
- [FACT] Current repository evidence does not justify treating `pkcs7_64` and `pkcs7b64` as one uniform literal wire field name.
- [FACT] Current repository evidence also does not justify assuming every `pkcs7b64` value is semantically identical to every `pkcs7_64` value at every flow step; some reviewed server examples specifically describe timestamp-attached or attached-server results.
- [UNKNOWN] The repository still lacks version-tagged runtime fixtures proving whether all currently deployed versions/layers preserve this naming pattern outside the reviewed docs, demos, and Java examples.

## Conclusion

The question can be closed editorially.

The repository can safely use editorial `PKCS#7 base64 payload` for the shared cross-layer concept and treat `pkcs7_64` and `pkcs7b64` as observed source-variant field names for that concept in the reviewed materials.

The repository should not collapse them into one literal wire field name, and it should preserve step-specific meanings when sources say more, such as server timestamp responses returning a timestamp-attached PKCS#7.

## Resolution type

`resolved-editorially`

## Repository changes applied

- `docs/inventories/glossary.md`
- `docs/inventories/entities.md`
- `docs/rfc/RFC-0001-terminology-and-architecture.md`
- `docs/rfc/RFC-0002-local-bridge.md`
- `docs/rfc/RFC-0003-signing-and-authentication-flows.md`
- `docs/validation/conformance-plan.md`
- `docs/validation/open-questions.md`

## Next step

No additional work is required for editorial closure.

If later RFCs need wire-level compatibility claims, collect version-tagged fixtures showing actual local responses and server request/response bodies for auth, timestamp, and verify flows.

## Last updated

2026-03-22
