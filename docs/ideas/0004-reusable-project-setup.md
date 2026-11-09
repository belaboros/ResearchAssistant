# 0004 · Reuse this setup for new repositories

- **Status:** in-progress (template published; release tag and repo protection pending)
- **Captured:** 2026-11-09
- **Spec:** none (process idea). Template: `../sdd-template-for-claude` → https://github.com/belaboros/sdd-template-for-claude

## Original idea

> I want to keep the "setup/spec-driven-development" for long-term
> to be able to start new git repos quickly and reuse this basic setup.
>
> What is your suggestion to make it smooth?

## Q&A log

### 2026-11-09 · idea · Q1
**Asked by:** user
**Q:** What is your suggestion to make it smooth?
**A:** Claude: don't keep it as a long-lived branch. It contains ResearchAssistant-specific content (ideas, sessions, the SECURITY.md URL), it can't create a new repo by itself, and it will drift from `main`. Instead, merge PR #1 here and extract a separate, generic **template repository** (`claude-sdd-template`) managed with **Copier**. `copier copy` creates a new project with the name and owner filled in, a post-copy task runs `pre-commit install`, and an optional `bootstrap-github.sh` applies the GitHub protection settings. `copier update` later pulls template improvements into existing projects. Simpler alternative: a GitHub "template repository" plus a bootstrap script, with no way to update projects later.
**Effect:** awaiting the user's choice (Q2–Q4)

### 2026-11-09 · idea · Q2
**Asked by:** Claude
**Q:** [Claude] Which approach: Copier template (recommended), a GitHub template repository plus a bootstrap script, or a Claude Code plugin plus a scaffold skill?
**Options offered:** Copier / GitHub template / plugin
**A:** "Let's use the copier approach."
**Effect:** Copier template built in `../sdd-template-for-claude` (copier.yml, `template/`, smoke-test CI, `tools/bootstrap-github.sh`)

### 2026-11-09 · idea · Q3
**Asked by:** Claude
**Q:** [Claude] Name and visibility of the template repository? Suggested: `belaboros/claude-sdd-template`, public.
**Options offered:** free text
**A:** "The name of the new git repo: belaboros/sdd-template-for-claude" (visibility not stated; Claude assumed public, as suggested)
**Effect:** creating the public repo was blocked by Claude Code's auto-mode safety check, so it was handed back to the user (Q5)

### 2026-11-09 · idea · Q4
**Asked by:** Claude
**Q:** [Claude] OK to merge PR #1 into ResearchAssistant `main` first, so the template is extracted from the merged state?
**Options offered:** yes / no
**A:** "I merged the PR to main."
**Effect:** template extracted from the merged `main`

### 2026-11-09 · idea · Q5
**Asked by:** Claude
**Q:** [Claude] Creating the public GitHub repo was blocked by Claude Code's auto-mode safety check. Should it be public, and will you create it yourself or allow Claude to?
**Options offered:** free text
**A:** "I created the git repo: https://github.com/belaboros/sdd-template-for-claude. Put the local files into it." (created public, with an Apache-2.0 LICENSE)
**Effect:** template rebased onto the user's initial commit (their LICENSE kept, the duplicate LICENSE.md dropped) and pushed to `main`; all CI checks pass; creating a project from `gh:belaboros/sdd-template-for-claude` works. The `v1.0.0` tag and the repo protection settings were blocked by the auto-mode safety check and are pending the user's go-ahead.
