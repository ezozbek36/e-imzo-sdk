# Glossary

## How to use this glossary

- This glossary is editorial and descriptive.
- It normalizes naming used across the RFC set without overriding source wording.
- Uncertain equivalences remain marked as open or ambiguous rather than silently merged.

## Canonical terms

| term                     | scope/layer                  | preferred meaning in this repository                                                                                                              | common aliases / source variants                                       | status            |
|--------------------------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------|-------------------|
| local runtime            | local                        | The locally installed E-IMZO desktop-side component that exposes browser-callable capabilities.                                                   | local crypto service, local E-IMZO runtime, local bridge plane runtime | preferred         |
| local WebSocket endpoint | local transport              | The localhost WebSocket surface used by the browser wrapper to reach the local runtime.                                                           | `/service/cryptapi`                                                    | preferred         |
| browser wrapper          | browser/local                | The browser-side JavaScript wrapper that sends requests to the local WebSocket endpoint.                                                          | local bridge wrapper, `CAPIWS`, `EIMZOEXT`                             | preferred         |
| plugin/module family     | local API surface            | A version-sensitive group of local runtime methods exposed under a named module/plugin.                                                           | plugin, module, family (`pkcs7`, `pfx`, `idcard`, `ytks`)              | preferred         |
| challenge                | server/mobile flow           | Editorial canonical term for the signing input returned by challenge-oriented endpoints and later signed by the local or mobile flow. Preserve literal payload spellings in quoted evidence. | `challenge`, `challange`                                               | preferred         |
| challange                | server/mobile field spelling | Observed source spelling variant for the same signing-input role in reviewed mobile auth-init materials. Preserve when quoting evidence or mapping concrete payloads.                          | `challange`                                                            | source-variant    |
| key handle               | local runtime                | Generic editorial label for a loaded-key reference returned by the local runtime and reused in later calls.                                       | local key reference, runtime handle                                    | preferred         |
| keyId                    | local runtime                | One observed key-handle field name used by some local flows or versions.                                                                          | `keyId`                                                                | version-sensitive |
| pfxId                    | local runtime                | One observed PFX-specific key-handle field name.                                                                                                  | `pfxId`                                                                | version-sensitive |
| tokenId                  | local runtime                | One observed token-specific key-handle field name.                                                                                                | `tokenId`                                                              | version-sensitive |
| ytksId                   | local runtime                | One observed YTKS-specific key-handle field name.                                                                                                 | `ytksId`                                                               | version-sensitive |
| pkcs7b64                 | server REST                  | One observed base64 PKCS#7 field spelling shown in server-side timestamp and verification materials.                                              | `pkcs7b64`                                                             | version-sensitive |
| pkcs7_64                 | local runtime                | One observed base64 PKCS#7 field spelling shown in local runtime responses and wrapper-facing examples.                                           | `pkcs7_64`                                                             | version-sensitive |
| verification result      | server/mobile verification   | Editorial label for structured verification output returned by verification-oriented endpoints.                                                   | `pkcs7Info`, verification info, verify result                          | preferred         |
| documentId               | mobile async                 | The observed mobile-flow correlation identifier used for polling and finalization.                                                                | mobile document id, `documentId`                                       | preferred         |
| request id               | mobile async                 | Generic request-correlation label that may overlap with mobile identifiers in future evidence, but is not yet the repository-wide preferred term. | request identifier, requestId                                          | open              |
| status code              | cross-RFC result surface     | Numeric result/state indicator returned by local, server, or mobile surfaces.                                                                     | `status`, code, negative status                                        | preferred         |
| state                    | editorial lifecycle label    | Editorial lifecycle name mapped onto numeric statuses where useful; not a guaranteed source-level contract.                                       | lifecycle state, pending/ready/finalized                               | ambiguous         |

## Cross-RFC terminology watchlist

- editorial `challenge` term vs observed `challange` source variant
- `pkcs7b64` vs `pkcs7_64`
- generic `key handle` vs plugin-specific handle fields (`keyId`, `pfxId`, `tokenId`, `ytksId`)
- `local runtime` vs `local WebSocket endpoint` vs `browser wrapper`
- `status code` vs editorial `state`
