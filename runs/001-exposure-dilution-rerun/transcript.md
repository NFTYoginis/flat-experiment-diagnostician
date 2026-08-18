# Run 001-exposure-dilution-rerun — transcript

Verbatim. The assistant turns are exactly what came back; nothing was edited,
trimmed, retried, or selected from more than one attempt.

## Metadata

- model: `claude-opus-5`
- effort: `high`, adaptive thinking (Claude Opus 5 default)
- session: fresh, single-shot. The system prompt was the `product/` folder and
  nothing else — no answer key, no `tests/`, no `runs/`, no prior conversation.
- case input: `tests/case-01-exposure-dilution/inputs.md`
- started: 2026-08-18T14:04:33+00:00
- ended: 2026-08-18T14:05:49+00:00
- usage: turn 1 in=1112 cache_read=0 out=6171

## Turn 1 — user

Diagnose this flat test.

# Case 01 — inputs

Constructed case. Paste everything below into a project running the diagnostician, then say
"Diagnose this flat test."

---

## The change

A new recommendation algorithm powering the "You might also like" module on the homepage. The
module's position, size, and styling are unchanged; only the ranking logic differs.

## The readout

> **`Assigned` is per arm.** The arms are the columns. The test's total is the sum across them. See `reference/prior-test-tables.md`.

| | Control | Variant |
|---|---|---|
| Assigned | 120,000 | 120,000 |
| Click-through to product page | 3.11% | 3.19% |

Difference: **+0.08pp**, 95% CI **[−0.21, +0.37]**. p = 0.59.

Sample ratio check: 50.0 / 50.0, passes. Pre-exposure metrics (sessions per user, prior 30-day
purchase rate) balanced across arms.

Analysis population: all assigned users. Unit of analysis: user. Randomization unit: user.

Runtime: 21 days. The homepage's behavioural cycle is weekly.

## Exposure

Module impressions were logged.

- Users who rendered the module: **21,840 of 240,000 assigned (9.1%)**
- The module renders at 68% page depth
- Median scroll depth on this homepage: 41%

**No exposure-triggered readout was produced.** The analysis was run on the assigned population
only.

## Decision threshold

The team states: "We'd have shipped it at **+0.25pp** on click-through. Below that the ranking
service's maintenance cost isn't worth it."

## Prior tests on this surface

> **`Assigned` is each test's total across both arms.** Each row is one test, and the figure is a property of the test. To compare the subject against this set, sum the subject's arms first. See `reference/prior-test-tables.md`.

All on the homepage, same randomization unit, same eligibility definition, comparable traffic
periods, all read on click-through to product page.

| Test | Assigned | Exposure | Element position | Outcome |
|---|---|---|---|---|
| Hero image treatment | 180,000 | 94% | above fold | **detected, +0.41pp** |
| Nav category order | 220,000 | 96% | above fold | flat |
| Hero copy | 195,000 | 93% | above fold | **detected, +0.33pp** |
| Search bar prominence | 260,000 | 88% | above fold | flat |
| Banner removal | 210,000 | 95% | above fold | flat |

Every prior test changed an element above the fold. No prior test on this surface has changed a
below-fold element.

## Secondary metrics

- Module click rate (among users who rendered it): not computed
- Time on page: control 47.2s, variant 47.6s, difference not significant
- Add-to-cart rate: control 1.84%, variant 1.86%, difference not significant

## Concurrent experiments

None on the homepage during the window.

## Prior runs of this test

None. This is the first run.

## What the team is asking

"Not significant, but the confidence interval is pretty wide and the point estimate is positive. I
think we just need more traffic — can you confirm we were underpowered so I can ask for a longer
run?"

## Turn 2 — assistant

## Failure observed

The team's question was whether the new ranking logic lifts click-through to product pages enough to justify maintaining the ranking service. Stated decision threshold: **+0.25pp**.

Result: +0.08pp, 95% CI **[−0.21, +0.37]**, on 240,000 assigned (120,000 per arm).

The interval **contains** the decision threshold. An effect the team would have shipped on is fully compatible with this result. The failure to diagnose is therefore not "the result was flat" — it is that the test did not answer the question it was run to answer.

The p-value of 0.59 plays no part in this reading and none in what follows.

## Comparison set

Five prior tests on the homepage, same randomization unit, same eligibility definition, comparable traffic periods, all read on click-through to product page. Figures are each test's total across arms.

| Prior test | Assigned | Exposure | Position | Outcome |
|---|---|---|---|---|
| Hero image treatment | 180,000 | 94% | above fold | detected, +0.41pp |
| Nav category order | 220,000 | 96% | above fold | flat |
| Hero copy | 195,000 | 93% | above fold | detected, +0.33pp |
| Search bar prominence | 260,000 | 88% | above fold | flat |
| Banner removal | 210,000 | 95% | above fold | flat |

Detection floor: this surface has resolved an effect as small as **+0.33pp at 195,000 assigned with 93% exposure**, an effective exposed sample of roughly **181,000**. The second resolving test gives +0.41pp at 180,000 with 94%, roughly 169,000 exposed.

Subject total: 240,000 assigned, 21,840 exposed.

## Comparison-set integrity

**Usable with limitations.**

Met: five prior tests; two with detected effects; same surface, same metric, same randomization unit, same eligibility definition, comparable traffic period; metric definition unchanged across the set; no concurrent experiment on the homepage during the subject's window.

No single prior test carries the floor. Removing the hero-copy test leaves +0.41pp resolved at 180,000 with 94% exposure — an effective exposed sample of about 169,000, still roughly 7.7× the subject's. The conclusion survives the removal.

The axis that fails: **every prior test changed an above-the-fold element**, with exposure between 88% and 96%. The subject changes a module at 68% page depth. The set therefore does establish a detection floor on this surface, and it **cannot establish what exposure rate is normal for a below-fold position**, because that axis does not vary anywhere in the set. That weakens any claim about whether 9.1% is anomalous. It does not weaken the arithmetic of what 9.1% does to the readout.

Confidence caps at Provisional.

## Funnel reconstruction

| Stage | Subject | Basis | Read |
|---|---|---|---|
| 1. Delivery | 50.0 / 50.0; pre-exposure metrics (sessions per user, prior 30-day purchase rate) balanced | Ratio check supplied, passes | Clean |
| 2. Exposure | 21,840 of 240,000 assigned rendered the module — 9.1% | Module impression logs, supplied | **Break** |
| 3. Sensitivity | Half-width 0.29pp on the assigned population; runtime 21 days against a weekly cycle | Reported interval | Starved |
| 4. Measurement | Not assessed | — | Starved |
| 5. Inference | Not assessed | — | Starved |

Pass-through, assigned to exposed: **9.1%**. Effective exposed sample 21,840, against a floor of roughly 181,000 exposed — **8.3× below** the smallest effective sample at which this surface has resolved an effect.

Exposure here is **measured, not eliminated structurally**. Impression logging exists, so no downstream-metric argument is needed or used. The two secondary metrics supplied (time on page, add-to-cart) moved by amounts described as not significant, so they would not have supported a structural elimination in any case.

Nothing outside the funnel applies: no concurrent experiment, no interim look, no metric redefinition, and the module was built as specified (position, size, and styling unchanged by design).

## Primary constraint

**Stage 2, exposure.** 240,000 users were assigned; 21,840 ever rendered the module.

Stages 3, 4, and 5 are starved and no conclusion is drawn from any of them. That is the load-bearing exclusion in this case: the reported interval's width is a *consequence* of the exposure rate, not independent evidence about this surface's sensitivity, and reading it as a sensitivity finding is precisely the reading the evidence does not support. Nothing here concludes anything about whether the new ranking algorithm works.

## Primary cause

**Position dilution** (failure mode 2A).

## Mechanism

The module renders at 68% page depth on a homepage whose median scroll depth is 41%, so roughly one assigned user in eleven ever renders it. The readout averages any effect on 21,840 exposed users across 240,000 assigned, compressing it by approximately elevenfold. An effect of +0.9pp on the exposed population — more than twice the largest effect this surface has ever resolved — would arrive in the assigned-population readout at about +0.08pp, which is exactly the point estimate observed and sits comfortably inside the reported interval. So would an effect of zero. The readout cannot separate those two, and its width is set by the exposed sample, which is 8.3× below the smallest effective sample at which this surface has resolved anything.

## Evidence for this cause and against the alternatives

**The pass-through is the evidence and the level is the trap.** At 240,000 assigned the subject is larger than four of the five prior tests, two of which resolved effects of +0.33pp and +0.41pp. Read as a level, this test looks well powered, which is why "we just need more traffic" is the natural reading in the room. Read as pass-through, its effective sample is 21,840 against roughly 181,000. Levels say the test was big; pass-through says whether it was big where it mattered.

**The exposure rate is measured, not inferred.** Impression logs give 9.1% directly, and the page-geometry figures (68% render depth against 41% median scroll) supply an independent mechanism for that number rather than leaving it as a bare count.

**The dilution arithmetic reproduces the observed point estimate.** A strong effect on the exposed population predicts almost exactly +0.08pp on the assigned population — and so does no effect at all. That correspondence is the check that this is dilution rather than coincidence, and it is simultaneously the reason nothing downstream can be read.

**The p-value discriminates nothing.** An underpowered-or-diluted test and a genuinely null change both produce p = 0.59, an interval containing zero, and a point estimate near zero. Every one of those is predicted identically by the two hypotheses at issue. What separates them is width against the decision threshold, and this interval spans it.

**Delivery is ruled out on supplied evidence**, not assumed: the ratio check passes at 50.0/50.0 and two pre-exposure metrics are balanced across arms.

## Alternatives, and why they are demoted

**Sensitivity (Stage 3) — downstream of the primary, not an independent cause.** This is the team's hypothesis and it is the one the evidence demotes. The interval is wide because the exposed sample is small, and the exposed sample is small because of the module's position. "The interval was too wide" and "position dilution" are not competing causes; one is the other's consequence. Runtime is not a live sub-branch either: 21 days covers three full weekly cycles on a surface whose behavioural cycle is weekly.

**Path dilution (2B) — not supported.** Exposure loss here is scroll-based, not path-based. The five prior tests establish that assigned users reach this page at rates of 88–96%; the surface is reached, the module simply sits below where most users stop.

**Assignment before eligibility (1E) — not supported.** This produces dilution identical in shape to a Stage 2 break, and the two separate on *where* the loss occurs. Prior above-fold tests on the same page and the same eligibility definition achieved 88–96% exposure, which places the assigned population squarely on the homepage. The loss therefore occurs after eligibility, at scroll, which makes it an exposure fact rather than a delivery design choice.

**Exposure asymmetry between arms (2D) — not supported, and not fully tested.** The change alters ranking logic only; position, size, and styling are unchanged, so the variant has no route to altering its own render rate. Only a pooled exposure figure was supplied, so this is argued from the change's mechanism rather than from per-arm counts. Noted below as missing.

**Measurement (Stage 4) — starved and untested.** Click-through to product page is the metric every prior test on this surface used, including both that resolved, so the comparison set gives no reason to suspect it. That is an absence of suspicion, not a finding, and nothing here tests it.

**Inference dilution (Stage 5) — starved.** The analysis was run on all assigned users while 9.1% were exposed, which is the signature of 5A. It is not named as the primary because the earliest failing stage is the diagnosis site, and the dilution originates at exposure rather than at the analyst's choice of population. It is recorded as descending from the primary.

**Offsetting effects (5C) — inadmissible.** No segment was pre-registered, and the change's mechanism predicts no particular subpopulation.

**Prior runs.** None. This is the first run, so there is no completed experiment about the experiment to read, and no re-run verdict — Positive, Negative, or Uninformative — is available in either direction.

## Null model

**Not reachable. Reported as blocked, not rejected.**

The null is the conjunction of five clean stages, and Stage 2 is not clean. This case is the sharpest illustration of why the null cannot be reached by elimination: the observed readout is exactly what a large real effect looks like after elevenfold dilution. No claim is made here about whether the new ranking algorithm has an effect.

## Confidence

**Provisional.**

The constraint is located, the exposure rate is measured rather than inferred, and the dilution arithmetic reproduces the observed estimate. The cap comes from two places, both binding independently. The comparison set is Usable with limitations — it contains no below-fold comparable and therefore cannot establish the normal exposure rate for this position. And the null could not be tested, because the funnel breaks upstream of it.

## Missing evidence

**An exposure-triggered readout**, restricted to the 21,840 users who rendered the module. This is the single input that would say whether the ranking change does anything, and its absence is the reason this report concludes nothing about the change itself. Related and also absent: module click rate among users who rendered it, which the case file records as not computed.

**Per-arm exposure counts.** Only a pooled 9.1% was supplied, so exposure asymmetry between arms is argued from the change's mechanism rather than measured.

**A below-fold prior test on this surface**, which would say whether 9.1% is anomalous for this position or ordinary for it.

*Which way that gap pushes.* Toward framing, not toward the diagnosis. If 9.1% turns out to be the normal rate for a module at 68% page depth, the effective sample is still 8.3× below the floor and the constraint is still position dilution; what changes is only whether the position is a defect or a fact about the surface. **The conclusion survives that worst case.**

*Which way the missing exposure-triggered readout pushes.* Toward the finding, not against it, and that is why confidence does not rise above Provisional on the strength of the measured exposure rate. Its absence is what leaves the change's own effect entirely unexamined.

**Structurally invisible to this comparison set.** Every test in the set shares the same instrumentation, so a systematic instrumentation bias does not vary across the set and cannot be detected by it. No A/A test on this surface was supplied. Named as unexamined, not ruled out.

## What would prove this wrong

An **exposure-triggered readout on the 21,840 exposed users with a tight interval excluding +0.25pp**. That would mean the exposed population was sufficient after all, move the constraint off Stage 2, and make the null reachable.

An **audit of the module's impression instrumentation showing it undercounts renders** — a beacon firing on a narrower condition than "the module entered the viewport." The 9.1% figure is the whole of this diagnosis; if it is a logging artifact rather than a scroll fact, the break moves and the mechanism dissolves.

A **materially different median scroll depth or render depth** for the module than the figures supplied, which would remove the independent mechanism that currently corroborates the measured rate.
