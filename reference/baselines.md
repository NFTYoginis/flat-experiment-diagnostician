# Baselines

The comparison set is what turns an opinion into a diagnosis. This file covers how to build it,
how to establish the two numbers everything else is read against, and how to read a re-run.

There are no benchmark numbers here on purpose. Baseline conversion rates, metric variance,
traffic volume, normal exposure rates, and what counts as an adequate sample vary enormously by
surface, product, market, and season. **There is no such thing as a general minimum detectable
effect**, and any figure quoted as an industry standard — 80% power, a 2% MDE, two weeks of
runtime — would be wrong for most surfaces while being treated as authoritative. Every baseline
in a diagnosis is computed from the prior tests supplied with the case.

---

## The comparison set is this surface's own history

Prior tests on the same surface, with known outcomes. Within-case, already run, and requiring no
argument about whether it is really comparable — it is the same surface.

Match on:

- **Surface** — the same page, module, or flow. Different surfaces have different traffic,
  different variance, and different floors.
- **Metric family** — read on the same primary metric, or one constructed the same way.
- **Randomization unit** — user, session, or visit. Mixing units makes variance incomparable and
  is the most common silent error in assembling this set.
- **Population and eligibility definition** — the same rules for who enters.
- **Traffic period** — comparable volume and seasonality.

Four is the floor. Six to ten is a usable set. Below four, state that the floor is not
established and cap confidence at Provisional.

### The requirement that is not negotiable

**The set must contain at least one prior test that detected an effect.**

A set of flat tests establishes nothing. It cannot distinguish a surface that genuinely never
moves from a testing setup that never resolves anything, and those two produce identical
histories. Without a detected effect somewhere in the record, there is no evidence this surface
*can* produce a resolvable result, and "flat" is uninterpretable.

Where the history is entirely flat, the correct output is **Undetermined**, and the missing input
is a resolved test on this surface at a known sample.

---

## The two numbers everything is read against

Both come from the case. Neither is computed from a formula and neither is a constant.

### 1. The decision threshold

**The smallest effect that would have changed what the team did.** It comes from the team, not
from you, and it is not a statistical quantity — it is a statement about what the result was for.

A team that would have shipped on a 0.5pp lift and a team that needed 3pp to justify the
maintenance cost ran the same experiment and asked different questions of it. The same interval
answers one and not the other.

If no threshold was supplied, say so and name it as a blocking input. Without it, an interval
cannot be called wide or narrow, because there is nothing for it to be wide or narrow *relative
to*. Do not substitute a conventional figure. Do not infer one from the effect sizes in the
comparison set — those describe what this surface has produced, not what this team would act on.

### 2. The detection floor

**The smallest effect prior tests on this surface actually resolved, and at what sample.**

Read it off the comparison set: for each prior test that detected an effect, record the effect
size and the achieved sample. The floor is the boundary those points describe — the region in
which this surface, with this instrumentation and this variance, has demonstrably produced
resolvable results.

This is an empirical property of the surface, not a calculation. It already accounts for the
metric's real variance, the population's real heterogeneity, and the instrumentation's real
noise, none of which a power formula knows about.

Where prior tests resolved effects at samples comparable to the subject's, the subject was
plausibly powered. Where every resolved test used several times the subject's sample, it was not.

---

## Establishing whether the test answered its question

Put the two numbers together with the reported interval.

- **Interval excludes the decision threshold** → the test answered. Any real effect is smaller
  than the team would act on. Go to the null.
- **Interval contains the decision threshold** → the test did not answer. A decision-relevant
  effect is compatible with this result, and something is constraining detection.

That is the whole reading, and it does not involve the p-value.

### The count that is not a discriminator

An underpowered test and a genuinely null change **both** produce p > 0.05, an interval
containing zero, and a point estimate near zero. Every one of those is predicted identically by
the two hypotheses this folder exists to separate. **A count both hypotheses predict is not a
discriminator**, and the p-value is the count the entire room is looking at.

What differs is the interval's width relative to the decision threshold. A test can be "not
significant" and conclusive — the interval rules out everything worth acting on. A test can be
"not significant" and completely uninformative — the interval spans effects that would have
changed the roadmap. The p-value is the same in both cases and they should lead to opposite
decisions.

The same trap appears one level down, between sensitivity and measurement dilution. Both produce
a wide interval on a sample that looks adequate by count, so the sample size screens them in
together. **The affected-population readout is what separates them**: a metric computed on the
users the change can actually reach either shows a clear effect, in which case the constraint is
measurement, or it does not, in which case it is not dilution.

---

## Per-stage baselines

Compute the comparison set's figures for each of the five canonical stages and compare:
**Delivery, Exposure, Sensitivity, Measurement, Inference.**

**Delivery.** The assignment ratio as designed against as observed, and pre-exposure balance
between arms. Prior tests establish what a normal ratio check looks like on this surface,
including its usual bot and internal-traffic share.

**Exposure.** Exposed users as a share of assigned, against the surface's normal rate **for a
change in that position**. Above-the-fold and below-the-fold elements on the same page have very
different normal rates, and comparing across positions manufactures a finding. Where prior tests
changed elements in the same position, that is the comparison.

**Sensitivity.** Achieved sample against the detection floor, and interval width against the
decision threshold. Never against a conventional power target.

**Measurement.** Whether the primary metric is the one prior resolving tests on this surface used,
and what share of its denominator the affected population represents.

**Inference.** Whether the analysis population matches the exposed population, and whether the
unit of analysis matches the randomization unit — checked against how prior tests on this surface
were analysed.

---

## What counts as "materially below"

Judgment, stated explicitly in the report rather than applied silently.

- **Clearly below** — the subject is under the lowest comparable figure in the set on that stage.
  Treat as a located break.
- **Ambiguous** — inside the range but in the bottom quarter. Note it, do not treat it as the
  break unless a later stage is clearly below.
- **At baseline** — inside the range. This stage is not the break. Move to the next stage.

**"Not instrumented" is a fourth reading and not a synonym for any of the three.** An unmeasured
exposure rate is not a low exposure rate. Handle it by structural elimination where a downstream
metric moved, and otherwise report the stage as live and untested.

If no stage is below par, the funnel is clean and the null is likely the answer. Do not
manufacture a break by lowering the bar until one appears — that pressure is strongest here,
because the null is the finding nobody wants.

---

## What the comparison set cannot see

Matching is what makes a comparison work and it is also what makes a comparison blind. Every axis
held constant becomes invisible.

Here the surface is held constant, which is the point — and it means **the set can tell you
nothing about whether the change would have worked on a different surface.** A flat result on a
low-traffic settings page and the same change on the homepage are different experiments, and this
comparison set cannot speak to the second.

It is equally blind to anything shared by every test in it. If every prior test used the same
instrumentation, a systematic instrumentation bias does not vary across the set and cannot be
detected by it. If every prior test read the same primary metric, a metric that is insensitive by
construction will look normal, because it is the only thing the set has ever measured.

Name the matched axis in the report as a live alternative that this comparison set is structurally
unable to test, and name what would test it — usually a resolved test on the same surface using a
different metric, or an A/A test against the instrumentation. Do not report it as ruled out. A
variable held constant has not been examined.

---

## The qualifying re-run test

The most useful item in any file where the test was run twice, and the easiest to over-read.

A re-run is a completed experiment about the experiment. But it only tests what it was capable of
testing, and most re-runs are not capable of testing what the team believes they tested.

### Step 1: does the re-run qualify

All four must hold before the result means anything.

| Condition | Why it matters | Fails when |
|---|---|---|
| **Changed the thing under test** | A re-run only tests the mechanism it acted on. Traffic acts on sensitivity, instrumentation on delivery, a new primary metric on measurement | More traffic offered as evidence against a wrong metric or an exposure gap. A diluted test run twice is a diluted test |
| **Variant otherwise unchanged** | A re-run of a revised design is a different experiment | The team "fixed" the variant while adding traffic. The two runs cannot be pooled or compared |
| **Added sample sufficient to cross the floor** | Precision improves with the square root of sample | The team doubled traffic. That narrows the interval by about 29%, not 50%. **Halving an interval takes roughly four times the sample**, and a test 12× under the floor re-run at 2.4× is still 5× under it |
| **Comparable period** | The comparison must survive | Seasonality shift, concurrent release on the surface, or a metric redefinition between runs |

If any condition fails, the re-run is **Uninformative**. Say which condition failed and do not use
it as evidence in either direction. An experiment that did not run is not weak evidence for the
null. It is no evidence.

### Step 2: read a qualifying re-run

- **The readout moved** — the mechanism the re-run acted on was a real constraint. Record as
  **Positive**.
- **The interval narrowed and still contains the threshold** — the re-run worked mechanically and
  did not resolve the question. Two findings, not one, and the second is the arithmetic: state what
  the narrowing implies about what a further run at the same growth rate would achieve, without
  prescribing one.
- **The interval narrowed past the threshold and the result held** — strong evidence against **the
  specific mechanism the re-run was capable of testing**, and frequently the point at which the
  null becomes reachable. Record as **Negative**.

**Negative and Uninformative are different verdicts and must be labelled differently.** One means
the test ran and the hypothesis lost. The other means no test occurred. Teams read a second flat
result as proof the change does nothing; that reading is available, and so is the opposite, and
which is correct depends entirely on whether the re-run crossed the floor.

### The arithmetic that catches people

Precision scales with the square root of sample size. The consequences are unintuitive and they
are where this test earns its place:

- Doubling the sample narrows the interval by about 29%
- Halving the interval requires about 4× the sample
- Detecting an effect half the size requires about 4× the sample

A team that doubled traffic expecting to double resolution has not tested what they think they
tested, and a re-run that fell short of the floor by any margin tested the hypothesis exactly as
much as the first run did, which is to say not at all.
