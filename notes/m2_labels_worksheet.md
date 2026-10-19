# Milestone 2 worksheet: design your labels

The labels are yours. This sheet gives you the structure, facts about the data
that should shape your design, and the stress-test prompts ready to paste.

---

## Facts about r/labrats posts that matter for label design

Measured by Claude on the 240 posts in `labels.csv` (r/labrats, Oct 6–19, 2026):

- **The model reads only about the first 96 words** (title + opening). Training
  uses `MAX_LENGTH = 128` tokens, and the median post is 122 words (167
  tokens). Only **40%** of posts fit completely.
  → A label you can only decide from the *end* of a post (e.g. "the update at
  the bottom says it was resolved") is one the model can't see. Labels that show
  up in the title and opening are learnable.
- **Almost 4 in 10 r/labrats posts are image or link posts** (189 of the 500
  newest: photos, memes, screenshots, news links). They're excluded from the
  collection because a text model can't see the picture or the article.
- Posts range from 5 words to 1,300+. The title is included at the top of every
  post's text in `labels.csv`.

## Constraints from the brief

- 2–4 labels. A post should fit **exactly one** label almost every time.
- No label above **70%** of your data. For every label to have **≥ 8 posts in a
  30-post test split**, each needs roughly **27%+** of 200 rows (see
  `m3_criteria_worksheet.md`). That's easy with 2 labels and tight with 3–4.
- Stuck after 45 minutes? **Drop to two labels.** Write down what the third was
  and why it didn't hold. That counts as a finding.

---

## Your labels

Use the post numbers from `notes/m1_reading_labrats.md` for examples.

### Label 1: `________`
- **Definition (one sentence):**
- **Example A:** post # __
- **Example B:** post # __

### Label 2: `________`
- **Definition:**
- **Example A:** post # __
- **Example B:** post # __

### Label 3 (optional): `________`
- **Definition:**
- **Example A:** post # __
- **Example B:** post # __

### The hardest boundary
- **Which two labels:**
- **A real post that sits between them:** post # __
- **The decision rule I'll use every time:**
  (shape: "If the post ___, it's `x`, even if ___.")

---

## Stress tests (paste these into Claude once your definitions exist)

**1. Eight boundary posts** (from the brief):

> Here are my label definitions for r/labrats posts: [paste]. Write eight
> realistic r/labrats posts that sit right on the boundaries between these
> labels. Don't tell me which label each belongs to.

Then label all eight yourself. If any stops you for more than a few seconds,
tighten that definition **now**, before labelling 200.

**2. Your borderline post** (the "working on your own" prompt from the brief):

> Here are my label definitions and one post that sits between two of them.
> Make the strongest case for each of the two labels. Don't tell me which one is
> right and don't rewrite my definitions.

If both cases are strong, your boundary needs the decision rule above.

---

## When you're done

1. Copy definitions, examples and the decision rule into README → **Label Taxonomy**.
2. Put your definitions in `label_definitions.txt`, one per line as
   `label: definition`, deleting the three example lines. `baseline.py` uses this
   file as its entire prompt next unit.
3. Set `LABELS = [...]` in notebook section 2 to the exact same spellings.
4. Commit.
