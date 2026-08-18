# Reading a prior-test table

A case file carries two kinds of table, and both use the word **Assigned**. This file states
what the word means in each, because until now the folder used it in both places and defined
it in neither.

---

## The two shapes

**The subject's readout** breaks one test out by arm. The arms are the columns:

```
|          | Control | Variant |
| Assigned | 620,000 | 620,000 |
```

**The prior-test table** lists tests. Each row is one test:

```
| Test                       | Assigned | Exposure | Outcome              |
| Guest checkout prominence  | 890,000  | 97%      | detected, +0.31pp    |
```

---

## The convention

**`Assigned` means the number of users assigned to the unit that row or column represents.**

In the subject's readout the unit is an arm, so the figure is per-arm and the test's total is
the sum across arms. In the prior-test table the unit is a whole test, so the figure is that
test's **total across both arms**.

A prior test listed at 890,000 assigned ran 890,000 users in total, roughly 445,000 per arm at
an even split. A subject listed at 620,000 per arm ran 1,240,000 in total.

**To compare a subject against the prior-test table, sum the subject's arms first.** The
comparison is test-total against test-total.

---

## Why this reading and not the other

The competing reading is that `Assigned` is per-arm everywhere, on the grounds that one word in
one document should mean one thing.

It does mean one thing. It means "users assigned to this unit," and the unit is set by the
table, not by the word. That is not an inconsistency; it is how every column label in both
tables already behaves.

Four things in the fixtures point the same way:

1. **The row's unit is the test.** In the prior table each row *is* a test, and its other
   columns are properties of that test — its exposure rate, the position of the element it
   changed, what it found. A sample size in that row is the same kind of thing: a property of
   the test. The per-arm reading would make `Assigned` the only column in the row scoped to
   something the row is not about.

2. **The document splits by arm exactly when it means arms, and never otherwise.** Every
   per-arm figure in every case file appears in a table whose columns are `Control` and
   `Variant`. The prior table has no arm columns and no arm-scoped qualifier — no "per arm",
   no "per cell", no split. Under the per-arm reading the label would be silently carrying a
   qualifier the fixture never writes, in the one table where the fixture had the room and the
   occasion to write it.

3. **Prose about a single prior run uses the same shape.** Case 03 describes an earlier
   attempt "at 180,000 assigned" — one figure, one test, no arms mentioned. That is a test
   total in ordinary English, and it is the prior-table row rendered as a sentence.

4. **It is what the artifact is imitating.** These tables imitate an experiment-platform
   summary of completed tests. A one-line-per-test summary reports the test's sample size;
   per-arm counts appear in the detailed readout of the test being examined. The fixture
   reproduces that split exactly — detail for the subject, summary for the history.

---

## What follows for the method

`reference/baselines.md` establishes the detection floor from the prior tests that resolved an
effect, and compares the subject's achieved sample against it. That comparison is only
meaningful between commensurable figures, so it runs on test totals on both sides:

- Take each resolving prior test's `Assigned` figure as its total.
- Sum the subject's arms to get the subject's total.
- Place the subject against the floor those points describe.

Reading one side per-arm and the other as a total is the error this file exists to prevent. It
moves the subject by a factor of two against the set, in whichever direction the mistake is
made, and nothing downstream would show it.

---

## The residual

**The fixtures are the only evidence here.** This convention is a decision about what a
constructed case file means, and constructed case files are the only place the word appears.
It is not a claim about what any particular experiment platform prints, and a real readout that
labels a summary column `Assigned` should be asked rather than assumed.

**Every case file now states its convention inline**, so no future reader has to reconstruct
this argument. That is the actual fix; this file is the reasoning behind it. If a case file is
ever added without the inline statement, the ambiguity is back and this file will not prevent
it.
