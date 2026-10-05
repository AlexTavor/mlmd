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
