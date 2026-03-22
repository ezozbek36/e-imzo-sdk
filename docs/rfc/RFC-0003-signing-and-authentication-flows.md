# RFC-0003 — Signing and Authentication Flows

## Status

[OBSERVED] Draft (initial flow normalization).

## Purpose

[OBSERVED] Describe evidenced signing/authentication flows and boundaries between local signing and server/mobile verification.

## Non-goals

- [OBSERVED] No normative replay protection claims beyond documented status semantics.
- [OBSERVED] No cryptographic proof claims beyond endpoint/result fields.
- [OBSERVED] No redesign of signing flow choreography.

## Evidence base

| Tag        | Source                                                                             | Why used                                                      |
| ---------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| [OBSERVED] | `docs/phases/phase-1/canonical.md`                                                 | Consolidated flow findings and ambiguities.                   |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/README.md`                                     | Sequence narratives and endpoint behavior.                    |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/index.php`, `auth.php`     | Challenge-based auth example.                                 |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/cabinet.php`, `verify.php` | Signing + timestamp + attached/detached verification example. |
| [OBSERVED] | `vendor/e-imzo-resources/knowledge-base/migration-guide.md`                        | Multi-signer join and local-method deprecation notes.         |

## Descriptive model

### Signing-related flows evidenced by docs/demo

| Tag        | Flow                                    | Evidence                                                                               |         |
| ---------- | --------------------------------------- | -------------------------------------------------------------------------------------- | ------- |
| [OBSERVED] | Desktop challenge authentication        | `/frontend/challenge` -> local `create_pkcs7` -> `/backend/auth`.                      |         |
| [OBSERVED] | Desktop document signing with timestamp | local `create_pkcs7` -> `/frontend/timestamp/pkcs7` -> `/backend/pkcs7/verify/*`.      |         |
| [OBSERVED] | Attached vs detached verification paths | Demo toggles `attached` vs `detached` and posts either PKCS#7 only or `data64|pkcs7`. |
| [OBSERVED] | Multi-signer composition guidance       | `/frontend/pkcs7/join` documented in migration guidance for combining signed payloads. |         |

### Challenge-based authentication flow

| Tag             | Step | Description                                                                                        |
| --------------- | ---- | -------------------------------------------------------------------------------------------------- |
| [OBSERVED]      | 1    | Browser calls `/frontend/challenge` and receives `challenge`, `ttl`, `status`.                     |
| [OBSERVED]      | 2    | Browser/local bridge signs `challenge` using selected local key.                                   |
| [OBSERVED]      | 3    | Backend relays PKCS#7 to `/backend/auth` with `Host` and `X-Real-IP`.                              |
| [OBSERVED]      | 4    | Server returns auth status and `subjectCertificateInfo` on success.                                |
| [OPEN QUESTION] | 5    | Replay and strict single-use enforcement are implied by status semantics but not fully formalized. |

### Artifacts produced/consumed

| Tag             | Artifact                                     | Produced by                                        | Consumed by                                                     |
| --------------- | -------------------------------------------- | -------------------------------------------------- | --------------------------------------------------------------- |
| [OBSERVED]      | `challenge` + `ttl`                          | `/frontend/challenge`                              | local signing call and `/backend/auth` validation path.         |
| [OBSERVED]      | Local PKCS#7 (base64)                        | local `create_pkcs7`                               | `/backend/auth` or timestamp endpoint.                          |
| [OBSERVED]      | Timestamped PKCS#7 (`pkcs7b64`)              | `/frontend/timestamp/pkcs7`                        | verification endpoints and app archive logic in demo narrative. |
| [OBSERVED]      | Verification result (`pkcs7Info`, statuses)  | `/backend/pkcs7/verify/*`                          | backend acceptance/rejection logic.                             |
| [OPEN QUESTION] | Stable canonical field names in all versions | mixed docs/examples (`pkcs7b64`, `pkcs7_64`, etc.) | unresolved normalization need.                                  |

### Boundaries between local signing and remote verification

| Tag             | Boundary                                                        | Description                                                                              |
| --------------- | --------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| [OBSERVED]      | Local-only signing step                                         | Private-key operation and PKCS#7 creation occur in local runtime flow descriptions/demo. |
| [OBSERVED]      | Server verification/timestamp step                              | Server endpoints perform timestamp attach and verification result formation.             |
| [OBSERVED]      | Backend policy step                                             | Backend decides session/document acceptance based on status+payload.                     |
| [OPEN QUESTION] | Full authoritative crypto-responsibility matrix across versions | not explicitly formalized in sources.                                                    |

### Attached/detached distinctions

| Tag             | Distinction                 | Evidence                                                                                   |         |
| --------------- | --------------------------- | ------------------------------------------------------------------------------------------ | ------- |
| [OBSERVED]      | Attached                    | Verified via `/backend/pkcs7/verify/attached`; includes embedded document in result model. |         |
| [OBSERVED]      | Detached                    | Verified via `/backend/pkcs7/verify/detached`; payload format includes `data64|pkcs7`. |
| [OPEN QUESTION] | Detached sample consistency | Manual detached section has a conflicting curl URL example.                                |         |

### Flow diagrams in text form

**[OBSERVED] Desktop challenge authentication**

```text
Browser -> /frontend/challenge -> {challenge, ttl, status}
Browser -> Local bridge create_pkcs7(challenge, keyRef) -> {pkcs7}
Browser -> App backend (pkcs7)
App backend -> /backend/auth (pkcs7 + Host + X-Real-IP) -> {status, subjectCertificateInfo}
Backend session logic -> Browser redirect/deny
```

**[OBSERVED] Desktop document signing + verification**

```text
Browser -> Local bridge create_pkcs7(document, keyRef, detached?) -> {pkcs7}
Browser -> /frontend/timestamp/pkcs7(pkcs7) -> {pkcs7b64, timestampedSignerList, status}
Browser/backend -> /backend/pkcs7/verify/attached OR /detached -> {status, message, pkcs7Info}
App backend/business layer -> accept/reject + archive result
```

**[OBSERVED] Multi-signer (documented guidance path)**

```text
Signer A/B/... each produce timestamped attached PKCS#7
Verifier checks each signed payload
Integrator calls /frontend/pkcs7/join iteratively
Result -> combined PKCS#7 with multiple signatures
```

### Proposed abstraction boundary

| Tag                    | Boundary                                                                                                            |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------- |
| [PROPOSED ABSTRACTION] | Separate `AuthChallengeFlow` and `DocumentSignFlow` as distinct descriptive processes sharing local-sign primitive. |
| [PROPOSED ABSTRACTION] | Model attached/detached as payload transport mode, not as separate cryptographic trust model.                       |

## Open questions

| Tag             | Question                                                                                                    |
| --------------- | ----------------------------------------------------------------------------------------------------------- |
| [OPEN QUESTION] | Is challenge strictly single-use across all deployments or only implied by status `-20` and narrative text? |
| [OPEN QUESTION] | How replay prevention is guaranteed beyond challenge lookup + expiry.                                       |
| [OPEN QUESTION] | Idempotency guarantees for repeated verify/timestamp calls.                                                 |
| [OPEN QUESTION] | Exact method/documentation mismatch handling strategy for detached verify sample URL conflict.              |

## Risk of incorrect assumptions

- [OPEN QUESTION] Assuming all flows must always include timestamping before verification.
- [OPEN QUESTION] Assuming consistent TTL or challenge reuse policy beyond documented endpoint behavior.
- [OPEN QUESTION] Assuming join flow semantics equal old local append methods one-to-one.

## Next validation steps

1. [OBSERVED] Capture request/response fixtures for attached and detached verification from demo runs.
2. [OBSERVED] Add explicit lifecycle tests for expired challenge and repeated challenge submission.
3. [OBSERVED] Validate join flow behavior with two and three signer samples.
