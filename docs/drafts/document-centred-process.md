# Draft: the process, centred on documents

Status: draft by Claude, 2026-10-08, for Frank and Alex. Follows D-26 and D-27. Not yet in
process.md.

## The rule

The next step is always to work on the next document of the current phase. A phase can have
several documents in progress at once. A document is worked on and iterated; it is not "finished"
once. Work that produces no document of its own (code, an attack, mutation testing, a reading
pass, a release) is defined by a document and records its result in one (D-27).

Each document has:
- **For:** its purpose line, canonical. mlmd introduces it with this line, word for word.
- **Needs:** the documents it is written from.
- **Ready when:** what makes it good enough for the documents that need it. A standing document
  is never ready; it is **current** or not.
- **Work it governs:** work without a document of its own, and where that work's result goes.

## Phase 1: Requirements

| Document | For | Needs | Ready when |
| --- | --- | --- | --- |
| `docs/vision.md` | Decide the whole product: what it is and which MVP each feature belongs to | the operator; existing code and specs, if any | no large question open; each feature has an MVP or "none yet"; fresh-session review settled |
| `docs/prd/mvp-N.md` | Decide one MVP: what it's for and what result would be a no | vision.md | its behaviors are listed by id; review settled |
| `docs/behaviors.md` | State what the product does: contract, failure, edges | the PRD | every behavior the MVP adds has contract, Failure and Edges |
| `features/*.feature` | Make each behavior testable | behaviors.md | every new behavior has a scenario, tagged with its id |
| `docs/open-questions.md` | Settle the questions that change behavior but not structure | all of the above | no medium question blocks phase 2 (it may also run beside phase 2) |

Governs: the cheapest prototypes (`docs/prototypes/<name>.md`, a deep dive from vision.md).

## Phase 2: Functional design and plan

| Document | For | Needs | Ready when |
| --- | --- | --- | --- |
| `docs/architecture.md` | Divide the product into logical components and walk every behavior through them | behaviors.md | every behavior walked; review (kind hld) settled |
| `docs/stack.md` | Pick what supports the requirements and the architecture | architecture.md | every choice is a decision with who made it |
| `docs/spikes/<name>.md` | Prove a risky assumption before anything is built on it | assumptions.md (high fragility) | a verdict, written back into assumptions.md |
| `.pdd/plan.json` | Plan the build: cycles of work items, with their dependencies | all of the above | every behavior delivered by an item; structure check passes |

## Phases 3 and 4: one cycle per logical component (or group) the plan builds next

| Document | Phase | For | Needs | Ready when |
| --- | --- | --- | --- | --- |
| `docs/hld/<cycle>.md` | 3 | Design a cycle that changes the architecture | architecture.md, the plan | design review settled |
| `docs/lld/W<id>-<name>.md` | 3 | Design one work item | the HLD if any, behaviors | the cycle's LLDs reviewed together |
| `docs/cycles/<cycle>.md` | 4 | Build, attack and merge each item, then check the cycle: mutation testing and a reading pass | the cycle's LLDs | every item merged; attack findings, survivors and reading-pass findings settled or turned into plan items |
| `docs/prd/mvp-N.md` (verdict) | 4 | Judge the MVP and release it | the plan's final item | verdict written; release tagged on the operator's word |

## Standing documents (every phase; current, never ready)

`CLAUDE.md`, `docs/rules.md`, `docs/glossary.md`, `docs/footguns.md`, `docs/assumptions.md`,
`docs/open-questions.md`, `UNHANDLED_ISSUES.md`, `.pdd/constitution.md`. The foundation check
(BR-40) reports whether they are current.

## After an MVP

A later iteration may go any way a usual feature-changing iteration goes (D-27): it can reopen
the Vision, a PRD, behaviors or the architecture, through a change request (BR-18), and continues
from the earliest document it changes.

## Settled (Frank and Alex, 2026-10-08, D-28)

1. A session never crosses a document: each document's work starts in a new session, and a
   document may take several sessions (BR-38).
2. The attack, mutation testing and reading pass of a cycle write to a cycle report,
   `docs/cycles/<cycle>.md`.
3. `docs/stack.md` stays in phase 2.
