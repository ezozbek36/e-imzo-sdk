# RFC-0004 — Verification, Timestamp, and Certificate-Related Operations

## Status

[OBSERVED] Draft (initial descriptive partition).

## Purpose

[OBSERVED] Consolidate observed verification/timestamp/certificate-related operations and clarify local-vs-remote operational placement.

## Non-goals

- [OBSERVED] No claim of full PKI policy semantics beyond observed status/result fields.
- [OBSERVED] No independent trust model redesign.
- [OBSERVED] No cryptographic algorithm validation claims.

## Evidence base

| Tag        | Source                                                                             | Why used                                                                |
| ---------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/README.md`                                     | Primary verify/timestamp endpoint contracts and response structures.    |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/cabinet.php`, `verify.php` | Practical endpoint invocation shapes.                                   |
| [OBSERVED] | `vendor/e-imzo-resources/apidoc/API.v-4.73.md`, `API.v-5.00.md`, `API.v-6.3.md`    | Local verify/timestamp/truststore-related helper method presence drift. |
| [OBSERVED] | `vendor/e-imzo-resources/knowledge-base/migration-guide.md`, `integration-faq.md`  | Migration guidance away from deprecated local helpers.                  |

## Descriptive model

### Observed verification operations

| Tag        | Operation                        | Plane                   | Notes                                                                                   |
| ---------- | -------------------------------- | ----------------------- | --------------------------------------------------------------------------------------- |
| [OBSERVED] | `/backend/pkcs7/verify/attached` | Server REST             | Validates attached PKCS#7; returns `pkcs7Info` with signer/cert/time/timestamp details. |
| [OBSERVED] | `/backend/pkcs7/verify/detached` | Server REST             | Detached variant; expects `data64|pkcs7` payload style in docs/demo.                    |
| [OBSERVED] | `/backend/mobile/verify`         | Server/mobile           | Final verification in mobile sign flow; returns cert + verification info.               |
| [OBSERVED] | Local `pkcs7.verify_*` helpers   | Local (older snapshots) | Present in v4/v5 snapshots; deprecation/removal direction in newer notes/v6.            |

### Timestamp-related operations

| Tag             | Operation                            | Plane                   | Notes                                                                        |
| --------------- | ------------------------------------ | ----------------------- | ---------------------------------------------------------------------------- |
| [OBSERVED]      | `/frontend/timestamp/pkcs7`          | Server REST             | Attaches timestamp token to PKCS#7; returns `pkcs7b64` and signer list.      |
| [OBSERVED]      | Local `attach_timestamp_token_pkcs7` | Local (older snapshots) | Present in older apidoc snapshots; migration docs recommend server endpoint. |
| [OPEN QUESTION] | Timestamp source/policy controls     | Server/external         | Source docs show outputs and TSA metadata but not full control contract.     |

### Certificate/truststore-related operations

| Tag             | Surface                                                                                  | Plane       | Notes                                                            |
| --------------- | ---------------------------------------------------------------------------------------- | ----------- | ---------------------------------------------------------------- |
| [OBSERVED]      | Verify responses include cert chain, OCSP response, validity windows, policy identifiers | Server REST | Evidenced in `/backend/pkcs7/verify/attached` response examples. |
| [OBSERVED]      | Local `x509`, `crl`, `truststore`, `truststore-jks` plugin families                      | Local       | Present in older snapshots; module set varies by version.        |
| [OPEN QUESTION] | Required use of local truststore helpers in modern flows                                 | Cross-layer | Not consistently specified in current docs.                      |

### What appears local vs remote

| Tag             | Function class                                    | Appears local                        | Appears remote                                            |
| --------------- | ------------------------------------------------- | ------------------------------------ | --------------------------------------------------------- |
| [OBSERVED]      | PKCS#7 creation                                   | yes (`create_pkcs7`)                 | no direct equivalent in auth/sign initiation              |
| [OBSERVED]      | Timestamp attachment                              | legacy local helper in old snapshots | yes (`/frontend/timestamp/pkcs7`)                         |
| [OBSERVED]      | Signature verification                            | legacy local helper in old snapshots | yes (`/backend/pkcs7/verify/*`, `/backend/mobile/verify`) |
| [OBSERVED]      | Certificate result materialization                | partly local via helper plugins      | rich structured server responses                          |
| [OPEN QUESTION] | definitive authority in mixed-version deployments | unclear                              | unclear                                                   |

### Operational dependencies if evidenced

| Tag        | Dependency                             | Where it appears                                  |
| ---------- | -------------------------------------- | ------------------------------------------------- |
| [OBSERVED] | VPN connectivity/key material          | server launch/config and troubleshooting sections |
| [OBSERVED] | `Host` and `X-Real-IP` forwarding      | endpoint docs and demo backend relays             |
| [OBSERVED] | Redis for clustered/mobile state paths | server config and mobile status codes             |
| [OBSERVED] | CA material updates (`jar`/`lib`)      | migration + troubleshooting notes                 |

### What is explicitly unclear

| Tag             | Unclear point                                                                                      |
| --------------- | -------------------------------------------------------------------------------------------------- |
| [OPEN QUESTION] | Detached verify section has a conflicting curl URL target in manual examples.                      |
| [OPEN QUESTION] | Full cross-version responsibility matrix between local verify helpers and server verify endpoints. |
| [OPEN QUESTION] | Stability of local certificate/truststore plugin methods in v6+ environments.                      |

### Proposed abstraction boundary

| Tag                    | Boundary                                                                                                            |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------- |
| [PROPOSED ABSTRACTION] | Treat server verification outputs as canonical verification artifact for app decisions in current recommended flow. |
| [PROPOSED ABSTRACTION] | Keep local certificate helper capabilities as optional/versioned extension plane, not baseline contract.            |

## Open questions

- [OPEN QUESTION] Detached verify sample conflict (section target vs curl URL) and authoritative call shape.
- [OPEN QUESTION] Explicit remote/local division of certificate-status checks across all versions.
- [OPEN QUESTION] Whether timestamp token source and policy behavior are configurable in all deployments.

## Risk of incorrect assumptions

- [OPEN QUESTION] Assuming local helper deprecations imply universal absence in all installed clients.
- [OPEN QUESTION] Assuming all verify responses always contain identical nested fields.
- [OPEN QUESTION] Assuming truststore plugin behavior is equivalent to server trust configuration.

## Next validation steps

1. [OBSERVED] Record endpoint fixture sets for attached and detached verification including failing statuses.
2. [OBSERVED] Build a “local helper availability by version” annex from `API.v-*`.
3. [OBSERVED] Validate operational dependency diagnostics (`/ping`, `/info`, VPN mismatch scenarios) against troubleshooting guidance.
