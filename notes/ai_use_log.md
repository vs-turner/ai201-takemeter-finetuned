# AI use log

Raw notes for the README's **How I Used AI** section.

## 2026-10-18 (evening)

I used Claude to:

| What I had Claude do | Where it landed |
|---|---|
| Set up `.venv`, install deps, run `test.py` (9 pass). Device: **mps** | local only |
| Compare r/labrats with r/neuroscience via public RSS; I picked r/labrats | (conversation) |
| Pull a 40-post reading sample | `notes/m1_reading_labrats.md` |
| Dry-run notebook sections 1–5 on practice data outside the repo (~10 s on mps) | not committed |
| Download the baseline model (`facebook/bart-large-mnli`) | `~/.cache/huggingface` |
| Measure post lengths vs. the 128-token window | `notes/m2_labels_worksheet.md` |
| Collect text posts into `labels.csv` (240 from Oct 6–19 + 63 reserve), pick 20 cold rows (seed 2026) | `labels.csv`, `notes/collection_sources.csv`, `notes/reserve_posts.csv` |
| Draft worksheets for Milestones 1–4 (no labels/definitions/criteria yet) | `notes/` |
| Draft factual README lines (collection, base model, defaults, device) | `README.md` |

I chose the labels myself later: definitions, decision rule, five criteria, and
the 20 cold rows.

## 2026-10-19 — pre-label and my review

I had Claude pre-label the 220 non-cold rows as `career` / `culture` /
`technical` before I had written definitions. Its working reading is in
`notes/prelabel_notes.md`. Before my review: technical 121, culture 51,
career 48. Cold rows untouched. 56 rows flagged as borderline.

Then I:

1. Labelled the 20 cold rows first, without reading pre-labels or
   `prelabel_notes.md`. Left `note = cold`.
2. Wrote my definitions and career-vs-culture decision rule.
3. Read every pre-labelled row (not only the flagged ones): 211 kept as
   `pre-labelled (already filled), checked`, 6 as
   `pre-labelled (checked by you), corrected from <old>`, 3 deleted
   (coursework lab-report help; whale joke; outsider optogenetics question).
4. Put definitions in the README and `label_definitions.txt`, wrote five
   criteria in `criteria.md`, set
   `LABELS = ["career", "culture", "technical"]` in notebook section 2.
5. Committed before training so I can run the notebook on this data.

**Corrections I made (6):**

| Was | Became | Why (short) |
|---|---|---|
| culture | technical | workplace task accommodations → practical how-to |
| technical | career | "worth publishing" systematic review → path/publishing call |
| career | culture | undergrad-volunteer red flags from mentor POV → lab norms |
| technical | culture | "can't be normal lab practice?" → norms, not a protocol |
| culture | career | how to graduate despite PI → path/outcome |
| culture | career | are Research Technicians expected to lead projects? → role |

**Final counts (237 rows):** technical 126 (53.2%), career 57 (24.1%),
culture 54 (22.8%).
