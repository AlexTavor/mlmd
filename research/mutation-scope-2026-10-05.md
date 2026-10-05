# What a mutation run over the whole codebase buys (2026-10-05)

The evidence behind mutation testing on what the batch changed, and a run over the whole codebase
only on the operator's word (process.md, phase 7). The operator, on 2026-10-05, of a run over every
mutant still going after plague's batch had been merged and deployed: "Why are we running a full
mutation test, anyway? Did we touch any of that stuff, that we're not waiting for it?" On the
session's answer, that it re-judged code no batch had changed: "Good, let's make this SOP and update
mlmd".

## Sources

- plague's mutation runner, `tools/mutants.ts`: hand-written mutants, each breaking one deciding
  line of a behavior, each judged against the whole suite in a scratch worktree of the commit, four
  at a time; the verdicts are caught, lives, broken (no longer compiles) and timed out.
- The runs' output, in the session's scratch folder, not committed, with each file's creation and
  last write as its start and end; the session's transcript, `841e21b0-3f00-43d3-9427-681f2e7ab15b`.
- plague's commits: `e9f91e3`, MVP 4 after batch 4-7, the tree of main's `caed1e7`; `e4c5c26`, after
  batch 4-8, the phone's notes, was merged.

## The runs

| Run | Commit | Mutants | Started | Ended | Took | Verdict |
| --- | --- | --- | --- | --- | --- | --- |
| Every mutant (W63's plan) | `e9f91e3` | 532 | 2026-10-04 20:26:53 | 22:37:43 | 2 h 11 min | all caught |
| Every mutant, again | `e4c5c26` | 604 | 2026-10-04 23:13:49 | 2026-10-05 01:27:07 | 2 h 13 min | all caught |
| Batch 4-8's own behaviors (`--only B97,B98,B99`) | `e4c5c26` | 66 | 01:27:23 | 01:42:02 | 14 min 39 s | all caught |

Other work ran beside each: review sessions, a dev server and test runs beside the first two; the
third read a load average of 23, 18 and 18 (one, five and fifteen minutes) once. The times are what
the runs took, not their floor.

## What changed between the two runs over everything

`git diff --name-only e9f91e3...e4c5c26` lists 61 files: batch 4-8's HUD, pause menu, coins and
goals, the tutorial's lessons and arrow, the sound's button and voices, their specs, and documents.
None is under `game/`, the simulation, which holds 287 of the 604 mutants. Of the 604 mutants, 135 sit
on a changed file or on a module whose own spec changed (`mutantsChanged` in `tools/mutants.ts`, on
plague's main since `99fe0ec`), across 19 behaviors; 66 are the batch's own behaviors.

So the second run judged 469 mutants on files, and modules with specs, no one had changed since the
first run caught them. It found nothing new: a mutant's verdict moves only when its code or the
tests that catch it move.

## What a run over everything could still catch

A mutant on an unchanged module caught only by a test in another module's spec, when the batch
changed that spec: the changed-files scope takes the mutants of `x.ts` when `x.spec.ts` changes,
not those of every module a changed spec reaches. Neither run over everything found one; how often
it happens was not measured.

## The rule taken

A batch's mutation run takes the mutants on the files it changed and on the modules whose tests it
changed, each against the whole suite (plague: `npm run mutants -- --changed main`, from the
batch's branch). A run over the whole codebase only on the operator's word.
