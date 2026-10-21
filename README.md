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

Posts come from r/Afrobeats, a public Reddit community for Nigerian and wider
African pop music. I started with posts about Nigeria's "Big 3" — Burna Boy,
Davido and Wizkid — but there were only 26 of them, so I widened it to the
whole subreddit. Most of what people write there is song recommendations,
quick reactions, and people promoting or selling things, with the occasional
full album review. The classifier sorts each post into `review`, `rec`, `hype`
or `promo`.

**Candidate distinctions (Milestone 1, from reading the comments):**

1. **Some posts say *why*, most just name a song.** "Burna boy Alone" or
   "Ease your mind by wizkid" versus "Burna Boy's 'Ye' for me … the horn riff
   alone could lift a whole mood."
2. **Some talk about the sound, others about the artist.** Posts about the
   music mention specific things you can hear (a horn riff, a choral effect,
   "minimalistic", "major Hip hop influences"). Posts about the artist praise
   the person or their status: "You are the best thing that has happened to
   Africa … after Bob Marley and FELA", "2021 will be big for Davido 🔥🔥".
3. **Some posts criticise, most only praise.** "My only criticism is that a lot
   of the songs end up sounding pretty similar … No Fit Vex" and "No shade to
   this song, but I think there are soooo many better songs" versus "Still my
   favorite WizKid song" and "So proud of Burna boy".


---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `review`

**Definition:** Says why a song or album works or doesn't, by pointing at
something you can hear in the music: a sound, an instrument, a production
choice, a genre influence.

**Example 1:**
> Burna Boy's "Ye" for me. Was going through a stressful move to a new city
> and that track's confidence just carried me through — the horn riff alone
> could lift a whole mood.

**Example 2:**
> Burna Boy has some major Hip hop influences eventhough his music is such a
> fusion of different musical influences, check out the following: …

### `rec`

**Definition:** Names a song, album or artist without giving any reason,
whether as a single title or as a list.

**Example 1:**
> An oldie but Tested Approved and Trusted by Burna Boy

**Example 2:**
> 1.wizkid-Made in lagos
> 2.burnaboy-african Giant
> 3.dayyo-nuts n caramel
> 4.ckay-boyfriend
> 5.omah lay - get layd

### `hype`

**Definition:** Talks about the artist rather than the music: praise for the
person, their status, their career or a prediction about them.

**Example 1:**
> So proud of Burna boy..You are the best thing that has happened to Africa
> and the black man musically after Bob Marley and FELA...
> Keep the Nigerian flag flying

**Example 2:**
> 2021 will be big for Davido 🔥🔥

### `promo`

**Definition:** Selling, buying or promoting something: concert tickets, the
poster's own music, mix or playlist, an event, or a request to collaborate.

**Example 1:**
> Can I please get it for $50 please

**Example 2:**
> Afrobeats One of a kind mix follow me there I have more

**Why `promo` was added after Milestone 2:** when I collected posts, more than
half of r/Afrobeats was ticket resale and self-promotion. Only about 80 of 256
posts fit the first three labels, so without `promo` I couldn't reach 150.
Those posts are a real part of what the community writes, so I labelled them
rather than throwing them away.

**The third Milestone 1 distinction, criticism vs. praise, did not become a
label.** Only 2 of the 26 comments I read about the Big 3 criticised anything,
so a `criticism` label would have had too few examples to learn from. A
critical post about the sound is a `review`.

### The hardest boundary

**Which two labels:** `review` and `hype`.

**The borderline post:**
> This is actually huge. Davido spent 15 years taking Afrobeats to the world,
> so doing a residency in Lagos to bring it back home feels right. Residencies
> are usually a Vegas thing so seeing one in Lagos is a statement. Respect.

It argues a case and gives reasons, like a `review`, but the reasons are about
Davido's career, not about anything in his music.

**The decision rule I used every time:**
If the reason is something you can hear in the music, it's `review`. If the
reason is about the artist's career, status or achievements, it's `hype`, even
when it's argued carefully. The post above is `hype`.

A second rule for the `rec` / `hype` line: a post that names a song with only
feeling and no reason ("Still my favorite WizKid song and my favorite Drake
feature! 😍") is `rec`. It's `hype` only if it's about the artist rather than a
song.

A third rule for the `rec` / `promo` line: a post that names a song is `rec`
if it's someone else's music, and `promo` if the poster is pushing their own
music, mix, playlist or event ("This is Justizziebwoi [link]", "Follow us and
share if you can").

A fourth rule for posts about the genre as a whole: if it describes how
Afrobeats sounds or where its sound comes from ("both have roots in West
African traditional music … influenced by dancehall"), it's `review`. If it
only argues about what the name covers, it fits no label and I left it out.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** public comments and posts from r/Afrobeats,
pulled through the Pullpush Reddit archive (Reddit blocks direct downloads).
I collected about 500 over two downloads and kept 220. I dropped posts in
other languages (Spanish, French, Portuguese, Arabic), off-topic posts, bot
posts, near-duplicate playlist links, chit-chat that fits no label ("Thank you
so much!"), posts about the subreddit itself ("we should cross-promote with
other subs"), arguments about what the word "Afrobeats" covers, and two posts
containing a personal phone number or email address.

**How I labelled them:** I labelled 20 posts cold first, with only my
definitions and no AI suggestions (marked `cold`). The other 200 were
pre-labelled by Claude using my definitions and decision rules. I then read
every one of them in batches of 25 and confirmed or corrected each label
(marked `pre-labelled, checked`). Before I checked them, Claude also re-read
its own pre-labels against my rules, changed two and removed one post that
fit no label.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `promo` | 118 | 53.6% |
| `rec` | 54 | 24.5% |
| `hype` | 25 | 11.4% |
| `review` | 23 | 10.5% |
| **Total** | 220 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "I see Gratitude topping this chart soon"
>
> *Could have been:* `hype` or `rec`
>
> *I chose, because:* `rec`. It's a prediction, which sounds like `hype`, but
> it's about a song (Asake's "Gratitude"), not the artist, and it gives no
> reason. My `rec` / `hype` rule puts it in `rec`.

**2.**
> *The post:* "Just watched this lool and was gonna maybe post it here 🤣🤣🤣.
> Love how honest and profound this song/performance feels. 🙌🙌🙌"
>
> *Could have been:* `review` or `rec`
>
> *I chose, because:* `review`. "Honest and profound" is weaker than naming a
> sound, but it describes how the song itself comes across when you hear it,
> not the artist and not just the title.

**3.**
> *The post:* "This is a sign of the times. Yaybo Baba. A true legend and
> powerfully important piece of the growth and progression of the culture of
> our music. Cups in the air to Our Uncle Fela. Here we played with his
> drummer friend and brother from another mother Tony Allen…"
>
> *Could have been:* `hype` or `promo`
>
> *I chose, because:* `hype`. Most of the post honours Fela as an artist. The
> end mentions the poster's own band, but it doesn't link anything or ask
> people to listen, so promotion isn't the main point.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`, fine-tuned on my laptop's CPU
(`device: cpu`, torch 2.14.1+cpu). The device is recorded in `results.json`.

**Settings:** the notebook defaults: 3 epochs, learning rate 2e-5, batch size
16, 128 tokens per post, seed 42.

**Anything I changed from the defaults, and why:** nothing. I ran the defaults
first so I have an honest starting point to diagnose in unit 6.

**Split sizes:** 154 train / 33 validation / 33 test.

| Label | Train | Val | Test |
|---|---|---|---|
| `promo` | 82 | 18 | 18 |
| `rec` | 38 | 8 | 8 |
| `hype` | 17 | 4 | 4 |
| `review` | 17 | 3 | 3 |

`review` has only 3 posts in the test split and `hype` only 4, both under
about 8, so their F1 will move a lot between seeds.

**What happened:** the run finished, but the model predicts `promo` for every
test post. Accuracy is 0.545, the same as always guessing `promo`, and `rec`,
`hype` and `review` all have F1 0. The training loss only fell from 1.31 to
1.12 over three epochs (30 steps), so with 154 posts and the default settings
the model never got past the biggest label. That's the first thing to
diagnose in unit 6.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I couldn't find posts to read, so I asked Claude
  (Claude Code) to read r/Afrobeats comments about Burna Boy, Davido and
  Wizkid and write my Milestone 1 distinctions.
- *What came back:* Reddit blocked direct downloads, so Claude pulled 26 real
  comments through the Pullpush archive. My first distinction ("some posts
  compare streaming or chart numbers") didn't appear in any of them.
- *What I changed:* I dropped the streaming-numbers distinction and used three
  that the real comments showed: says why vs. just names a song, about the
  sound vs. about the artist, and criticises vs. only praises.

**Moment 2**

- *What I asked for:* to widen the community to reach 200 posts.
- *What came back:* Claude collected about 500 posts and found only about 80
  fit my three labels. The rest were mostly ticket resale and
  self-promotion. It offered four options: add a fourth label, wait for
  r/Nigeria, switch to soccer, or collect by hand.
- *What I changed:* I chose to add a fourth label, `promo`, because those
  posts are a real part of what the community writes.

**Pre-labelling disclosure:** Claude pre-labelled 200 of my 220 posts using my
definitions and decision rules. I labelled the other 20 cold, on my own,
before seeing any of Claude's labels. Claude then showed me the 200
pre-labels in batches of 25 and I read and confirmed every one (`note` =
`pre-labelled, checked`).

**Other AI help to disclose:** Claude drafted the label definitions, decision
rules and hard-case write-ups from my distinctions and choices, and I
reviewed them. For `criteria.md` I chose every target number and picked each
reason from options Claude offered. Claude also ran the training notebook for
me.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



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
