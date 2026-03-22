# RFC-0001 — Terminology and Architecture

## Status

[OBSERVED] Draft (initial Phase 2 cut from Phase 1 evidence).

## Purpose

[OBSERVED] Establish shared terminology, actor/boundary framing, and a high-level architecture partition used by the RFC bundle. This partition is editorial and descriptive, not a final protocol or SDK contract.

## Non-goals

- [OBSERVED] No protocol redesign.
- [OBSERVED] No SDK surface definition.
- [OBSERVED] No reverse engineering or undocumented cryptographic guarantees.

## Evidence base

| Tag        | Source                                                                            | Why used                                                            |
| ---------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| [OBSERVED] | `docs/phases/phase-1/canonical.md`                                                | Consolidated Phase 1 findings and conflicts.                        |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/README.md`                                    | Primary integration manual, endpoint families, sequence narratives. |
| [OBSERVED] | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/e-imzo.js`                | Local WebSocket wrapper and service URL behavior.                   |
| [OBSERVED] | `vendor/e-imzo-resources/apidoc/README.md`                                        | Versioned local API evidence model.                                 |
| [OBSERVED] | `vendor/e-imzo-resources/apidoc/API.v-4.73.md`, `API.v-5.00.md`, `API.v-6.3.md`   | Local plugin/module surface drift.                                  |
| [OBSERVED] | `vendor/e-imzo-resources/knowledge-base/migration-guide.md`, `integration-faq.md` | Operational deltas and current integration caveats.                 |

## Descriptive model

### Canonical terminology

| Tag             | Canonical term                  | Source aliases                           | Layer         | Notes                                                                                           |
| --------------- | ------------------------------- | ---------------------------------------- | ------------- | ----------------------------------------------------------------------------------------------- |
| [NORMALIZED]    | Local WebSocket endpoint        | `/service/cryptapi`                      | Local         | Endpoint exposed on localhost by local E-IMZO runtime.                                          |
| [NORMALIZED]    | Local bridge wrapper            | `CAPIWS`, `EIMZOEXT`                     | Local         | Browser-side JS wrapper used to call local endpoint/modules.                                    |
| [NORMALIZED]    | Frontend endpoint family        | `/frontend/*`                            | Server        | Documented as callable from frontend/backend depending on method.                               |
| [NORMALIZED]    | Backend endpoint family         | `/backend/*`                             | Server        | Documented as backend-only by policy.                                                           |
| [NORMALIZED]    | Mobile upload callback route    | `/frontend/mobile/upload` (`UPLOAD URL`) | Mobile/server | Callback route associated with onboarding `siteId`/`SiteID`.                                    |
| [NORMALIZED]    | Challenge field                 | `challenge`, `challange`                 | Server/mobile | Spelling is inconsistent in source materials; this RFC set keeps both spellings visible.        |
| [NORMALIZED]    | Local key handle field variants | `keyId`, `pfxId`, `tokenId`, `ytksId`    | Local         | Field naming is plugin/version-specific; equivalence is treated as observed-but-non-uniform.    |
| [OPEN QUESTION] | PKCS#7 base64 field variants    | `pkcs7b64`, `pkcs7_64`                   | Cross-plane   | Similar intent appears in sources, but canonical equivalence across all versions is not proven. |

### Actors

| Tag        | Actor                              | Role                                                                       |
| ---------- | ---------------------------------- | -------------------------------------------------------------------------- |
| [OBSERVED] | End user                           | Chooses key/device, confirms password/PIN, initiates auth/sign.            |
| [OBSERVED] | Browser frontend                   | Calls local bridge and selected `/frontend/*` APIs; polls mobile status.   |
| [OBSERVED] | Local E-IMZO runtime               | Provides plugin methods via localhost WebSocket.                           |
| [OBSERVED] | Application backend                | Calls `/backend/*`; handles app session/business logic.                    |
| [OBSERVED] | E-IMZO-SERVER                      | REST surface for challenge/auth/verify/timestamp/mobile endpoints.         |
| [OBSERVED] | Mobile-side signing/upload path    | Consumes deeplink/QR payload and posts signed data via mobile upload flow. |
| [OBSERVED] | VPN/OCSP/TSA external dependencies | Used by server during certificate/timestamp related checks.                |

### Trust boundaries

| Tag        | Boundary                                     | Description                                                                      |
| ---------- | -------------------------------------------- | -------------------------------------------------------------------------------- |
| [OBSERVED] | Browser ↔ Local bridge                       | Localhost WebSocket dependency, local app installation required.                 |
| [OBSERVED] | Browser ↔ App backend                        | App-specific session and business boundary.                                      |
| [OBSERVED] | Backend ↔ E-IMZO-SERVER                      | Server-side verification/auth boundary (`Host`, `X-Real-IP` shown in docs/demo). |
| [OBSERVED] | E-IMZO-SERVER ↔ external VPN services        | Connectivity required for status/timestamp-related operations.                   |
| [OBSERVED] | Mobile API callback ↔ hosted upload endpoint | Mobile subsystem pushes PKCS#7 to configured upload URL.                         |

### System boundaries

| Tag             | System                            | In scope                                                             |
| --------------- | --------------------------------- | -------------------------------------------------------------------- |
| [OBSERVED]      | Local bridge plane                | Local bridge wrapper behavior and plugin/module method inventory.    |
| [OBSERVED]      | Server REST subsystem             | `/frontend/*`, `/backend/*`, `/ping`, `/info`, status tables.        |
| [OBSERVED]      | Mobile/deeplink subsystem         | `/frontend/mobile/*`, polling, upload callback, `/backend/mobile/*`. |
| [OPEN QUESTION] | Internal state internals          | Redis record model and retention rules are not fully specified.      |

### RFC ownership map (high-level)

| Tag        | Area                                         | Primary RFC owner                                             | Notes                                                                                 |
| ---------- | -------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| [OBSERVED] | Local endpoint/module surface and drift      | [RFC-0002](./RFC-0002-local-bridge.md)                        | Includes local WebSocket endpoint behavior, wrapper calls, and module/version drift.  |
| [OBSERVED] | Signing/auth choreography                    | [RFC-0003](./RFC-0003-signing-and-authentication-flows.md)    | Includes challenge/auth and desktop signing sequence narratives.                      |
| [OBSERVED] | Verification/timestamp/certificate semantics | [RFC-0004](./RFC-0004-verification-timestamp-certificate.md)  | Keeps operation semantics and local-vs-remote placement notes.                        |
| [OBSERVED] | Mobile async lifecycle                       | [RFC-0005](./RFC-0005-mobile-flow.md)                         | Covers status transitions, polling, and mobile finalize paths.                        |
| [OBSERVED] | Cross-cutting failure framing                | [RFC-0006](./RFC-0006-preliminary-error-model.md)             | Main place for provisional error taxonomy and failure-layer categorization.           |

### Proposed abstraction boundary

| Tag                    | Boundary                                                                                                                                 |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| [PROPOSED ABSTRACTION] | Use three planes (`local bridge`, `server REST`, `mobile async`) as editorial partition for this RFC bundle; treat this as provisional.  |
| [PROPOSED ABSTRACTION] | Keep operational prerequisites (VPN/Redis/proxy/API-key domain registration) in environment framing, not core protocol semantics.        |

## Open questions

| Tag             | Question                                                                                                                                                   |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [OPEN QUESTION] | How `/backend/*` caller isolation is enforced in real deployments (policy is documented; enforcement mechanism is partially externalized to proxy config). |
| [OPEN QUESTION] | Exact callback authenticity/integrity model for `/frontend/mobile/upload`.                                                                                 |
| [OPEN QUESTION] | Whether mobile-side signing and upload roles are one actor or two distinct actors across deployments (see RFC-0005 detail).                                |
| [OPEN QUESTION] | Cross-version compatibility guarantees for local plugin families and handle naming stability.                                                              |
| [OPEN QUESTION] | Complete responsibility split for cryptographic validation between local and server layers across versions.                                                |
| [OPEN QUESTION] | Whether the current three-plane partition remains the best organizing model after more cross-version evidence is collected.                                |

## Risk of incorrect assumptions

- [OPEN QUESTION] Assuming manual API-key bootstrap is universal despite v6 auto-load notes.
- [OPEN QUESTION] Assuming one uniform challenge field spelling and shape.
- [OPEN QUESTION] Assuming local plugin surfaces are stable across app versions.
- [OPEN QUESTION] Treating the current RFC partition as fixed architecture rather than editorial organization.

## Next validation steps

1. [OBSERVED] Build a version-by-version capability matrix from `API.v-*`.
2. [OBSERVED] Re-run demo flows against representative client versions and record payload diffs.
3. [OBSERVED] Add explicit evidence snippets for proxy isolation and mobile callback hardening controls.
