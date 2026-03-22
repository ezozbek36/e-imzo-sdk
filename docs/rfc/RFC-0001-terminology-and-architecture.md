# RFC-0001 — Terminology and Architecture

## Status

[OBSERVED] Draft (initial Phase 2 cut from Phase 1 evidence).

## Purpose

[OBSERVED] Establish a stable descriptive architecture vocabulary and boundary model across local desktop/WebSocket, server REST, and mobile flows.

## Non-goals

- [OBSERVED] No protocol redesign.
- [OBSERVED] No SDK surface definition.
- [OBSERVED] No reverse engineering or undocumented cryptographic guarantees.

## Evidence base

| Tag        | Source                                                                     | Why used                                                            |
|------------|----------------------------------------------------------------------------|---------------------------------------------------------------------|
| [OBSERVED] | `Phase1.md`                                                                | Consolidated Phase 1 findings and conflicts.                        |
| [OBSERVED] | `e-imzo-resources/e-imzo-doc/README.md`                                    | Primary integration manual, endpoint families, sequence narratives. |
| [OBSERVED] | `e-imzo-resources/e-imzo-doc/example.uz/php/demo/e-imzo.js`                | Local WebSocket wrapper and service URL behavior.                   |
| [OBSERVED] | `e-imzo-resources/apidoc/README.md`                                        | Versioned local API evidence model.                                 |
| [OBSERVED] | `e-imzo-resources/apidoc/API.v-4.73.md`, `API.v-5.00.md`, `API.v-6.3.md`   | Local plugin/module surface drift.                                  |
| [OBSERVED] | `e-imzo-resources/knowledge-base/migration-guide.md`, `integration-faq.md` | Operational deltas and current integration caveats.                 |

## Descriptive model

### Canonical terminology

| Tag          | Canonical term               | Source aliases                           | Layer         | Notes                                                             |
|--------------|------------------------------|------------------------------------------|---------------|-------------------------------------------------------------------|
| [NORMALIZED] | Local cryptapi service       | `/service/cryptapi`                      | Local         | WebSocket endpoint exposed on localhost by E-IMZO runtime.        |
| [NORMALIZED] | Local bridge wrapper         | `CAPIWS`, `EIMZOEXT`                     | Local         | Browser JS wrapper for local calls.                               |
| [NORMALIZED] | Frontend endpoint family     | `/frontend/*`                            | Server        | Documented as callable from frontend/backend depending on method. |
| [NORMALIZED] | Backend endpoint family      | `/backend/*`                             | Server        | Documented as backend-only by policy.                             |
| [NORMALIZED] | Mobile upload callback route | `/frontend/mobile/upload` (`UPLOAD URL`) | Mobile/server | Callback route associated with onboarding `SiteID`.               |
| [NORMALIZED] | Challenge value              | `challenge`, `challange`                 | Server/mobile | Spelling is inconsistent in source materials.                     |
| [NORMALIZED] | Local key handle             | `keyId`, `pfxId`, `tokenId`, `ytksId`    | Local         | Handle naming is plugin/version-specific.                         |

### Actors

| Tag        | Actor                              | Role                                                                       |
|------------|------------------------------------|----------------------------------------------------------------------------|
| [OBSERVED] | End user                           | Chooses key/device, confirms password/PIN, initiates auth/sign.            |
| [OBSERVED] | Browser frontend                   | Calls local bridge and selected `/frontend/*` APIs; polls mobile status.   |
| [OBSERVED] | Local E-IMZO runtime               | Provides plugin methods via localhost WebSocket.                           |
| [OBSERVED] | Application backend                | Calls `/backend/*`; handles app session/business logic.                    |
| [OBSERVED] | E-IMZO-SERVER                      | REST surface for challenge/auth/verify/timestamp/mobile endpoints.         |
| [OBSERVED] | ID-CARD mobile app/system          | Consumes deeplink/QR payload and posts signed data via mobile upload flow. |
| [OBSERVED] | VPN/OCSP/TSA external dependencies | Used by server during certificate/timestamp related checks.                |

### Trust boundaries

| Tag        | Boundary                                     | Description                                                                      |
|------------|----------------------------------------------|----------------------------------------------------------------------------------|
| [OBSERVED] | Browser ↔ Local bridge                       | Localhost WebSocket dependency, local app installation required.                 |
| [OBSERVED] | Browser ↔ App backend                        | App-specific session and business boundary.                                      |
| [OBSERVED] | Backend ↔ E-IMZO-SERVER                      | Server-side verification/auth boundary (`Host`, `X-Real-IP` shown in docs/demo). |
| [OBSERVED] | E-IMZO-SERVER ↔ external VPN services        | Connectivity required for status/timestamp-related operations.                   |
| [OBSERVED] | Mobile API callback ↔ hosted upload endpoint | Mobile subsystem pushes PKCS#7 to configured upload URL.                         |

### System boundaries

| Tag             | System                            | In scope                                                             |
|-----------------|-----------------------------------|----------------------------------------------------------------------|
| [OBSERVED]      | Local desktop/WebSocket subsystem | `CAPIWS` wrapper behavior and plugin method inventory.               |
| [OBSERVED]      | Server REST subsystem             | `/frontend/*`, `/backend/*`, `/ping`, `/info`, status tables.        |
| [OBSERVED]      | Mobile/deeplink subsystem         | `/frontend/mobile/*`, polling, upload callback, `/backend/mobile/*`. |
| [OPEN QUESTION] | Internal state internals          | Redis record model and retention rules are not fully specified.      |

### Observed subsystems and integration surfaces

| Tag        | Subsystem   | Integration surfaces                                                                                                                                      |
|------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [OBSERVED] | Local       | `ws://127.0.0.1:64646/service/cryptapi`, `wss://127.0.0.1:64443/service/cryptapi`, `version`, `apidoc`, `apikey`, plugin calls.                           |
| [OBSERVED] | Server REST | `/frontend/challenge`, `/backend/auth`, `/frontend/timestamp/pkcs7`, `/backend/pkcs7/verify/{attached,detached}`, `/frontend/pkcs7/{join,make-attached}`. |
| [OBSERVED] | Mobile REST | `/frontend/mobile/{auth,sign,status,upload}`, `/backend/mobile/{authenticate/{documentId},verify}`.                                                       |
| [OBSERVED] | Operational | Reverse proxy routing, Redis settings, VPN key/config, host/IP forwarding.                                                                                |

### High-level sequence summaries

| Tag        | Sequence                      | Summary                                                                                                                          |
|------------|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| [OBSERVED] | Desktop authentication        | Browser gets challenge, local signs challenge, backend calls `/backend/auth`, app establishes session.                           |
| [OBSERVED] | Desktop signing               | Browser signs document locally, calls `/frontend/timestamp/pkcs7`, backend verifies via `/backend/pkcs7/verify/*`.               |
| [OBSERVED] | Mobile authentication/signing | Frontend requests mobile init, polls `/frontend/mobile/status`, mobile upload occurs, backend finalizes via `/backend/mobile/*`. |

### Proposed abstraction boundary

| Tag                    | Boundary                                                                                                                     |
|------------------------|------------------------------------------------------------------------------------------------------------------------------|
| [PROPOSED ABSTRACTION] | Treat three integration planes as separate contracts: `LocalBridgeContract`, `ServerRestContract`, `MobileAsyncContract`.    |
| [PROPOSED ABSTRACTION] | Keep operational prerequisites (VPN/Redis/proxy/API-key domain registration) as environment profile, not protocol semantics. |

## Open questions

| Tag             | Question                                                                                                                                                   |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [OPEN QUESTION] | How `/backend/*` caller isolation is enforced in real deployments (policy is documented; enforcement mechanism is partially externalized to proxy config). |
| [OPEN QUESTION] | Exact callback authenticity/integrity model for `/frontend/mobile/upload`.                                                                                 |
| [OPEN QUESTION] | Cross-version compatibility guarantees for local plugin families and handle naming stability.                                                              |
| [OPEN QUESTION] | Complete responsibility split for cryptographic validation between local and server layers across versions.                                                |

## Risk of incorrect assumptions

- [OPEN QUESTION] Assuming manual API-key bootstrap is universal despite v6 auto-load notes.
- [OPEN QUESTION] Assuming one uniform challenge field spelling and shape.
- [OPEN QUESTION] Assuming local plugin surfaces are stable across app versions.

## Next validation steps

1. [OBSERVED] Build a version-by-version capability matrix from `API.v-*`.
2. [OBSERVED] Re-run demo flows against representative client versions and record payload diffs.
3. [OBSERVED] Add explicit evidence snippets for proxy isolation and mobile callback hardening controls.
