# UX test findings: code-and-specs-first start

Test of mlmd's first-run experience, run by hand from process.md and docs/ux/initial-experience.md.
Tester: Frank Verbruggen (technical expert), Windows 10, Claude Code desktop app.

| Date | Step | Source | Finding |
| --- | --- | --- | --- |
| 2026-10-05 | 3 (read code and specs) | agent | The script is inconsistent about order: process.md "Using mlmd" says mlmd *writes* the business meaning into the documents and then asks about gaps; initial-experience.md §4 says it lists the documents and then asks, with writing implied after. Unclear whether the user sees drafts before the first gap question. This run follows initial-experience.md: list, ask, write at topic end. |
| 2026-10-05 | 2 (orientation) | agent | The orientation must come "before the first question", but in the code-and-specs start the first question after it ("which code and specs?") is a setup question, not an interview question. The phase line has to explain 1a before the user has said what they are building. |
| 2026-10-05 | 3 (which code and specs) | agent | Frank answered with a name ("Import MTG Judge"), not a path. mlmd had to search the disk and found two candidates: `~/git/mtg-judge` (repo) and `~/Documents/Frank/MtG Judge!` (the older ChatGPT package, also copied into the repo under docs/reference/). The design has no step for resolving a name to a source or for confirming the pick. In this run mlmd chose the repo and said so, without asking. |
| 2026-10-05 | 3 (read code and specs) | agent | **Phase mismatch.** MTG Judge is far past 1a: PRD v0.5 with 66 decisions, 24 accepted ADRs, a plan, and the build has started (iteration E3, monorepo scaffold). The orientation screen still says "you are here: 1a", which is wrong for this project. The code-and-specs start assumes a project with code but no requirements process behind it. It needs a way to place an imported project at its real phase, or at least to say "1a is filled from your PRD, then we skip ahead". |
| 2026-10-05 | 3 (read code and specs) | agent | Volume: the specs come to about 230 KB of prose plus 348 golden cases, and the code is a stub. "Read the code and specs" took several large reads before the user saw anything, with no progress shown. The design should show progress while reading, or read the index first and say what it will skip. |
| 2026-10-05 | 4 (gap questions) | agent | Only 5 gap questions for a 66-decision PRD went fast, and Frank answered in 1–4 words. One question at a time suited this. Q4 offered numbered options, and the bare "1" reply worked well. Consider offering options by default when there is a real choice. |
| 2026-10-05 | 4 (gap questions) | agent | The interview had only one topic (the whole product), so the "summary at the end of each topic" came only at the very end. For an import, the per-topic summary is the user's first view of anything written. The design doesn't say how an import is divided into topics. |
| 2026-10-05 | 5 (summary) | agent | Most of the content was copied from the PRD, not decided in the session, so "who decided" is mostly old PRD ids. The summary has to separate *imported* from *decided today*, or the user can't see what changed. |
| 2026-10-05 | 5 (summary) | agent | The source repo keeps living (PRD v0.5, OWNER-ANSWERS). After an import there are two masters: the PRD and the mlmd documents. The design says nothing about retiring the source or keeping the two in sync. Same risk as the source project's own R11. |

## Frank's responses (2026-10-05, out of role; fixes agreed with Alex)

1. Phase mismatch: fine. Run the user through every phase from 1a instead; mlmd is meant for enterprise applications.
2. Two masters: the new version is the only one mlmd cares about.
3. Name, not a path: when unsure which sources are meant, let the user pick.
4. Order: the process is iterative. For brownfield, interpret and write, then ask. "Write what users mean."
5. Imported vs decided in the summary: asked for a suggestion.
6. Topics for an import: asked for a suggestion.
7. Reading without progress: fine; show something if it takes longer than 20–30 seconds.
8. Orientation before the setup question: loved it as is.
9. One question at a time with options: loved it.

## Session 2: 1a design-review and 1b (2026-10-05)

| Date | Step | Source | Finding |
| --- | --- | --- | --- |
| 2026-10-05 | 1 (status line) | agent | §6 defines the status line as "phase, progress, next question", but a review has no question queued yet and no measurable progress until its findings exist. The line had to say "review not started; 0 of ? findings". The design needs a status form for non-interview steps. |
| 2026-10-05 | 2 (review) | agent | "Fresh session" plus "read only the committed files" isn't enough for an imported vision: the review could only find V1 and V5 by re-reading the PRD (about 66 KB). The rule that the next session reads only output files conflicts with reviewing an import against its source. |
| 2026-10-05 | 2 (review) | agent | process.md doesn't say how review findings are settled with the user. This run asks one finding at a time, like gap questions, and records them in ux-test/docs/review-vision.md. |
| 2026-10-05 | 2→1b (transition) | agent | The design-review and 1b ran in one session. process.md says the review runs "in a fresh session", but nothing says whether 1b may follow in the same one; the sliding rule (+100K) allowed it. Reading PRD §6–8 for 1b added ~25K tokens; the vision alone wasn't enough to start 1b. |
| 2026-10-05 | 1b (start) | agent | Order conflict: the code-and-specs start says "write what the sources mean, then ask gaps", but 1b's first gap ("Keep it small": what is MVP 1 for?) decides which behaviors get written at all. Writing ~45 behaviors first would waste work if the scope shrinks. This run asks the scope question first. A scope gap should come before the writing. |
| 2026-10-05 | 1b (start) | agent | Spike S1 (voice) hasn't run in MTG Judge. That isn't a 1b gap (spikes are phase 3), so voice stays "droppable". The design doesn't say how mlmd shows a dependency on a later phase during an earlier one. |
| 2026-10-06 | 1b, scope question | Frank | "The Minimum Viable Product is a full product", so MVP 1 keeps its full scope, "but the phases to get there can be split to keep overview". process.md 1b's "Keep it small: expect to push for this" reads as shrinking the MVP. Frank wants a full MVP, split for overview into build steps. The recommended option (split the MVP) went against his view of what an MVP is. Proposal for process.md (not edited): in 1b, "keep it small" pushes back on features that don't serve what the MVP is for, and the split for overview happens in phase 4's batches. |
| 2026-10-06 | 1b, question 3 | agent | Frank's answer ("unresolved only on infractions or illegal board states") raised a new contradiction with FR-Q-6, so "3 of 3" became "4 of 4". The count in the design promises a fixed total, but follow-up questions can add to it. The design should say how a count grows, for example "question 4 of 4 (1 added)". |
| 2026-10-06 | sliding context | agent | The new-session suggestion came at a topic boundary, not at the +100K threshold: the next topic (writing ~45 behaviors) is large and would build on a context already holding the full PRD. D-19 defines only token thresholds. A suggestion "before a large topic" would be useful too. In the desktop app mlmd can't create a card by hand, so the test shows the first prompt as text. |
| 2026-10-06 | 5a summary | agent | Two decisions were mlmd's own ("so work could continue"). The §5a "decided today" table has a "who" column, but nothing tells the user to check mlmd's decisions. This run marks them "please check" in the summary. process.md says "Read every decision marked as the agent's", but the summary is the only place the user sees them. |

## Frank's responses to session 2 (2026-10-06, out of role)

1. Scope: "I do not want the MVP to be small, I want my sessions making it to be small." Building the MVP is a series of sliding context windows, each one small. He's very happy with the new-session suggestion at the end of the 1b interview.
2. Order conflict in 1b: report to Alex when he starts. **For Alex:** in the code-and-specs start, "write what the sources mean, then ask gaps" conflicts with 1b, where the scope gap decides which behaviors get written. Proposal: ask scope gaps before writing.
3. Reviewing an import: reading the existing documents is always allowed when needed, and only then.
4. mlmd's own decisions, after explanation: option 1. Each topic summary ends with a separate block, "mlmd decided, please confirm", one numbered line each; the user replies "ok" or e.g. "2: no".
5. Status line for a review, after explanation: fine. "When talking in a normal interaction, I don't need to know everything in advance." A step without questions names itself as "next".
6. Growing count: fine as shown ("4 of 4 (1 added)").
7. Cards: mlmd can make a card in the desktop app (it has done so before). Finding withdrawn: use the card.
