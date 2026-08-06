# Failure modes

Organized by funnel stage, because the stage where the experiment breaks determines which causes
are even eligible. Each entry gives the signature that distinguishes it, and the
counter-signature that rules it out.

---

Canonical funnel, used throughout: **1. Delivery · 2. Exposure · 3. Sensitivity ·
4. Measurement · 5. Inference.** Build failures, early stopping, concurrent experiments, and
broken metrics are outside the funnel.

**A break at any stage makes the null unreachable.** "The change does nothing" is the conjunction
of five clean stages, not the residual when nothing else is found.

---

## Stage 1 — Break at delivery

The variant was not served as assigned. Nothing downstream means anything: the experiment that
ran is not the experiment that was designed.

### 1A. Sample ratio mismatch

- **Signature:** observed assignment split departs from the designed split by more than chance.
  Frequently accompanied by a difference in pre-exposure characteristics between arms, which
  randomisation should have removed.
- **Rules it out:** the ratio check passes and pre-exposure metrics are balanced.
- **Note:** cheap to check and it invalidates everything after it. Check it before anything
  interpretive. A test with a ratio mismatch has not produced a comparison.

### 1B. Flag or targeting misconfiguration

- **Signature:** the variant condition evaluates incorrectly for a subset — a plan tier, a
  locale, a client version, a logged-out state. Assignment logs and rendering logs disagree.
- **Rules it out:** a flag audit showing the condition evaluating as designed across segments.

### 1C. Cached or pre-rendered control

- **Signature:** users assigned to the variant received the control experience from a cache, an
  edge render, or a service worker. Effect concentrated in returning users, absent in new ones.
- **Rules it out:** cache-busting confirmed, or the effect shape does not sort on visit history.

### 1D. Bot or internal traffic

- **Signature:** unfiltered automated traffic inflates both arms identically, adding denominator
  without behaviour and compressing any real difference toward zero.
- **Rules it out:** documented bot filtering, and traffic shape consistent with the surface's
  history.

### 1E. Assignment before eligibility

- **Signature:** users were randomised before it was known whether they would encounter the
  surface, so the assigned population is far larger than the eligible one.
- **Rules it out:** assignment triggered at the point of eligibility.
- **Note:** this produces dilution identical in shape to a Stage 2 exposure break, and the two
  are separated by *where* the loss occurs — before eligibility is a delivery design choice,
  after it is an exposure fact. Both are real; only one is fixed by moving the element.

---

## Stage 2 — Break at exposure

Delivered, and not seen. The effective sample is not the assigned sample, and every downstream
figure computed on the assigned population is diluted by a factor nobody has stated.

### 2A. Position dilution

- **Signature:** the changed element sits below the fold, behind a click, in a collapsed section,
  or in a tab. Exposure rate far below the surface's normal rate for a change in that position.
  The assigned-population interval is wide and an exposure-triggered readout, where one exists, is
  materially different.
- **Rules it out:** exposure rate at or above the surface's normal rate.

### 2B. Path dilution

- **Signature:** the change sits on a path most assigned users never take — a secondary flow, an
  error state, an edge-case branch. Exposure is low because the surface is rarely reached, not
  because the element is hard to see.
- **Rules it out:** the affected path carries a normal share of assigned traffic.

### 2C. Exposure not instrumented

- **Signature:** no impression logging for the changed element. Exposure cannot be measured
  directly.
- **Handling:** this is not automatically a break. Use structural elimination — any metric that
  moved and could only move if the element was seen proves exposure occurred and bounds it from
  below. If nothing moved anywhere, the stage stays live and untested, and that must be stated
  rather than assumed either way.

### 2D. Exposure asymmetry between arms

- **Signature:** the variant is seen at a different rate than the control, because the change
  itself altered how often the element renders. The comparison is then between different
  populations.
- **Rules it out:** exposure rates comparable across arms.
- **Note:** this is a real finding and not merely a nuisance — a change that alters its own
  exposure rate has an effect, and the flat primary metric may be hiding it.

---

## Stage 3 — Break at sensitivity

Seen, and the design could not resolve an effect of the size that matters.

The whole stage is read against the **decision threshold** and the **surface's detection floor**.
Neither is a statistical constant; both come from the case. See `baselines.md`.

### 3A. Sample below the surface's floor

- **Signature:** achieved sample materially below the smallest sample at which a prior test on
  this surface resolved an effect. Interval contains the decision threshold.
- **Rules it out:** achieved sample at or above the floor, and interval excluding the threshold.

### 3B. Metric variance too high for this sample

- **Signature:** the primary metric is high-variance — revenue per user, session duration,
  count data with a long tail — and the interval is wide despite an adequate sample by count. Prior
  tests on this surface that detected effects used a lower-variance metric.
- **Rules it out:** variance comparable to prior tests that resolved.

### 3C. Runtime short of a behavioural cycle

- **Signature:** the test ran for a period shorter than the surface's natural repeat cycle —
  weekly for a weekday-skewed surface, monthly for a billing surface — so a portion of the
  behaviour the change targets never had the opportunity to occur.
- **Rules it out:** runtime covering at least one full cycle, matching prior tests on the surface.

### 3D. Triggered analysis shrank the population

- **Signature:** the analysis was restricted to users who took some action, and that restriction
  cut the sample below the floor. Frequently a well-intentioned fix for dilution that trades one
  problem for another.
- **Rules it out:** the triggered population still clears the floor.

---

## Stage 4 — Break at measurement

Powered, and pointed at the wrong thing. The test could have detected something; it was not
looking where the change acts.

### 4A. Metric downstream of the mechanism

- **Signature:** the primary metric sits several steps beyond what the change touches, so any
  effect is attenuated at each step. A secondary metric closer to the mechanism moved clearly
  while the primary did not.
- **Rules it out:** no closer metric moved either.

### 4B. Affected population is a small share of the denominator

- **Signature:** the change acts on a subset — one flow, one state, one entry path — while the
  primary metric is computed over the whole surface. A real effect on the subset is divided by a
  denominator dominated by users the change cannot reach.
- **Rules it out:** the affected population is most of the denominator.
- **Note:** this is arithmetic, not statistics, and it is worth stating as arithmetic. An effect
  on 4% of the denominator arrives in the primary metric at roughly 4% of its size.
- **Does NOT separate it from 3A:** interval width, or the p-value, or the achieved sample. A
  test underpowered for the effect and a test measuring a diluted effect both produce a wide
  interval on an adequate-looking sample, and the count of users screens them in together.
  **The affected-population readout decides.** If a metric computed on the users the change can
  actually reach shows a clear effect, the constraint is measurement and more traffic will not
  fix it. If that readout is flat too, the constraint is not dilution.

> **General form of that rule.** When two causes in the same stage produce the same count on the
> same measure, that count screens them in together and cannot rank them. Find the property that
> differs between the two and read that instead. A count that both hypotheses predict is not a
> discriminator.

### 4C. Measurement window closes before the behaviour

- **Signature:** the metric is attributed within a window shorter than the decision cycle it
  measures — a purchase metric on a considered purchase, a retention metric read at seven days for
  a monthly product.
- **Rules it out:** window covering the behaviour's natural latency, matching prior tests.

### 4D. Metric insensitive by construction

- **Signature:** the primary metric is bounded, heavily floored or ceilinged, or already near
  saturation, so the change has little room to move it.
- **Rules it out:** the metric has demonstrated movement on this surface before.

---

## Stage 5 — Break at inference

The effect is in the data and the readout does not show it.

### 5A. Dilution by unexposed users in the analysis

- **Signature:** the analysis population is the assigned population rather than the exposed one,
  and exposure was materially below 100%. The exposure-triggered readout differs from the
  assigned readout.
- **Rules it out:** analysis restricted to exposed users, or exposure near total.

### 5B. Wrong unit of analysis

- **Signature:** randomisation at one unit and analysis at another — assigned by user, analysed by
  session — which mis-states the variance and usually narrows intervals rather than widening them.
- **Rules it out:** unit of analysis matching the randomisation unit.

### 5C. Offsetting effects

- **Signature:** two pre-registered populations, or two components of a composite metric, moved in
  opposite directions and the aggregate is flat. Legitimate only where the split was pre-specified
  or predicted by the change's mechanism in advance.
- **Rules it out:** no pre-specified split, in which case this cause is **not admissible** — see
  the note below.
- **Note:** this is the entry in the taxonomy most likely to be abused, and it is the reason the
  segment prohibition exists. A pre-registered split that offsets is a finding. A split discovered
  by scanning after a flat headline is an artifact, and reporting it is worse than reporting
  nothing because it arrives carrying a number. **Admissible only if pre-registered, or if the
  change's mechanism predicts the split in advance** — a checkout change that can only affect users
  who reach checkout predicts its own population without anyone scanning for it.

### 5D. Readout is an interim look

- **Signature:** the result reported is from a peek before the planned stopping point, or the test
  was stopped early on a flat interim.
- **Rules it out:** the readout is at the pre-planned stopping point.
- **Note:** frequently belongs outside the funnel. If the stop was a decision rather than an
  analysis artifact, route it out.

---

## Outside the funnel — Not an experiment failure

- **The variant was not built as specified.** The implementation departs from the design
  document. The test is of something else, and diagnosing the readout answers the wrong question.
- **Early stopping as a decision.** Someone chose to end it. That is a process finding.
- **Concurrent experiment on the same surface** changing the control experience mid-run.
- **Metric broken or redefined during the run**, making the two halves incomparable.

Say plainly that the constraint sits outside the experiment, and stop.

---

## The null: the change does nothing

The test was run correctly, and the change does not produce an effect the team would act on.

- **Signature:** delivery clean with the assignment ratio as designed; exposure at or above the
  surface's normal rate; achieved sample clearing the surface's detection floor established from
  prior tests that detected effects; the primary metric downstream of the mechanism on the
  affected population; analysis population matching the exposed population; **and the confidence
  interval excluding the decision threshold.**
- **Rules it out:** any stage below par, or an interval containing an effect the team would have
  acted on.

The last condition is load-bearing. The rest establish that the test was capable of answering; that
one is the answer.

This is the most common correct finding in this domain and the one hardest to deliver, because
somebody spent a quarter building the thing. It is not reached by failing to find something else.
It is reached by clearing every stage on evidence.
