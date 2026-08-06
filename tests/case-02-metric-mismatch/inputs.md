# Case 02 — inputs

Constructed case. Paste everything below into a project running the diagnostician, then say
"Diagnose this flat test."

---

## The change

A redesigned returns-initiation flow: fewer steps, clearer eligibility messaging, and a saved
address default. It is reachable only from the order-history page, and only for orders inside the
returns window.

## Run 1

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
