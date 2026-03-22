# RFC-0002 — Local Bridge / Local Crypto Service Surface

## Status

[OBSERVED] Draft (descriptive baseline).

## Purpose

[OBSERVED] Describe the observable local bridge plane (local WebSocket endpoint + browser wrapper + plugin/module families) without restating it as a normative protocol standard.

## This RFC owns

- local runtime and localhost WebSocket endpoint framing
- browser wrapper and plugin/module family distinctions
- local surface drift and version-sensitive handle naming

## This RFC does not own

- cross-RFC terminology and system-boundary framing (see [RFC-0001](./RFC-0001-terminology-and-architecture.md))
- signing/auth choreography beyond local placement (see [RFC-0003](./RFC-0003-signing-and-authentication-flows.md))
- cross-cutting failure taxonomy ownership (see [RFC-0006](./RFC-0006-preliminary-error-model.md))

## See also

- [RFC-0001 — Terminology and Architecture](./RFC-0001-terminology-and-architecture.md)
- [RFC-0003 — Signing and Authentication Flows](./RFC-0003-signing-and-authentication-flows.md)
- [RFC-0006 — Preliminary Error Model](./RFC-0006-preliminary-error-model.md)

## Non-goals

- [OBSERVED] No plugin behavior guarantees beyond observed docs.
- [OBSERVED] No reverse-engineering of local binary internals.
- [OBSERVED] No attempt to freeze a single cross-version API.

## Evidence base

| Tag        | Source                                                                          | Why used                                                       |
| ---------- | ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/e-imzo.js`              | Local URL, request dispatch pattern, websocket error handling. |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/e-imzo-client.js`       | Version checks, key loading usage, plugin selection patterns.  |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/e-imzo-init.js`         | API-key bootstrap pattern and local failure messaging.         |
| [OBSERVED] | `vendor/e-imzo-resources/apidoc/API.v-4.73.md`, `API.v-5.00.md`, `API.v-6.3.md` | Module families and argument-level surfaces.                   |
| [OBSERVED] | `vendor/e-imzo-resources/apidoc/generate-apidoc.mjs`                            | `version`/`apidoc` request model and version binding logic.    |

## Descriptive model

### Scope

| Tag        | Covered                                                                                                        |
| ---------- | -------------------------------------------------------------------------------------------------------------- |
| [OBSERVED] | Browser-to-local WebSocket endpoint invocation surface (`version`, `apidoc`, `apikey`, plugin function calls). |
| [OBSERVED] | Local module family inventory and version drift.                                                               |
| [OBSERVED] | Local key-handle loading/unloading behavior and observed assumptions.                                          |
| [OBSERVED] | Local runtime failure modes at connection and call level.                                                      |

### What this RFC excludes

| Tag        | Excluded                                                                                          |
| ---------- | ------------------------------------------------------------------------------------------------- |
| [OBSERVED] | Server REST semantics and business session logic.                                                 |
| [OBSERVED] | Mobile lifecycle semantics except where they constrain local assumptions.                         |
| [OBSERVED] | Cross-cutting failure taxonomy ownership (see [RFC-0006](./RFC-0006-preliminary-error-model.md)). |

### Observed API families/modules

| Tag        | Family                 | v4.73            | v5.00            | v6.3                     | Notes                                                        |
| ---------- | ---------------------- | ---------------- | ---------------- | ------------------------ | ------------------------------------------------------------ |
| [OBSERVED] | Core control           | wrapper mediated | wrapper mediated | wrapper mediated         | Wrapper calls `version`, `apidoc`, `apikey`, `callFunction`. |
| [OBSERVED] | `pkcs7`                | broad helper set | broad helper set | `create_pkcs7` observed  | Strong drift; helper removal noted by migration docs.        |
| [OBSERVED] | `pfx`                  | yes              | yes              | yes                      | `load_key`/`unload_key` and related functions persist.       |
| [OBSERVED] | `idcard`               | yes              | yes              | yes                      | Presence persists; details vary.                             |
| [OBSERVED] | `ytks`                 | yes              | yes              | yes                      | Handle naming (`ytksId`) observed.                           |
| [OBSERVED] | `tunnel`               | yes              | yes              | not observed in v6.3 TOC | Version-sensitive capability.                                |
| [OBSERVED] | truststore/crl related | yes              | yes              | changed/reduced          | Certificate helper landscape changes across versions.        |
| [OBSERVED] | v6-era additions       | no               | partial (`app`)  | `app`, `uzgrd` etc.      | New module mix in v6.3 snapshot.                             |

### Request/response surface at descriptive level

| Tag             | Aspect                      | Observed shape                                                                                                                             |
| --------------- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| [OBSERVED]      | Transport                   | WebSocket to localhost endpoint chosen by page protocol (`ws` or `wss`).                                                                   |
| [OBSERVED]      | Generic request             | JSON object with function name and optional plugin+arguments (wrapper-driven).                                                             |
| [OBSERVED]      | Meta requests               | `{name:"version"}`, `{name:"apidoc"}`, `{name:"apikey", arguments:[domain,key,...]}`.                                                      |
| [OBSERVED]      | Typical success model       | Response objects with `success` plus function-specific fields (e.g., `major/minor`, `pkcs7_64`, `keyId`).                                  |
| [OBSERVED]      | Local PKCS#7 field spelling | Reviewed local API docs and wrapper/demo examples use `pkcs7_64` as one observed local field/argument spelling for base64 PKCS#7 material. |
| [OPEN QUESTION] | Canonical error schema      | Not unified in source; observed as `reason` and websocket close codes in wrapper/demo logic.                                               |

### Handle/key loading behavior

| Tag             | Behavior                                                                             | Evidence status                    |
| --------------- | ------------------------------------------------------------------------------------ | ---------------------------------- |
| [OBSERVED]      | `load_key` returns runtime handle used in later calls (e.g., sign operations).       | Evidenced in apidoc and demo code. |
| [OBSERVED]      | Handle naming is plugin-specific (`pfxId`, `tokenId`, `ytksId`, generic `id` usage). | Evidenced, non-uniform.            |
| [OBSERVED]      | `unload_key` methods exist for relevant plugins in multiple versions.                | Evidenced.                         |
| [OBSERVED]      | Some docs describe key availability as time-limited after load.                      | Evidenced in v6.3 pfx description. |
| [OPEN QUESTION] | Exact lifetime/eviction/timeout semantics for loaded handles.                        | Not fully specified.               |

### Lifetime/availability semantics

| Tag             | Observation                                                                               |
| --------------- | ----------------------------------------------------------------------------------------- |
| [OBSERVED]      | Local E-IMZO runtime must be installed and reachable on localhost WebSocket endpoint.     |
| [OBSERVED]      | Browser websocket support is treated as prerequisite by demo init code.                   |
| [OBSERVED]      | API-key domain setup is shown as bootstrap in manual/demo; v6 notes add auto-load nuance. |
| [OPEN QUESTION] | Runtime reconnection/backoff contract is not standardized in provided sources.            |

### Plugin/service assumptions

| Tag             | Assumption                                                                                |
| --------------- | ----------------------------------------------------------------------------------------- |
| [OBSERVED]      | Local desktop runtime present and running.                                                |
| [OBSERVED]      | Domain registration/API-key alignment with frontend origin remains required in FAQ notes. |
| [OBSERVED]      | Mixed-version clients require explicit compatibility handling.                            |
| [OPEN QUESTION] | Exact boundary between runtime-level API-key enforcement and server-side controls.        |

### Local failure modes

| Tag             | Class                                                                    | Surface                                                                      |
| --------------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| [OBSERVED]      | Transport capability                                                     | Browser lacks WebSocket support.                                             |
| [OBSERVED]      | Service connectivity                                                     | WebSocket connect/close failure; non-1000 close codes surfaced.              |
| [OBSERVED]      | Runtime processing                                                       | `data.success=false` with textual `reason`.                                  |
| [OBSERVED]      | Secret-entry failures                                                    | Wrong password patterns handled in demo (`BadPaddingException` text branch). |
| [OPEN QUESTION] | Deterministic mapping from low-level local errors to stable error codes. |                                                                              |

[OBSERVED] Cross-plane error categorization and taxonomy ownership is centralized in [RFC-0006](./RFC-0006-preliminary-error-model.md).

### Proposed abstraction boundary

| Tag                    | Boundary                                                                                                                                                                          |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [PROPOSED ABSTRACTION] | Use an observed local request/response envelope (`transport`, `request`, `response`, `runtime_error`) as editorial structure without asserting stable method set across versions. |
| [PROPOSED ABSTRACTION] | Treat plugin families as versioned capability flags, not hardcoded invariants.                                                                                                    |

## Open questions

- [OPEN QUESTION] Which local methods are guaranteed across all supported E-IMZO versions?
- [OPEN QUESTION] Is `apikey` call still mandatory in all runtime contexts in v6 deployments?
- [OPEN QUESTION] What is the formal lifecycle of loaded key handles under idle/disconnect conditions?

## Risk of incorrect assumptions

- [OPEN QUESTION] Assuming v5 plugin set on v6 installations.
- [OPEN QUESTION] Assuming every response has same field contract.
- [OPEN QUESTION] Assuming helper methods marked deprecated still execute in target installations.
- [OPEN QUESTION] Assuming one literal local field name will also appear unchanged on server REST surfaces.

## Next validation steps

1. [OBSERVED] Capture `apidoc` live snapshots from additional installed versions.
2. [OBSERVED] Build a machine-readable local capability matrix keyed by app version.
3. [OBSERVED] Add reproducible local failure fixture catalog (close codes, no-service, wrong-password, unsupported method).
