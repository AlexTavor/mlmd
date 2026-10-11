# mlmd

A process for building software with a coding agent (Claude Code), and mlmd, the Claude Code plugin
that will run it. It is for developers who know how to build software and have not built much with
an agent: it leaves out the engineering they already know and covers what changes when an agent
writes the code. The readers are experienced developers, not juniors.

- `process.md`: the process. Four phases (requirements; functional design and plan; technical
  design; implementation), worked document by document, delivered in Product Increments.
- `docs/requirements.md`: the requirements, open questions and decision log (BR-, OQ-, D- ids).
- `docs/ux/`: how mlmd feels to the operator. `docs/pocs/`: mlmd's own PoCs.
- `docs/assumptions.md`, `docs/footguns.md`: mlmd's own standing documents.
- `ux-test/`: findings and learnings from running mlmd on a test project.
- `research/`: the evidence behind it, one dated file per piece of research.
- `UNHANDLED_ISSUES.md`: issues found outside the current task.

Background outside this repository. Read it before re-deriving any of it:
- `~/PersonalKB/workflow-codification-2026-09-26.md`: evidence from tiny, skyline and naval.
- `~/PersonalKB/guided-build-prior-art-2026-09-28.md`: prior art (GSD, BMAD, Superpowers, Spec Kit).
- `~/PersonalKB/drafts/greenfield-process.md`: the first draft (2026-09-26), superseded.
- `~/PersonalKB/drafts/superpowers-layer-plan.md`: the Superpowers plan, dropped 2026-09-29.

## Rules

- Alex and Frank own the process together (D-16). A change to `process.md` or the requirements
  that the owner in this session has not agreed to is raised as a proposal in chat, not made as an
  edit. A change only one of them agreed says so, and names what the other is to confirm.
- "Operator" means the person running mlmd on a project (BR-35), not the owners of this repository.
- A claim about how a project ran names its source: a file, a commit or a transcript.
- Research ends as a file in `research/`. The chat reply only points to it.
