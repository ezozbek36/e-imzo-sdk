# RFC-0005 — Mobile Flow

## Status

[OBSERVED] Draft (included because Phase 1 provides explicit mobile evidence).

## Purpose

[OBSERVED] Describe observed mobile ID-card async flow, actors, identifiers, and status lifecycle without inferring undocumented guarantees.

## Non-goals

- [OBSERVED] No redesign of deeplink or onboarding process.
- [OBSERVED] No assertion of callback security guarantees not documented.
- [OBSERVED] No mobile SDK design.

## Evidence base

| Tag        | Source                                                                                                             | Why used                                                                       |
| ---------- | ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| [OBSERVED] | `docs/phases/phase-1/canonical.md`                                                                                 | Consolidated mobile entities and ambiguities.                                  |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/README.md`                                                                     | Mobile endpoint descriptions, status code tables, sequence diagrams.           |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/eimzoidcard/index.html`, `js/e-imzo-mobile.js`             | Polling logic, deeplink/QR payload assembly, app redirect/finalization wiring. |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/eimzoidcard/user_auth_result.php`, `doc_verify_result.php` | Backend finalize call examples.                                                |
| [OBSERVED] | `vendor/e-imzo-resources/knowledge-base/integration-faq.md`                                                        | Onboarding and upload URL operational constraints.                             |

## Descriptive model

### Scope

| Tag        | Included                                                                                       |
| ---------- | ---------------------------------------------------------------------------------------------- |
| [OBSERVED] | `/frontend/mobile/{auth,sign,status,upload}` and `/backend/mobile/{authenticate,verify}` flow. |
| [OBSERVED] | Identifier lifecycle (`siteId`, `documentId`, challenge hash input).                           |
| [OBSERVED] | Polling and finalization transitions from demo/manual.                                         |

### Observed mobile actors and identifiers

| Tag        | Item                     | Description                                                                    |
| ---------- | ------------------------ | ------------------------------------------------------------------------------ |
| [OBSERVED] | Frontend app/site        | Initiates auth/sign, generates QR/deeplink payload, polls status.              |
| [OBSERVED] | E-IMZO mobile app        | Consumes deeplink payload and triggers signature flow with ID-card.            |
| [OBSERVED] | ID-CARD mobile subsystem | Uploads PKCS#7 to configured upload URL.                                       |
| [OBSERVED] | App backend              | Calls `/backend/mobile/authenticate/{documentId}` or `/backend/mobile/verify`. |
| [OBSERVED] | `siteId`                 | Correlates onboarding/site configuration.                                      |
| [OBSERVED] | `documentId`             | Correlation key for polling/finalization.                                      |
| [OBSERVED] | `challenge/challange`    | Auth signing input (spelling conflict exists).                                 |

### Lifecycle/state transitions

| Tag             | State transition                  | Trigger                                                                             |
| --------------- | --------------------------------- | ----------------------------------------------------------------------------------- |
| [OBSERVED]      | `init` -> `pending` (`status=2`)  | `auth`/`sign` started; polling begins.                                              |
| [OBSERVED]      | `pending` -> `ready` (`status=1`) | Mobile-side upload completed and status endpoint reflects readiness.                |
| [OBSERVED]      | `ready` -> `finalized`            | Backend calls authenticate/verify endpoint and receives success payload.            |
| [OBSERVED]      | `pending` -> `error` (`status<0`) | Redis/state/validation/time-window failures per status tables.                      |
| [OPEN QUESTION] | `pending` expiry duration         | TTL/retention not fully specified; inferred via `-2` “not found/expired” semantics. |

### Polling/status/finalization

| Tag             | Step                     | Observed behavior                                                                                                     |
| --------------- | ------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| [OBSERVED]      | Polling                  | Demo repeatedly posts `documentId` to `/frontend/mobile/status` every ~1s until success/failure/timeout UI threshold. |
| [OBSERVED]      | Auth finalization        | Backend reads result via `/backend/mobile/authenticate/{documentId}` in demo.                                         |
| [OBSERVED]      | Sign finalization        | Backend posts `{documentId, document(base64)}` to `/backend/mobile/verify`.                                           |
| [OPEN QUESTION] | Authenticate HTTP method | Manual sequence references POST, method example uses GET.                                                             |

### Ambiguities and missing guarantees

| Tag             | Ambiguity                                                                                      |
| --------------- | ---------------------------------------------------------------------------------------------- |
| [OPEN QUESTION] | Formal callback authentication/integrity model for `/frontend/mobile/upload` is not specified. |
| [OPEN QUESTION] | Canonical challenge field spelling/shape across mobile endpoints is inconsistent.              |
| [OPEN QUESTION] | Record lifetime and finalization idempotency for `documentId` is not formally specified.       |
| [OPEN QUESTION] | Several backend-mobile negative statuses map to “see logs” without explicit semantics.         |

### Proposed abstraction boundary

| Tag                    | Boundary                                                                                                          |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------- |
| [PROPOSED ABSTRACTION] | Model mobile flow as async two-phase process: `init + poll` then `backend finalize`.                              |
| [PROPOSED ABSTRACTION] | Keep onboarding (`siteId` issuance, upload URL approval) as operational provisioning layer outside protocol flow. |

## Open questions

- [OPEN QUESTION] Which method is authoritative for `/backend/mobile/authenticate` (POST vs GET mismatch in manual artifacts)?
- [OPEN QUESTION] How upload callback authenticity should be validated in integrator backends.
- [OPEN QUESTION] Whether timeout/retry/backoff behavior has normative bounds beyond demo UX logic.

## Risk of incorrect assumptions

- [OPEN QUESTION] Treating polling timeout UI behavior from demo as protocol guarantee.
- [OPEN QUESTION] Assuming one-to-one mapping between frontend-mobile and backend-mobile status domains.
- [OPEN QUESTION] Assuming callback reachability errors (for example 499 reports) are protocol violations instead of deployment issues.

## Next validation steps

1. [OBSERVED] Capture end-to-end mobile trace fixtures for success, timeout, and record-expired paths.
2. [OBSERVED] Validate method ambiguity for `/backend/mobile/authenticate` against runtime behavior.
3. [OBSERVED] Add explicit checklist for upload URL reachability and reverse-proxy preconditions.
