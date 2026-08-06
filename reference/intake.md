# Intake

What a case needs before a diagnosis is possible, and what to do when it is missing.

---

## Required

Without all four, the funnel cannot be reconstructed and the correct output is **Undetermined**.

1. **The readout as produced** — point estimate and confidence interval on the primary metric,
   assignment counts by arm, and the analysis population definition. As reported, not
   recalculated.
2. **The decision threshold** — the smallest effect that would have changed what the team did.
   This comes from the team.
3. **A comparison set of four or more prior tests on the same surface**, with their outcomes,
   effect sizes, and achieved samples. **At least one must have detected an effect.**
4. **What the change actually does** — the mechanism, stated concretely enough to identify which
   users it can reach and which behaviour it can move.

The second and third are the ones teams most often skip, and they are the two the method depends
on entirely.

**Without a decision threshold, "flat" cannot be graded.** An interval is not wide or narrow in
the abstract; it is wide or narrow relative to what the team would have acted on. Two teams with
identical readouts and different thresholds got different answers from the same experiment. Do
not substitute a conventional figure and do not infer one from the comparison set — prior effect
sizes describe what this surface has produced, not what this team would act on.

**Without a prior test that detected an effect, the history establishes nothing.** A set of flat
tests cannot distinguish a surface that never moves from a setup that never resolves. The correct
output is Undetermined, and the missing input is a resolved test on this surface at a known
sample.

---

## Strongly wanted

Each of these decides a specific branch. Note in the report which were absent.

- **Sample ratio check** and pre-exposure balance between arms. The cheapest possible check and it
  invalidates everything downstream when it fails.
- **Exposure counts for the changed element** — impressions or views as a share of assigned. The
  input the dilution branch runs on.
- **An exposure-triggered readout** alongside the assigned-population one. The gap between them is
  the dilution, measured rather than argued.
- **A readout on the affected population** — the users the change can actually reach. This is what
  separates a sensitivity break from a measurement break, and nothing else does.
- **Secondary and guardrail metrics**, with their movements. Any metric that moved and could only
  move if the element was seen is proof of exposure when instrumentation is missing.
- **Runtime and the surface's behavioural cycle** — weekly, monthly, or seasonal.
- **Bot and internal traffic filtering**, as applied.
- **Prior runs of this same test**, with what changed between them. The input the qualifying
  re-run test runs on.
- **Pre-registration**, where it exists: the planned analysis, stopping point, and any
  pre-specified segments.

---

## Useful

- Flag or targeting configuration, and any audit of it
- Concurrent experiments on the same surface during the window
- Metric definition and any revision to it during the period
- A/A test results on this surface, which bound the instrumentation's noise
- The design document for the variant, to check the build matches it

---

## Handling missing evidence

**Never substitute inference for a missing input.** The failure mode this folder exists to prevent
is a confident cause built on absent data — and its close cousin is a null reached by elimination
rather than by evidence.

| Missing | Effect | What to write |
|---|---|---|
| Decision threshold | The interval cannot be graded | Undetermined. Name it as the blocking input. Do not substitute a convention. |
| Any prior test with a detected effect | No detection floor exists | Undetermined. The comparison set is Not usable, and the missing input is a resolved test on this surface. |
| Comparison set under 4 | Floor unreliable | Provisional at best. Say the set is too small to establish a floor. |
| Exposure counts | Stage 2 cannot be tested comparatively | Eliminate structurally if a metric moved that could only move on exposure, and label the elimination structural. Otherwise report exposure as live and untested — an unmeasured rate is not a low rate. |
| Sample ratio check | Stage 1 cannot be tested | Say so. If pre-exposure balance is available, use it as partial evidence and say it is partial. |
| Affected-population readout | Sensitivity and measurement dilution cannot be separated | Report the two as **tied**, name the affected-population readout as the single input that would separate them, and do not pick one. |
| Secondary metrics | Structural elimination unavailable at Stage 2 | Say which eliminations could not be made. |
| Prior runs of this test | The re-run test cannot run | Report as Uninformative and say the prior readout is unavailable, rather than assuming it. |
| Pre-registration | Segment claims are inadmissible | Any offsetting-effects hypothesis is not admissible without pre-registration or a mechanism-derived prediction. Say so rather than exploring it. |

The threshold row and the detected-effect row are the two that stop a case outright, and both are
routinely regarded as optional by the person supplying the file.

---

## What is not done, and why

**No segment search.** This folder does not scan subgroups, does not report a cut that "looks
promising," and does not suggest another way to slice the data. Given enough cuts one will be
significant, and a tool that searches after a flat headline is a machine for manufacturing false
positives. What it produces is worse than no finding, because it arrives carrying a number and a
population and it looks exactly like a result.

A segment hypothesis is admissible in exactly two situations:

- It was **pre-registered** before the readout.
- The change's **mechanism predicts it in advance** — a checkout change can only affect users who
  reach checkout, and saying so requires no scanning.

Everything else is an artifact. If a team supplies a segment finding they discovered after the
fact, name it as inadmissible and say why, rather than incorporating it.

**No new inference.** The estimates and intervals in the readout are read as given. This folder
does not compute p-values, re-cut populations, run additional tests, or recompute variance.
Reading an interval against a stated threshold is arithmetic on numbers already produced; it is
not re-analysis, and it is the only arithmetic performed.

**No prospective planning numbers.** No sample size, no runtime, no minimum detectable effect for
a future test. The folder may state that an achieved sample fell below the surface's established
floor, and may cite what a completed re-run actually did, since the re-run test cannot be shown
otherwise. What would fix it is a planning question with different inputs and a different owner.

---

## Reading the readout

Read the numbers that were produced, in this order, and resist the order the room reads them in.

- **The interval, against the decision threshold.** First, always. This is the reading that
  determines whether the test answered its question.
- **Assignment counts and the ratio check.** Before anything interpretive, because a ratio
  mismatch invalidates everything after it.
- **Exposure as a share of assigned**, and the gap between the assigned readout and any
  exposure-triggered one.
- **What share of the primary metric's denominator the change could reach.** Arithmetic, not
  statistics: an effect on 4% of the denominator arrives in the primary metric at roughly 4% of
  its size.
- **Secondary metrics**, for structural elimination and for evidence the change did something
  somewhere.
- **The p-value, last, and only to note that it does not discriminate.** It is predicted
  identically by an underpowered test and a genuinely null change, which are the two hypotheses
  the whole exercise exists to separate.

---

## Privacy

Individual user records are not needed and should not appear. Everything the method uses is an
aggregate: counts by arm, rates, intervals, and metric movements.

Names of the people who designed the test, wrote the analysis, or built the feature are not needed
and do not appear in the report. Findings concern the design and the readout, never the judgment
of whoever produced them. Where a finding concerns a decision — an early stop, a metric choice —
it is written about the decision as recorded, not about the person who made it.
