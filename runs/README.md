# Runs

Recorded runs. Not descriptions of runs.

| File | What it is |
|---|---|
| `expected.md` | The prediction for this run, **committed before the transcript existed**, in its own commit. `git log` shows the order. |
| `transcript.md` | Verbatim, both sides. Model, effort, timestamps and token usage in the metadata block. Nothing edited, retried, or selected from more than one attempt. |
| `findings.md` | The scoring, assertion by assertion, including where a run disagreed with a key and won. |
| `SHA256SUMS` | The folder files that produced it. `shasum -a 256 -c runs/NNN-*/SHA256SUMS` from the repo root. |

## The missing fourth file

The reference layout is transcript, findings, **verify-output**, answer key. There is no
`verify-output.txt` here **because this product has no verifier**. The nine-gate checker built
for `listing-stall-diagnostician` is not copied in, deliberately: extruding that shape across
the family is gated on listing-stall passing a cold gate run by a session that did not build it,
and a five-way sweep of an unproven shape is the architecture that produced the original defect.

So three of the four files are real and the fourth is absent with a reason. `SHA256SUMS` is here
in its place — it pins what produced the run, which is the property the receipt most needs. This
is a stated deviation, not an oversight.

**Open:** no executable gate on this product's output. Closes at `code`, after the cold gate.

## What is here

Three blind re-runs of the three cases, against the folder as it now ships.

| Run | Why it was re-run |
|---|---|
| [`001-exposure-dilution-rerun`](001-exposure-dilution-rerun/) | **Control.** Its key was never repaired. A disagreement here would mean the folder changed under the keys. |
| [`002-metric-mismatch-rerun`](002-metric-mismatch-rerun/) | **The repair-circularity test.** Its key was repaired in `fff5bb9` after a run beat it. |
| [`003-null-no-effect-rerun`](003-null-no-effect-rerun/) | The case the `Assigned` ambiguity could have flipped, now seeing a case file that states the convention inline. |

All three pass. 002 is the one that matters: a fresh session with no access to the key
reproduced the repair's own argument in its own words, which is what turns "the key agrees with
the run it was repaired from" into evidence rather than tautology.

It also caught a defect in the re-run key I wrote for it — I asserted "Stage 4, inference" where
Stage 4 is measurement. Fourth key in this family to be wrong, and the first where the error was
mine in the same session.

## How cold these are

Fresh API session per run. The system prompt was the uploaded folder — `identity.md`, `rules.md`,
`examples.md` and `reference/` — and nothing else. No `tests/`, no `runs/`, no key, no prior turn.

**The scoring is the builder's.** A run that disagreed with a key was adjudicated by the same
session that wrote it. That is weaker than an independent scorer.

[`../tests/RUNS.md`](../tests/RUNS.md) is the record of the earlier blind runs, which left no
transcripts.
