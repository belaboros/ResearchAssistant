# 0003 · Security checks for a public repository

- **Status:** done
- **Captured:** 2026-11-09
- **Spec:** none (process idea). Decisions: [ADR-0008](../architecture/adr/0008-security-checks-for-a-public-repository.md), [ADR-0009](../architecture/adr/0009-github-repository-protection.md)

## Original idea

> I allow you to push changes to GitHup for this repository.
>
> This is a public repo on GitHub, let's add extra checks (gitleaks and others) to make it safe/secure.

## Q&A log

### 2026-11-09 · idea · Q1
**Asked by:** Claude (asked in session; logged here because it led to this idea)
**Q:** [Claude] Is the GitHub repo public? If it is, I'd add the `gitleaks` pre-commit check before the first push, because from then on session files get pushed regularly.
**Options offered:** free text
**A:** "This is a public repo on GitHub, let's add extra checks (gitleaks and others) to make it safe/secure." (Also answers idea 0002 Q2.)
**Effect:** ADR-0008; `git push` added to the allowed permissions in `.claude/settings.json`

### Defaults chosen without asking
- Local hooks: gitleaks, detect-private-key, large files > 500 KB, merge conflicts, JSON/YAML, shellcheck, zizmor.
- CI: full-history gitleaks on push to main, on PRs and weekly; SHA-pinned actions; Dependabot with a 7-day cooldown.
- GitHub repo settings were first left unchanged (secret scanning and push protection were already on); the extra settings were offered to the user (Q2/Q3).

### 2026-11-09 · idea · Q2
**Asked by:** Claude
**Q:** [Claude] Should I open a pull request from `setup/spec-driven-development` into `main`? That would run the security workflow for the first time, and you could merge once it passes.
**Options offered:** free text
**A:** "Create pull-request to main."
**Effect:** PR #1 opened; both security checks pass

### 2026-11-09 · idea · Q3
**Asked by:** Claude
**Q:** [Claude] Should I also change these GitHub repository settings? Protect `main` so the security workflow has to pass before merge; private vulnerability reporting; Dependabot security updates; secret scanning for generic secrets and validity checks.
**Options offered:** free text
**A:** "Let's turn on the protection you suggested with GitHub repo settings."
**Effect:** ADR-0009. Everything enabled except generic-pattern secret scanning and validity checks, which GitHub left disabled (they appear to need a paid add-on)
