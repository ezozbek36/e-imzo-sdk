# e-imzo-sdk

`e-imzo-sdk` is a specification-first repository for documenting observed E-IMZO integration behavior and preparing a stable foundation for future SDK implementations. The repository name is intentional: the SDK is the destination, while this phase focuses on building the specification layer first.

## Why this repository exists

Official E-IMZO materials are useful but often fragmented across manuals, API docs, demos, and operational notes. This repository consolidates that evidence into a clean-room, descriptive specification layer first. SDK implementation is planned, but it follows specification maturity.

## Current focus

- Phase 1 domain modeling and terminology normalization.
- RFC drafting for integration flows, boundaries, and error surfaces.
- Stabilizing repository structure, canonical documents, and review discipline.

## Repository layout

- `docs/phases/` - phase-based analysis outputs.
- `docs/rfc/` - RFC drafts and RFC index.
- `docs/inventories/` - structured inventories (entities, operations, errors, gaps).
- `docs/validation/` - conformance and validation planning.
- `sdk/` - future SDK/reference implementation boundary.
- `vendor/e-imzo-resources/` - source/evidence corpus (submodule area).

## Canonical documents

- Phase 1 canonical document: [docs/phases/phase-1/canonical.md](docs/phases/phase-1/canonical.md)
- RFC index: [docs/rfc/README.md](docs/rfc/README.md)
- Agent/contributor rules: [AGENTS.md](AGENTS.md)
- Project status snapshot: [STATUS.md](STATUS.md)
- Project roadmap: [ROADMAP.md](ROADMAP.md)

## Current status

The repository is in specification and RFC refinement stage. Core documents exist, but inventories, validation scaffolding, and SDK-facing boundaries are still being stabilized.

## Next steps

- Stabilize Phase and RFC documents as canonical references.
- Review RFC set for terminology and boundary consistency.
- Expand inventories and validation/conformance artifacts.
- Define a clear reference SDK boundary under `sdk/`.
