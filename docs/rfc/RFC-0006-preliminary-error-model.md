# RFC-0006 — Preliminary Error Model

## Status

[OBSERVED] Draft (preliminary; normalization layer explicitly non-authoritative).

## Purpose

[OBSERVED] Organize observed failures across local bridge, server REST, mobile async flow, and operational environment; define a tentative normalized taxonomy for later validation work.

## Non-goals

- [OBSERVED] No final universal error code schema.
- [OBSERVED] No claim that all errors map one-to-one to currently observed codes.
- [OBSERVED] No replacement of source status tables.

## Evidence base

| Tag        | Source                                                                                           | Why used                                         |
|------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------|
| [OBSERVED] | `Phase1.md`                                                                                      | Baseline error classes and contradictions.       |
| [OBSERVED] | `e-imzo-resources/e-imzo-doc/README.md`                                                          | Official status tables and HTTP transport notes. |
| [OBSERVED] | `e-imzo-resources/e-imzo-doc/example.uz/php/demo/e-imzo-init.js`, `e-imzo.js`                    | Local websocket/runtime error handling examples. |
| [OBSERVED] | `e-imzo-resources/knowledge-base/troubleshooting.md`, `integration-faq.md`, `migration-guide.md` | Operational and migration-era failure patterns.  |

## Descriptive model

### Observed failure classes

| Tag        | Class                        | Examples                                                                                                                             |
|------------|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| [OBSERVED] | Local transport/runtime      | Browser lacks WebSocket, localhost bridge unavailable, websocket close/error, wrong-password handling patterns.                      |
| [OBSERVED] | HTTP transport-level         | Endpoint docs repeatedly call out HTTP 400 (bad request params) and HTTP 503 (server/log issue).                                     |
| [OBSERVED] | Server validation-level      | `/backend/auth` and `/backend/pkcs7/verify/*` negative statuses (`-1`, `-5`, `-10`, `-11`, `-12`, `-20`, `-21`, `-22`, `-23`).       |
| [OBSERVED] | Mobile async/state-level     | `/frontend/mobile` and `/backend/mobile` statuses (`2` pending, Redis/state and verification failures in negative range).            |
| [OBSERVED] | Operational/dependency-level | VPN mismatch, CA package staleness, Redis connectivity/state expiry, payload encoding mismatches, platform/service readiness issues. |

### Transport-level vs validation-level vs operational-level failures

| Tag             | Partition                      | Evidence shape                                                                                                |
|-----------------|--------------------------------|---------------------------------------------------------------------------------------------------------------|
| [NORMALIZED]    | Transport                      | WebSocket connect/close, HTTP 400/503, connectivity to upstream services.                                     |
| [NORMALIZED]    | Validation                     | Signature/certificate/time-window/status-table failures from backend/mobile verification APIs.                |
| [NORMALIZED]    | Operational                    | Environment mismatch, outdated server package, proxy/header misconfiguration, onboarding/reachability issues. |
| [OPEN QUESTION] | Cross-layer composite failures | Many production incidents combine transport + validation + operational root causes.                           |

### Ambiguities

| Tag             | Ambiguity                                                                                                |
|-----------------|----------------------------------------------------------------------------------------------------------|
| [OPEN QUESTION] | Some negative codes are generic “see logs” buckets, not semantically specific.                           |
| [OPEN QUESTION] | Local runtime textual errors are not mapped to a stable numeric taxonomy in docs.                        |
| [OPEN QUESTION] | Interactions between expiry semantics (`challenge` TTL vs mobile `documentId` lifetime) are not unified. |

### [PROPOSED ABSTRACTION] Preliminary normalized taxonomy

| Tag                    | Proposed category   | Source-mapped examples                               | Notes                                                  |
|------------------------|---------------------|------------------------------------------------------|--------------------------------------------------------|
| [PROPOSED ABSTRACTION] | `E.LOCAL.TRANSPORT` | websocket unavailable/closed, no local module        | Browser/local runtime channel failures.                |
| [PROPOSED ABSTRACTION] | `E.LOCAL.RUNTIME`   | plugin call failed (`success=false`, `reason`)       | Local function execution/validation issues.            |
| [PROPOSED ABSTRACTION] | `E.SERVER.HTTP`     | HTTP 400/503 from REST calls                         | Transport/protocol envelope failures.                  |
| [PROPOSED ABSTRACTION] | `E.SERVER.AUTH`     | `/backend/auth` status negatives                     | Challenge/auth validation failures.                    |
| [PROPOSED ABSTRACTION] | `E.SERVER.VERIFY`   | `/backend/pkcs7/verify/*` status negatives           | Signature/timestamp/certificate verification failures. |
| [PROPOSED ABSTRACTION] | `E.MOBILE.STATE`    | mobile `status=2`, `-1`, `-2` etc.                   | Async/persistence/state progression issues.            |
| [PROPOSED ABSTRACTION] | `E.MOBILE.VERIFY`   | `/backend/mobile` verification/time-window negatives | Mobile finalize verification failures.                 |
| [PROPOSED ABSTRACTION] | `E.OPS.DEPENDENCY`  | VPN/Redis/CA/proxy/platform issues                   | Operational environment dependency failures.           |
| [PROPOSED ABSTRACTION] | `E.DATA.ENCODING`   | invalid base64, payload mismatch/digest mismatch     | Request construction/encoding errors.                  |

## Open questions

- [OPEN QUESTION] Which observed errors should be treated as retriable vs terminal?
- [OPEN QUESTION] How to standardize logging correlation across local/frontend/backend/mobile steps.
- [OPEN QUESTION] Whether status code meanings drift by server version and deployment profile.

## Risk of incorrect assumptions

- [OPEN QUESTION] Conflating source status codes with final SDK/domain error types before fixture validation.
- [OPEN QUESTION] Treating knowledge-base operational patterns as strict protocol-level guarantees.
- [OPEN QUESTION] Overfitting taxonomy to currently visible demo/manual paths only.

## Next validation steps

1. [OBSERVED] Build error fixture inventory grouped by endpoint + layer + dependency precondition.
2. [OBSERVED] Add trace IDs across demo relay points to map multi-layer failures.
3. [OBSERVED] Validate taxonomy against real failing scenarios (VPN down, Redis unavailable, invalid base64, expired challenge/documentId).
