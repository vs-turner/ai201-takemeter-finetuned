# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall accuracy at least 0.58 on the held-out set

**Why this target:** With 237 labelled posts, `technical` is 53% of the file, so
a majority-class guesser already scores about 0.53 accuracy. 0.58 is a clear
step past that without pretending three overlapping lab-life labels are easy.
The target has to hold on seeds 42, 7 and 2024.

---

## 2. Macro F1 at least 0.55 on the held-out set

**Why this target:** Always guessing `technical` would look almost acceptable on
accuracy but crush macro F1 (roughly 0.23 with this balance). 0.55 forces the
model to be useful on `career` and `culture` too, not only the biggest class.
Again across all three seeds.

---

## 3. Every label has an F1 of at least 0.40 on the held-out set

**Why this target:** `culture` (54) and `career` (57) each put about 8–9 posts
in a ~36-post test split. One or two mistakes move that F1 a lot, so 0.40 is
strict enough to mean "learned something" without demanding 0.60 on eight
examples. Must hold for `career`, `culture` and `technical` on every seed.

---

## 4. No label is more than 55% of `labels.csv`

**Why this target:** The brief caps any label at 70%; I set 55% because
`technical` already sits at 53.2% after review. If I had to top up from
`reserve_posts.csv` and accidentally pushed one class higher, the file would
stop teaching the minority boundaries. Checkable from the counts table before
training.

---

## 5. Accuracy on the most-confident third of test predictions at least 0.70

**Why this target:** `results.json` reports
`confidence.accuracy_most_confident_third`. On ~36 test posts that third is
about 12 predictions. If high-softmax guesses are not clearly better than the
overall 0.58 bar, the model is guessing loudly on the career/culture boundary
I already find hard by hand. Must hold across all three seeds.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
