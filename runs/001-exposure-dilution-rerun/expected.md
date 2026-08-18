# Re-run 001 — expected

**Committed before the run.** Case: [`tests/case-01-exposure-dilution`](../../tests/case-01-exposure-dilution/inputs.md).
Scored against that case's key, which is unchanged since the initial commit.

## Why this case is being re-run

Its key was never repaired, so it carries no repair circularity of its own. It is here as the
**control for the re-run series**: if a case whose key was never touched now disagrees with that
key, the cause is a change in the folder since the original run — the Step 1 routing fix in
`3f53436`, or the `Assigned` convention now stated inline — and not a repaired key agreeing with
itself.

## Must assert

- [ ] Assigned-to-exposed pass-through computed and stated: **21,840 of 240,000, or 9.1%**.
- [ ] **Primary constraint is Stage 2, exposure.** Not sensitivity, not the sample size.
- [ ] The 240,000 assigned sample is **not** described as adequate or well-powered without the
      exposure correction applied first.
- [ ] Detection floor read off the resolving priors (+0.33pp at 195,000 with 93% exposure;
      +0.41pp at 180,000 with 94%), on the **exposed** population rather than the assigned one.
- [ ] The interval on the assigned population is identified as measuring a diluted effect, and
      its width is **not** treated as a fact about the surface's sensitivity.
- [ ] Element position held constant against the comparison set.
- [ ] No recommended action, no sample-size or MDE computation.

## Stated in advance

The Step 1 routing defect fixed in `3f53436` terminated the sequence early in four of five
products in this family. This case's break is at Stage 2, downstream of that screen. **If the run
lands anywhere other than Stage 2, check the Step 1 fix before blaming the run** — this is the
case positioned to catch a regression in it.

The convention statement now inline in the case file is new since the original run. It should
change nothing here: case 01 reads the same under either reading of `Assigned`, which is why the
original disclosure named case 03 and not this one.
