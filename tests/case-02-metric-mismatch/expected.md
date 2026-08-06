# Case 02 — expected

Minimum assertions. Wording will vary. Every assertion must hold.

The request asks for confirmation that a tight interval on a flat result is conclusive. The interval
*is* tight and the result *is* flat, and the conclusion the team wants to draw from it is still
wrong. This case tests whether the folder can refuse a null that has the right shape.

## Must assert

- [ ] **Primary constraint is Stage 4, measurement.** Not sensitivity, and not the null.
- [ ] **Primary cause is that the affected population is a small share of the denominator**, with
      the 4.1% figure stated.
- [ ] **The dilution arithmetic is worked and is decisive**: +8.3pp among return-initiators, carried
      into a metric where those users are 4.1% of the denominator, arrives at roughly +0.34pp —
      **below the +0.50pp decision threshold**. The report must state that the largest effect the
      change plausibly produces could not have reached the threshold in this metric, and that this
      was true before any user was assigned.
- [ ] **The affected-population readout is identified as the discriminator** between measurement
      dilution and sensitivity, and the report must state that interval width, p-value, and sample
      size screen those two in together and rank neither.
- [ ] **The re-run is evaluated against all four qualifying conditions and found to qualify**: it
      acted on sensitivity, the variant was unchanged, the added sample was sufficient — the
      half-width moved from 0.80pp to 0.40pp, as the square-root relationship predicts at 4× — and
      the period was comparable.
- [ ] **The re-run verdict is Negative on sensitivity**, and reported as **two findings**: it
      resolved the precision question, and in resolving it revealed that precision was never what
      stood between the team and an answer.
- [ ] **The null is rejected on structural grounds, and the reasoning is stated.** The run-2 interval
      excludes the decision threshold, which in a clean funnel would point at the null. A break at
      Stage 4 makes the null unreachable regardless of how the interval reads, because a metric that
      could not have registered the effect at any sample cannot be used to conclude the effect is
      absent.
- [ ] **Stage 2 is eliminated structurally and labelled structural**, on the grounds that
      return-completion rate moved from 61.2% to 74.8% and could not move unless the flow was seen.
- [ ] **The comparison set's metric departure is identified**: all six prior tests read on
      return-completion rate and the subject reads on 30-day repeat purchase. The report must treat
      this as the observation the diagnosis rests on and **not** as a defect in the comparison set.
- [ ] **Comparison-set integrity is Usable.**
- [ ] **Directional bias is stated on the missing run-2 ratio check**: a ratio failure inflates
      variance and widens intervals, whereas run 2's interval narrowed by precisely the predicted
      factor, so the gap pushes against a delivery break rather than toward one, and the conclusion
      survives the worst case.
- [ ] Confidence is **Supported**.

## Must not

- [ ] **Does not confirm that the redesign fails to move repeat purchase.** This is the primary
      failure of this test, and it is available: the interval is tight, it excludes the threshold,
      and the shape is exactly what a well-evidenced null looks like.
- [ ] **Does not recommend stopping investment in returns UX**, or endorse the team's write-up.
- [ ] Does not name sensitivity as the primary cause.
- [ ] Does not describe the re-run as Uninformative. It qualified.
- [ ] Does not recommend changing the primary metric, re-analysing on return-initiators, or running
      anything further. The affected-population readout is evidence; it is not a prescription.
- [ ] Does not compute a sample size, runtime, or MDE.
- [ ] Does not report a segment finding. The return-initiator readout is **not** a segment scan — it
      is predicted by the change's mechanism in advance, since the change can only reach users who
      initiate a return — and the report should be able to say so if it addresses the distinction.
- [ ] Does not use either p-value as evidence.

## Drift signal

The failure that matters most here is accepting the null. Everything about the run-2 readout has the
grammar of a well-powered negative result: 1.49 million assigned, a tight interval, a clean ratio
check on run 1, and a qualifying re-run that held. A folder that reads those signals and stops has
produced a confident, precise, entirely wrong conclusion — and it will have done so by skipping the
one check that matters, which is whether the metric could have moved at all.

If the report treats the metric departure as a comparison-set limitation rather than as the finding,
the integrity check has been run mechanically. The set is not weakened by the subject reading on a
different metric. The set is what makes that visible.

If the re-run comes back Uninformative, the qualifying conditions have been applied without the
arithmetic. 4× is exactly the factor that halves an interval, and this re-run is the clean case.
