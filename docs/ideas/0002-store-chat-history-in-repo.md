# 0002 · Store Claude chat history in the repo

- **Status:** done
- **Captured:** 2026-11-09
- **Spec:** none (process idea). Decision: [ADR-0007](../architecture/adr/0007-session-history-as-curated-transcripts.md)

## Original idea

> Claude chat history will be stored regularly into this git repo.
> What is your recommendation?

## Q&A log

### 2026-11-09 · idea · Q1
**Asked by:** user
**Q:** Claude chat history will be stored regularly into this git repo. What is your recommendation?
**A:** Claude: commit curated session summaries to `docs/sessions/`, generated automatically when a session ends. Don't commit raw transcripts to the main branch. They hold full tool output (file contents, command output, possible secrets), personal details and local paths; they make the repo large, produce diffs nobody can review, and would be read back into Claude's context by later searches. If raw transcripts are needed for audit, keep them in a separate private repo (or at least an orphan branch), scanned with a secret scanner before each push, and blocked from Claude's reads.
**Effect:** awaiting the user's answers to Q2 and Q3

### 2026-11-09 · idea · Q2
**Asked by:** Claude
**Q:** [Claude] Is the GitHub repo public, or will it be?
**Options offered:** free text
**A:** "This is a public repo on GitHub, let's add extra checks (gitleaks and others) to make it safe/secure."
**Effect:** became idea [0003](0003-security-checks-for-public-repo.md) / ADR-0008

### 2026-11-09 · idea · Q3
**Asked by:** Claude
**Q:** [Claude] Which should I set up: curated summaries only, summaries plus raw transcripts in a separate private repo, or summaries plus raw transcripts in this repo on an orphan branch?
**Options offered:** curated summaries only / summaries + separate private repo / summaries + orphan branch in this repo
**A:** Answered by the user's instruction in Q5: a curated file in this repo, no raw transcripts.
**Effect:** ADR-0007

### 2026-11-09 · idea · Q4
**Asked by:** user
**Q:** What is included in the chat summaries?
**A:** Claude proposed this content: a header (date, session id, model, branch, start/end commit), the user's requests verbatim, outcome, decisions with links to ADRs, ideas and specs touched, files changed, commits, questions asked with their answers (linking to the idea Q&A logs), checks that were run and their results, problems and deviations, and open items with next steps. Not included: tool output, file contents, Claude's reasoning, environment details, secrets, personal data beyond the git author. Written by Claude at session end (it needs judgement, so not a script), then scanned for secrets before commit.
**Effect:** proposal; awaiting Q2/Q3

### 2026-11-09 · idea · Q5
**Asked by:** user (instruction, in reply to Q3/Q4)
**Q:** Session history shall be saved as summary containing:
 - summary: informal summary of what happened during the session (max 5 sentences)
 - my requests
 - your responses (without your thinking , tools you call and their output)

What to leave out:
 - your (Claude's) process
 - tools that you call
 - output of tools you call
**A:** Claude built it: `tools/sessions/save-session.sh` (jq extraction, so requests and replies are copied verbatim, plus a Haiku call for the summary), run by a `SessionEnd` hook, writing to `docs/sessions/`.
**Effect:** ADR-0007; Q2 (is the repo public?) still open; it decides whether a secret scan is needed before push

### 2026-11-09 · idea · Q6
**Asked by:** user (instruction)
**Q:** The name of the session history file should contain the time as well: YYYYMMDD-HHMMSS-daba91b7.md. Please modify ADR-0007
**A:** Claude edited ADR-0007 in place (it wasn't committed yet) and changed the script. The time is the session's first request, in UTC.
**Effect:** ADR-0007 updated; `2026-11-09-daba91b7.md` renamed to `20261109-131159-daba91b7.md`
