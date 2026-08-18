# Run 002-metric-mismatch-rerun — transcript

Verbatim. The assistant turns are exactly what came back; nothing was edited,
trimmed, retried, or selected from more than one attempt.

## Metadata

- model: `claude-opus-5`
- effort: `high`, adaptive thinking (Claude Opus 5 default)
- session: fresh, single-shot. The system prompt was the `product/` folder and
  nothing else — no answer key, no `tests/`, no `runs/`, no prior conversation.
- case input: `tests/case-02-metric-mismatch/inputs.md`
- started: 2026-08-18T14:05:49+00:00
- ended: 2026-08-18T14:06:53+00:00
- usage: turn 1 in=1400 cache_read=30586 out=5461

## Turn 1 — user

Diagnose this flat test.

# Case 02 — inputs

Constructed case. Paste everything below into a project running the diagnostician, then say
"Diagnose this flat test."

---

## The change

A redesigned returns-initiation flow: fewer steps, clearer eligibility messaging, and a saved
address default. It is reachable only from the order-history page, and only for orders inside the
returns window.

## Run 1

> **`Assigned` is per arm.** The arms are the columns. The test's total is the sum across them. See `reference/prior-test-tables.md`.

| | Control | Variant |
|---|---|---|
| Assigned | 186,000 | 186,000 |
| 30-day repeat purchase rate | 22.41% | 22.49% |

Difference: **+0.08pp**, 95% CI **[−0.72, +0.88]**. p = 0.84.

Sample ratio check: 50.1 / 49.9, passes. Pre-exposure metrics balanced.

Analysis population: all assigned users. Unit of analysis: user. Randomization unit: user.

## Run 2 — the re-run

The team re-ran at 4× the sample, on the assumption that run 1 was underpowered.

| | Control | Variant |
|---|---|---|
| Assigned | 744,000 | 744,000 |
| 30-day repeat purchase rate | 22.38% | 22.46% |

Difference: **+0.08pp**, 95% CI **[−0.32, +0.48]**. p = 0.70.

**Nothing about the variant changed between runs.** No design change, no copy change, no eligibility
change. Same 30-day measurement window. No concurrent release on the surface. No seasonality shift —
both runs sat inside the same quarter, away from promotional periods.

Sample ratio check for run 2: **not supplied.** Run 2 used the same assignment service with no
configuration change.

## Decision threshold

The team states: "We'd have rolled it out at **+0.50pp** on 30-day repeat purchase. That's the bar
finance set for the engineering cost."

## Exposure

Impressions on the redesigned flow were **not logged.** Exposure cannot be measured directly.

## Affected population

Users who initiated a return during the window: **4.1% of assigned.**

**Readout on return-initiators only** (supplied by the team's analyst, computed on the run-2
population):

| | Control | Variant |
|---|---|---|
| Return-completion rate | 61.2% | 74.8% |
| 30-day repeat purchase rate | 38.1% | 46.4% |

Repeat-purchase difference among return-initiators: **+8.3pp**, 95% CI **[+4.9, +11.7]**.

## Prior tests on this surface

> **`Assigned` is each test's total across both arms.** Each row is one test, and the figure is a property of the test. To compare the subject against this set, sum the subject's arms first. See `reference/prior-test-tables.md`.

All on the returns flow, same randomization unit, same eligibility definition, comparable traffic
periods. All read on **return-completion rate**.

| Test | Assigned | Outcome |
|---|---|---|
| FAQ link placement | 240,000 | **detected, +2.1pp** |
| Return-label email timing | 310,000 | **detected, +4.4pp** |
| Copy simplification | 198,000 | flat |
| Reason-dropdown order | 420,000 | flat |
| Refund status page | 275,000 | **detected, +1.8pp** |
| Entry point in account nav | 355,000 | flat |

No prior test on this surface has read on 30-day repeat purchase rate.

## Concurrent experiments

None on the returns flow during either window.

## Pre-registration

The analysis plan for both runs specified 30-day repeat purchase as primary. No segments were
pre-registered.

## What the team is asking

"We ran it twice, second time at four times the traffic, and got the same nothing both times. The
interval is tight now. I think that's conclusive — the redesign doesn't move repeat purchase and we
should stop investing in returns UX. Can you confirm before I write it up?"

## Turn 2 — assistant

## Failure observed

The team's question was whether the redesigned returns-initiation flow lifts 30-day repeat purchase enough to justify the engineering cost. Stated decision threshold: **+0.50pp**.

Run 1 (372,000 assigned in total): +0.08pp, 95% CI **[−0.72, +0.88]**. The interval contains the threshold, so run 1 did not answer the question.

Run 2 (1,488,000 assigned in total): +0.08pp, 95% CI **[−0.32, +0.48]**. This interval excludes the threshold, and the half-width fell from 0.80pp to 0.40pp — exactly what a 4× sample increase predicts.

The failure is not that both results were flat. It is that the primary metric could not have registered a decision-relevant effect at any sample this test could have run at. See the mechanism.

## Comparison set

Six prior tests on the returns flow, same randomization unit, same eligibility definition, comparable traffic periods. All six read on **return-completion rate**.

| Prior test | Assigned (test total) | Outcome |
|---|---|---|
| FAQ link placement | 240,000 | detected, +2.1pp |
| Return-label email timing | 310,000 | detected, +4.4pp |
| Copy simplification | 198,000 | flat |
| Reason-dropdown order | 420,000 | flat |
| Refund status page | 275,000 | detected, +1.8pp |
| Entry point in account nav | 355,000 | flat |

Detection floor: this surface has resolved effects down to **+1.8pp on return-completion at 275,000 assigned**. The subject's run-2 total of 1,488,000 sits well above every sample in the set.

## Comparison-set integrity

**Usable with limitations.** Six prior tests, three with detected effects, matched on surface, randomization unit, population definition, and traffic period. No single test carries the floor: removing the refund-status page still leaves +2.1pp and +4.4pp resolved.

The axis that fails is **metric family**, and it does two things at once.

It is the departure the diagnosis rests on. The subject matches six prior tests on surface, unit, and eligibility, and differs on exactly one axis — the primary metric — and that axis carries the finding.

It is also a real gap in what the set can baseline. A set reading entirely on return-completion rate cannot say what sample this surface needs to resolve a **repeat-purchase** effect. The floor quoted above is a floor for a different metric. Per the Step 0.5 rung, confidence caps at **Provisional**. The finding below does not depend on that floor — it rests on the 4.1% denominator share and the affected-population interval, both measured — but the survival of the finding does not lift the grade.

## Funnel reconstruction

| Stage | Subject | Basis | Read |
|---|---|---|---|
| 1. Delivery | 50.1 / 49.9, pre-exposure balanced (run 1) | Ratio check supplied for run 1 | Clean |
| 2. Exposure | Eliminated structurally | See below | Clean |
| 3. Sensitivity | Half-width 0.40pp at 1,488,000 total | Sample far above every prior test in the set; interval narrowed as the square-root relation predicts | Clean after run 2 |
| 4. Measurement | Affected population is 4.1% of the metric's denominator | Return-initiator share, supplied | **Break** |
| 5. Inference | Analysis on all assigned while the change reaches 4.1% | Readout definition | **Starved** |

**Stage 2 is eliminated structurally, not comparatively.** Impressions on the redesigned flow were not logged, so exposure cannot be measured directly. It does not need to be: return-completion rate among initiators moved from 61.2% to 74.8%, and that metric cannot move unless the flow was reached and used. A downstream metric that moved proves the upstream stage did not break and bounds exposure from below. Recorded as structural elimination — an unmeasured exposure rate is not a low one, and this is not a comparative reading against a normal rate for the position.

## Primary constraint

**Stage 4, measurement.**

## Primary cause

**Affected population is a small share of the denominator (taxonomy 4B).**

## Mechanism

The change is reachable only from order history, only for orders inside the returns window, and it acts only on users who initiate a return. Return-initiators are **4.1%** of the assigned population. The primary metric, site-wide 30-day repeat purchase, is computed across all assigned users, so any effect is divided by a denominator overwhelmingly composed of users the change cannot reach. The arithmetic closes the case: among return-initiators the change lifts 30-day repeat purchase by **+8.3pp, 95% CI [+4.9, +11.7]**. Carried into the site-wide metric at 4.1% of the denominator, that arrives at roughly **+0.34pp**, with even the interval's upper bound arriving at roughly **+0.48pp** — below the team's +0.50pp threshold. The largest effect compatible with the affected-population readout cannot reach the bar in the metric chosen to measure it, and that was true before a single user was assigned.

## Evidence for this cause and against the alternatives

The **affected-population readout is the discriminating evidence**, and it is the only thing separating this from a sensitivity break. A test underpowered for the effect and a test measuring a diluted effect both produce a wide interval on a sample that looks adequate by count; the sample size screens them in together and cannot rank them. What differs is what a readout on the users the change can reach shows. Here it shows +8.3pp with an interval nowhere near zero, alongside a return-completion movement from 61.2% to 74.8%. The change does something substantial to the population it can act on. The site-wide metric cannot see it.

This readout is admissible without pre-registration because **the change's mechanism predicts the population in advance** — a flow reachable only by users who initiate a return predicts its own population without anyone scanning for it. It is not a segment search, and no segment search was performed.

The comparison set points independently to the same place: every prior test on this surface that resolved an effect read on return-completion, at samples between 240,000 and 310,000. The subject departs on exactly one axis, and that axis carries the finding.

The p-values (0.84, 0.70) appear nowhere in this reasoning. They are predicted identically by an underpowered test and a genuinely null change, which are the two hypotheses at issue, so they discriminate nothing.

## Alternatives, and why they are demoted

**Sensitivity (Stage 3) — not supported, and the re-run settles it.** All four qualifying conditions hold: the re-run changed the thing under test (more traffic acts on sensitivity); the variant was otherwise unchanged, confirmed explicitly; the added sample was sufficient, halving the half-width from 0.80pp to 0.40pp exactly as the square-root relation predicts; and the period was comparable, with no concurrent release, no seasonality shift, and the same 30-day window. **This is a qualifying re-run, and its verdict is Negative.** A qualifying re-run produced no change in the site-wide readout. That is strong evidence against the specific mechanism the re-run was capable of testing — sensitivity — and against nothing else. It says nothing about the metric, because more traffic cannot act on a metric choice.

**The change does nothing (the null) — rejected on structural grounds.** See below.

**Exposure dilution (Stage 2) — not supported.** Eliminated structurally, as above.

**Delivery (Stage 1) — not supported.** Run 1's ratio check passes and pre-exposure metrics are balanced.

**Inference dilution (Stage 5) — downstream of the primary, not an independent cause.** The analysis population is the assigned population while the change reaches 4.1% of it, but the metric definition and the analysis population are the same decision here, and Stage 4 is the earlier break. Nothing independent is concluded at Stage 5.

**Offsetting effects (5C) — inadmissible.** No segments were pre-registered, and the mechanism predicts no split beyond the return-initiator population already read.

**"We ran it twice and got the same nothing."** The two runs are one qualifying re-run of a sensitivity hypothesis, not two independent verdicts on the change. A diluted measurement run twice is a diluted measurement.

## Null model

**Rejected**, and rejected structurally rather than evidentially. The null requires all five stages clean, and Stage 4 is not clean. Run 2's interval excludes the decision threshold, which in a clean funnel would point at the null — but a metric that could not have registered the effect at any sample cannot be used to conclude the effect is absent. This is a **Stage 4 break wearing a null's shape**: the interval is precise about a quantity nobody wanted measured, and a reading that stopped at the interval would deliver the opposite finding with high confidence.

## Confidence

**Provisional.** The funnel locates the constraint, the affected-population readout separates the two live causes at the break stage, and the null was tested and rejected. The cap comes from the comparison set: Usable with limitations, because no prior test on this surface has read on 30-day repeat purchase and the set therefore cannot baseline that metric.

Named as unexamined rather than ruled out: every test in the set shares the same instrumentation and the same primary metric family, so a systematic instrumentation bias or a metric insensitive by construction would not vary across the set and **this comparison set is structurally unable to detect either**.

## Missing evidence

**Sample-ratio check for run 2.** Not supplied; only run 1's. **Which way it pushes:** run 2 used the same assignment service with no configuration change, so a ratio failure would have to be newly introduced; and a ratio failure inflates variance and widens intervals, whereas run 2's interval narrowed by precisely the factor the sample increase predicts. The gap pushes **against** a delivery break rather than toward one, and the finding survives the worst case in which run 2's ratio was mildly off.

**Exposure instrumentation on the redesigned flow.** Absent; handled by structural elimination above, which bounds exposure from below but does not measure it.

**A prior test on this surface that resolved an effect on 30-day repeat purchase.** Its absence is what caps the integrity verdict. **Which way it pushes:** the finding rests on the 4.1% denominator share and the affected-population interval, neither of which needs the return-completion floor, so the gap does not manufacture this finding — it limits how much the set can corroborate it.

Locale, device, and tenure splits were not supplied and are not requested. None was pre-registered, the mechanism predicts none, and scanning them after a flat headline would manufacture a result.

## What would prove this wrong

A return-initiator share materially above 4.1%. The arithmetic that closes this case is a ratio; if the change reaches a third of the denominator rather than a twenty-fifth, it reopens.

An affected-population readout whose interval includes zero, or one computed on a population that does not match the users the flow can reach. Either would remove the evidence that the change works where it acts and move the finding toward the null.

Evidence that the return-completion movement from 61.2% to 74.8% was produced by something other than the redesign — a concurrent change, a metric redefinition, or a population shift between runs. That would break the structural elimination at Stage 2 and put exposure back in play as live and untested.
