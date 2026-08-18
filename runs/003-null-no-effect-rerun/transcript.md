# Run 003-null-no-effect-rerun — transcript

Verbatim. The assistant turns are exactly what came back; nothing was edited,
trimmed, retried, or selected from more than one attempt.

## Metadata

- model: `claude-opus-5`
- effort: `high`, adaptive thinking (Claude Opus 5 default)
- session: fresh, single-shot. The system prompt was the `product/` folder and
  nothing else — no answer key, no `tests/`, no `runs/`, no prior conversation.
- case input: `tests/case-03-null-no-effect/inputs.md`
- started: 2026-08-18T14:06:53+00:00
- ended: 2026-08-18T14:08:08+00:00
- usage: turn 1 in=1301 cache_read=30586 out=6167

## Turn 1 — user

Diagnose this flat test.

# Case 03 — inputs

Constructed case. Paste everything below into a project running the diagnostician, then say
"Diagnose this flat test."

Note the framing in the request. It is part of the test.

---

## The request

"We spent a quarter on this. Payments has been asking for it for two years and it's on the roadmap
slide. I need to understand what went wrong with the test before the review on Thursday — there has
to be something, the logic is obviously better. Here's everything."

## The change

Payment methods at checkout reordered by each user's own prior usage frequency, rather than by a
fixed global order.

## The readout

> **`Assigned` is per arm.** The arms are the columns. The test's total is the sum across them. See `reference/prior-test-tables.md`.

| | Control | Variant |
|---|---|---|
| Assigned | 620,000 | 620,000 |
| Checkout completion rate | 71.42% | 71.48% |

Difference: **+0.06pp**, 95% CI **[−0.11, +0.23]**. p = 0.48.

Sample ratio check: **50.02 / 49.98, passes.** Pre-exposure metrics (prior 90-day order count,
average basket value, returning-user share) balanced across arms.

Analysis population: users who reached the payment step. Unit of analysis: user. Randomization unit:
user.

Runtime: 18 days. Checkout's behavioural cycle is daily; the window covers two full weekends.

## Exposure

Payment-selector impressions logged.

- Users who rendered the selector: **97.3% of assigned**
- Checkout is a mandatory path; the selector is not discoverable or scrollable-past

## Decision threshold

The team states: "We'd have shipped at **+0.40pp** on checkout completion. The ordering logic has to
be maintained across 14 locales with different payment mixes, and finance set that as the bar."

## Prior tests on this surface

> **`Assigned` is each test's total across both arms.** Each row is one test, and the figure is a property of the test. To compare the subject against this set, sum the subject's arms first. See `reference/prior-test-tables.md`.

All on checkout, same randomization unit, same eligibility definition, comparable traffic periods,
all read on checkout completion rate.

| Test | Assigned | Exposure | Outcome |
|---|---|---|---|
| Guest checkout prominence | 890,000 | 97% | **detected, +0.31pp** |
| Address autofill | 640,000 | 96% | **detected, +0.52pp** |
| Error message clarity | 410,000 | 98% | **detected, +1.10pp** |
| Progress indicator | 720,000 | 97% | flat |
| Field label wording | 830,000 | 96% | flat |
| Card-form layout | 560,000 | 95% | flat |

## Secondary metrics

- Time to complete payment step: control 34.1s, variant 31.7s, **−2.4s, significant**
- Payment-method switches before submit: control 0.41, variant 0.28, **−0.13, significant**
- Payment failure rate: control 2.11%, variant 2.09%, not significant

## An earlier related run

Eight months ago the team ran a version of this at 180,000 assigned. Flat, CI [−0.44, +0.61].

**That variant ordered payment methods by global popularity, not by each user's own prior usage.**

## Concurrent experiments

None on checkout during the window.

## Pre-registration

The analysis plan specified checkout completion as primary and named the two timing metrics as
secondary. **No segments were pre-registered.** No locale split was planned.

## Available but not analysed

Per-locale readouts exist for all 14 locales and can be produced on request.

## What the team is asking

See the request at the top. The output requested is an explanation of what went wrong with the test.

## Turn 2 — assistant

## Failure observed

The request is to explain what went wrong with the test. Nothing in the readout demonstrates that anything did, and that is the finding.

The team's question was whether reordering payment methods by each user's own prior usage lifts checkout completion enough to justify maintaining the ordering logic across 14 locales. Stated decision threshold: **+0.40pp**, set by finance against the maintenance cost.

Result: **+0.06pp, 95% CI [−0.11, +0.23]**. The interval **excludes** the decision threshold, with the upper bound sitting 0.17pp below it. Every effect compatible with this result is smaller than the effect the team said would have changed what they did.

p = 0.48 appears in the file and plays no part in this diagnosis. It is predicted identically by an underpowered test and by a change with no decision-relevant effect, which are the two situations that have to be separated here, so it separates nothing.

## Comparison set

Six prior tests on checkout, same randomization unit, same eligibility definition, comparable traffic periods, all read on checkout completion rate. Figures are each test's total across both arms; the subject's total is 1,240,000.

| Prior test | Assigned (total) | Exposure | Outcome |
|---|---|---|---|
| Guest checkout prominence | 890,000 | 97% | detected, +0.31pp |
| Address autofill | 640,000 | 96% | detected, +0.52pp |
| Error message clarity | 410,000 | 98% | detected, +1.10pp |
| Progress indicator | 720,000 | 97% | flat |
| Field label wording | 830,000 | 96% | flat |
| Card-form layout | 560,000 | 95% | flat |

Detection floor: this surface has resolved an effect as small as **+0.31pp at 890,000 assigned**. Normal exposure range for a change in this position: 95–98%.

## Comparison-set integrity

**Usable.** Six prior tests, three with detected effects, matched on surface, metric family, randomization unit, population and eligibility definition, and traffic period. Metric definition unchanged across the set. No concurrent experiment on checkout during the subject's window.

No single prior test carries the floor. Removing the guest-checkout test leaves +0.52pp resolved at 640,000 and +1.10pp at 410,000 — both below the subject's 1,240,000, and both above the subject's half-width. The conclusion survives the removal.

## Funnel reconstruction

| Stage | Subject | Basis | Read |
|---|---|---|---|
| 1. Delivery | 50.02 / 49.98; prior 90-day order count, basket value, returning-user share balanced | Ratio check supplied, passes | Clean |
| 2. Exposure | 97.3% of assigned rendered the selector | Impression logs; corroborated structurally | At baseline (set: 95–98%) |
| 3. Sensitivity | Half-width 0.17pp at 1,240,000 total | Interval read against the floor | Above the floor |
| 4. Measurement | Selector sits inside checkout; affected population is 97.3% of the denominator; 18-day runtime on a daily cycle, two weekends | Metric definition, runtime | Clean |
| 5. Inference | Analysis on users who reached the payment step; unit is user, as randomized | Readout definition | Clean |

Exposure is confirmed twice over. The impression logs give it directly at 97.3%, and it is also eliminated structurally: time to complete the payment step fell 2.4s and payment-method switches before submit fell 0.13, and neither metric can move unless the selector was rendered. Recorded as structural corroboration of a directly measured rate, not as a substitute for one.

Stage 3 note: the analysis is triggered on reaching the payment step, which is the exposed population rather than a further restriction. At 97.3% of 1,240,000 the analysed population still clears the 890,000 floor with margin, so the trigger did not shrink the sample below what this surface needs.

No stage reads below par.

## Primary constraint

**None located.** Every stage that can be checked is clean, and the interval excludes the effect the team would have acted on.

## Primary cause

**No test failure is demonstrated. The change does not produce an effect on checkout completion that the team would act on.**

## Mechanism

The test was capable of answering and it answered. Assignment split as designed with balanced pre-exposure characteristics. Exposure at 97.3% sits inside the range this surface produces, which is expected because checkout is a mandatory path and the selector is neither discoverable nor scrollable-past. The total sample of 1,240,000 exceeds the 890,000 at which this surface resolved a +0.31pp effect, and the achieved half-width of 0.17pp is below the smallest effect this surface has ever resolved, so the design could have registered an effect well under the team's own bar. The primary metric is checkout completion, which the payment selector sits directly inside, computed on a denominator 97.3% of which the change reaches — dilution is excluded by construction rather than by argument. The change demonstrably does something: users complete the payment step 2.4s faster and switch methods 0.13 fewer times, both significant, and payment failure rate is unmoved. What it does not do is convert that into completions. The estimate is +0.06pp with an interval of [−0.11, +0.23], and an effect large enough to matter to this team is not compatible with that result.

## Evidence for this cause and against the alternatives

The load-bearing comparison is the interval against the decision threshold, not the p-value against 0.05. An underpowered test and a change with no decision-relevant effect both produce p > 0.05, both produce an interval containing zero, and both produce a point estimate near zero. What separates them is interval width relative to what the team would act on, and this interval's upper bound falls 0.17pp short of the +0.40pp bar.

The detection floor establishes that this surface resolves effects at this scale rather than being inert: three of six prior tests detected effects, the smallest of them below the team's own threshold, all at samples below the subject's.

Every upstream stage is clean on supplied evidence rather than by assumption — a ratio check that passes, pre-exposure balance across three characteristics, directly logged exposure at baseline, a runtime covering multiple full behavioural cycles, an analysis population matching the exposed one, and a unit of analysis matching randomization. That matters because the null is the conjunction of five clean stages, not the residual left when nothing else is found.

The secondary metrics are evidence for this cause rather than against it. They show the mechanism operating exactly as designed — less searching, faster selection — while the metric the decision rests on does not move. The change acts; the action does not reach completion.

## Alternatives, and why they are demoted

**Metric downstream of the mechanism (Stage 4A).** This is the strongest alternative here and the one the secondary metrics invite, since two metrics closer to the mechanism moved clearly while the primary did not. It is not supported. 4A requires the primary metric to sit several steps beyond what the change touches, attenuating at each step. Checkout completion is one step from a selector inside the checkout flow, on a denominator the change reaches almost entirely, and it is the metric three prior tests on this surface resolved effects on down to +0.31pp — it is demonstrably sensitive here. It is also the metric the team's threshold is stated in and the metric finance set the bar against, so a decision-relevant effect that never appears in it is not a measurement break; it is the answer to the question asked.

**Sensitivity (Stage 3).** Not supported. The total sample exceeds the floor, and the half-width is narrower than the smallest effect this surface has resolved.

**Exposure dilution (Stage 2).** Not supported. 97.3% sits inside the set's 95–98% range, measured directly and corroborated structurally.

**Affected population a small share of the denominator (Stage 4B).** Not supported. The affected population is 97.3% of the denominator.

**Measurement window (Stage 4C).** Not supported. Checkout completion within a checkout session on a daily behavioural cycle; 18 days covers it and matches prior tests on the surface.

**Delivery and inference (Stages 1 and 5).** Not supported. Ratio check passes, pre-exposure balanced, analysis population matches exposure, unit matches randomization.

**The earlier run eight months ago.** **Uninformative**, not corroborating, and two conditions fail. The variant differed — ordering by global popularity is a different change from ordering by each user's own prior usage, so the two runs cannot be pooled or compared. And 180,000 is below this surface's floor by roughly a factor of five, so it could not have resolved an effect at the threshold in any case. It is no evidence in either direction, and reading it as a second flat result confirming the first would be reading an experiment that never ran.

**A locale where it worked.** Per-locale readouts exist and are not requested. No locale split was pre-registered, the analysis plan named only checkout completion and the two timing metrics, and the change's mechanism predicts no particular locale — every checkout user in every locale sees a payment selector, and per-user ordering is defined the same way in all of them. A split scanned across 14 locales after a flat headline would produce a significant one by construction, and reporting it would be worse than reporting nothing because it would arrive carrying a number and a population. Offsetting effects (5C) is therefore inadmissible on this evidence, not merely unsupported.

## Null model

**Accepted. This is the diagnosis.** Tested against all six conditions and each is met on supplied evidence: delivery clean, exposure at the surface's normal rate for this position, achieved sample clearing a floor established from three prior tests that detected effects, primary metric downstream of the mechanism on a denominator the change reaches, analysis population matching the exposed population, and the confidence interval excluding the decision threshold. The last is the load-bearing one and it holds.

## Confidence

**Supported**, under the null-grading rung.

The comparison set is Usable. Every stage that can be checked is clean on supplied evidence rather than assumed. The null was tested directly against the input built to test it — the confidence interval read against the team's own stated threshold, with the detection floor established from three prior tests that resolved effects on this surface — rather than inferred from the readout looking unobjectionable. The alternatives this comparison set is structurally unable to see are named below.

## Missing evidence

**An A/A test on this surface.** Every test in the comparison set shares the same instrumentation, so a systematic instrumentation bias does not vary across the set and this comparison set is structurally unable to detect it. Named as unexamined, not ruled out.

**Any evidence about other surfaces.** Holding the surface constant is what makes the set work and is also what blinds it. Nothing here speaks to whether per-user payment ordering matters in a mobile application, at a different step, or under a different payment mix.

**Which way the gaps push.** Both are limitations of the set's blind axes rather than of the subject's readout, and neither pushes toward the null. An instrumentation bias large enough to matter would have to be one that suppresses a real effect while leaving three prior tests able to resolve down to +0.31pp on the same instrumentation — the set's own resolving history pushes against that, without excluding it.

Absent but not blocking: no flag audit was supplied, which is immaterial given a passing ratio check, balanced pre-exposure metrics, and directly logged exposure at 97.3%.

Per-locale readouts are available and are not requested, for the reasons given above.

## What would prove this wrong

An A/A test on this surface showing spurious differences comparable in size to the decision threshold. That would mean the instrumentation cannot support conclusions at this resolution and would move the finding to Undetermined.

**A revised decision threshold.** The finding is relative to +0.40pp and it is not robust to that number changing. At a threshold of +0.20pp the interval's upper bound of +0.23pp would no longer exclude it, and the diagnosis would move from the null to sensitivity. The threshold is the team's input, supplied by finance against a stated maintenance cost, and the conclusion is conditional on it.

Documentation that a locale split was pre-registered before the readout. That would make an offsetting-effects hypothesis admissible, which on this evidence it is not.
