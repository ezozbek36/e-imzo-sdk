# Sources Inventory

## How to use this file

- A "source" here means any repository artifact used as evidence, editorial synthesis, or navigation aid during the current spec work.
- This is an editorial source inventory for orientation; it is not a provenance ledger or line-by-line citation index.

## Source groups

| source group                     | location                                                                                | role in the repository                                              | what it is useful for                                                                    | limitations                                                                          |
|----------------------------------|-----------------------------------------------------------------------------------------|---------------------------------------------------------------------|------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| phase documents                  | `docs/phases/phase-1/`                                                                  | Current descriptive baseline before the RFC split                   | Consolidated findings, early contradictions, broad capability/entity/error extraction    | Editorial synthesis, not primary runtime evidence                                    |
| RFC documents                    | `docs/rfc/`                                                                             | Phase 2 descriptive partition of the observed system                | Ownership boundaries, compact framing by topic, open-question carry-forward              | Derived from other evidence; not a replacement for vendor materials                  |
| glossary                         | `docs/inventories/glossary.md`                                                          | Shared terminology support artifact                                 | Canonical editorial terms, alias watchlist, ambiguity-preserving wording                 | Not a final domain model; does not prove behavior by itself                          |
| open questions                   | `docs/validation/open-questions.md`                                                     | Shared ambiguity register across the RFC bundle                     | Active conflicts, validation priorities, assumptions to avoid                            | Intentionally unresolved; not a source of settled semantics                          |
| README / navigation layers       | `docs/index.md`, `docs/*/README.md`, `vendor/e-imzo-resources/README.md`                | Navigation and editorial entry points                               | Finding the right document family quickly, understanding document purpose                | Useful for orientation, weak evidence for runtime behavior                           |
| vendor integration manual        | `vendor/e-imzo-resources/e-imzo-doc/README.md`                                          | Main upstream-style integration manual in the corpus                | Endpoint families, documented flows, status tables, deployment assumptions               | Can contain ambiguities or conflicting examples; not a complete compatibility matrix |
| vendor demos / runnable examples | `vendor/e-imzo-resources/e-imzo-doc/example.uz/php/demo/`                               | Concrete integration examples                                       | Real call sequences, wrapper usage, relay behavior, mobile polling/finalization patterns | Example code is not automatically universal or typo-free                             |
| vendor generated local API docs  | `vendor/e-imzo-resources/apidoc/API.v-*.md`, `vendor/e-imzo-resources/apidoc/README.md` | Versioned snapshots of the local WebSocket/plugin surface           | Observed method names, module families, argument shapes, version drift                   | Limited to surfaced local API snapshots; does not define server or mobile semantics  |
| vendor knowledge-base digest     | `vendor/e-imzo-resources/knowledge-base/`                                               | Structured operational notes extracted from later posts/discussions | Migration deltas, runtime caveats, deployment problems, recurring integration mistakes   | Operationally rich but not always protocol-complete or normative                     |

## Source confidence notes

- Generated local API docs are strong evidence for surfaced local names, modules, and version-specific availability.
- Demos are strong evidence for observed choreography and payload handling, but they should not be treated as universal contracts.
- The main integration manual is the main server/mobile reference, but conflicts inside examples should remain visible.
- Knowledge-base notes are especially useful for version drift and operational failure modes, not for inventing undocumented guarantees.
- Phase docs, RFCs, glossary, and open-question files are repository-local editorial layers built from the evidence above.
