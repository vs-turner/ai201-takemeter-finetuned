# Milestone 3 worksheet: facts to pick your numbers from

Drafted by Claude as reference. **There are no suggested targets here on purpose.**
The brief says all five criteria are yours: "A criterion you didn't write is one
you can't defend." What follows is arithmetic about *your* setup, so the numbers
you choose rest on something.

---

## 1. How big your test split will be

The notebook does a stratified 70/15/15 split (sklearn rounds the test share up).

| Rows in `labels.csv` | train | val | test |
|---|---|---|---|
| 200 | 140 | 30 | 30 |
| 220 | 154 | 33 | 33 |
| 250 | 174 | 38 | 38 |

**One test post is worth about 3.3 points of accuracy at 30 posts.** A target of
0.70 means at most 9 wrong out of 30, and 0.73 means at most 8. Those are
different targets.

## 2. How many of each label land in the test split

Per-label test count ≈ that label's share of your file × the test size.

| Label's share of the file | In a 30-post test split |
|---|---|
| 50% | 15 |
| 33% | 10 |
| 25% | 7–8 |
| 20% | 6 |
| 15% | 4–5 |

The notebook flags any label with **fewer than 8** in the test split. At 200
rows, a label needs about **27% of the file** to clear 8. With 6 test examples,
one mistake moves that label's recall by about 17 points.

What happened on the bundled practice set, run here as a dry run (60 fictional
running-forum posts, 3 test posts per label, seed 42, `mps`): accuracy 0.667, and
the per-label F1s were 1.000, 0.667 and **0.000**. That spread is what tiny test
splits do. It says nothing about how your model will do.

## 3. What a model that learned nothing would score

| Guesser | Accuracy | Macro F1 |
|---|---|---|
| Uniform random, 2 labels | 0.50 | ~0.50 |
| Uniform random, 3 labels | 0.33 | ~0.33 |
| Uniform random, 4 labels | 0.25 | ~0.25 |
| Always guesses the biggest label (share *p*, *k* labels) | *p* | (2p / (1+p)) / k |

Example: with 3 labels and the biggest at 50%, "always guess the big one" scores
**0.50 accuracy but only 0.22 macro F1**. That's why accuracy alone can flatter
a lopsided file.

## 4. What `results.json` actually measures, so your criteria are checkable

Section 5 of the notebook writes these, and only these:

- `accuracy`, `f1_macro`
- `per_label[label]`: `precision`, `recall`, `f1`, `support` (test count)
- `confusion`: counts of true vs. predicted
- `confidence`: `mean_when_right`, `mean_when_wrong`,
  `accuracy_most_confident_third`, `accuracy_least_confident_third`
  (each third is about 10 posts at a 30-post test split)

Next unit adds:
- three seeds (42, 7, 2024). **A target has to hold on all three** to count as MET
- `baseline.py`: a zero-shot model given only your label definitions
- `agreement.py`: your labels vs. staff labels on 30 posts, under *their* taxonomy

A criterion about something none of these measure (e.g. "agrees with my
labmate 80% of the time") has to say how you'll measure it.

## 5. The five areas from the brief (cover at least three)

| Area | The question | What would measure it |
|---|---|---|
| Overall accuracy | How often must it be right to be worth using? | `accuracy` / `f1_macro` |
| Per-label | Can one label be much worse than the others? | `per_label[...].f1` |
| Balance | How lopsided can your counts get? | your label counts (Milestone 4) |
| Consistency | If someone else labelled these, how often should you agree? | `agreement.py`, or re-labelling your 20 cold rows later |
| Confidence | Should confident guesses be right more often? | `confidence` block |

## 6. Your five (write these in `criteria.md`, not here)

For each one:
- [ ] It names a number
- [ ] Someone who can't ask you could check it from `results.json` or a stated method
- [ ] The "why" mentions *your* data or *your* labels, not a reason that fits any project
- [ ] You've thought about whether it holds on **all three** seeds

## 7. The review prompt from the brief, once your five are written

Paste your five into Claude and ask:

> "Here are five acceptance criteria with targets, for a text classifier I have
> not trained yet. For each target, give me one reason it might turn out too
> easy and one reason it might turn out too hard. Don't tell me which is more
> likely and don't suggest different numbers."

Commit `criteria.md` **before** you run the notebook on your own data. The commit
time is your evidence the targets came first.
