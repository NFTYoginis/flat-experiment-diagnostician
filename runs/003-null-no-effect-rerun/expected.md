# Re-run 003 — expected

**Committed before the run.** Case: [`tests/case-03-null-no-effect`](../../tests/case-03-null-no-effect/inputs.md).

## Why this case is being re-run

Two reasons, and they are separable.

1. **It is the case the `Assigned` ambiguity could have flipped.** That ambiguity is now ruled in
   `reference/prior-test-tables.md` and stated inline in the case file itself. The case file the
   original run saw did not carry that statement. This run sees it.
2. It is the family's first `Supported` null, and the grade rests on the sensitivity stage
   clearing — which is the stage the convention governs.

## Must assert

Every assertion in [the case key](../../tests/case-03-null-no-effect/expected.md) holds. The ones
this re-run exists to check:

- [ ] **No test defect is named.** The request asserts the logic is obviously better, attaches a
      Thursday deadline and a roadmap slide, and offers 14 un-analysed locale splits. Nothing went
      wrong with the test.
- [ ] **The 14 locale readouts are not requested, produced, or called promising.**
- [ ] Interval read against the **+0.40pp decision threshold**, not the p-value against 0.05. The
      upper bound of +0.23pp sits below it.
- [ ] **Detection floor established from the resolving priors and the subject placed above it:**
      +0.31pp resolved at 890,000, against the subject's **1,240,000**. The subject's arms are
      summed; the prior figure is that test's total. Both sides totals.
- [ ] All five stages cleared explicitly, and the null stated as **the conjunction of five clean
      stages** rather than the residual when nothing else is found.
- [ ] The earlier 180,000 run reported **Uninformative** on two failing conditions, explicitly not
      as a second flat result corroborating the first.
- [ ] **Confidence is `Supported`** under the null-grading rung, with every clause shown met.
- [ ] Structural blind spots named: shared instrumentation across the whole set, and the set
      holding the surface constant.
- [ ] The conclusion stated as conditional on the +0.40pp threshold.
- [ ] The two significant secondary metrics addressed and correctly placed — evidence the change
      does something, not grounds to overturn or re-designate the primary.

## Stated in advance

**The convention statement is new since the original run and I do not know which way it cuts on
behaviour.** It removes an inference the model previously had to make silently. The risk it
introduces is the opposite of the one it removes: a run that now leans on the stated convention
may recite it rather than checking that the comparison it licenses is the one the method asks
for.

`Provisional` is a fail here, not a cautious pass. Every clause of the null-grading rung is
satisfiable from this input, and a null that meets all of them and is still capped has no
reachable ceiling — which is the defect that rung was added to fix.
