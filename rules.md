# Rules

How you diagnose. Run these in order. Do not skip ahead to a cause because the p-value is
obvious.

---

## Step 0 — Build the comparison set

You cannot diagnose a flat test in isolation. "No significant difference" means nothing until
you know what this surface has done before.

The comparison set is **prior tests on the same surface with known outcomes**. It is within-case
and it arrives already run. Require at least four, ideally six to ten, matched on:

- **Same surface** — the same page, module, or flow. A different surface has different traffic,
  different variance, and a different detection floor.
- **Same metric family** — tests read on the same primary metric, or on one built the same way.
- **Same randomization unit** — user, session, or visit. Mixing units makes variance
  incomparable.
- **Comparable population definition** — the same eligibility and exposure rules.
- **Comparable traffic period** — the surface's volume and seasonality.

**The set must contain at least one prior test that detected an effect.** This is not optional
and it is the requirement teams most often fail. A set of flat tests cannot establish a detection
floor, because it cannot distinguish a surface that never moves from a testing setup that never
resolves anything. Without a detected effect in the history, "flat" is uninterpretable and the
correct output is Undetermined.

If the team supplies fewer than four, say so and treat every conclusion as provisional. A
comparison set of two is an anecdote.

**Prior tests are a natural comparison baseline, not an experimental control.** They reduce
confounding — same surface, same instrumentation, same traffic shape — and they do not eliminate
it. The surface changed between tests, the population grew, the seasonality moved, other
experiments ran concurrently, and the metric definition may have been revised. Treat the
baseline as strong evidence about this surface, not as proof that the only difference is the
change under test.

---

## Step 0.5 — Comparison-set integrity check

Run this before computing a detection floor. A confident sensitivity finding drawn from a
comparison set that cannot support a floor is the most likely way this folder produces a wrong
answer.

Check each:

- Same surface, same metric family, same randomization unit
- **At least one prior test with a detected effect**, or the floor cannot be established
- Comparable population and eligibility definitions
- Comparable traffic period and volume
- Metric definition unchanged across the set, or the change documented
- No prior test in the set contaminated by a concurrent experiment on the same surface
- No single prior test carrying the floor on its own

Then state one verdict in the report:

- **Usable** — four or more prior tests, at least one with a detected effect, matched on surface,
  metric, and unit
- **Usable with limitations** — name the specific axes that do not match and which branches those
  weaken. Confidence caps at Provisional.
- **Not usable** — no detected effect in the history, or the set cannot support a floor. Report
  Undetermined, name what a usable set would need, and stop.

If one prior test is carrying the floor on its own, remove it and see whether the conclusion
survives. Say so if it does not.

---

## Step 1 — Confirm the failure is real

The failure to diagnose is **not** "the result was flat." It is "the test did not answer the
question it was run to answer."

Establish two numbers before anything else:

1. **The decision threshold** — the smallest effect that would have changed what the team did.
   This comes from the team. It is not a statistical quantity and you do not compute it.
2. **The confidence interval on the primary metric**, as reported.

Then read:

- The interval **excludes** the decision threshold → on this screen the test looks like it
  answered the question, and the null is your likely finding.

  **Do not stop here, and do not go straight to Step 5.** This screen reads one number against
  one threshold, which is the same shortcut the preamble forbids, one level more sophisticated
  than reading a p-value. An interval can be precise about a quantity that was never capable of
  registering the effect — a change reaching 4% of a metric's denominator produces a tight
  interval near zero no matter how well it works. That is a **Stage 4 break wearing a null's
  shape**, and it is invisible from here.

  Run Steps 2 through 6 regardless. Step 5's own null criteria are a conjunction that includes
  the metric being downstream of the mechanism *on the affected population* and the analysis
  population matching the exposed one — neither of which can be checked without Steps 2 through
  4. The screen cannot be executed on its own terms.
- The interval **contains** the decision threshold → the test did not answer the question. A real,
  decision-relevant effect is compatible with this result. Continue; something is constraining
  detection.
- No decision threshold was supplied → say so. Without it, "flat" cannot be graded, because
  there is no standard against which the interval is wide or narrow. Name it as the blocking
  input.

**The p-value does not appear in this step, and it should not appear in your reasoning at all.**
See the discriminator note in Step 4.

---

## Step 2 — Reconstruct the funnel

Every experiment moves from assignment to conclusion through five stages. This funnel is
canonical. Use these five names and this order everywhere, in the analysis and in the report.

| Stage | Evidence | The condition it establishes |
|---|---|---|
| 1. Delivery | Assignment counts by arm, sample ratio check, flag audit, bot filtering | The variant was actually served to the users assigned to it |
| 2. Exposure | Impression or view counts for the changed element, as a share of assigned | Assigned users actually encountered the change |
| 3. Sensitivity | Achieved sample against the surface's detection floor; interval width | The design could have resolved an effect of the size that matters |
| 4. Measurement | Whether the primary metric is downstream of the change's mechanism, on the affected population, in the right window | The metric could move if the change worked |
| 5. Inference | Analysis population, unit of analysis, dilution, pre-registered segment structure | An effect present in the data would appear in the readout |

**A break at any stage makes the null unreachable.** That is the structural fact that governs
this domain: "the change does nothing" is the conjunction of five clean stages, not the default
when nothing else is found. You do not arrive at the null by elimination. You arrive at it by
clearing every stage on evidence.

**Some outcomes are outside the funnel.** Route them out and say plainly that the constraint sits
outside the experiment:

- The variant was never built as specified — an implementation that does not match the design
  document. That is a build failure, and the test is of something else.
- The experiment was stopped early on a peek, or the readout is an interim look treated as final.
- A concurrent experiment on the same surface changed the control experience mid-run.
- The metric itself was broken or redefined during the run.

---

## Step 3 — Locate the break, and stop reading downstream

This is the rule that makes this a diagnosis rather than an inventory.

**The earliest failing stage is the diagnosis site. Every stage after it is starved and therefore
uninformative.**

If the variant was never delivered, the exposure rate, the interval, and the metric choice tell
you nothing — you did not run the experiment you think you ran. If only 8% of assigned users ever
saw the change, the interval on the assigned population is measuring a diluted effect and its
width is not a fact about the surface's sensitivity.

Concretely:

- **Break at Stage 1 (delivery).** Diagnose assignment. Conclude nothing about the change, the
  metric, or the power. Live branches: flag misconfiguration, sample ratio mismatch, bot or
  internal traffic, cached or pre-rendered control, assignment before eligibility.
- **Break at Stage 2 (exposure).** Delivered and not seen. Live branches: the element sits below
  the fold, behind a click, in a tab, or on a path most assigned users never take. Nothing about
  sensitivity or the metric is in evidence, because the effective sample is not the assigned
  sample.
- **Break at Stage 3 (sensitivity).** Seen, and the design could not resolve an effect of the
  size at issue. Live branches: achieved sample below the surface's floor, high metric variance,
  runtime short of a full behavioural cycle, a triggered analysis that shrank the population.
- **Break at Stage 4 (measurement).** Powered, and pointed at the wrong thing. Live branches: the
  metric is several steps downstream of the mechanism, the affected population is a small share
  of the metric's denominator, the measurement window closes before the behaviour occurs.
- **Break at Stage 5 (inference).** The effect is in the data and the readout does not show it.
  Live branches: dilution by including unexposed users, wrong unit of analysis, a pre-registered
  segment structure that was not applied, offsetting effects in opposite directions.
- **No break at any stage.** The null. See Step 5, and expect that most well-run flat tests land
  here.

When you write the report, say explicitly which stages you are not drawing conclusions about,
and why.

### Two techniques that fall out of this rule

**Eliminate a stage structurally when its baseline is missing.** Exposure instrumentation for the
changed element is frequently absent — nobody logged an impression on the module. You cannot
measure exposure directly and you do not have to. **Any metric that moved and could only move if
the element was seen proves exposure occurred and bounds it from below.** A guardrail that
shifted, a secondary click, a change in time-on-page. A break at stage N starves stage N+1, so a
downstream metric that moved is proof the upstream stage did not break. Record the elimination as
structural rather than comparative, so a reader knows which kind of evidence it rests on.

**Compute pass-through between adjacent stages, not just levels.** Assigned to exposed is the
pair that carries this domain. A test with 240,000 assigned users sounds well powered. If 9% of
them ever reached the module, the effective sample is 21,600 and any real effect on those users
is diluted roughly elevenfold in an assigned-population readout. **Levels tell you the test was
big. Pass-through tells you whether it was big where it mattered.** Where an exposure-triggered
readout exists, compare it against the assigned-population readout: the gap between them is the
dilution, measured rather than argued.

---

## Step 4 — Run the discriminators for the break stage

See `reference/failure-modes.md` for the full taxonomy and the evidence signature of each cause.
Within the break stage, at least two causes will be live. Separate them with evidence, not
plausibility.

The discipline: for each candidate cause, ask what the readout would look like if that cause were
true and if it were false. If it would look the same either way, that cause is not decidable from
this evidence and you must say so rather than pick it.

**The p-value is the trap in this domain, and it is the whole trap.**

An underpowered test and a genuinely null change both produce p > 0.05. Both produce a confidence
interval containing zero. Both produce a point estimate near zero. **The p-value is predicted
identically by the two hypotheses this folder exists to separate, so it discriminates nothing at
all** — and it is the number the entire room is looking at.

What differs is **the width of the interval relative to the decision threshold**:

- An interval wide enough to contain effects the team would have acted on → the test could not
  answer the question. Sensitivity, or something upstream of it, is the constraint.
- An interval tight enough to exclude every effect the team would have acted on → the test
  answered the question, and the answer is that the change does not do enough to matter.

**Read the interval against the decision threshold, never the p-value against 0.05.** A test can
be "not significant" and conclusive. A test can be "not significant" and completely
uninformative. The p-value is identical in both cases and the two results should lead to opposite
decisions.

### The qualifying re-run test

If the test was re-run — with more traffic, a longer window, or a revised setup — that is the most
valuable item in the file. It is a completed experiment about the experiment. But it only tested
the mechanism it was capable of testing.

A re-run is diagnostically usable only when all four hold:

1. It **changed the thing the hypothesis is about.** More traffic tests sensitivity. It tests
   nothing about a wrong metric, a broken flag, or an exposure gap — a diluted test run twice is
   a diluted test. Fixing instrumentation tests delivery. Changing the primary metric tests
   measurement.
2. The **variant was otherwise unchanged.** A re-run of a revised design is a different
   experiment, and the two cannot be pooled or compared.
3. **The added sample was sufficient to cross the floor.** This is the condition that fails most
   often and it fails on arithmetic. Precision improves with the square root of sample: doubling
   traffic narrows the interval by about 29%, and **halving an interval requires roughly four
   times the sample.** A team that doubled traffic expecting to double resolution has not tested
   what they think they tested.
4. **The period was comparable** — no seasonality shift, no concurrent release on the surface, no
   change to the metric definition.

**Name the verdict. There are three, and two of them are constantly confused.**

- **Positive** — the readout moved after a qualifying re-run. What changed identifies the
  mechanism, and that is worth more than the fact of the change.
- **Negative** — the re-run qualified and the result held. Strong evidence against the specific
  mechanism the re-run was capable of testing. Not against detectability generally.
- **Uninformative** — one or more conditions failed. The experiment did not run. This is not weak
  evidence for either side; it is no evidence at all.

**Negative and Uninformative must be labelled differently in the report.** A re-run that crossed
the floor and held and a re-run that never approached the floor support completely different
conclusions, and collapsing them into "we ran it again and got nothing" is how this domain's folk
cause survives contact with the record.

When the conditions hold and the result held, write it as:

> A qualifying re-run produced no change in [the readout]. That is strong evidence against [the
> specific mechanism the re-run was capable of testing].

Teams routinely read a second flat result as proof the change does nothing. That reading is
available, and so is the opposite — that a test 12× under the floor was re-run at 2.4× and is
still 5× under it, and has therefore never tested the hypothesis once.

### Which way does the gap push

When evidence is missing or the comparison set carries a mismatch, do not stop at naming it. Work
out **which direction it biases the conclusion**, then ask whether the conclusion survives the
worst case.

A limitation that pushes *against* your finding strengthens it. If exposure instrumentation is
missing and every plausible exposure rate leaves the effective sample above the surface's floor,
its absence cannot have manufactured a sensitivity finding. If the comparison set's prior tests
ran on higher traffic than the subject and the subject still cleared the floor, the mismatch
cannot be what produced the clearance.

A limitation that pushes *toward* your finding is different in kind, and caps confidence whether
or not you can quantify it.

State the direction explicitly. "Exposure was not instrumented" tells a reader a fact. "Exposure
was not instrumented, and even at the lowest rate this surface has ever produced the effective
sample clears the floor" tells them what to do with it.

---

## Step 5 — Test the null model before committing

You must attempt to reject your own diagnosis before writing it.

The null: **the test was run correctly and the change does not produce an effect the team would
act on.**

This is not the residual category. It is a positive finding with its own evidence requirements,
and it is reached only when all of the following hold:

- Delivery is clean — assignment ratio as designed, no contamination
- Exposure is at or above the surface's normal rate for a change in that position
- The achieved sample clears the surface's detection floor, established from prior tests that
  detected effects
- The primary metric is downstream of the change's mechanism, on the affected population
- The analysis population matches the exposed population, with no dilution
- **The confidence interval excludes the decision threshold**

The last one is the load-bearing condition. Everything else establishes that the test was capable
of answering; that one is the answer.

If the null survives, the null is the diagnosis. Write it as a finding, not as an inability to
find something. This is the outcome the room is least prepared to hear, because somebody spent a
quarter building the thing and there is a roadmap slide with its name on it. Say it plainly. A
team that stops iterating on something that demonstrably does nothing has been handed the most
valuable output this folder produces.

---

## Step 6 — Rank, and name one, at three levels

Output exactly one diagnosis, stated at three distinct levels. Flattening them is what produces
the vague finding that sounds rigorous and cannot be acted on.

- **Primary constraint** — *where* the experiment breaks. One of the five stages, or none.
- **Primary cause** — *why* it breaks there. One entry from the taxonomy in
  `reference/failure-modes.md`.
- **Mechanism** — *how* that cause produces this specific readout, traced to the evidence.

Worked through:

```
Primary constraint:  Stage 2, exposure.
Primary cause:       Position dilution.
Mechanism:           The module sits below the fold on a page whose median scroll
                     depth is 41%, so 9% of assigned users ever rendered it. The
                     assigned-population readout averages a possible effect on
                     21,600 users across 240,000, compressing it by roughly
                     elevenfold and placing it well inside the interval.
```

"The interval was too wide" and "position dilution" are not competing causes. One is the
mechanism's consequence and the other is the category. Never present them as alternatives to each
other.

Everything else goes into one of two buckets:

- **Downstream of the primary.** Effects, not causes. Say which cause they descend from. A wide
  interval downstream of an exposure break belongs here.
- **Not supported by this evidence.** Plausible, unproven. Say what would settle them.

If two causes are genuinely tied, do not average them into a vague statement. Name both, say
precisely why the readout cannot separate them, and name the single input that would.

---

## Output contract

Every report uses these headings in this order. No additions, no reordering.

```
## Failure observed
## Comparison set
## Comparison-set integrity
## Funnel reconstruction
## Primary constraint
## Primary cause
## Mechanism
## Evidence for this cause and against the alternatives
## Alternatives, and why they are demoted
## Null model
## Confidence
## Missing evidence
## What would prove this wrong
```

**Length discipline.** Constraint is one line. Cause is one line. Mechanism is one paragraph. If
the mechanism takes three paragraphs, you have not finished diagnosing.

**Prohibited in output:**

- **Any segment finding not pre-registered or predicted by the change's mechanism in advance.**
  Do not scan subgroups, do not report a subgroup that "looks promising," and do not suggest
  cutting the data another way. This is absolute.
- **Any new statistical inference.** No computed p-values, no re-cut populations, no additional
  tests. You read the estimates and intervals that were produced.
- **Any prospective sample size, runtime, or minimum detectable effect.** You may state that the
  achieved sample fell below the surface's established floor, and you may cite figures already in
  the case file, since the qualifying re-run test cannot be shown without reference to what the
  re-run actually did. State the *effect* wherever it carries the argument ("still short of the
  floor by a factor this re-run could not close"), and cite the figure only when the effect alone
  would be unverifiable. The line is prospective versus historical, not numbers versus no numbers.
- A recommended action of any kind, including ship, kill, iterate, re-run, and hedged forms such
  as "consider" or "you may want to"
- Ranked lists of improvements
- Any prediction of what a future test would show
- Any statement about the judgment or performance of the person who designed the test, wrote the
  analysis, or built the feature
- Confidence language unsupported by the readout actually supplied

**Confidence must be stated as one of:**

- **Supported** — the comparison set is Usable, the funnel locates the constraint, and the
  discriminators separate the live causes
- **Provisional** — the constraint is located but at least one of these holds: a discriminator is
  missing, the comparison set is Usable with limitations, or the null model could not be tested
- **Undetermined** — the funnel cannot be reconstructed, the comparison set is Not usable, or no
  decision threshold was supplied

These caps are not advisory. **An untestable null caps confidence at Provisional no matter how
clean everything else is**, because the most likely alternative explanation was never examined.
Same for a limited comparison set.

### Grading a null

The three grades above describe a **located constraint**. A null has none, so read literally they
cap every null at Provisional forever, however good the evidence. That is backwards: a
well-evidenced null is the finding this folder exists to protect, and it should not be
permanently outranked by a weak positive one. Grade it on its own terms.

- **Supported** — the comparison set is Usable, every stage that can be checked is clean, **the
  null was tested directly against the input built to test it** — here, the confidence interval
  read against the team's stated decision threshold, with the surface's detection floor
  established from prior tests that detected effects — and the alternatives this comparison set is
  structurally unable to see are named.
- **Provisional** — as above, but the null is inferred from the readout looking clean rather than
  measured against a stated threshold and an established floor. This is where most nulls land,
  because the confirming input is the one nobody thinks to supply.
- **Undetermined** — the funnel cannot be reconstructed, or the comparison set is Not usable.

The distinction that matters is the middle clause. A null inferred from "nothing looks wrong with
the test" and a null confirmed against an interval that excludes the effect the team would have
acted on are different findings, and a team about to stop work on the strength of one deserves to
know which they were handed.

Undetermined is a legitimate outcome. Name the missing input and stop.
