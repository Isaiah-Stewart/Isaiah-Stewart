# Marginal analysis

Choosing how much to produce by comparing each additional unit's marginal cost against
its price, subject to capacity limits — and building the workbook that makes that
comparison visible unit by unit.

## Contents

| File | What it is | Status |
|------|-----------|--------|
| `spec.md` | Build specification for the Excel model: named input contract, sheet and region structure, calculation logic in named-range notation, acceptance criteria, named outputs | Complete |
| `model.xlsx` | The workbook built from `spec.md` — five sheets, 94 named ranges, 848 formulas, 54 in-workbook validation checks | Built; Solver run outstanding |

`spec.md` is the instruction sheet, not a summary of a finished model. It is written so the
workbook can be built from it alone, and so a reader can trace any reported figure back to
a stated input.

`model.xlsx` carries its own validation: `5_Checks` holds 54 computed PASS/FAIL tests plus
three audit rows, and `4_Results` reports the tally. The decision cells on `3_Optimize` ship
holding the optimal mix so those checks compute live; Solver is an Excel add-in and cannot be
written into the file, so running it once from the Region D parameters — and recording that
run in the sheet — is the outstanding step.

## Engagements

| Date | Engagement | Artifacts |
|------|-----------|-----------|
| 2026-10 | Farm planting mix — how many beds of tomatoes, carrots and mesclun maximize season profit under per-crop bed caps, a 64-bed total, and a cap of four temporary workers. Labor hours compound in the number of beds, so marginal cost rises and the highest-revenue crop does not take every bed | `spec.md` |

## Related documents

Documents framing or concluding this work live outside this folder, per the repository
structure: briefs written before the work in `docs/briefs/`, recommendations written after
it in `docs/decisions/`.
