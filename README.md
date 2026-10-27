# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

TakeMeter classifies text posts from **r/labrats**, a public subreddit for
people who work in research labs: undergrads, techs, grad students, postdocs
and PIs. It sorts each post into one of three labels — `career`, `culture`, or
`technical` — by what the writer is mainly asking about: their professional
path, people and life in the lab, or how to do the work itself. The model is a
fine-tuned DistilBERT trained on 200+ posts labelled for this project.



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `career`

**Definition:** The post's main ask is the writer's professional path — jobs,
applications, interviews, finding a lab or PI, leaving or finishing a program,
role expectations, or publishing as a career milestone.

**Example 1:**
> "Potential PI ghosted me twice" — postdoc interview no-shows; asking what to
> do next with the application.

**Example 2:**
> "Struggling to find an entry level lab assistant job *rant*" — job search and
> what employers actually want.

### `culture`

**Definition:** The post's main ask is people and life in the lab —
interpersonal dynamics, mentoring, workplace norms, emotions and venting,
humour, or field norms, without a path decision.

**Example 1:**
> "Handling lab conflicts" — toxic mentorship from the PI's spouse; asking how
> to raise it.

**Example 2:**
> "AI images/summaries in so many conference talks/posters" — field norms about
> AI-generated slides, not a methods question.

### `technical`

**Definition:** The post's main ask is how to do the work — methods, protocols,
reagents, equipment, troubleshooting experiments, sourcing supplies or services,
or safety and PPE for doing the work.

**Example 1:**
> "BMDM differentiation" — optimizing a macrophage culture protocol and whether
> late-adhering cells are usable.

**Example 2:**
> "Protein migration mystery" — HA-tagged protein running high on a Western;
> asking what went wrong.

### The hardest boundary

**Which two labels:** `career` vs `culture`

**The decision rule I used every time:**
If the writer is asking what to do about their next job, degree, program, or
role step (leave, apply, switch, quit, master out, finish the doctorate, what
this job title is supposed to do), it is `career`, even when a bad PI or messy
lab is the reason. If they are asking how people behave, what is normal, or how
to handle a person — without that path decision — it is `culture`.

Secondary rule for `culture` vs `technical`: if the post asks a practical
how-to about the work (including physical task accommodations and lab
safety/PPE), it is `technical`, even when emotional. If it is mainly venting or
gauging whether a practice is "normal," it is `culture`.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** r/labrats, through the subreddit's public
"new" RSS feed, collected 2026-10-18. `labels.csv` started as every **text**
post in that feed from 2026-10-06 to 2026-10-19 (240 posts), with the title on
the first line and the whole body below it, untrimmed. Out of the 500 newest
posts the collection excluded image and link posts (189: a text model can't see
the picture or the article), posts with an image embedded in the text (5),
non-English posts (2) and the monthly mod thread (1). No usernames are stored.
Each row's link and date are in `notes/collection_sources.csv`. 63 older posts
(Oct 1–6) from the same feed are held unlabelled in `notes/reserve_posts.csv`
for topping up a thin label. Collection was scripted by Claude (see How I Used
AI). During review I deleted 3 rows that did not belong (coursework lab-report
help, an off-topic whale joke, and an outsider optogenetics "mind control"
question), leaving **237** labelled posts.

**How I labelled them:** I labelled the first 20 rows cold (`note = cold`)
from the definitions above, before reading any other labels or
`notes/prelabel_notes.md`. I had Claude pre-label the other 220 as
`career` / `culture` / `technical`, then I read every pre-labelled row (not
only the flagged ones): 211 kept as
`pre-labelled (already filled), checked`, 6 changed to
`pre-labelled (checked by you), corrected from <old>`, and 3 deleted. Details
in `notes/ai_use_log.md`.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| technical | 126 | 53.2% |
| career | 57 | 24.1% |
| culture | 54 | 22.8% |
| **Total** | 237 | 100% |

**Three hard cases**

**1.**
> *The post:* "Accommodations in the lab" — student with cerebral palsy asking
> for pipetting / plate-streaking adaptations (dycem, etc.).
>
> *Could have been:* `culture` (disability and belonging) or `technical` (how
> to perform the techniques).
>
> *I chose, because:* `technical` — the decision rule treats a practical how-to
> about doing the work as technical even when the reason is accessibility. Same
> call on the carpal/cubital-tunnel workplace-accommodation pre-label I
> corrected from culture.

**2.**
> *The post:* "This can't be normal lab practice right??" — lists glove-less
> cell culture, shared incubators with yeast contamination, etc.
>
> *Could have been:* `technical` (aseptic technique) or `culture` (is this
> normal / am I overreacting).
>
> *I chose, because:* `culture` — the ask is whether the lab's behaviour is
> normal and acceptable, not how to run sterile culture. Corrected from the
> pre-label `technical`.

**3.**
> *The post:* "How to make sure you graduate with a doctorate when your PI is
> stopping you at every step."
>
> *Could have been:* `culture` (toxic PI) or `career` (finish the degree).
>
> *I chose, because:* `career` — the decision rule: path/outcome question
> (graduate with the doctorate) wins even when the reason is interpersonal.
> Corrected from the pre-label `culture`.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, `MAX_LENGTH` 128 tokens, seed 42.

**Device:** `mps` (Apple Silicon GPU), torch 2.14.1, transformers 4.57.6,
Python 3.12.13. Different hardware does the arithmetic in a slightly different
order, so the same data and seed can give slightly different numbers elsewhere.

**Anything I changed from the defaults, and why:** Nothing. All defaults.
_(TODO: update if you change anything.)_ Worth knowing about the defaults on
this data: the model reads 128 tokens, about the first 96 words. The median
post is 122 words, and only 40% of posts fit completely, so the model mostly
sees the title and opening.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->
237 rows → train 165 · val 36 · test 36 (stratified, seed 42).

| Label | train | val | test |
|---|---|---|---|
| career | 39 | 9 | 9 |
| culture | 38 | 8 | 8 |
| technical | 88 | 19 | 19 |

Every label has at least 8 in the test split, but `culture` is exactly at 8
and `career` at 9, so one post moves either label's recall by 11–13 points.
The run took about 2.5 minutes on `mps` (137 s training).

### Stretch: second run with one hyperparameter changed

**What I'm adding (written before the run):** a second training run with
`EPOCHS` changed from **3 to 10**. Everything else stays the same: same
`labels.csv`, seed 42 (so the same 165/36/36 split), lr 2e-5, batch 16,
`MAX_LENGTH` 128, `mps`. **Why:** the default run predicted `technical` for
all 36 test posts. With 165 training posts and batch size 16, 3 epochs is only
about 33 weight updates, and training loss had only fallen to about 0.82. Ten
epochs gives about 110 updates. The question is whether more training gets the
model off the majority label, or whether something else (the labels, the
data size) is the limit.

The output goes to `results_stretch_epochs10.json`. `results.json` stays the
default 3-epoch run, which is what the unit 6 baseline compares against.

**What moved:** _(numbers drafted by Claude from the two results files. Put
the interpretation in your own words.)_

Same 36 test posts in both runs (checked: identical `test_split.csv`).

| Measure | 3 epochs (`results.json`) | 10 epochs (`results_stretch_epochs10.json`) |
|---|---|---|
| Overall accuracy | 0.528 | **0.806** |
| Macro F1 | 0.230 | **0.743** |
| F1: `career` (n=9) | 0.000 | 0.667 |
| F1: `culture` (n=8) | 0.000 | 0.588 |
| F1: `technical` (n=19) | 0.691 | 0.973 |
| Accuracy, most-confident third (12 posts) | 1.000 | 1.000 |
| Accuracy, least-confident third (12 posts) | 0.083 | 0.583 |

10-epoch confusion matrix:

| true \ predicted | career | culture | technical |
|---|---|---|---|
| **career** | 6 | 3 | 0 |
| **culture** | 3 | 5 | 0 |
| **technical** | 0 | 1 | 18 |

- **The model got off the majority label.** At 3 epochs it called every post
  `technical`. At 10 it uses all three labels.
- **6 of the 7 remaining errors are `career` ↔ `culture`**, 3 in each
  direction. That's the boundary my decision rule covers. `technical` is
  nearly solved: 18 of 19 right, and nothing else gets called `technical`.
- **Why more epochs helped this much:** the learning rate decays linearly to
  zero over the whole run. The 3-epoch run spent its last epoch at a tiny
  learning rate, after only ~22 updates at a useful one. In the 10-epoch run,
  validation accuracy at epoch 3 was already 0.75 (vs. 0.53 in the default run)
  and levelled off at 0.806 from epoch 8.
- **Caveats:** one seed and 36 test posts, so one post is 2.8 points. I chose
  10 epochs *after* seeing the default run's test result, which makes this
  number a little optimistic. The three-seed runs next unit are the real test.
  Training took 158 s on `mps`.

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Collect every text post from r/labrats' public "new" RSS
  feed into `labels.csv`, filter non-text/non-English junk, reserve older posts,
  and pick 20 cold rows at random.
- *What came back:* 240 posts (Oct 6–19), 63 reserve posts, cold rows with
  `note = cold`, plus worksheets and draft README facts — no labels or
  definitions yet.
- *What I changed:* Kept the collection. I wrote the taxonomy, criteria and
  labels myself afterward.

**Moment 2**

- *What I asked for:* Pre-label the 220 non-cold rows as `career` / `culture` /
  `technical`.
- *What came back:* 220 pre-labels (technical-heavy, 56 flagged), plus a working
  reading of the three names in `notes/prelabel_notes.md`.
- *What I changed:* I labelled the cold 20 first, then checked all 220
  pre-labels against my definitions — kept 211, corrected 6, deleted 3 — and
  wrote the README taxonomy, `label_definitions.txt`, `criteria.md`, and
  notebook `LABELS`.

**Pre-labelling disclosure:** I had Claude pre-label 220 non-cold rows before
my definitions existed. I read every one against the final taxonomy; I labelled
the cold rows first with no look at those pre-labels. See
`notes/ai_use_log.md`.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.472 | 0.528 | +0.056 |
| Macro F1 | 0.446 | 0.230 | -0.215 |
| F1 — `career` | 0.452 | 0.000 | -0.452 |
| F1 — `culture` | 0.333 | 0.000 | -0.333 |
| F1 — `technical` | 0.552 | 0.691 | +0.139 |

Both columns are the same 36 posts in `test_split.csv`. The baseline used the definitions in `label_definitions.txt`. A second run with bare label names only (not the baseline I am reporting) scored 0.556 accuracy and 0.531 macro F1, so the written definitions were worth about −0.08 accuracy on this zero-shot model.

**What I predicted before I looked:**
The trained model will beat the baseline on `technical` and on overall accuracy, because that label is the largest and the most lexically distinct (protocols, reagents, equipment), while `career` vs `culture` is the boundary I already find hard by hand.

**What the gap actually means:**
The prediction held for accuracy (+0.056) and for `technical` (+0.139 F1). It did not hold as a real gain: macro F1 fell by 0.215 because the trained model called every test post `technical`, so `career` and `culture` scored 0. Fine-tuning learned the majority class, not the taxonomy. The zero-shot baseline is the better model on the two smaller labels.



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

Device: `mps` (Apple Silicon GPU), torch 2.14.1. `RUN_LABEL` stayed `"before"`.
Accuracy spread across seeds is 0.000, under ten points, so this is not a
friendly draw. It is the same result three times.

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.528 | 0.528 | 0.528 | 0.000 |
| Macro F1 | 0.230 | 0.230 | 0.235 | 0.004 |
| F1 — `career` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `culture` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `technical` | 0.691 | 0.691 | 0.704 | 0.013 |

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.58 | 0.528 | 0.528 | 0.528 | MISSED |
| 2. Macro F1 | ≥ 0.55 | 0.230 | 0.230 | 0.235 | MISSED |
| 3. Every label's F1 | ≥ 0.40 | 0.00 / 0.00 / 0.69 | 0.00 / 0.00 / 0.69 | 0.00 / 0.00 / 0.70 | MISSED |
| 4. No label over 55% of the file | ≤ 55% | 53.2% technical | same file | same file | MET |
| 5. Most-confident third accuracy | ≥ 0.70 | 1.000 | 1.000 | 1.000 | MET |

Criterion 3 is career / culture / technical. Criterion 4 is a property of `labels.csv`, not of the split.

### Confusion matrix

Seed 42, the first of the three (section 7). Rows are true labels.

| true \ predicted | career | culture | technical |
|---|---|---|---|
| **career** | 0 | 0 | 9 |
| **culture** | 0 | 0 | 8 |
| **technical** | 0 | 0 | 19 |

**My biggest off-diagonal number, and what it means:**
9 real `career` posts were called `technical`, and 0 real `technical` posts were called `career`. The model only emits `technical`. Seeds 7 and 2024 do the same (seed 2024 moves one `culture` post to `career` and still gets none of them right).



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
