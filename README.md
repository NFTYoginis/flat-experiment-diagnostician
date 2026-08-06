# Flat Experiment Diagnostician

**v0.1** · Status: built. Not yet exercised by a blind run, and no retrospective field validation.

A folder you drop into a Claude Project. Claude becomes a diagnostician that works out why an A/B
test showed no effect.

It is built for one moment: the readout is on the screen, nothing is significant, and the room is
deciding what it means. The explanation forming is that the change did not work.

That might be right. But a flat result is produced by two entirely different situations, and they
are not distinguishable from the thing everyone looks at first.

---

## Two problems, one p-value

**Either the test failed to detect an effect, or the change genuinely has none.**

Those need opposite responses, and the cost of confusing them runs both ways. A team that reads an
underpowered test as a verdict kills a working change. A team that reads a well-powered flat result
as "needs more traffic" spends another quarter re-running an experiment that already answered them.

Here is the problem. An underpowered test and a genuinely null change **both** produce p > 0.05,
**both** produce an interval containing zero, and **both** produce a point estimate near zero. Every
number the room is looking at is predicted identically by the two hypotheses, so none of them
separates the two.

What separates them is **the width of the interval against the smallest effect you would have acted
on**:

| Interval | Means |
|---|---|
| Contains the effect you'd have shipped for | The test could not answer. Something is constraining detection |
| Excludes every effect you'd have shipped for | The test answered. The change does not do enough to matter |

A test can be "not significant" and conclusive. A test can be "not significant" and completely
uninformative. **The p-value is identical in both cases and they should lead to opposite decisions.**

---

## The finding nobody wants

This folder reaches one conclusion more often than any other: **the test was fine and the change
does nothing.**

Somebody spent a quarter building the thing. There is a roadmap slide with its name on it. The path
of least resistance is always to find a reason the test was unfair, and there is always a candidate
— some dilution, some segment, some window.

So that branch is guarded harder than any other. **The null is the conjunction of five clean stages,
not the residual when nothing else is found.** It is not reached by failing to find something else.
It is reached by clearing every stage on evidence, and by an interval that excludes the effect size
that would have changed the decision.

When it is reached, it is stated plainly. A team that stops iterating on something that
demonstrably does nothing has been handed the most valuable output here.

---

## What it does

Reconstructs the path from assignment to conclusion, compares it against prior tests on the same
surface, finds the earliest stage that broke, and names the one cause the evidence supports.

Then it tells you what would prove it wrong.

## What it does not do

- **Go looking for a segment where it worked.** Given enough cuts, one will be significant. A tool
  that searches subgroups after a flat headline is a machine for manufacturing false positives, and
  what it produces is worse than no finding because it arrives carrying a number
- Re-analyse your data, compute new inference, or re-cut your population
- Recommend shipping, killing, iterating, or re-running
- Compute a sample size, a runtime, or an MDE for a future test
- Evaluate whoever designed the test, wrote the analysis, or built the feature
- Predict what a re-run would show

It stops at the cause. What you do about it is your decision.

---

## Setup

1. Create a Claude Project.
2. Upload the contents of this folder to the project knowledge.
3. Paste this into the project's custom instructions:

   > Follow identity.md and rules.md exactly. Run the diagnostic sequence in rules.md in
   > order and use the output contract at the end of that file. Consult reference/ as
   > needed. Do not recommend actions.

That is the whole install. No dependencies, no API keys, nothing to run.

---

## Running a case

Upload the case materials and say: **"Diagnose this flat test."**

### What you need

Four things. Without all four you will get an Undetermined result rather than a wrong one.

1. **The readout as produced** — point estimate, confidence interval, assignment counts by arm,
   analysis population
2. **Your decision threshold** — the smallest effect that would have changed what you did
3. **Four or more prior tests on the same surface**, with outcomes, effect sizes, and samples.
   **At least one must have detected an effect**
4. **What the change actually does**, concretely enough to identify which users it can reach

Items 2 and 3 are the ones teams skip, and they are the two the method depends on entirely.

**Without a decision threshold, "flat" cannot be graded.** An interval is not wide or narrow in the
abstract — it is wide or narrow relative to what you would have acted on. Two teams with identical
readouts and different thresholds got different answers from the same experiment. The folder will
not substitute a convention, because 80% power at a 2% MDE is a number from somebody else's surface.

**Without a prior test that detected an effect, your history establishes nothing.** A set of flat
tests cannot tell a surface that never moves from a setup that never resolves. That is what
establishes your surface's real **detection floor** — the smallest effect it has actually resolved,
and at what sample. It already accounts for your metric's variance and your instrumentation's noise,
which no power formula knows about.

### What sharpens it considerably

- **A readout on the affected population** — the users your change can actually reach. This is what
  separates an underpowered test from one measuring a diluted effect, and nothing else does
- **Exposure counts** for the changed element, as a share of assigned
- **The sample ratio check.** Cheapest possible check, and it invalidates everything downstream when
  it fails
- **Secondary and guardrail metrics.** Any metric that moved and could only move if the element was
  seen is proof of exposure when instrumentation is missing
- **Prior runs of this same test**, and what changed between them

Full intake list in [reference/intake.md](reference/intake.md).

---

## What you get back

A report with fixed headings, in this order:

Failure observed · Comparison set · Comparison-set integrity · Funnel reconstruction · Primary
constraint · Primary cause · Mechanism · Evidence for this cause and against the alternatives ·
Alternatives and why they are demoted · Null model · Confidence · Missing evidence · What would
prove this wrong

The diagnosis comes at three levels rather than one, because flattening them produces a finding that
sounds rigorous and cannot be acted on. **Constraint** is where the experiment breaks. **Cause** is
why it breaks there. **Mechanism** is how that cause produces this specific readout.

Three worked examples in [examples.md](examples.md), including one that finds nothing wrong with the
test and concludes the change does nothing.

---

## The three answers people don't expect

**"Your test is fine and your change does nothing."** The one nobody wants and the one that is
correct a large share of the time. It comes with the interval that rules out what you'd have shipped
for, and it comes as a finding rather than a shrug.

**"You measured something your change couldn't move."** If the change acts on 4% of your metric's
denominator, a real effect arrives at roughly 4% of its size. More traffic does not fix that ratio,
and a bigger test just measures the wrong thing more precisely.

**"Your re-run tested nothing."** Precision improves with the square root of sample. Doubling
traffic narrows the interval by about 29%, not 50% — and **halving an interval takes roughly four
times the sample.** A test 12× under your floor, re-run at 2.4×, is still 5× under it and has never
tested the hypothesis once.

---

## How it decides

Every experiment moves from assignment to conclusion through five stages: **delivery, exposure,
sensitivity, measurement, inference.** Find the earliest one that broke. **That stage is the
diagnosis site, and every stage after it is starved and tells you nothing.**

If only 9% of assigned users ever saw the change, the interval on the assigned population is
measuring a diluted effect, and its width is not a fact about your surface's sensitivity. Reading it
as one sends you to buy traffic for a problem traffic cannot fix.

So the useful figure is never how many users were assigned. It is **how many were exposed.** A test
with 240,000 assigned users sounds well powered; at 9% exposure the effective sample is 21,600, and
an effect on those users is diluted roughly elevenfold in an assigned-population readout.

Full method in [rules.md](rules.md). Cause taxonomy with evidence signatures in
[reference/failure-modes.md](reference/failure-modes.md).

---

## Files

```
flat-experiment-diagnostician/
├── README.md                      this file
├── identity.md                    who it is, what it diagnoses, what it refuses
├── rules.md                       the method and the output contract
├── examples.md                    three worked cases, one of them a null
└── reference/
    ├── failure-modes.md           causes by funnel stage, with evidence signatures
    ├── baselines.md               the detection floor, the threshold, and reading a re-run
    └── intake.md                  required inputs and missing-evidence handling
```

---

## Scope and honesty

The examples are constructed teaching cases with figures chosen to make each discriminator legible.
They are not real readouts.

There are deliberately no benchmark figures anywhere in this folder. **There is no such thing as a
general minimum detectable effect.** Baseline rates, metric variance, traffic, and normal exposure
vary by surface, product, market, and season, and any figure quoted as a standard — 80% power, a 2%
MDE, two weeks of runtime — would be wrong for most surfaces while being treated as authoritative.
Every baseline is computed from the prior tests you supply.

This reads the readout you produced. It does not compute new inference, re-cut your population, or
run additional tests. Reading an interval against your stated threshold is arithmetic on numbers
already produced, and it is the only arithmetic performed.

Everything the method uses is an aggregate: counts by arm, rates, intervals, and metric movements.
No individual user records are needed and none should be supplied.
