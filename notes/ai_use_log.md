# AI use log

A factual record of what Claude (Claude Code) did on this project, for the
README's **How I Used AI** section. The two "moments" and what you changed are
yours to write. This is the raw material.

## 2026-10-18 (evening, while I was asleep for most of it)

| What Claude did | Where it landed |
|---|---|
| Cloned the repo, made `.venv`, installed `requirements.txt` + `ipykernel`, ran `test.py` (9 pass). Device: **mps** | local only |
| Compared r/labrats with r/neuroscience by pulling each one's public RSS feed. Recommended r/labrats. | (conversation) |
| Pulled a 40-post reading sample from r/labrats (28 newest, 12 top-of-month, taken at even intervals through each list) | `notes/m1_reading_labrats.md` |
| Ran notebook sections 1–5 on the bundled **practice** data in a scratch folder outside the repo, to confirm training works on this machine (~10 s on mps, peak ~3.8 GB). No results from that run are in the repo. | not committed |
| Downloaded the baseline model (`facebook/bart-large-mnli`, ~1.6 GB) so unit 6 doesn't wait on it | `~/.cache/huggingface` |
| Measured post lengths vs. the 128-token window | `notes/m2_labels_worksheet.md` |
| **Collected all posts** for `labels.csv` from r/labrats' public "new" RSS feed, filtered to text posts (rules in README → The Dataset), 240 posts from Oct 6–19 + 63 reserve, and picked the 20 cold rows at random (seed 2026) | `labels.csv`, `notes/collection_sources.csv`, `notes/reserve_posts.csv` |
| Wrote the worksheets for Milestones 1–4. No labels, definitions or criteria in them. | `notes/` |
| Drafted the factual README lines (collection method, base model, default settings, device) | `README.md` |

**Not done by Claude (by design):** choosing the distinctions and labels,
writing the definitions and decision rule, writing the five criteria, labelling
the 20 cold rows.

## Still to come (add as it happens)

- [ ] Stress test of my label definitions (8 boundary posts). What changed?
- [ ] Strongest case for each side of my borderline post. What did I decide?
- [ ] Too-easy/too-hard review of my criteria. Did I change any wording?
- [ ] Pre-labelling of the non-cold rows. How many did I correct?
- [ ] Notebook run on my data
