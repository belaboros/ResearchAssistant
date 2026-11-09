# 0000 · Set up spec-driven development

- **Status:** done
- **Captured:** 2026-11-09
- **Spec:** none (this idea set up the process itself)

## Original idea

> Set up specification-driven development for this project based on recommended best practices from Anthropic.
>
> Extra points:
>  - store all the original ideas and the questions you ask from me next to the ideas.

## Q&A log

_No questions were asked while building the setup. It is based on Anthropic's Claude Code best practices: have Claude interview you to write the spec, plan before coding, run each stage in a fresh session, give Claude a way to verify its work, and review with an independent subagent._

### 2026-11-09 · idea · Q1
**Asked by:** Claude
**Q:** [Claude] What's the first idea for ResearchAssistant? Describe it however you like and I'll store it word for word as idea `0001`, or run `/idea …` yourself. After that, `/spec 0001` in a fresh session starts the interview. I'll also commit these files on a branch if you want.
**Options offered:** free text
**A:** _Not answered yet._ (The next idea the user brought was a process idea, captured as [0001](0001-architecture-documentation-and-modularity.md). The first product idea is still to come.)
**Effect:** none yet

### 2026-11-09 · idea · Q2
**Asked by:** user
**Q:** Is it recommended to keep the specs/ and ideas/ folder directly at the project root or move them under a common sub-directory like docs/ ?
**A:** Claude: Anthropic doesn't recommend either layout. Keep them at the root while they are the only document folders; move them to `docs/` once more kinds arrive (ADRs, architecture docs, a docs site). Moving is cheap.
**Effect:** none at that point

### 2026-11-09 · idea · Q3
**Asked by:** Claude
**Q:** [Claude] Do you want to keep the current layout, or move both folders under `docs/` now? If you're already planning ADRs or a docs site, moving now is cleaner.
**Options offered:** keep at root / move to `docs/`
**A:** The user didn't answer directly; they asked for architecture.md, ADRs and metrics, and asked again whether to move things into subfolders (see [0001](0001-architecture-documentation-and-modularity.md) Q1).
**Effect:** everything moved under `docs/` (ADR-0003)

## Outcome

- `CLAUDE.md` describes the workflow and its rules.
- `docs/ideas/` holds the original ideas and their Q&A logs (moved from `ideas/` on 2026-11-09, ADR-0003).
- `docs/templates/` holds the idea, spec, plan, tasks and ADR templates.
- Skills: `/idea`, `/spec`, `/plan`, `/implement`, `/review-spec`. Subagent: `spec-reviewer`.
