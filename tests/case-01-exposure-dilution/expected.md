# Case 01 — expected

Minimum assertions. Wording will vary. Every assertion must hold.

The request asks for confirmation that the test was underpowered so the team can buy more traffic.
That is the wrong diagnosis and it is the expensive kind of wrong, because more traffic at this
exposure rate does not fix it.

## Must assert

- [ ] **Primary constraint is Stage 2, exposure.** Not sensitivity.
- [ ] **The assigned-to-exposed pass-through is computed and stated**: 21,840 of 240,000, or 9.1%.
- [ ] **The effective sample is compared against the detection floor**, not against the assigned
      sample. The floor is established from the two prior tests that detected effects — +0.33pp at
      195,000 assigned with 93% exposure — giving an effective exposed sample of roughly 181,000,
      against the subject's 21,840.
- [ ] **The interval's width is identified as a consequence of the exposure rate, not as
      independent evidence about sensitivity.** The report must say explicitly that reading it as a
      sensitivity finding is what sends the team to buy traffic for a problem traffic cannot fix.
- [ ] **Primary cause is position dilution**, and the mechanism connects the module rendering at
      68% page depth against a median scroll depth of 41%.
- [ ] **The dilution arithmetic is worked**: an effect on the exposed population arrives in the
      assigned-population readout compressed by roughly elevenfold, and a large real effect on the
      exposed users predicts approximately the +0.08pp observed — which is also what no effect
      predicts. That equivalence is why nothing downstream can be concluded.
- [ ] **Stages 3 through 5 are declared starved** and no conclusion is drawn from them.
- [ ] **No conclusion is drawn about whether the new algorithm works.**
- [ ] **The null is reported as blocked rather than rejected.** A break at Stage 2 makes the null
      unreachable; the report must not claim the change has no effect and must not claim it has one.
- [ ] **Delivery is ruled out** using the ratio check and pre-exposure balance.
- [ ] **Comparison-set integrity is Usable with limitations**, with the failing axis named
      precisely: every prior test changed an above-fold element, so the set establishes a detection
      floor but **cannot establish a normal exposure rate for a below-fold position**, because that
      axis does not vary across it.
- [ ] **Directional bias is stated on that limitation.** If 9.1% turns out to be normal for this
      position, the constraint is unchanged — the effective sample is still far below the floor and
      the cause is still position dilution. What changes is whether the position is a defect or a
      fact. The conclusion survives the worst case.
- [ ] Confidence is **Provisional**, tied to the comparison-set limitation.
- [ ] **Missing evidence names the absent exposure-triggered readout** as the single input that
      would say whether the algorithm does anything.
- [ ] **Absence of a prior run is noted** as no completed experiment being available, rather than
      passed over silently.

## Must not

- [ ] **Does not confirm that the test was underpowered in the sense the team means.** Answering
      "yes, ask for more traffic" is the primary failure of this test. The report may state the
      effective sample is below the floor; it must locate the cause at exposure, not at sample size.
- [ ] Does not recommend moving the module, running longer, adding traffic, or triggering the
      analysis on exposed users. The exposure-triggered readout may appear under missing evidence
      only.
- [ ] Does not compute a required sample size, runtime, or MDE.
- [ ] Does not conclude the change works, or that it does not.
- [ ] Does not report a segment finding. None was pre-registered and none is predicted by the
      mechanism.
- [ ] Does not treat the flat time-on-page and add-to-cart movements as evidence the change does
      nothing. Those are diluted by the same factor.
- [ ] Does not use the p-value of 0.59 as evidence for anything.

## Drift signal

If the constraint lands at Stage 3, the starved-stage rule has stopped binding, and it has stopped
binding in the direction that costs the most: the team gets the answer they asked for, buys eleven
times the traffic, and learns nothing.

If the report concludes the algorithm does not work, the null has been reached by elimination
rather than by clearing the funnel — which is the specific failure the "null is the conjunction of
five clean stages" rule exists to prevent.

If the assigned sample of 240,000 is described as adequate or well-powered without the exposure
adjustment, levels have beaten pass-through.
