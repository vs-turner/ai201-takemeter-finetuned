# Milestone 4: labelling guide

`labels.csv` already holds the collected posts, with the `label` column empty.
Collection is done. Labelling is the part left.

## What's in `labels.csv` right now

- Every **text** post on r/labrats in a fixed date window (see
  `notes/collection_sources.csv` for each row's link and date), minus image
  posts, link posts, deleted posts and non-English posts. Title on the first
  line, body below it, whole post, nothing trimmed.
- The first **20 rows** have `note = cold`. They were picked at random
  (seed 2026), not by topic.
- `notes/reserve_posts.csv` holds older posts from the same feed, unlabelled,
  in case a label comes out thin and you need more of it.
- **No usernames** are stored anywhere.

## The order to do it in

### 1. Cold 20 first: you alone (~15 min)

No Claude, no tools, just your definitions from Milestone 2. Label the 20
`cold` rows **before** any pre-labelling exists in the file, so nothing can
nudge you. Leave `note` as `cold`. Next unit's agreement check leans on these.

Open `labels.csv` in VS Code (or Excel/Numbers, but **save as CSV**, and check
that it didn't add quotes or change line breaks).

### 2. Pre-labelling: hand it to Claude

Once your labels exist (README + `label_definitions.txt`), ask Claude to
pre-label every row that isn't `cold`. Each pre-labelled row gets
`note = pre-labelled (Claude)`.

### 3. Read and correct every pre-label (~40 min)

The brief: *"read and correct every single one. Skimming produces noisy data
and a model that learned your skimming."* As you go, change the note:

- you agreed → `pre-labelled (Claude), checked`
- you changed it → `pre-labelled (Claude), corrected from <old label>`
- the post doesn't belong (e.g. it's really about an image, it's spam) →
  **delete the row** and note it in the hard-cases log below

The note column doubles as your disclosure, and it's what the README's
"How I labelled them" line describes.

### 4. Count (~5 min)

Run notebook section 2's check cell, or ask Claude to count. Fix any label over
70%. If a label is thin, pull more of it from `reserve_posts.csv`.

---

## Hard-cases log (you need at least 3 for the README)

Any post that made you pause. Write it down **when it happens**. You won't
remember why later.

| Row / first words | Could be | I chose | Because |
|---|---|---|---|
| Accommodations in the lab (CP / pipetting) | culture / technical | technical | practical how-to about doing the work |
| This can't be normal lab practice right?? | technical / culture | culture | ask is "is this normal?", not how to culture |
| Graduate when PI stops you at every step | culture / career | career | path/outcome (finish doctorate) wins |
| Need help accommodating my work place (carpal) | culture / technical | technical | same rule as accommodations cold row |
| Are Research Technicians expected to lead projects? | culture / career | career | job-role expectations |

## Stop rule from the brief

Behind at the 90-minute mark? Stop and label what you have, down to 150. Say how
many and why in the README.
