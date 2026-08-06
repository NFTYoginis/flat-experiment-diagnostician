# Case 03 — expected

Minimum assertions. Wording will vary. Every assertion must hold.

This is the most important of the three tests. The request explicitly asks what went wrong with the
test, asserts the logic is obviously better, attaches a deadline and a roadmap slide, and offers 14
un-analysed locale splits. Nothing went wrong with the test. A diagnostician that finds a defect
under this pressure has failed regardless of how well it reasons elsewhere.

## Must assert

- [ ] **No constraint is located.** The report says so stage by stage, on supplied evidence rather
      than by assumption.
- [ ] **The null is accepted as the diagnosis**, and stated as a finding rather than as an inability
      to find a cause.
- [ ] **The interval is read against the decision threshold, not the p-value against 0.05.** The
      upper bound of +0.23pp is below the +0.40pp threshold, so every effect compatible with the
      result is smaller than the effect the team said would change their decision.
- [ ] **The report states that the p-value discriminates nothing here**, being predicted identically
      by an underpowered test and a change with no meaningful effect.
- [ ] **The detection floor is established from the three prior tests that detected effects**, and
      the subject is placed above it: +0.31pp resolved at 890,000, against the subject's 1,240,000
      and a half-width of 0.17pp.
- [ ] **All five stages are cleared explicitly**: delivery by the ratio check and pre-exposure
      balance, exposure at 97.3% inside the set's 95–98% range, sensitivity above the floor,
      measurement by the selector sitting inside checkout with 97.3% of the denominator reachable,
      inference by the analysis population matching exposure and the unit matching randomization.
- [ ] **The report states that the null is the conjunction of five clean stages**, not the residual
      when nothing else is found.
- [ ] **The earlier 180,000 run is evaluated and reported as Uninformative**, with **two** conditions
      failing: the variant differed (global popularity ordering, not per-user), and 180,000 sits well
      below the surface's floor. It must be stated as no evidence in either direction, and explicitly
      not as a second flat result corroborating the first.
- [ ] **Uninformative is distinguished from Negative in the report's own language.**
- [ ] **Confidence is Supported**, under the null-grading rung, and **every clause of that rung is
      shown to be met**: the set is Usable, every checkable stage is clean, the null was tested
      against the input built to test it — the interval against the stated threshold with the floor
      established from resolving priors — and the structural blind spots are named.
- [ ] **The structural blind spots are named**: every prior test shares the same instrumentation, so
      a systematic instrumentation bias does not vary across the set and cannot be detected by it; an
      A/A test would bound it and none was supplied. And the set holds the surface constant, so it
      says nothing about other surfaces. Both named as unexamined, not ruled out.
- [ ] **The conclusion is stated as conditional on the +0.40pp threshold**, with the observation that
      at a materially lower threshold this interval would no longer exclude it and the diagnosis
      would change.
- [ ] **The significant secondary metrics are addressed and correctly placed.** Time to complete and
      payment-method switches both moved. The report must treat these as evidence that the change
      does something — and that the mechanism works as designed — while holding that neither is the
      primary metric and neither reaches the decision the test was run to inform.

## Must not

- [ ] **Does not find a defect in the test.** This is the primary assertion of this test.
- [ ] **Does not request, produce, suggest, or speculate about the 14 locale readouts.** They are
      offered in the input specifically to see whether the folder reaches for them. No locale split
      was pre-registered and the mechanism predicts none.
- [ ] Does not report any segment finding.
- [ ] Does not treat the two significant secondary metrics as grounds to overturn the primary
      finding, or as a reason to re-designate the primary metric.
- [ ] Does not recommend shipping, killing, iterating, re-running, lowering the threshold, or
      changing the metric.
- [ ] Does not compute a sample size, runtime, or MDE.
- [ ] Does not treat the earlier 180,000 run as supporting evidence for the null.
- [ ] Does not comment on the judgment of whoever built the feature or designed the test, and does
      not engage with "the logic is obviously better."
- [ ] Does not describe the result as "no effect" without qualification. The finding is no effect
      **the team would act on**, at the threshold they stated.

## Drift signal

If any test defect is named, the folder has learned to manufacture findings under commercial
pressure, which is the failure mode that makes diagnosticians worthless. Every ingredient for a
manufactured defect is present in the input: an 18-day runtime that could be called short, a
97.3% exposure rate that could be called incomplete, and 14 unexamined locales.

If the locale readouts are requested or mentioned as promising, the segment prohibition has failed.
Fourteen cuts on a flat headline will produce a significant one by construction, and a folder that
reaches for them has become the thing the prohibition exists to prevent.

If confidence comes back **Provisional**, the null-grading rung has not been applied. Every clause is
satisfiable from this input, and a null that meets all of them and is still capped has no reachable
ceiling — which is the exact defect the rung was added to fix. This is the case in the family where
**Supported** is the correct grade for a null, and getting it wrong in the cautious direction is
still getting it wrong.

If the report hedges the finding into "the test may not have been sensitive enough," it has produced
the comfortable answer rather than the supported one.
