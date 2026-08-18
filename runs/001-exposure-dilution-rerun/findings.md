# Re-run 001 — findings

Scored against [`expected.md`](expected.md) and the unchanged case key.

**Result: pass. The control held, so the other two re-runs are readable.**

| Assertion | Result |
|---|---|
| Pass-through computed and stated: 21,840 of 240,000, 9.1% | ✅ |
| Primary constraint is Stage 2, exposure | ✅ |
| Stages 3–5 declared starved, no conclusion drawn from them | ✅ |
| 240,000 not described as adequate without the exposure correction | ✅ |
| Floor read on the exposed population, not the assigned one | ✅ |
| Assigned-population interval identified as measuring a diluted effect | ✅ |
| Element position held constant against the set | ✅ |
| Comparison-set integrity `Usable with limitations`, failing axis named | ✅ |
| Confidence `Provisional`, tied to the comparison-set limitation | ✅ |
| No recommended action, no sample-size or MDE computation | ✅ |

## What the control was for

This case's key was never repaired, so it carries no repair circularity. A disagreement here
would have meant the folder changed under the keys — the Step 1 routing fix in `3f53436`, or the
`Assigned` convention now stated inline in the case file.

Neither shows. The constraint landed at Stage 2, downstream of the screen that used to
terminate early in four of five products in this family, which is the regression this case is
positioned to catch. The convention statement changed nothing, as predicted — case 01 reads the
same under either reading, which is why the original disclosure named case 03 and not this one.
