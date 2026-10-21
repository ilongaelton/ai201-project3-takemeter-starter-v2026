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

## 1. Overall accuracy

Overall accuracy on the held-out test split is **at least 0.60**, on every one
of the three seeds.

**Why this target:** always guessing `promo` already scores about 0.54, so
0.60 means the model learned something beyond the biggest label. I don't
expect much higher because my boundaries are fuzzy: `review` / `hype` and
`rec` / `hype` each needed an extra decision rule before I could label them
consistently myself, so some mistakes on those lines are expected.

---

## 2. The two big labels

`promo` and `rec` each reach an **F1 of at least 0.70** on the test split, on
every seed.

**Why this target:** they're my two biggest labels, with 118 and 54 examples,
so the model sees enough of them to learn them properly. That's why their bar
is higher than for the small labels.

---

## 3. The two small labels

`review` and `hype` each reach an **F1 of at least 0.50** on the test split, on
every seed.

**Why this target:** they have only 23 and 25 examples, about 4 each in the
test split, so one wrong prediction moves F1 a lot, and the target has to hold
on all three seeds. They're also my hardest boundary: I needed a decision rule
to separate them myself, so I expect the model to confuse them.

---

## 4. Agreement with the staff labels

When I label the 30 staff posts under the staff's definitions in unit 6, I
agree with the staff label on **at least 80% of them (24 of 30)**.

**Why this target:** these aren't my labels and the posts come from a running
forum I don't know, so I'm allowing up to 6 disagreements out of 30. Below
that, I'd take it as a sign I don't apply written rules consistently, which
would also put my own 220 labels in doubt.

---

## 5. Confidence means something

Of the test posts the model predicts with a confidence of **0.80 or higher**,
**at least 80%** are correct.

**Why this target:** if the model says it's 80% sure, it should be right about
80% of the time. If confident predictions are wrong more often than that, its
confidence score means nothing and I couldn't trust it to sort posts on its
own.

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
