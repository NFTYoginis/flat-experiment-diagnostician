# Re-run 002 — expected

**Committed before the run.** Case: [`tests/case-02-metric-mismatch`](../../tests/case-02-metric-mismatch/inputs.md).

## Why this case is being re-run

**This is the repair-circularity test.** Its key was repaired in `fff5bb9` after a blind run
disagreed with it and won: the key had demanded `Usable` and `Supported`, the run returned
`Usable with limitations` and `Provisional`, and the run was right — `same surface, same metric
family` is the first integrity axis in `rules.md`, and a set reading entirely on
return-completion cannot baseline what sample this surface needs to resolve a repeat-purchase
effect.

A key repaired after a run agrees with that run by construction. In the sibling
`review-rejection-diagnostician` the same re-run found a fresh contradiction the first repair had
left behind, so the circularity here is assumed live rather than cosmetic.

## Must assert

- [ ] **Primary constraint is Stage 4, inference** — the metric answers a different question than
      the change can move, not a power problem.
- [ ] **The re-run at 4× sample is read as the discriminating evidence**: the interval narrowed
      from [−0.72, +0.88] to [−0.32, +0.48] and the point estimate did not move. More sample
      bought precision on the same near-zero effect.
- [ ] The metric departure is identified: six priors on return-completion, subject on 30-day
      repeat purchase.
- [ ] **Comparison-set integrity is `Usable with limitations`**, and the report does **not**
      resolve the tension by declaring the departure costless. It feeds the Stage 4 finding *and*
      closes the Stage 3 floor, and both are true at once.
- [ ] **Confidence is `Provisional`**, with the cap treated as binding rather than advisory.
- [ ] Directional bias stated on the missing run-2 ratio check.
- [ ] No recommended action, no re-run advice, no metric substitution proposed.

## Stated in advance

**The specific thing this re-run is looking for is a contradiction the first repair left behind.**
The repair changed the integrity grade and the confidence grade. It did not revisit whether every
other assertion in the key still follows from `Usable with limitations` — in particular whether
anything downstream of the Stage 3 floor was written assuming a clean set.

**Prediction:** the run returns Stage 4, `Usable with limitations`, `Provisional`, matching the
repaired key. What I am genuinely unsure about is whether it treats the closed Stage 3 floor as
having consequences the key does not currently list. If it does, the key is still incomplete and
this re-run has done its job.
