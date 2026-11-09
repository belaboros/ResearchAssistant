# Spec 0005 · Evidence-based research assistant

- **Status:** approved
- **Idea:** [ideas/0005-evidence-based-research-assistant.md](../../ideas/0005-evidence-based-research-assistant.md)
- **Last updated:** 2026-11-09

## Problem
Answers found on the open internet are often unreliable: outdated, incorrect, incomplete, or opinion presented as fact. Pages contradict each other, and old advice keeps circulating after newer research has replaced it. For example, the recommended way to handle a child's fever is very different today from 40 years ago. Finding the current, evidence-backed answer by hand means searching many sites and judging each one, which is slow and error-prone. The user wants one place to ask a question and get an answer built only from approved research sources, with the evidence visible.

## Goals
- Answer a free-text question using **only** sources on a curated, per-category allowlist.
- Make the evidence visible: every claim is cited and dated, sources are ranked by level of evidence, and the answer carries a confidence level.
- Surface disagreement and change over time instead of hiding them.
- Say so plainly when the approved sources do not support an answer.
- Keep the allowlist growing under the user's control, through discovery runs that propose new sources for the user to approve.
- Keep a history of questions and answers that can be re-checked later.

## Non-goals
- Multiple users, accounts, login, or public hosting. This is a single-user tool running locally.
- The legal category, or any category beyond medical, software engineering and GenAI, unless the user approves it at runtime (FR-4).
- Reading paywalled full text, or storing subscription credentials.
- Medical triage, red-flag banners or emergency guidance beyond a standard disclaimer.
- Languages other than English.
- Re-checking saved answers automatically.
- A "quick mode". The first release only has a thorough mode.

## Users and scenarios
1. As the user (a parent), I want to ask "What should I do if my child has a fever?" and get the current guideline-based answer with citations and dates, so that I can trust it over random websites.
2. As the user (a software engineer), I want to ask a software-engineering or GenAI question and get an answer built from research and authoritative sources, with any disagreements shown, so that I can see where the field agrees and where it doesn't.
3. As the user, when I ask a question that fits no configured category, I want the system to propose a new category and candidate sources for me to approve, so that coverage grows without letting in unvetted sites.
4. As the user, I want a weekly list of candidate new sources, each with its reason, to approve or reject, so that the allowlist stays current.
5. As the user, I want to reopen an old answer, see how old it is, and re-check it, so that I notice when the evidence has moved on.

## Functional requirements
| ID    | Requirement | Priority |
|-------|-------------|----------|
| FR-1  | The user can enter a free-text English question in a local web app and receive an answer. | must |
| FR-2  | Each question is assigned to one of the configured categories (initially medical, software engineering, GenAI), and the assigned category is shown with the answer. | must |
| FR-3  | The user can override the assigned category and re-run the question. | should |
| FR-4  | If a question fits no configured category, the system does not answer. It proposes a new category name and candidate sources, and answers only once the user approves the category and at least one source. | must |
| FR-5  | Each category has its own allowlist of approved sources. Answers use only content from that category's approved sources. | must |
| FR-6  | The user can view, add and remove approved sources per category. | must |
| FR-7  | Source discovery proposes candidate sources (a) once a week, (b) when a new category is proposed (FR-4), and (c) when the user asks for it. Each candidate shows why it was proposed. | must |
| FR-8  | A candidate source is never used for answers until the user approves it. The user can approve or reject each candidate. | must |
| FR-9  | The system retrieves relevant articles from several approved sources per question, reading full text where it is freely available and the abstract otherwise. | must |
| FR-10 | Every answer opens with a short plain-language summary, followed by the detailed answer. | must |
| FR-11 | Every factual claim in the answer has an inline citation that links to the supporting article. | must |
| FR-12 | Every cited source shows its publication date (or last-reviewed date), and whether the full text or only the abstract was used. | must |
| FR-13 | Sources are weighed by level of evidence (for example, guideline or systematic review > single study > expert opinion), and each citation shows its evidence level. | must |
| FR-14 | When current approved sources disagree, the answer presents each position with its supporting sources instead of picking one silently. | must |
| FR-15 | When the recommendation has changed over time, the answer includes a dated timeline of how it changed. | must |
| FR-16 | Every answer shows an overall confidence level (high / medium / low), based on how strong and consistent the evidence is, with a one-line reason. | must |
| FR-17 | If the approved sources contain no relevant evidence, the system says "no reliable evidence found in approved sources", lists the sources searched, gives no answer, and offers to run source discovery. | must |
| FR-18 | Every medical-category answer carries a "not medical advice; consult a qualified professional" disclaimer. | must |
| FR-19 | Questions, answers and the retrieved articles are saved locally. The user can browse past questions and reopen any answer. | must |
| FR-20 | A reopened answer shows the date it was produced, and has a "Re-check" action that re-runs the question and highlights what changed from the saved answer. | must |
| FR-21 | While an answer is being produced, the web app shows progress (for example, sources being searched or articles being read). | should |

## Non-functional requirements
- **Latency:** an answer (or the no-evidence outcome) within 5 minutes per question, measured on the golden set (90th percentile).
- **Cost:** under US$1 in external API and model costs per question, averaged over the golden set; each run's cost is recorded.
- **Deployment:** runs on the user's own machine and is served on localhost only. No login.
- **Privacy:** questions, answers and articles stay on the local machine, except for the calls needed to search sources and run models. No secrets or personal data in the public repo (CLAUDE.md rule 8).
- **Source compliance:** fetch only freely accessible content; respect robots.txt and the sites' rate limits.
- **Quality gate:** the golden question set (see AC-15) is the release gate for the first version.

## Acceptance criteria
- [ ] AC-1 (FR-1): Given the app is running locally, when the user submits an English question in the browser, then an answer page is displayed.
- [ ] AC-2 (FR-2): Given each golden-set question has an expected category, when it is submitted, then the assigned category matches the expected one for at least 90% of the set, and the category is shown on the answer.
- [ ] AC-3 (FR-3): Given an answered question, when the user changes the category and re-runs it, then the new answer cites only sources from the chosen category's allowlist.
- [ ] AC-4 (FR-4): Given a question outside all configured categories (e.g. a legal question), when it is submitted, then no answer is produced and a proposed category with at least one candidate source is shown; after the user approves them, re-submitting produces an answer.
- [ ] AC-5 (FR-5): Given any golden-set answer, when its citations are checked, then every cited URL belongs to a source on the approved allowlist of the answer's category.
- [ ] AC-6 (FR-6): Given the source management page, when the user adds and then removes a source, then the allowlist reflects each change, and answers produced after the removal never cite that source.
- [ ] AC-7 (FR-7): Given discovery is triggered on demand, when it completes, then a list of candidates appears, each with a stated reason; and a scheduled discovery run is configured to run once a week.
- [ ] AC-8 (FR-8): Given a pending (not yet approved) candidate source that contains relevant content, when a question is answered, then that candidate is not cited; after the user rejects it, it no longer appears in the pending list.
- [ ] AC-9 (FR-9, FR-12): Given any golden-set answer, then it cites at least two distinct approved sources (or returns the no-evidence outcome), and each citation is marked "full text" or "abstract".
- [ ] AC-10 (FR-10, FR-11): Given any golden-set answer, then it begins with a summary of at most 3 sentences, and every sentence of the detailed answer that states a fact has at least one inline citation link.
- [ ] AC-11 (FR-12, FR-13): Given any golden-set answer, then every citation shows a publication or last-reviewed date and an evidence level.
- [ ] AC-12 (FR-14, FR-15): Given a golden-set question known to have conflicting current sources, then the answer presents each position with its sources; given a question whose advice changed over time (the fever case), then the answer includes a dated timeline with at least two entries.
- [ ] AC-13 (FR-16): Given any answer, then it shows a confidence level of high, medium or low, plus a one-line reason.
- [ ] AC-14 (FR-17): Given a question that the approved sources do not cover, when it is submitted, then the response says "no reliable evidence found in approved sources", lists the sources searched, contains no answer text, and offers to run discovery.
- [ ] AC-15 (FR-1, FR-10–FR-16): Given the golden set (about 10 questions per category, each with a reference answer written by the user), when it is run, then at least 80% of answers match the reference answer's gist (judged by the user), and AC-5, AC-9, AC-10, AC-11 and AC-13 hold for every answer.
- [ ] AC-16 (FR-18): Given any medical-category answer, then the disclaimer is displayed; given any non-medical answer, then it is not.
- [ ] AC-17 (FR-19): Given several questions were asked, when the app is restarted, then all of them appear in the history and each can be reopened with its original answer and citations.
- [ ] AC-18 (FR-20): Given a saved answer, when it is reopened, then its production date is shown; when "Re-check" is used, then a new answer is produced and the differences from the saved answer are highlighted, and the saved answer is kept.
- [ ] AC-19 (FR-21): Given a question is being processed, then the page shows at least one progress update before the answer arrives.
- [ ] AC-20 (NFR latency, cost): Given a full golden-set run, then the 90th-percentile time per question is at most 5 minutes and the average recorded cost is under US$1.

## Edge cases and failure modes
- **Source unreachable or rate-limited:** skip it, continue with the other sources, and list it as "not reached" with the answer. If this leaves no evidence, use the no-evidence outcome (FR-17).
- **Only abstracts available:** answer from the abstracts, mark the citations as "abstract", and take this into account in the confidence level.
- **Question is ambiguous or fits two categories:** assign the most likely category, show it, and let the user override it (FR-3).
- **Retracted or withdrawn article:** if the source marks it as retracted, do not cite it as support.
- **All approved sources are old:** answer, show the dates, and let the confidence level reflect the age.
- **Discovery finds nothing:** report that no candidates were found; nothing changes.
- **User removes the last source in a category:** questions in that category get the no-evidence outcome with a hint to add sources.
- **Cost or time budget reached mid-run:** stop gathering, answer from what has been collected if that is enough, and say the run was cut short. Otherwise use the no-evidence outcome.
- **Model or search API unavailable:** show a clear error. Nothing partial is saved as an answer.
- **Non-English question:** say that only English is supported.

## Open questions
- Who writes the golden question set and the reference answers? The assumption is the user writes them, possibly from a first draft by Claude, before `/implement` finishes.
- What do the starting allowlists contain for each category (for example WHO, NICE, CDC, Cochrane and PubMed for medical; arXiv and ACM for software engineering and GenAI)? This can be settled in `/plan` and approved by the user then.
