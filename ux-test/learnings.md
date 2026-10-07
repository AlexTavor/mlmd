# UX test learnings: code-and-specs-first start (sessions 1–3)

Consolidated 2026-10-07 from `ux-test/findings.md` (sessions 1–3, Frank's responses) against
`process.md` and `docs/ux/initial-experience.md` on main at `38d8aeb`. The test ended in 1b, at the
first behaviors gap question (Frank, 2026-10-07).

## A. Already in the design

| # | Learning | Where | Source |
| --- | --- | --- | --- |
| A1 | When unsure which sources the user means, show candidates; the user picks. | process.md Using mlmd; initial-experience §4 | session 1, response 3 |
| A2 | Show progress when reading takes over ~20 s. | same | session 1, response 7 |
| A3 | Import: interpret and write first, then ask gaps. | same | session 1, response 4 |
| A4 | Run every phase from 1a, however far the project got. | same | session 1, response 1 |
| A5 | Topics of an import are the sections the phase fills; count per topic and overall. | same | session 1, response 6 |
| A6 | Import summary in three parts: decided today, changed from source, imported unchanged. | initial-experience §5a | session 1, response 5 |
| A7 | After import, mlmd's documents are the only master. | process.md Using mlmd | session 1, response 2 |
| A8 | A session opens with its goal (1–2 sentences), then the status line, then the first question. | initial-experience §6 | session 3 |
| A9 | A session never crosses a phase; the review belongs to its phase (resolves "review and 1b in one session"). | process.md §0, D-21 | session 2 finding; Alex, D-21 |

## B. Agreed by Frank, not yet in the design

| # | Learning | Proposed change | Source |
| --- | --- | --- | --- |
| B1 | mlmd's own decisions need explicit confirmation. | Each topic summary ends with a separate block "mlmd decided, please confirm", one numbered line each; the user replies "ok" or "2: no". initial-experience §5/§5a; process.md 1a/1b "You". | session 2, response 4 |
| B2 | A question count can grow. | Show it as "question 4 of 4 (1 added)". initial-experience §4. | session 2, response 6 |
| B3 | A step without questions (a review, writing) has no "next question". | The status line names the step itself as "next". initial-experience §6. | session 2, response 5 |
| B4 | Every session ends with commit **and merge to main**, before the next card. | process.md §0 "A session ends by writing its output files and committing them" → "…committing and merging them into main". | session 2, Frank's rule |
| B5 | Suggest a new session before a large topic, not only at +100K. | process.md §0 and the master rule: add "or before a large new topic". | session 2, response 1 |
| B6 | A review may read the source documents when needed, and only then. | process.md 1a/1b Review: "a fresh session reads the documents under review; it reads the sources only when a check needs them". | session 2, response 3 |
| B7 | The MVP isn't small; the sessions are. | process.md 1b "Keep it small" → push back on features that don't serve what the MVP is for; split the work for overview into phase 4 batches and small sessions, not into smaller MVPs. | session 2, scope; response 1 |
| B8 | Scope gaps come before writing. | Code-and-specs start: "write, then ask gaps" except gaps that decide what gets written (scope), which are asked first. For Alex. | session 2, response 2 |

## C. Open: needs Frank's decision

| # | Learning | Proposal | Source |
| --- | --- | --- | --- |
| C1 | The standing documents were imported silently and never shown; 1b started without them. The 1a topic list names "standing documents", but 1a's "Done when" doesn't check them. | 1a "Done when" adds: "the standing documents exist in the form above, and the user has seen each one's summary". The standing-documents topic is never skipped, even on an import. | session 3, Frank |
| C2 | An import summary's "imported unchanged" was false: glossary.md claimed 26 terms but held 4. | Before a summary, mlmd checks each "imported unchanged" count against the written file. | session 3, agent |
| C3 | Review findings had no defined way to be settled. | Settle like gap questions: one at a time, recorded in `docs/reviews/<doc>.md` with status. | session 2, agent |
| C4 | A dependency on a later phase (a feature waiting on a spike) has no place in an earlier one. | Mark it "depends on spike <n>" in the Vision's feature line and in open-questions.md "needed by". | session 2, agent |
| C5 | A gap question explained the conflict but not what the answer changes. | Each question ends with one line: what the answer decides. (Frank found the full intro overcompensating; this one is per question, not per session.) | session 3, agent |

## Frank's decisions (2026-10-07)

- A: ok.
- B1, B2, B4, B5, B6, B7, B8: agreed, written into process.md (§0, 1a, 1b) and initial-experience.md (§4, §5, §5a). B3: no need, dropped.
- C2, C3, C4, C5: agreed and written. C3 goes in process.md 1a Review.
- C1: the standing-documents gate is written into 1a "Done when". Frank widened it: "As a user, I want to know what I need to focus my attention on next and might want to have some overview of what my entire process looks like. Not having any overview on this makes me feel lost." The overview's form is open.
- C1 overview: option 3 (Frank, 2026-10-07): a two-line status (phase map, current topics with "your focus") plus a pinned overview artifact republished at each commit; `/mlmd:status` in the CLI. Written into initial-experience §6 and process.md Using mlmd.
- D1 resume after import (Frank, 2026-10-07): "if a session is resumed, the mlmd scripts should detect that after import and not ask everything again. Progress needs to be visible for the user and repeating all steps feels bad." Written into initial-experience §4. The trial state for the test project is saved in that project's repository, so a next test resumes there.
