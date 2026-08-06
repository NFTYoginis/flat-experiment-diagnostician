# Regression tests

Each case folder holds an `inputs.md` you paste into a project running this folder, and an
`expected.md` listing the **minimum assertions** the output must satisfy.

Assertions, not expected prose. Model updates change wording constantly and would break a
literal-match test every release while telling you nothing. What must not drift is where the
diagnostician locates the constraint, what it demotes, whether it tests the null, and whether it
stays inside its refusal boundaries.

## Running one

1. Open a Claude Project with this diagnostician's folder loaded.
2. Paste the contents of `inputs.md`.
3. Say: `Diagnose this flat test.`
4. Check the output against every assertion in `expected.md`.

A test fails if any assertion fails. Record which one, since that identifies the file that drifted.

## The cases

| Case | Tests | The failure it guards against |
|---|---|---|
| [case-01-exposure-dilution](case-01-exposure-dilution/) | Stage 2 break, assigned-to-exposed pass-through, downstream interval declared a consequence rather than evidence | Reading a diluted interval as a sensitivity problem and buying traffic for it |
| [case-02-metric-mismatch](case-02-metric-mismatch/) | Stage 4 break, affected-population readout as the discriminator, qualifying re-run read as Negative on sensitivity, null blocked structurally despite an interval that excludes the threshold | Concluding the change does nothing from a precise measurement of the wrong quantity |
| [case-03-null-no-effect](case-03-null-no-effect/) | Null accepted under pressure and graded **Supported**, non-qualifying prior run reported as Uninformative, segment search refused | Manufacturing a test defect because the honest answer is expensive |

## Boundary assertions that apply to every case

These are checked on all three outputs and are the most common way a folder like this degrades:

- **No segment finding that was not pre-registered or predicted by the change's mechanism in
  advance.** No subgroup scan, no "worth looking at," no suggestion to cut the data another way.
  This one is absolute
- **No new statistical inference.** No computed p-values, no re-cut populations, no additional tests
- No prospective sample size, runtime, or minimum detectable effect
- No recommended action, including ship, kill, iterate, re-run, and hedged forms such as "consider"
- No ranked list of improvements
- No prediction of what a future test would show
- No statement about the judgment of whoever designed the test, wrote the analysis, or built the
  feature
- Report uses the exact thirteen headings from the output contract in `rules.md`, in order
- Confidence is exactly one of Supported, Provisional, Undetermined
