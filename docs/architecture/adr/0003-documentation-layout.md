# ADR-0003: Keep all project documentation under `docs/`

- **Status:** accepted
- **Date:** 2026-11-09
- **Related:** [idea 0000 Q&A log](../../ideas/0000-spec-driven-development-setup.md), [idea 0001](../../ideas/0001-architecture-documentation-and-modularity.md)

## Context
`ideas/` and `specs/` started at the repository root. The project now also needs an architecture document, ADRs, metric snapshots and templates. Five or more documentation folders at the root would crowd out the code.

## Decision
We will keep all documentation under `docs/`:
`docs/ideas/`, `docs/specs/`, `docs/templates/`, and `docs/architecture/` (which contains `architecture.md`, `components.json`, `adr/` and `metrics/`).
Tooling that enforces rules (hooks, the language registry) lives in `tools/quality/`, not in `docs/`.

## Alternatives considered
| Option | Why not chosen |
|--------|----------------|
| Each folder at the root | Clutters the root; no single place to start reading |
| Architecture docs next to the code (`src/**/README.md`) | Fine for module notes, but there is no single overview; may be added later |

## Consequences
- **Positive:** one entry point (`docs/README.md`); `docs/` can be published as a site later.
- **Negative:** paths are slightly longer in skills and links.
- **Follow-up:** skills and `CLAUDE.md` reference the `docs/` paths.
