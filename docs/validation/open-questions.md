# Open Questions

## How to use this file

- This file is the canonical tracker for unresolved questions in this repository.
- Questions move through a small lifecycle so research progress stays visible.
- A question is not closed only because a note exists; it closes only after the relevant repository artifacts are updated.
- Focused research notes live under [`docs/research/open-questions/`](../research/open-questions/README.md).

## Status model

- `open` — recorded, no focused research yet
- `researching` — active investigation is in progress
- `needs-evidence` — current materials are insufficient
- `needs-validation` — a likely interpretation exists, but stronger validation is still needed
- `resolved-editorially` — the repository now has a safe editorial treatment, even if system truth is not fully proven
- `resolved-with-evidence` — the question is closed with sufficiently strong repository evidence
- `deferred` — intentionally postponed
- `wont-resolve-now` — not worth resolving at the current stage

## Progress summary

| status                   | count |
| ------------------------ | ----- |
| `open`                   | 7     |
| `researching`            | 0     |
| `needs-evidence`         | 0     |
| `needs-validation`       | 0     |
| `resolved-editorially`   | 1     |
| `resolved-with-evidence` | 0     |
| `deferred`               | 0     |
| `wont-resolve-now`       | 0     |

## Question tracker

| ID      | question                                           | status                 | owner      | main artifact                                                                            | next step                                                                                                                 | resolution target                                                                                                                                                                               |
| ------- | -------------------------------------------------- | ---------------------- | ---------- |----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Q-001` | challenge vs challange equivalence                 | `resolved-editorially` | ezozbek36  | [Q-001 note](../research/open-questions/Q-001-challenge-vs-challange.md)                 | Preserve literal payload spellings, but use editorial `challenge`/`challenge value` for the shared signing-input concept. | [Glossary](../inventories/glossary.md), [Entity Inventory](../inventories/entities.md), [RFC-0001](../rfc/RFC-0001-terminology-and-architecture.md), [RFC-0005](../rfc/RFC-0005-mobile-flow.md) |
| `Q-002` | pkcs7b64 vs pkcs7_64 equivalence                   | `open`                 | unassigned | [Q-002 note](../research/open-questions/Q-002-pkcs7b64-vs-pkcs7_64.md)                   | Compare local signing and server timestamp/verify payload roles.                                                          | [RFC-0001](../rfc/RFC-0001-terminology-and-architecture.md), [RFC-0002](../rfc/RFC-0002-local-bridge.md), [RFC-0003](../rfc/RFC-0003-signing-and-authentication-flows.md)                       |
| `Q-003` | one-vs-two mobile-side actors                      | `open`                 | unassigned | [Q-003 note](../research/open-questions/Q-003-mobile-actors.md)                          | Extract actor statements from mobile sources into one comparison note.                                                    | [RFC-0001](../rfc/RFC-0001-terminology-and-architecture.md), [RFC-0005](../rfc/RFC-0005-mobile-flow.md)                                                                                         |
| `Q-004` | mobile upload callback authenticity/integrity      | `open`                 | unassigned | [Q-004 note](../research/open-questions/Q-004-mobile-callback-authenticity-integrity.md) | Collect explicit callback protection evidence from current sources.                                                       | [RFC-0001](../rfc/RFC-0001-terminology-and-architecture.md), [RFC-0005](../rfc/RFC-0005-mobile-flow.md)                                                                                         |
| `Q-005` | `/backend/mobile/authenticate` method mismatch     | `open`                 | unassigned | [Q-005 note](../research/open-questions/Q-005-mobile-authenticate-method-mismatch.md)    | Preserve conflicting method examples side by side.                                                                        | [RFC-0005](../rfc/RFC-0005-mobile-flow.md)                                                                                                                                                      |
| `Q-006` | detached verify documentation conflict             | `open`                 | unassigned | [Q-006 note](../research/open-questions/Q-006-detached-verify-conflict.md)               | Preserve detached verify text and example conflict side by side.                                                          | [RFC-0003](../rfc/RFC-0003-signing-and-authentication-flows.md), [RFC-0004](../rfc/RFC-0004-verification-timestamp-certificate.md)                                                              |
| `Q-007` | local-vs-server cryptographic responsibility split | `open`                 | unassigned | [Q-007 note](../research/open-questions/Q-007-local-vs-server-crypto-responsibility.md)  | Build a version-sensitive responsibility matrix from current sources.                                                     | [Phase 1 Canonical](../phases/phase-1/canonical.md), [RFC-0001](../rfc/RFC-0001-terminology-and-architecture.md), [RFC-0004](../rfc/RFC-0004-verification-timestamp-certificate.md)             |
| `Q-008` | status-code semantics drift by version/deployment  | `open`                 | unassigned | [Q-008 note](../research/open-questions/Q-008-status-code-semantics-drift.md)            | Compare status meanings across mobile and error-model materials.                                                          | [Errors Inventory](../inventories/errors.md), [RFC-0005](../rfc/RFC-0005-mobile-flow.md), [RFC-0006](../rfc/RFC-0006-preliminary-error-model.md)                                                |

## Closure rule

Move a question to `resolved-editorially` or `resolved-with-evidence` only when all of the following are true:

1. There is a research summary or equivalent evidence note.
2. The repository records a clear conclusion.
3. The target repository artifacts have been updated.
4. This tracker reflects the result.

## Assumptions to avoid

- Do not treat a likely interpretation as closed until the target artifacts are updated.
- Do not silently normalize conflicting names, actors, endpoints, or status meanings.
- Do not treat one research note as system truth when the broader repository still shows ambiguity.
- Do not assume local/server/mobile boundaries are fully settled unless the evidence and target documents say so.
