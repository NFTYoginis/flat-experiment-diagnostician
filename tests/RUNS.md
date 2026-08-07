# Run record

3 blind runs, 3 cases. Case 01 **22/22**. Case 02 the run was right and the key was wrong on the integrity grade; corrected. Case 03 is the family's first **Supported** null, reached by clearing five stages rather than by exhausting alternatives.

## How these were run

Each case was run in a **fresh session that had never seen this folder**, by someone who did not
build it. The session was given the folder and the case inputs and explicitly forbidden from
opening any `expected.md`. It returned a diagnosis. Only afterwards was that diagnosis scored,
line by line, against the assertions written before the run.

That order is the point. An answer key written from the spec tests whether the folder is
self-consistent. A key scored against a blind run tests whether it is *right*, and the way you
find out it wasn't is that a run disagrees and turns out to have the better argument.

## What the runs actually changed

Across the family, **fifteen blind runs found zero defects in any `rules.md`.** Every defect was
in an answer key, a fixture, or a worked example — the layer that *demonstrates* the rules rather
than the layer that states them. An example teaches by demonstration and a rule constrains by
assertion, and demonstration wins, so an example that breaks its own rule is worse than no
example at all.

Six keys were proven wrong by runs and corrected. In each case the run followed the folder's
stated contract and the key wanted a more decisive grade than the contract allows.

## What is still open

The corrected keys have been fixed but **not re-run**. A key repaired after a run now agrees with
that run by construction, which is the same circularity as a key written from spec. Closing it
means running those cases again, blind, against the corrected key.

`tests/` is deliberately excluded from what you upload into a Claude Project. See the README.
