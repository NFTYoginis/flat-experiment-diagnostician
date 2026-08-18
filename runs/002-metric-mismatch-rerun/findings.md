# Re-run 002 — findings

Scored against [`expected.md`](expected.md) and the key repaired in `fff5bb9`.

**Result: the repair was not circular. A fresh session reproduced its reasoning independently,
argument by argument.**

| Assertion | Result |
|---|---|
| Primary constraint is Stage 4, measurement | ✅ |
| Cause is affected population as a small share of the denominator (4B) | ✅ |
| Re-run at 4× read as the discriminating evidence; interval narrowed, estimate did not move | ✅ |
| Metric departure identified: six priors on return-completion, subject on 30-day repeat purchase | ✅ |
| **Comparison-set integrity `Usable with limitations`** | ✅ |
| **The tension is not resolved by declaring the departure costless** | ✅ |
| **Confidence `Provisional`, cap treated as binding** | ✅ |
| Directional bias stated on the missing run-2 ratio check | ✅ |
| No recommended action, no re-run advice, no metric substitution | ✅ |

## Why this closes the circularity rather than restating it

The key was repaired after a blind run disagreed with it: the key had demanded `Usable` and
`Supported`, the run returned `Usable with limitations` and `Provisional`, and the run won. A key
repaired that way agrees with that run by construction, so agreeing with it again proves nothing
unless the agreement is reached independently.

It was. This session saw the folder and the case and no key, and produced the repair's own
argument in its own words:

> The axis that fails is **metric family**, and it does two things at once. It is the departure
> the diagnosis rests on. […] It is also a real gap in what the set can baseline. A set reading
> entirely on return-completion rate cannot say what sample this surface needs to resolve a
> **repeat-purchase** effect. The floor quoted above is a floor for a different metric.

That is the repair commit's reasoning — *"It is both. It feeds the Stage 4 break and it closes
the Stage 3 floor, and the second does not stop being true because the first is useful"* —
reached from the contract rather than from the key.

It also went one clause further than the key does: *"the gap does not manufacture this finding —
it limits how much the set can corroborate it."* The key requires the departure to be treated as
both finding and defect; it does not say which direction the defect pushes. Naming the direction
is `rules.md`'s own which-way-does-the-gap-push discipline applied to the integrity verdict, and
the key should require it. Logged below.

## The contradiction the re-run found — in my key, not the folder's

[`expected.md`](expected.md) for this re-run asserts *"Primary constraint is **Stage 4,
inference**"*. Stage 4 is **measurement**; inference is Stage 5. The case key has it right and I
wrote mine from memory of the stage list without checking it against `rules.md`.

The run is right and my key was wrong, in the same week and the same repo where two other keys
were wrong in the same direction. That is now four across this family, and the pattern is not
that keys are hard — it is that a key written without re-reading the contract inherits whatever
the author half-remembers of it.

## Open, from this run

**FLAT-KEY-1** — `tests/case-02-metric-mismatch/expected.md` requires the metric departure to be
held as both finding and set defect, but does not require the report to state which direction the
defect pushes. The re-run stated it unprompted and `rules.md` asks for it everywhere else.
**Closes at:** `artifact` — the assertion is added to the case key.
