# Pre-labelling notes (Claude, 2026-10-19)

Rows 21–240 of `labels.csv` (the 220 non-cold rows) were pre-labelled by Claude
as `career`, `culture` or `technical`. **Rows 1–20 (`cold`) are untouched.**
Label those yourself, without looking at these notes, before you review anything.

## How Claude read your three label names

You gave the label names but no definitions yet, so this is the reading Claude
used to stay consistent. **These are not your definitions.** Write your own in
the README, then check every pre-label against *yours*.

- `technical`: about doing the work itself, such as methods, protocols,
  reagents, equipment, buying or storing supplies, data analysis,
  troubleshooting results, safety procedures and PPE.
- `career`: about someone's professional path, such as jobs, applications,
  interviews, CVs, salary, PhD/postdoc/master's decisions, leaving science,
  conferences, publishing timelines.
- `culture`: about people and life in the lab, such as PIs and colleagues,
  mentoring, workplace conflict, authorship disputes, lab norms, emotions and
  venting, humour, science news.

Rules Claude applied on the hardest calls (yours to keep or overturn):
- **career vs culture:** if the post is asking for a *decision about the
  writer's path* (leave, apply, switch, quit), it's `career`, even when a bad PI
  is the reason. If it's about *how people in the lab behave* or what's normal
  there, it's `culture`.
- **culture vs technical:** if the post asks a *practical question about the
  work*, it's `technical`, even when it's emotional. If it's mainly venting or
  sharing a feeling about the work, it's `culture`.

## Counts (pre-labelled rows only)

| Label | Count | Share |
|---|---|---|
| technical | 121 | 55.0% |
| culture | 51 | 23.2% |
| career | 48 | 21.8% |
| **Total** | 220 | |

No label is over 70%. With all 240 rows, the test split is about 36 posts, so
`career` and `culture` get roughly 8 each, right at the notebook's "thin" line.
`notes/reserve_posts.csv` has 63 more posts if you want to top up either one.

## The 56 flagged rows: start here

Their note reads `pre-labelled (Claude); unsure: ...` or `...; keep or delete?`.

| Flag | Rows |
|---|---|
| unsure: career vs culture | 26 |
| unsure: culture vs technical | 22 |
| unsure: career vs technical | 3 |
| fits no label well / off-topic, keep or delete? | 5 |

These are where your decision rule matters, and where your **three hard cases**
for the README will come from. Examples: "Burning bridges with PI to switch
jobs", "This can't be normal lab practice right??", "How would you handle your
advisor reaching out to work on a paper 5 years after graduating?".

## Reviewing

Every row needs your eyes, not only the flagged ones. Change the note as you go:
- agree → `pre-labelled (Claude), checked`
- changed → `pre-labelled (Claude), corrected from <old>`
- doesn't belong → delete the row (and mention it in the README)

Filter for the flags in VS Code: search `labels.csv` for `unsure` or `delete?`.
