# Examples

> **Reading the samples below.** Subject figures are stated as test totals; the case
> files they come from give them per arm, two columns. Prior-test figures are each
> test's total. Both sides of every floor comparison are totals — see
> `reference/prior-test-tables.md`.

Three worked cases. They show the reasoning, not just the conclusion.

The cases are constructed for teaching, with figures chosen to make each discriminator legible.
They are not real readouts. The third one finds nothing wrong with the test and nothing wrong with
the analysis, and concludes the change does not do enough to matter — which is the most common
correct finding in this domain and the one a room is least prepared to hear.

Between them they show the three possible verdicts on a prior run: one qualifying re-run that
returned a negative result, one case with no prior run to read, and one non-qualifying prior run
treated as no evidence at all.

**No case reports a segment finding.** None was pre-registered in any of them, and none is
searched for.

---

## Case 1 — Break at Stage 4

**Input:** Redesigned returns-initiation flow. Two runs: 372,000 assigned, then a re-run at
1,488,000. Six prior tests on the same surface. Decision threshold supplied. Affected-population
readout supplied.

### Failure observed

The team's question was whether the redesigned returns flow lifts 30-day repeat purchase enough
to justify rolling it out. Their stated decision threshold is **+0.50pp**.

Run 1: +0.08pp, 95% CI **[−0.72, +0.88]**. The interval contains the threshold, so the test did
not answer the question.

Run 2, at 4× the sample: +0.08pp, 95% CI **[−0.32, +0.48]**. This interval excludes the
threshold, though barely, and the width behaved exactly as the sample increase predicts.

The failure is not that the result was flat. It is that **no sample this test could have run at would have let this
metric answer this question** — see the mechanism.

### Comparison set

Six prior tests on the returns flow, same randomization unit, same eligibility definition,
comparable traffic periods. All six read on **return-completion rate**.

| Prior test | Assigned | Outcome |
|---|---|---|
| FAQ link placement | 240,000 | detected, +2.1pp |
| Return-label email timing | 310,000 | detected, +4.4pp |
| Copy simplification | 198,000 | flat |
| Reason-dropdown order | 420,000 | flat |
| Refund status page | 275,000 | detected, +1.8pp |
| Entry point in account nav | 355,000 | flat |

Detection floor: this surface has resolved effects down to **+1.8pp on return-completion at
275,000 assigned**.

### Comparison-set integrity

**Usable with limitations.** Six prior tests, three with detected effects, matched on surface,
randomization unit, population definition, and traffic period. No single test carries the floor:
removing the refund-status page still leaves resolved effects at +2.1pp and +4.4pp.

The axis that fails is metric family, and it does two things at once. All six prior tests read on
return-completion rate; the subject reads on site-wide 30-day repeat purchase.

It is the departure the diagnosis rests on. The subject differs from six tests that share its
surface, its unit, and its eligibility on exactly one axis, and that axis carries the finding. The
set is what makes it visible.

It is also a real gap in what the set can baseline. `Same surface, same metric family` is the first
axis in the Step 0.5 check, and it fails here: a set reading entirely on return-completion cannot
say what sample this surface needs to resolve a repeat-purchase effect. The Stage 3 floor quoted
below is a floor for a different metric.

Both are true, and the second does not stop being true because the first is useful. Per the
integrity rung, confidence caps at Provisional — which binds whether or not the finding needs the
floor, and this finding does not, since it rests on the 4.1% denominator share and the
affected-population interval, both measured. Say the finding survives the gap. Do not use its
survival to lift the grade.

### Funnel reconstruction

| Stage | Subject | Basis | Read |
|---|---|---|---|
| 1. Delivery | 50.1 / 49.9 split, pre-exposure balanced | Ratio check supplied for run 1 | Clean |
| 2. Exposure | Eliminated structurally | See below | Clean |
| 3. Sensitivity | Half-width 0.40pp at 1,488,000 | Above the floor after the re-run | Clean after run 2 |
| 4. Measurement | Affected population is 4.1% of the denominator | Return-initiators as a share of assigned | **Break** |
| 5. Inference | Analysis on all assigned while the change reaches 4.1% | Readout definition | **Starved** |

**Stage 2 is eliminated structurally rather than comparatively.** Impressions on the redesigned
flow were not logged, so exposure cannot be measured directly. It does not need to be:
return-completion rate among initiators moved from 61.2% to 74.8%, and that metric cannot move
unless the flow was seen. A moved downstream metric proves the upstream stage did not break, and
bounds exposure from below. Recorded as structural.

### Primary constraint

**Stage 4, measurement.** The test was delivered, seen, and — after the re-run — precisely
estimated. It was pointed at a metric that could not have shown a decision-relevant effect no
matter what the change did.

Stages 1 through 3 are not the diagnosis site, and no conclusion is drawn about the assignment,
the exposure rate, or the adequacy of the sample. All three demonstrably worked by run 2. Stage 5
is clean and is not the constraint.

### Primary cause

**Affected population is a small share of the denominator.**

### Mechanism

The change acts only on users who initiate a return, and return-initiators are **4.1%** of the
assigned population. The primary metric, site-wide 30-day repeat purchase rate, is computed across
all assigned users, so any effect is divided by a denominator overwhelmingly composed of users the
change cannot reach.

The arithmetic closes the case. Among return-initiators the change lifts 30-day repeat purchase by
**+8.3pp, 95% CI [+4.9, +11.7]** — a clear effect on the population it can act on. Carried into the
site-wide metric at 4.1% of the denominator, that arrives as roughly **+0.34pp**. The team's
decision threshold is +0.50pp. **The largest effect the change plausibly produces cannot reach the
threshold in the metric chosen to measure it**, and that was true before a single user was
assigned.

### Evidence for this cause and against the alternatives

The affected-population readout is the discriminating evidence, and it is the only thing that
separates this from a sensitivity problem. A test underpowered for the effect and a test measuring
a diluted effect both produce a wide interval on a sample that looks adequate by count — the
sample size screens them in together and cannot rank them. What differs is what a readout on the
users the change can actually reach shows. Here it shows +8.3pp with an interval nowhere near
zero. The change works. The metric cannot see it.

The comparison set independently points to the same place. Every prior test on this surface that
resolved an effect read on return-completion rate, at samples between 240,000 and 310,000. The
subject departs from the set on exactly one axis — the primary metric — and that axis is the one
carrying the finding.

Arithmetic beats inference here and is worth stating as arithmetic rather than statistics: an
effect on 4.1% of a denominator arrives at roughly 4.1% of its size, and no sample size changes
that ratio.

### Alternatives, and why they are demoted

**Sensitivity (Stage 3).** Not supported, and the re-run is readable here. The team re-ran at 4×
the sample on the assumption the first result was underpowered. Conditions: it changed the thing
under test — more traffic acts on sensitivity; the variant was otherwise unchanged; the added
sample was sufficient, narrowing the half-width from 0.80pp to 0.40pp exactly as the square-root
relationship predicts; and the period was comparable with no concurrent release on the surface.

**That is a qualifying re-run, and it tested sensitivity.** The interval narrowed by half and the
estimate held at +0.08pp. **Negative.** Sensitivity is not the constraint. Two findings, not one:
the re-run resolved the precision question, and in resolving it revealed that precision was never
what stood between this team and an answer.

**The change does nothing (the null).** Rejected, and the reason is structural rather than
evidential. The null requires every stage clean, and Stage 4 is not clean. The run-2 interval
excludes the decision threshold, which in a clean funnel would point at the null — but a metric
that could not have registered the effect at any sample cannot be used to conclude the effect is
absent. The interval is precise about a quantity nobody wanted measured.

**Exposure dilution (Stage 2).** Eliminated structurally, as above.

**Delivery (Stage 1).** Not supported. Run 1's ratio check passes and pre-exposure metrics are
balanced.

**Inference dilution (Stage 5).** Not supported. The analysis population matches the exposed
population and the unit of analysis matches the randomization unit.

### Null model

Rejected, on the structural ground above: a break at Stage 4 makes the null unreachable regardless
of how the interval reads. Stating this explicitly matters here, because the run-2 interval has
exactly the shape a null is read off, and a folder that stopped at the interval would deliver the
wrong finding with high confidence.

### Confidence

**Provisional.** The funnel locates the constraint, the affected-population readout separates the
two live causes at the break stage, and the null was
tested and rejected on structural grounds.

### Missing evidence

The sample-ratio check for run 2 was not supplied; only run 1's was.

**Which way it pushes.** Run 2 used the same assignment service with no configuration change, so a
ratio failure appearing only in run 2 would have to be newly introduced. And a ratio failure
inflates variance and widens intervals, whereas run 2's interval narrowed by precisely the factor
the sample increase predicts. **The gap pushes against a delivery break rather than toward one**,
and the conclusion survives the worst case in which run 2's ratio was mildly off.

Also absent: exposure instrumentation on the redesigned flow, handled by structural elimination
above.

### What would prove this wrong

A return-initiator share materially above 4.1% — if the change reaches, say, a third of the
denominator, the arithmetic that closes this case reopens. Or an affected-population readout whose
interval includes zero, which would remove the evidence that the change works at all and move the
finding toward the null.

---

## Case 2 — Break at Stage 2

**Input:** New recommendation algorithm on a homepage module. One run, 240,000 assigned. Five
prior homepage tests. Decision threshold supplied. No exposure-triggered readout.

### Failure observed

The team's question was whether the new algorithm lifts click-through to product pages. Stated
decision threshold: **+0.25pp**.

Result: +0.08pp, 95% CI **[−0.21, +0.37]**. The interval contains the threshold. The test did not
answer the question.

### Comparison set

Five prior tests on the homepage, same randomization unit, same eligibility definition, comparable
traffic periods, all read on click-through to product page.

| Prior test | Assigned | Exposure | Outcome |
|---|---|---|---|
| Hero image treatment | 180,000 | 94% | detected, +0.41pp |
| Nav category order | 220,000 | 96% | flat |
| Hero copy | 195,000 | 93% | detected, +0.33pp |
| Search bar prominence | 260,000 | 88% | flat |
| Banner removal | 210,000 | 95% | flat |

Detection floor: effects down to **+0.33pp resolved at 195,000 assigned with 93% exposure**, giving
an effective exposed sample of roughly 181,000.

### Comparison-set integrity

**Usable with limitations.** Five prior tests, two with detected effects, matched on surface,
randomization unit, population, metric, and period.

The limitation: **every prior test changed an above-the-fold element**, with exposure between 88%
and 96%. The subject changes a module below the fold. The set therefore establishes a detection
floor, which it does, and **cannot establish a normal exposure rate for a below-fold position**,
because that axis does not vary across it.

Confidence caps at Provisional.

### Funnel reconstruction

| Stage | Subject | Basis | Read |
|---|---|---|---|
| 1. Delivery | 50.0 / 50.0, pre-exposure balanced | Ratio check supplied | Clean |
| 2. Exposure | 9.1% of assigned, 21,840 users | Module impression logs | **Break** |
| 3. Sensitivity | Half-width 0.29pp on the assigned population | Reported interval | Starved |
| 4. Measurement | Not assessed | — | Starved |
| 5. Inference | Not assessed | — | Starved |

Pass-through, assigned to exposed: **9.1%**. The module renders at 68% page depth on a page whose
median scroll depth is 41%.

### Primary constraint

**Stage 2, exposure.** 240,000 users were assigned and 21,840 ever rendered the module. The
effective sample is **8.3× below** the smallest effective sample at which this surface has resolved
an effect.

Stages 3 through 5 are starved and nothing is concluded from them. This matters more than the
phrasing suggests: the reported interval's width is a *consequence* of the exposure rate, not
independent evidence about the surface's sensitivity, and reading it as a sensitivity finding would
send the team to buy traffic for a problem traffic cannot fix.

Nothing is concluded about whether the new algorithm works.

### Primary cause

**Position dilution.**

### Mechanism

The module sits at 68% page depth on a homepage whose median scroll depth is 41%, so roughly one
assigned user in eleven ever renders it. The readout averages any effect on 21,840 exposed users
across 240,000 assigned, compressing it by approximately elevenfold. An effect of +0.9pp on the
exposed population — twice the largest effect this surface has ever resolved — would arrive in the
assigned-population readout at about +0.08pp, which is the point estimate observed and sits well
inside the reported interval. The test cannot distinguish that from nothing.

### Evidence for this cause and against the alternatives

The pass-through is the evidence and the levels are the trap. 240,000 assigned is a larger sample
than four of the five prior tests, three of which were adequate to resolve an effect. Read as a
level, this test looks well powered. Read as pass-through, its effective sample is 21,840 against a
floor of roughly 181,000.

The arithmetic of dilution matches the observed point estimate, which is the check that
distinguishes this from a coincidence: a strong effect on the exposed population predicts almost
exactly the +0.08pp seen, and so does no effect at all. That is precisely why nothing downstream can
be concluded.

Delivery is ruled out by the ratio check and pre-exposure balance.

### Alternatives, and why they are demoted

**Sensitivity (Stage 3).** Downstream of the primary and not independent. The interval is wide
because the exposed sample is small, and the exposed sample is small because of the position. Adding
traffic at this exposure rate would need roughly eleven times the assigned volume to reach what a
prior above-the-fold test achieved at 195,000.

**The change does nothing (the null).** Unreachable. A break at Stage 2 makes the null unavailable
regardless of the readout, and this case is the clearest illustration of why: the observed result is
exactly what a large real effect would produce after elevenfold dilution.

**Measurement (Stage 4).** Starved and untested. Click-through to product page is the metric every
prior test on this surface used, so there is no reason from the comparison set to suspect it, but
that is not a finding and it is not tested here.

**Prior run.** None. This test has been run once, so there is no completed experiment to read,
which is worth stating rather than leaving as an absence.

### Null model

Not reachable. Reported as blocked rather than rejected: the funnel breaks at Stage 2, and the null
requires every stage clean. No claim is made about whether the change has an effect.

### Confidence

**Provisional.** The constraint is located and the mechanism is well supported by the measured
exposure rate and the arithmetic, but the comparison set is Usable with limitations — it contains no
below-fold comparable and therefore cannot establish what exposure this position normally produces.

### Missing evidence

**An exposure-triggered readout**, restricted to the 21,840 users who rendered the module. That is
the single input that would say whether the algorithm does anything, and its absence is why this
report concludes nothing about the change itself.

A below-fold prior test on this surface, which would say whether 9.1% is anomalous for the position
or ordinary for it.

**Which way that gap pushes.** If 9.1% turns out to be the normal rate for this position, the
finding does not change — the effective sample is still 8.3× below the floor, and the constraint is
still position dilution. What changes is only whether the position is a defect or a fact. **The
conclusion survives the worst case**, and the missing comparable affects the framing rather than the
diagnosis.

### What would prove this wrong

An exposure-triggered readout with a tight interval excluding the decision threshold. That would
mean the exposed population was large enough after all, move the constraint off Stage 2, and make
the null reachable.

---

## Case 3 — Null

**Input:** Payment-method reordering at checkout. 1,240,000 assigned. Six prior checkout tests.
Decision threshold supplied. Full readout including ratio check, exposure, and an earlier run of a
related test.

### Failure observed

The team's question was whether reordering payment methods by per-user frequency lifts checkout
completion enough to justify maintaining the ordering logic across 14 locales. Stated decision
threshold: **+0.40pp**.

Result: +0.06pp, 95% CI **[−0.11, +0.23]**.

The interval **excludes** the decision threshold. Every effect compatible with this result is
smaller than the effect the team said would change their decision. The test answered the question it
was run to answer.

### Comparison set

Six prior tests on checkout, same randomization unit, same eligibility definition, comparable traffic
periods, all read on checkout completion rate.

| Prior test | Assigned | Exposure | Outcome |
|---|---|---|---|
| Guest checkout prominence | 890,000 | 97% | detected, +0.31pp |
| Address autofill | 640,000 | 96% | detected, +0.52pp |
| Error message clarity | 410,000 | 98% | detected, +1.10pp |
| Progress indicator | 720,000 | 97% | flat |
| Field label wording | 830,000 | 96% | flat |
| Card-form layout | 560,000 | 95% | flat |

Detection floor: this surface has resolved an effect as small as **+0.31pp at 890,000 assigned**.

### Comparison-set integrity

**Usable.** Six prior tests, three with detected effects, matched on surface, randomization unit,
population definition, metric, and traffic period. No single test carries the floor: removing the
guest-checkout test still leaves +0.52pp resolved at 640,000.

The subject's sample of 1,240,000 exceeds the sample at which this surface resolved its smallest
detected effect, and the subject's half-width of 0.17pp is below that effect. The floor is cleared
with margin.

### Funnel reconstruction

| Stage | Subject | Basis | Read |
|---|---|---|---|
| 1. Delivery | 50.02 / 49.98; pre-exposure metrics balanced | Ratio check supplied, passes | Clean |
| 2. Exposure | 97.3% of assigned | Selector impression logs | At baseline (set: 95–98%) |
| 3. Sensitivity | Half-width 0.17pp at 1,240,000 | Reported interval against the floor | Above the floor |
| 4. Measurement | Selector sits inside checkout; affected population is 97.3% of the denominator | Metric definition | Clean |
| 5. Inference | Analysis on exposed users; unit is user, as randomized | Readout definition | Clean |

No stage reads below par.

### Primary constraint

**None located.** Every stage that can be checked is clean, and the interval excludes the effect the
team would have acted on.

### Primary cause

**No test failure is demonstrated. The change does not produce an effect the team would act on.**

### Mechanism

The test was capable of answering and it answered. Delivery split as designed with balanced
pre-exposure metrics. Exposure at 97.3% sits inside the range this surface produces, which is
expected because checkout is a mandatory path rather than a discoverable one. The sample of
1,240,000 exceeds the 890,000 at which this surface resolved a +0.31pp effect, so the design could
have detected an effect well below the team's threshold. The metric is checkout completion, which
the payment selector sits directly inside, and it is computed on a denominator 97.3% of which the
change can reach — this is the rare case where dilution is not a concern by construction.

Against that, the estimate is +0.06pp with an interval of [−0.11, +0.23]. The upper bound is
**0.17pp below the decision threshold**. A real effect large enough to matter to this team is not
compatible with this result.

### Evidence for this cause and against the alternatives

The load-bearing comparison is the interval against the threshold, not the p-value against 0.05. An
underpowered test and a change with no meaningful effect both produce a non-significant result, both
produce an interval containing zero, and both produce a point estimate near zero — the p-value is
predicted identically by the two hypotheses this folder exists to separate and discriminates neither.
What separates them is width relative to what the team would act on, and this interval is narrow
enough to exclude it.

The detection floor establishes that the surface can resolve effects at this scale: three prior
tests detected effects, one of them smaller than the subject's threshold, at samples below the
subject's.

Every stage upstream is clean on supplied evidence rather than by assumption. That matters because
the null is the conjunction of five clean stages, not the residual left when nothing else is found.

### Alternatives, and why they are demoted

**Sensitivity (Stage 3).** Not supported. The sample exceeds the floor, and the interval is narrower
than the smallest effect this surface has resolved.

**Exposure dilution (Stage 2).** Not supported. 97.3% exposure sits inside the set's range.

**Measurement (Stage 4).** Not supported. The affected population is 97.3% of the denominator and
the metric is directly downstream of the mechanism.

**Delivery and inference (Stages 1 and 5).** Not supported. Ratio check passes, pre-exposure balanced,
analysis population matches exposure, unit matches randomization.

**"We tested this before and it was flat too."** The team cites an earlier run, eight months ago, at
180,000 assigned. That run is **Uninformative** rather than corroborating, and two conditions fail.
The variant differed — it ordered by global popularity rather than per-user frequency, which is a
different change. And 180,000 is below the surface's floor by a factor of roughly five, so it could
not have resolved an effect at the threshold in any case. It is no evidence in either direction, and
it must not be read as a second flat result confirming the first.

**A segment where it worked.** Not searched for and not admissible. No segment was pre-registered,
and the change's mechanism predicts no particular subpopulation — every checkout user sees a payment
selector. A subgroup found by scanning after a flat headline would be an artifact.

### Null model

**Accepted. This is the diagnosis.** Every stage is clean on supplied evidence, and the interval
excludes the decision threshold.

### Confidence

**Supported**, under the null-grading rung in `rules.md`, and every clause is met rather than assumed.

The comparison set is Usable. Every stage that can be checked is clean. The null was **tested
directly against the input built to test it** — the confidence interval read against the team's
stated threshold, with the detection floor established from three prior tests that resolved effects
on this surface — rather than inferred from the readout looking unobjectionable. And the
alternatives this comparison set is structurally unable to see are named below.

This is a case where Supported is earned rather than assumed, and where the grade carries weight: the
team is being told to stop, and the strength of that finding is the whole of its value.

### Missing evidence

**An A/A test on this surface.** Every test in the comparison set shares the same instrumentation, so
a systematic instrumentation bias does not vary across the set and **this comparison set is
structurally unable to detect it.** An A/A result would bound the noise floor directly. None was
supplied, and this is named as unexamined rather than ruled out.

**Any evidence about other surfaces.** The set holds the surface constant, which is what makes it
work, and it means nothing here speaks to whether per-user payment ordering would matter in the
mobile application, at a different step, or in locales with different payment mixes. Named, not
tested.

Locale-level readouts were not supplied. They are **not** requested: no locale split was
pre-registered, the mechanism predicts no particular locale, and a scan across 14 of them would
produce a significant result by construction.

### What would prove this wrong

An A/A test on this surface showing spurious differences of a size comparable to the decision
threshold, which would mean the instrumentation cannot support conclusions at this resolution and
would move the finding to Undetermined.

A revised decision threshold. The finding is relative to +0.40pp, and it is not robust to that
number changing: at a threshold of +0.20pp this interval would no longer exclude it, and the
diagnosis would become sensitivity rather than the null. **The threshold is the team's input and the
conclusion is conditional on it**, which is worth stating plainly rather than burying.

---

**Why this case is in the set.**
The request was to find out what went wrong with the test. Nothing went wrong with the test. It was
delivered correctly, seen by almost everyone assigned, sized well above the floor this surface has
demonstrated, pointed at the metric the change acts on, and analysed on the right population. It
answered the question it was asked.

The answer is that the change does not move checkout completion by an amount this team said would
matter. Naming that is the diagnosis, and it is the hardest one to deliver here, because a quarter
of work and a two-year request sit behind the change. What to do with a well-tested change that
does nothing is the team's decision, and this folder does not have an opinion about it.
