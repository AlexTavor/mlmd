# What a design review costs (2026-10-04)

The evidence behind one design review per batch (process.md, phase 6, step 2). The operator, on
2026-10-04, while plague's level editor was being planned with 14 reviews running at once, one per
document: "We're being very wasteful with design reviews, reviewing really tiny LLDs." The
operator accepted the planning session's proposal the same day: one review per batch, its HLD and
LLDs read together from one file, and a review of its own for an item sized XL or rated high risk.

## Sources

- plague's review log, `docs/review-log.md` on plague's branch `review-costs`, commit `c3c001e`, not
  on main: every workflow review in plague from 2026-10-01 to 2026-10-04, written from the session
  transcripts by `tools/reviewlog.ts`, with each workflow's own minutes, tokens and calls.
- The session that planned plague's level editor, transcript
  `0f2c7b23-573c-483f-b5ef-391f917c45a8`: the results of its design-review workflows, which report
  tokens, minutes, the files the reviewer read and the findings raised.
- Each reviewed document's length is counted in words at the commit its review read: plague
  `61825d5` for batch E-1's, `8a01557` for W103's, W104's and W109's, and `1b4a1b5` for batch
  E-2's.

## Reviews of one document

The review log has 74 design reviews of one HLD or one LLD (55 LLDs, 19 HLDs), leaving out three
that stopped within a minute:

- tokens: 189k to 480k, median 324k;
- minutes: 6.3 to 40.3, median 23.75;
- findings raised: 0 to 8, median 5.

Its 8 reviews of whole designs (MVPs 1 to 4, the editor, the performance pass and the lively town)
cost 323k to 463k tokens, median 374.5k.

## The cost does not follow the document's length

The reviews of the editor's plan:

| Review | Words | Files read | Findings | Tokens | Minutes |
| --- | --- | --- | --- | --- | --- |
| Batch E-1's HLD | 2,204 | 55 | 5 | 384,875 | 28.2 |
| W100's LLD | 3,550 | 53 | 8 | 352,730 | 25.9 |
| W101's LLD | 1,858 | 55 | 7 | 364,150 | 24.0 |
| W102's LLD | 1,898 | 49 | 6 | 336,164 | 21.8 |
| W103's LLD | 2,609 | 51 | 8 | 344,256 | 23.6 |
| W104's LLD | 2,161 | 41 | 7 | 329,753 | 20.1 |
| W109's LLD | 1,703 | 47 | 5 | 391,760 | 30.4 |
| Batch E-2's HLD with the LLDs of W105 to W108, as one file | 6,369 | 62 | 6 | 396,182 | 22.0 |

The reviewer reads the code and the standing documents before the document it was given: 41 to 62
files in these reviews. W109's LLD, the shortest here, cost about as much as the five documents of
batch E-2 read together, at 3.7 times its length.

## What the batch review found

Six findings across its five documents, each settled in them the same day (plague
`docs/hld/E-2-the-editor.md`, "The design review"). The single reviews of the same plan raised 5 to
8 each. Not measured: what four reviews of W105 to W108 alone would have found that the batch
review did not.

## What was not reviewed

Batch E-3's HLD and W110's LLD had no batch review: the planning session told the operator so at
the time, and W109, rated high risk, had its own. plague's `UNHANDLED_ISSUES.md` records it.
