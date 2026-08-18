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
