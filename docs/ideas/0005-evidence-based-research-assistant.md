# 0005 · Evidence-based research assistant

- **Status:** specified
- **Captured:** 2026-11-09
- **Spec:** [specs/0005-evidence-based-research-assistant/spec.md](../specs/0005-evidence-based-research-assistant/spec.md)

## Original idea

> This is a ResearchAssistant project helps to find answers to questions from research articles.
>
> Pain point: reliability of content from the Internet
>  - any website, article, YouTube video may contain information which is not reliable, outdated, incorrect, incomplete, ...
>  - different webpages may contain contradicting advices, opinions (presented as as facts)
>  - old advices may not be supeseded by new information based on the latest research
>
> Example case:
>  - What shall I do if my child has a fewer?
>  - The recommended official procedure was completely different 40 years ago than nowadays and may change in the future.
>  - Parents can open this application and get answers faster from approved research websites than searching the internet manually 
>
> About this application:
>  - you may ask any question
>  - the questions are categorized as medical, software engineering, genAI, legal, ...
>  - the application tries to find reliable set of website which store articles supported by facts, evidence, 
> experiments, ...
>  - sources: research institutes, universities, ...
>  - different set of websites are used for medical questions than for software engineering questions
>  - downloads related articles from multiple sources
>  - the articles are analysed and synthesized
>  - the final outcome is presented

## Q&A log

<!--
Append one entry per question, oldest first. Never delete entries.

### YYYY-MM-DD · <stage: idea | spec | plan | implement | review | architecture> · Q<n>
**Asked by:** Claude | user
**Q:** [<skill · stage>] <question exactly as asked>
**Options offered:** <list, or "free text">
**A:** <the answer, verbatim if it is the user's; a summary if it is Claude's>
**Effect:** <what changed because of it, e.g. "added FR-4 to spec">
-->

### 2026-11-09 · spec · Q1
**Asked by:** Claude
**Q:** [spec · clarifying question] Who are the first real users of the research assistant?
**Options offered:** Just me (personal tool); Family and friends; The general public
**A:** Just me (personal tool)
**Effect:** Single-user tool; no accounts, multi-tenancy or public abuse protection in the first release (non-goal).

### 2026-11-09 · spec · Q2
**Asked by:** Claude
**Q:** [spec · clarifying question] Which question categories should the first release cover?
**Options offered:** Medical / health; Software engineering; GenAI; Legal (multi-select)
**A:** Medical / health, Software engineering, GenAI
**Effect:** Three categories in scope; legal is a non-goal for the first release. Medical answers need safety handling.

### 2026-11-09 · spec · Q3
**Asked by:** Claude
**Q:** [spec · clarifying question] How should people use it in the first release?
**Options offered:** Web app; Command line; Inside Claude (MCP/skill)
**A:** Web app
**Effect:** The first release is a browser-based web app.

### 2026-11-09 · spec · Q4
**Asked by:** Claude
**Q:** [spec · clarifying question] Who decides which websites count as 'approved' sources for each category?
**Options offered:** Curated allowlist (Recommended); Allowlist + automatic discovery; Fully automatic
**A:** Curated allowlist + find potential new sites weekly, for questions in a new category or when the user asks to update the list of allowed sites.
**Effect:** Curated per-category allowlist is the only source of answers; a discovery job proposes candidate sites weekly, for a new category, or on demand. Follow-up needed on whether candidates need approval.

### 2026-11-09 · spec · Q5
**Asked by:** Claude
**Q:** [spec · clarifying question] When source discovery finds a candidate new website, what happens before it is used for answers?
**Options offered:** I approve each one (Recommended); Auto-add, I can remove
**A:** I approve each one (Recommended)
**Effect:** Discovered sites are candidates only, never used until the user approves them; each shows why it was proposed.

### 2026-11-09 · spec · Q6
**Asked by:** Claude
**Q:** [spec · clarifying question] What should happen when you ask a question that fits none of the configured categories?
**Options offered:** Propose a new category; Refuse and explain; Answer from a general list
**A:** Propose a new category
**Effect:** Out-of-category question ⇒ system proposes a category name plus candidate sources; no answer until the user approves.

### 2026-11-09 · spec · Q7
**Asked by:** Claude
**Q:** [spec · clarifying question] How should the answer handle sources that disagree, or advice that has changed over time (like your fever example)?
**Options offered:** Prefer newest guidance; Show the disagreement; Weigh by evidence level; Show a timeline (multi-select)
**A:** Show the disagreement, Weigh by evidence level, Show a timeline
**Effect:** Answers rank evidence by level, show disagreements with their sources, and include a dated timeline of how advice changed. "Prefer newest" was not chosen: recency alone does not decide.

### 2026-11-09 · spec · Q8
**Asked by:** Claude
**Q:** [spec · clarifying question] What must every answer include?
**Options offered:** Inline citations; Publication dates; Confidence level; Plain-language summary (multi-select)
**A:** Inline citations, Publication dates, Confidence level, Plain-language summary
**Effect:** All four become must-have answer-content requirements.

### 2026-11-09 · spec · Q9
**Asked by:** Claude
**Q:** [spec · clarifying question] What should happen when the approved sources contain no good evidence for a question?
**Options offered:** Say so, no answer (Recommended); Best-effort, flagged low
**A:** Say so, no answer (Recommended)
**Effect:** No-evidence outcome: explicit "no reliable evidence found", list of sources searched, offer to run discovery. No partial answer.

### 2026-11-09 · spec · Q10
**Asked by:** Claude
**Q:** [spec · clarifying question] How should medical questions that might be urgent (e.g. a fever in a newborn) be handled?
**Options offered:** Red-flag banner + disclaimer; Disclaimer only; Nothing special
**A:** Disclaimer only
**Effect:** Every medical answer carries a "not medical advice, consult a doctor" disclaimer; no red-flag triage banner (non-goal).

### 2026-11-09 · spec · Q11
**Asked by:** Claude
**Q:** [spec · clarifying question] What are your limits on time and cost per question?
**Options offered:** Thorough: ~5 min, <$1; Balanced: ~1 min, <$0.25; Two modes: quick and deep
**A:** Thorough: ~5 min, <$1
**Effect:** NFR: an answer within ~5 minutes and under US$1 per question; full-text reading of many articles is acceptable.

### 2026-11-09 · spec · Q12
**Asked by:** Claude
**Q:** [spec · clarifying question] Where should it run, and should it keep past questions?
**Options offered:** Local, keep history; Cloud-hosted, keep history; Local, no history
**A:** Local, keep history
**Effect:** Runs locally (localhost on the user's Mac); stores questions, answers and downloaded articles for later review. Cloud hosting and login are non-goals.

### 2026-11-09 · spec · Q13
**Asked by:** Claude
**Q:** [spec · clarifying question] Many research articles are paywalled. What may the assistant read?
**Options offered:** Open access + abstracts (Recommended); Also my own subscriptions; Abstracts only
**A:** Open access + abstracts (Recommended)
**Effect:** Full text only where freely available, abstract otherwise; each citation shows whether full text or abstract was used. No paywall credentials (non-goal).

### 2026-11-09 · spec · Q14
**Asked by:** Claude
**Q:** [spec · clarifying question] Since history is kept, what should happen when you reopen an old answer?
**Options offered:** Show age + 'Re-check' button; Automatic weekly re-check; Static archive
**A:** Show age + 'Re-check' button
**Effect:** Saved answers show their date; a user-triggered re-check re-runs the question and highlights what changed. No automatic re-checking.

### 2026-11-09 · spec · Q15
**Asked by:** Claude
**Q:** [spec · clarifying question] How will we know the first release is good enough?
**Options offered:** Golden question set (Recommended); My judgement in daily use
**A:** Golden question set (Recommended)
**Effect:** Release gate: a golden set of ~10 questions per category with known current answers, evaluated against the acceptance criteria.

### 2026-11-09 · spec · Q16
**Asked by:** Claude
**Q:** [spec · clarifying question] Which languages must questions and answers support?
**Options offered:** English only; Ask in any language
**A:** English only
**Effect:** English only in the first release; other languages are a non-goal.

### 2026-11-09 · spec · Q17
**Asked by:** Claude
**Q:** [spec · approval] Do you approve spec 0005 as summarised above (goals, 21 functional requirements, 20 acceptance criteria, two open questions left for /plan)?
**Options offered:** Approve; Changes needed
**A:** Approve
**Effect:** Spec 0005 set to approved; idea set to specified. Open questions (golden-set authorship, seed allowlists) deferred to /plan.
