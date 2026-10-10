# mlmd: the foundation documents as the operator experiences them

How the documents that define things feel to the operator: `docs/glossary.md`, `docs/rules.md`,
`docs/footguns.md`, and (scope to be decided) `docs/assumptions.md` and `CLAUDE.md`. When each
first appears, how terms, rules and traps come up, what the operator sees, decides and feels, and
whether these documents give confidence that the work stays sound. Not the mechanics: those are
Alex's (see For Alex below). Limited to the first Product Increment, like `flow.md`.

Status: working draft by Claude, 2026-10-10. Frank's decisions go under Decisions; the rest is not
agreed yet. The map is read from process.md at `66c0e7a` (Standing documents, Using mlmd, phases 1
to 4), BR-39 and BR-40, and `initial-experience.md` §5b, not from a run. Evidence: the mtg-judge UX
test, where these documents got no attention and the work felt rushed and sloppy (D-22;
`ux-test/findings.md`, session 3, 2026-10-07).

## Journey map

| # | Moment | Sees | Does | Feels |
| --- | --- | --- | --- | --- |
| 1 | `/mlmd:start` | Nothing of these documents. CLAUDE.md is written with its imports; the four files are created, empty | Nothing | Nothing. They exist without the operator knowing |
| 2a | Code-and-specs start: the foundation pass (BR-39) | Drafts of all four, then "go through it together, or get mlmd's impression" | Picks one | Together: a walk through dozens of entries before the first Vision question. Impression: a paragraph to take on trust |
| 2b | Idea-first start | No pass; the files stay empty | Nothing | Nothing |
| 3 | Vision interview, each topic | Terms join the glossary as they come up, unseen. A term used two ways stops the interview to settle it. Constraints become rules. At topic end: the foundation check, four lines and mlmd's call, "go deeper or move on" (BR-40) | Answers "move on", mostly | The first check reassures. On idea-first, "footguns: empty" each time. By the 4th check it repeats |
| 4 | Vision, standing-documents topic | Each document's summary | Reads, confirms | The first real look, after the interview that filled them |
| 5 | Vision review | Findings; "a belief nobody chose" becomes an assumption | Settles findings | Assumptions arrive as review findings, not as the operator's beliefs |
| 6 | behaviors.md | "Read every rule" | Reads, or skims | Reading load (flow.md F4) |
| 7 | architecture.md | Boundary rules, glossary entries for the parts, assumptions with their fragility | Reads | Assumptions start to matter: high fragility means a PoC |
| 8 | PoCs, spikes | Footguns appear | Reads verdicts | The traps become concrete |
| 9 | The plan and the gates | Each gate seen to fail on a real case | Little | Nothing marks the moment a rule stops being a promise and becomes a check |
| 10 | Build items | Merge questions. The standing documents change in each item's commit, unseen. A foundation check after every item, since a work item is a topic | "Move on", per item | Noise, or invisible. Either way no sense that the foundation is growing |

## Experience problems

- **S1 Built behind the operator's back.** Terms, rules and assumptions are added as they come up,
  without the operator seeing them (3, 10). The first full view comes at the Vision's last topic
  (4). In mtg-judge they were imported silently and never shown.
- **S2 A verdict without the evidence.** The foundation check gives counts and a judgement
  ("23 terms, solid"), not the entries. The operator can't tell sound from said to be sound
  without opening four files. Confidence came from mlmd's judgement (D-22), but there is nothing
  under it to glance at.
- **S3 The same check, every topic.** After every topic of every document, every review and every
  work item. Unchanged, it is noise, and "move on" becomes a reflex: the rubber stamp of flow.md F4
  and F5. On an idea-first start footguns are empty through phase 1 by design, and the check says
  so each time.
- **S4 "Go deeper" has no shape.** The operator can't tell what it costs (one question, a round,
  a session of its own) or what they would do in it. Going deeper on an empty footguns file in
  phase 1 means nothing.
- **S5 The foundation pass is all or nothing.** Walk every drafted entry together, or take a
  paragraph on trust. Neither puts the contradictions first: mtg-judge's R1 ("the PRD is the
  master copy") contradicted mlmd's own rule that its documents are the only master.
- **S6 Rules don't show whether they hold.** Tier (gated, reviewed, registered) is the agent's
  word; the operator's question is "will this be checked, or is it a promise?". In phase 1 no gate
  exists yet, and the moment a rule becomes a real check (9) passes unmarked. A rule the agent adds
  during the build binds every session from then on, without the operator having seen it.
- **S7 The operator's own domain gets no special place.** The glossary (their business terms) and
  the rules from their constraints are where they know more than the agent. Footguns and most
  assumptions are the agent's to find. The check treats all four alike.
- **S8 Nobody says why these documents matter.** They are imported into every session through
  CLAUDE.md: that is what keeps the work sound, and the operator is never told.

## Decisions

- **Scope** (Frank, 2026-10-10): the glossary, rules and footguns, and assumptions.md. CLAUDE.md
  only as what every session loads (S8); its trust boxes and Claims rule stay out.
- **First part: the foundation check** (Frank, 2026-10-10): S2, S3 and S4.
- **S2, what the check shows** (Frank, 2026-10-10): each entry that is new or changed since the
  last check, one line each, then the totals and mlmd's call. When an entry warrants a deeper
  discussion or explanation, mlmd is not held to one line: it takes the time to explain the issue
  fully and works it out with the operator.
- **S3, when it shows** (Frank, 2026-10-10): the full check only when an entry changed or mlmd's
  call is that something needs attention; otherwise one line ("Foundation: unchanged, still
  sound") and the work continues without a stop. mlmd safeguards the product's integrity: a check
  doesn't only list the changes, it checks whether each one changes anything else meaningful in
  the product the operator is working on (another entry, a behavior, a feature, a decision), and
  says so. Changes BR-40: proposed, waiting for Frank's yes.
- **S4, going deeper** (Frank, 2026-10-10): each go-deeper option names its subject and size ("2
  questions on A4, here"). It is offered only where there is something to deepen; an empty
  footguns file in phase 1 reads "expected until the PoCs", with no option. The operator can
  answer "1, but in a new session": mlmd then commits, offers the deep dive as a card, and brings
  the operator back to where they left (as any deep dive, BR-38).

## For Alex

Mechanics gaps noticed while mapping. Not Frank's questions.

1. BR-18 lets the standing documents change freely and lists glossary, footguns, assumptions,
   open questions and unhandled issues, but not rules.md. Does a new rule need the decider? The
   behaviors step has the operator "read every rule", which suggests it does.
2. process.md's Sessions counts a work item and a review as topics, so BR-40 runs a foundation
   check after every item and every review. Intended?
3. The glossary's Retired terms are "barred from new prose, identifiers and tests". Nothing
   enforces that. A gated rule?
4. A phase 1 rule has no gate to name, since the gates are built in phase 2. Which tier does it
   carry until then, and what moves it?
5. A footgun's Verified date: nothing says who re-checks it, or when.
