# Conformance Plan

## 1. Purpose

- [FACT] This document is a validation planning artifact for the current repository state.
- [FACT] It translates the current descriptive RFC set into practical validation targets, evidence needs, and ambiguity tracking.
- [FACT] It does not define an implementation, an executable conformance suite, or an SDK surface.
- [FACT] It does not claim protocol completeness.
- [FACT] It identifies what can and cannot be validated at the current maturity of the repository.

## 2. Scope

### Covered by this plan

- [FACT] terminology stability checks across Phase 1, the glossary, and the RFC set
- [FACT] cross-RFC consistency checks for actors, boundaries, flows, field names, and status semantics
- [FACT] flow-level validation targets for desktop auth/sign flows, verification/timestamp flows, and mobile async flows
- [FACT] version-drift-sensitive areas where current evidence already shows changes across observed E-IMZO versions
- [FACT] unresolved areas that currently block stronger validation or conformance claims

### Not yet covered by this plan

- [FACT] a full executable conformance suite
- [FACT] cryptographic correctness proofs
- [FACT] environment-specific deployment certification
- [FACT] final SDK compatibility or interoperability claims
- [FACT] any assumption that undocumented behavior is stable

## 3. Validation layers

| validation layer                     | what is being validated                                                                                             | current repo artifacts that support it                                                                                                                                                 | current confidence | current limitations                                                                  |
|--------------------------------------|---------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|--------------------------------------------------------------------------------------|
| editorial validation                 | Section ownership, terminology usage, and explicit fact/inference/open-question discipline across docs              | `docs/phases/phase-1/canonical.md`, `docs/inventories/glossary.md`, `docs/rfc/RFC-0001-terminology-and-architecture.md` through `docs/rfc/RFC-0006-preliminary-error-model.md`         | Medium-High        | Validates document discipline, not runtime behavior                                  |
| documentation consistency validation | Whether RFCs and Phase 1 outputs agree on terms, actors, boundaries, endpoint families, and stated responsibilities | `docs/phases/phase-1/canonical.md`, all RFCs, `docs/validation/open-questions.md`                                                                                                      | Medium             | Conflicts are known in several areas and not yet resolved by fixtures                |
| flow and sequence validation         | Whether documented flow steps are supported by demos/manuals for desktop auth, sign/verify, and mobile async flows  | `vendor/e-imzo-resources/e-imzo-doc/README.md`, demo files cited by RFC-0003 and RFC-0005, `docs/rfc/RFC-0003-signing-and-authentication-flows.md`, `docs/rfc/RFC-0005-mobile-flow.md` | Medium             | Mostly source-based today; few preserved request/response fixtures exist in the repo |
| version-drift validation             | Whether field names, module families, helper availability, and endpoint usage drift across observed versions        | `vendor/e-imzo-resources/apidoc/API.v-4.73.md`, `API.v-5.00.md`, `API.v-6.3.md`, knowledge-base migration notes, `docs/phases/phase-1/canonical.md`                                    | Medium-High        | Evidence is snapshot-based and incomplete across versions and deployment profiles    |
| ambiguity tracking validation        | Whether known conflicts and unknowns are explicitly preserved rather than silently normalized                       | `docs/validation/open-questions.md`, contradiction sections in Phase 1 and RFCs                                                                                                        | High               | Tracks uncertainty well, but does not resolve it without additional evidence         |
| future fixture-backed validation     | Whether normalized claims can later be checked against preserved examples, traces, and failures                     | Current RFC next-step sections and validation notes                                                                                                                                    | Low                | Most required fixtures are not yet collected in this repository                      |

## 4. Validation targets by RFC

| RFC      | main claims or surfaces that could be validated                                                                                             | validation type                                            | current evidence source                                                                                                                   | blocker or ambiguity                                                                                                            | next validation step                                                                                                        |
|----------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| RFC-0001 | editorial three-plane partition, actor map, trust boundaries, canonical term usage, boundary between local/server/mobile framing            | editorial, cross-RFC consistency, version-drift review     | `docs/phases/phase-1/canonical.md`, `docs/inventories/glossary.md`, `docs/rfc/RFC-0001-terminology-and-architecture.md`                   | mobile actor split, incomplete local-vs-server responsibility split; `challenge`/`challange` now has an editorial treatment but still lacks version-tagged fixtures | Build a source-backed terminology and boundary conflict matrix with explicit version notes                                  |
| RFC-0002 | local WebSocket endpoint framing, wrapper request model, plugin/module family inventory, handle-field drift, API-key bootstrap notes        | source-based, version-drift, limited fixture-backed later  | `docs/rfc/RFC-0002-local-bridge.md`, `vendor/e-imzo-resources/apidoc/API.v-*`, local demo wrapper/init files                              | incomplete live-version coverage, unknown handle lifetime semantics, v6 API-key behavior nuance                                 | Extract a version-by-version local capability matrix and preserve representative local request/response examples            |
| RFC-0003 | challenge-auth flow, document-sign flow, attached vs detached verification path, join/multi-signer guidance, local-to-server handoff points | flow-level, cross-source consistency, fixture-backed later | `docs/rfc/RFC-0003-signing-and-authentication-flows.md`, manual and demo auth/verify files, migration notes                               | detached verify endpoint conflict, replay/idempotency unknowns, field-name drift                                                | Collect normalized request/response examples for challenge, timestamp, attached verify, and detached verify                 |
| RFC-0004 | server verification operations, timestamp endpoint semantics, certificate-related result framing, local-helper vs server-endpoint placement | source-based, version-drift, manual validation later       | `docs/rfc/RFC-0004-verification-timestamp-certificate.md`, manual verify/timestamp sections, demo relay files, `API.v-*`, migration notes | detached verify sample conflict, timestamp policy opacity, truststore/helper stability, responsibility split ambiguity          | Build a version-sensitive responsibility matrix and preserve success/failure verification result examples                   |
| RFC-0005 | mobile async lifecycle, `siteId`/`documentId` usage, status progression, polling/finalization sequence, backend-mobile finalization surface | flow-level, real-integration/manual, deployment-sensitive  | `docs/rfc/RFC-0005-mobile-flow.md`, mobile demo files, manual mobile sections, integration FAQ                                            | authenticate method mismatch, callback authenticity/integrity unknown, actor split ambiguity, `documentId` lifetime unknown     | Capture a real end-to-end mobile trace with status polling, upload callback, and finalization details by version/deployment |
| RFC-0006 | cross-layer failure buckets, transport vs validation vs operational partition, status-code grouping, ambiguity preservation                 | editorial, taxonomy consistency, fixture-backed later      | `docs/rfc/RFC-0006-preliminary-error-model.md`, Phase 1 error inventory, manual status tables, troubleshooting notes                      | version/deployment drift in status meanings, logs-only buckets, no stable local numeric taxonomy, retriable-vs-terminal unknown | Create an error fixture inventory keyed by layer, endpoint, precondition, and observed status/result                        |

## 5. High-value validation targets

| target                                                | why it matters                                                                                                                                      | what kind of validation is needed                                                                          | validation mode                           |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| local layer terminology consistency                   | RFC-0001 and RFC-0002 depend on stable editorial distinctions between local runtime, local WebSocket endpoint, browser wrapper, and plugin families | cross-document terminology sweep and conflict matrix                                                       | source-based                              |
| `challenge` vs `challange` field drift                | This affects auth and mobile documentation, payload normalization, and any future fixture schema                                                    | side-by-side payload comparison from desktop auth and mobile flows, ideally by version                     | source-based first, fixture-based later   |
| `pkcs7b64` vs `pkcs7_64` field drift                  | This is a cross-plane normalization risk between local outputs and server payloads                                                                  | compare local signing outputs and server timestamp/verify payload roles for the same flow                  | source-based first, fixture-based later   |
| detached verify endpoint conflict                     | RFC-0003 and RFC-0004 cannot make stronger conformance statements while the manual section and curl example disagree                                | confirm the effective endpoint shape with preserved examples or real integration traces                    | fixture-based or real-integration/manual  |
| mobile authenticate method mismatch                   | RFC-0005 currently contains a material method ambiguity for `/backend/mobile/authenticate`                                                          | validate accepted method, request form, and response shape against a representative integration            | real-integration/manual                   |
| mobile callback authenticity and integrity boundary   | This is a major trust-boundary gap and prevents stronger claims about what the upload callback proves                                               | collect deployment guidance, traces, or version-specific materials that show how the callback is protected | real-integration/manual                   |
| local-vs-server responsibility split                  | RFC-0001, RFC-0003, and RFC-0004 all depend on a disciplined distinction between legacy local helpers and server-preferred flows                    | build a version-by-version matrix of local helper presence vs server endpoint usage                        | source-based first, fixture-based later   |
| status/state semantics drift by version or deployment | RFC-0005 and RFC-0006 cannot support stronger conformance claims if the same status means different things in different environments                | compare success and failure examples across at least one older and one newer environment profile           | fixture-based and real-integration/manual |

## 6. Evidence and fixture needs

| category                                | needed future material                                                                                                                             | why it is needed                                                                                 | current repo status                                                                                |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| normalized request/response examples    | preserved examples for challenge issuance, local signing, `/backend/auth`, timestamping, attached verify, detached verify, and mobile finalization | Needed to turn narrative flow descriptions into checkable conformance surfaces                   | Partial source examples exist; normalized preserved fixtures do not                                |
| version-specific endpoint samples       | examples tied to specific E-IMZO app/server versions                                                                                               | Needed to separate stable behavior from version drift                                            | Versioned local API docs exist; equivalent version-tagged flow fixtures are limited                |
| field-variant examples                  | examples showing `challenge`/`challange`, `pkcs7b64`/`pkcs7_64`, and handle-field variants in context                                              | Needed to validate whether terms are aliases, drift, or distinct fields                          | `challenge`/`challange` now has an editorial treatment; broader side-by-side preserved examples are still not yet assembled      |
| mobile lifecycle examples               | end-to-end traces for `auth`, `sign`, `status`, upload callback, and backend finalization                                                          | Needed to validate lifecycle claims and identify deployment-sensitive steps                      | Demo code exists; preserved real traces do not                                                     |
| failure examples                        | expired challenge, detached verify errors, Redis/state failures, invalid base64, local runtime unavailable, wrong-password branches                | Needed to validate RFC-0006 against actual observed failures instead of editorial grouping alone | Error tables and troubleshooting notes exist; fixture corpus does not                              |
| environment and dependency observations | notes or traces covering proxy/header setup, VPN/connectivity, CA state, onboarding/site registration, and version pairing                         | Needed to distinguish protocol behavior from deployment preconditions                            | Operational notes exist in manuals and knowledge-base docs; structured validation artifacts do not |

## 7. Blockers to stronger conformance claims

- [CONFLICT] The repository still contains unresolved field drift around `pkcs7b64` vs `pkcs7_64`; `challenge` vs `challange` now has an editorial treatment rather than a still-open terminology question.
- [CONFLICT] The detached verify documentation conflict prevents stronger claims about the authoritative detached verification call shape.
- [CONFLICT] The `/backend/mobile/authenticate` method mismatch prevents stronger claims about mobile finalization semantics.
- [UNKNOWN] The local-vs-server cryptographic responsibility split is still descriptive and version-sensitive rather than fully validated.
- [UNKNOWN] The authenticity and integrity model for the mobile upload callback is not stabilized in current source material.
- [UNKNOWN] Status-code meanings may drift by version or deployment profile, especially in mobile and operationally sensitive cases.
- [UNKNOWN] The repository does not yet contain a preserved fixture corpus sufficient to support stronger flow or error conformance claims.

## 8. Suggested next validation work

1. Build a source-backed conflict matrix covering terminology drift, field drift, endpoint/method mismatches, and responsibility-split ambiguities.
2. Extract version-sensitive request/response examples from current manual, demo, and `API.v-*` materials into a small evidence annex or fixture inventory.
3. Create a terminology drift checklist keyed to the glossary and RFC ownership boundaries so future edits do not silently normalize unstable terms.
4. Collect ambiguity-specific fixtures for the highest-risk conflicts first: detached verify, mobile authenticate method, and mobile upload callback boundary.
5. Define a future compatibility matrix format that can record local version, server version, observed capability set, and unresolved notes without yet claiming support guarantees.

## 9. What not to assume

- [UNKNOWN] Do not assume the current editorial three-plane partition is complete protocol truth.
- [UNKNOWN] Do not assume similarly named fields are equivalent across local, server, and mobile surfaces without version-specific evidence.
- [UNKNOWN] Do not assume server-preferred modern flows erase all legacy local helper behavior in older deployments.
- [UNKNOWN] Do not assume demo polling, timeout, or retry behavior is a protocol guarantee.
- [UNKNOWN] Do not assume mobile upload callbacks are authenticated or integrity-protected unless evidence shows how.
- [UNKNOWN] Do not assume status tables alone are enough to prove stable conformance semantics across versions or environments.
