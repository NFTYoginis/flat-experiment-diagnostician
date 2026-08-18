# Run record

**4 blind runs, 3 cases.** Case 01 **22/22**. Case 02 the run was right and the key was wrong on
the integrity grade; corrected. Case 03 is the family's first **Supported** null, reached by
clearing five stages rather than by exhausting alternatives. A fourth run, a re-run, found the
`rules.md` routing defect described below.

## How these were run

Each case was run in a **fresh session that had never seen this folder**, by someone who did not
build it. The session was given the folder and the case inputs and explicitly forbidden from
opening any `expected.md`. It returned a diagnosis. Only afterwards was that diagnosis scored,
line by line, against the assertions written before the run.

That order is the point. An answer key written from the spec tests whether the folder is
self-consistent. A key scored against a blind run tests whether it is *right*, and the way you
find out it wasn't is that a run disagrees and turns out to have the better argument.

## What the runs actually changed

Most defects these runs found were in an answer key, a fixture, or a worked example — the layer
that *demonstrates* the rules rather than the layer that states them. An example teaches by
demonstration and a rule constrains by assertion, and demonstration wins, so an example that
breaks its own rule is worse than no example at all.

Six keys were proven wrong by runs and corrected. In each case the run followed the folder's
stated contract and the key wanted a more decisive grade than the contract allows.

**An earlier version of this file said the runs had found zero defects in any `rules.md`.** That
was true when it was written and false within a day, and it stayed on `main` after the commit that
falsified it. The fourth run found Step 1 routing an excludes-threshold interval straight to the
null — in the very case whose interval excludes the threshold and whose answer is a Stage 4 break.
The screen terminated before the rest of the method could correct it. Four of the five products in
this family were carrying the same defect.

That correction matters more than the clean number it replaced. A run that can only ever find
fault with the demonstration layer is not reaching the layer that governs. This one reached it.

## What is still open

**The `Assigned` convention — closed.** Subject tables give a figure per arm; prior-test tables
give one figure per test. Nothing said which was which, and under one reading every detection
floor doubled. It is ruled in [`reference/prior-test-tables.md`](../reference/prior-test-tables.md)
and **stated inline in every case file**, which is the part that closes it.

The rationale was committed before anything was recomputed, in its own commit, and argues from
the fixture rather than from what the ruling would cost — `git log` shows the order. The
recomputation that followed changed no figure: every computation in the folder had already been
summing the subject's arms and reading prior figures as totals. The defect was an undocumented
convention, not a wrong number.

**The corrected keys have been fixed but not re-run.** A key repaired after a run now agrees with
that run by construction, which is the same circularity as a key written from spec. Closing it
means running those cases again, blind, against the corrected key. In the sibling
`review-rejection-diagnostician` this was done, and the re-run found a fresh contradiction the
first repair had left behind — so the circularity here should be assumed live, not cosmetic.

`tests/` is deliberately excluded from what you upload into a Claude Project. See the README.
