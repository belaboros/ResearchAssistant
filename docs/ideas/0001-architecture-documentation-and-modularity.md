# 0001 · Architecture documentation, ADRs and enforced modularity

- **Status:** done (process set up; enforcement for each language is added later with `/add-language`)
- **Captured:** 2026-11-09
- **Spec:** none (process idea). Decisions: ADR-0001, ADR-0003 … ADR-0006

## Original idea

> The architecture shall be documented in an
>  - architecture.md file
>  - and list of ADRs
>
> Goal:
>  - any average engineer should be able to understand the current latest architecture by reading the architecture.md file.
>  - after reading the series of ADRs, the reader should understand what architectural decisions were made.
>
> The architecture.md shall contain diagrams to help quick visual understanting.
>  - context diagram and explanation: application and the external users/components it uses
>  - component diagram: logical components, their relations/dependencies
>
> Maintainability and modularity
>  - goal: high-cohesion and low-coupling
>  - calculate metrics to proof that maintainability, modularity is healthy
>  - cyclomatic complexity, efferent coupling, afferent coupling
>  - diagrams show the coupling between modules/logical components of the application
>  - metrics are calculated and stored as a proof
>
> Enforce high-cohesion and low-coupling
>  - when a new prohramming language (python, javascript, rust, go, ...) is introduced add hooks, linters, checkers, rules to enforce high-cohesion, low coupling and maintainability.
>
> Question: do you recommend moving ideas, specs, ADRs, ... to subfolders?

## Q&A log

### 2026-11-09 · architecture · Q1
**Asked by:** user
**Q:** do you recommend moving ideas, specs, ADRs, ... to subfolders?
**A:** Claude: yes, now. With architecture.md, ADRs, metric snapshots and templates added, there would be five or more documentation folders, which is the point where grouping them under `docs/` pays off. Layout: `docs/{ideas,specs,templates,architecture/{adr,metrics}}`; enforcement tooling goes in `tools/quality/`.
**Effect:** moved, and recorded as ADR-0003

### Defaults chosen without asking (can be changed with a superseding ADR)
- Diagrams: Mermaid flowcharts in the C4 style (ADR-0004).
- Thresholds: CC ≤ 10 passes, 11–15 warns, > 15 fails; Ce ≤ 4 passes, 5–7 warns, > 7 fails; 0 cycles; 0 boundary violations (ADR-0005, metrics/README.md).
- Enforcement: a PreToolUse hook blocks unregistered languages; `/add-language` sets up the linter, boundary checker, collector, pre-commit, PostToolUse lint hook and CI (ADR-0006).
- Snapshots are stored on each `/review-spec` and on demand, not on every commit.

## Outcome
- `docs/architecture/architecture.md`: context, component and coupling sections (empty until code exists).
- `docs/architecture/components.json`: the single source of truth for components and allowed dependencies.
- `docs/architecture/adr/`: ADR-0001 … 0006.
- `docs/architecture/metrics/`: definitions, thresholds, snapshot schema, `latest.md`, `history/`.
- `tools/quality/`: language registry and the PreToolUse gate. Skills: `/adr`, `/arch-metrics`, `/add-language` (with `playbook.md`).
