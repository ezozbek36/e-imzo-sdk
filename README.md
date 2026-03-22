> [!WARNING]
> **Independent community project.** Not affiliated with, endorsed by, or associated
> with YT LLC (`yt.uz`) or any official E-IMZO authority.
> See [DISCLAIMER.md](./DISCLAIMER.md) for full details.

# e-imzo-sdk

`e-imzo-sdk` is an independent, community-driven specification effort for E-IMZO
integrations. The goal is to produce the clear, developer-friendly documentation
that the official materials do not provide.

## Why this exists

Official E-IMZO integration materials are fragmented across manuals, API docs,
demos, and operational notes — and often incomplete or unclear. Developers
integrating E-IMZO into real products regularly hit undocumented behavior, ambiguous
error surfaces, and missing guidance for non-trivial flows.

This repository consolidates observed integration behavior into a clean-room,
descriptive specification layer. SDK implementation follows specification maturity —
not the other way around.

## Repository layout

| Path                       | Purpose                                                     |
|----------------------------|-------------------------------------------------------------|
| `docs/phases/`             | Phase-based analysis outputs                                |
| `docs/rfc/`                | RFC drafts and RFC index                                    |
| `docs/inventories/`        | Structured inventories (entities, operations, errors, gaps) |
| `docs/validation/`         | Conformance and validation planning                         |
| `sdk/`                     | Future SDK / reference implementation boundary              |
| `vendor/e-imzo-resources/` | Source / evidence corpus (submodule)                        |

## Key documents

- **Phase 1 canonical:** [`docs/phases/phase-1/canonical.md`](./docs/phases/phase-1/canonical.md)
- **RFC index:** [`docs/rfc/README.md`](./docs/rfc/README.md)
- **Project status:** [`STATUS.md`](./STATUS.md)
- **Roadmap:** [`ROADMAP.md`](./ROADMAP.md)
- **Contributor / agent rules:** [`AGENTS.md`](./AGENTS.md)
- **Disclaimer:** [`DISCLAIMER.md`](./DISCLAIMER.md)

## Current status

Phase 1 domain modeling is complete and canonical. Six RFC drafts (RFC-0001 through RFC-0006) are in active review. Structured inventories (glossary, entities, errors) and an open-questions tracker (8 questions, 2 resolved editorially, 6 open) are in place. SDK boundaries under `sdk/` are not yet defined.

## Next steps

- Resolve open questions Q-003 through Q-008 in [`docs/validation/open-questions.md`](./docs/validation/open-questions.md).
- Review RFC set for cross-RFC terminology and boundary consistency.
- Define an initial reference boundary for `sdk/`.

## Legal

This is an independent community project. See [DISCLAIMER.md](./DISCLAIMER.md) and [LICENSE](./LICENSE).
