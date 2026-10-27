# Spec — Farm planting optimization model

**Purpose.** Instruction sheet for an Excel workbook that chooses how many beds of
tomatoes, carrots and mesclun to plant to maximize season profit, subject to per-crop
bed caps, a total bed cap, and a temp-labor cap. Someone who has never seen the case
should be able to build the workbook from this file alone.

**Economic rule.** Plant another bed of a crop while that bed's revenue exceeds its
marginal cost; stop when marginal cost passes revenue. Marginal cost rises because the
labor formula compounds in `q`, so the highest-revenue crop does not win every bed.

**Source.** Case givens and the labor formula as recorded in
`Farm case: labor formula and project goal` (2026-10-26), plus the case givens
reference panel.

---

## 0. Build principles — the simplest model that answers the question

This is a small problem. Three decision variables, five constraints, one non-linear
cost driver. The workbook should look like that, and no bigger. A model that is hard to
audit is wrong even when its number is right.

**Hard limits on the build:**

- **Five sheets.** If a block fits on a sheet that already exists, it does not get a new
  one. One job per sheet.
- **Roughly 30 named cells, 2 named functions, 3 schedule blocks.** That is the whole
  model. The schedules are the only large regions: 20 + 20 + 30 = 70 rows.
- **Every formula fits on one line and reads left to right.** If a formula needs a
  comment to be understood, name an intermediate instead of commenting it.
- **No helper column used only once** — inline it. No intermediate name used only once,
  unless the name is what makes a formula readable.
- **No VBA, no macros, no Power Query, no pivot tables, no iterative calculation, no
  circular references.** `LAMBDA` is used for exactly the two functions in §3.2 and
  nowhere else.
- **Named ranges everywhere; no cell addresses.** One structural exception, `PRIOR`,
  defined in §3.3.
- **Solver runs once**, from the region in §4. No manual goal-seeking, no typed answers,
  no "adjusted" cells.

**Efficiency note.** The schedules in §3.3 plus the three bed caps are enough to get the
answer by hand: walk each crop's MC column until it passes that crop's revenue, stop
there or at the cap, and check the total against 64 beds. Solver exists to confirm that
reasoning and to make the optimum re-derivable when an input changes — not to substitute
for it. Build the schedules first and read the answer off them; if Solver then disagrees,
the error is in the model, not the economics.

**Done looks like:** a reader who has never seen the case can trace any reported figure
back to a §1 input in under 30 seconds, and every check in §5 passes.

---

## 1. Inputs — named contract

Every input is a named cell on `1_Inputs`. Nothing downstream may reference a literal
number; if a value appears in a formula it must appear here first.

### 1.1 Season and farm

| Name | Value | Unit | Source |
|------|-------|------|--------|
| `in_WeeksSeason` | 36 | weeks | Case givens — season length |
| `in_BedsTotal` | 64 | beds | Case givens — beds available across all crops |
| `in_FixedCost` | 20,000 | $ / season | Case givens — fixed costs |

### 1.2 Crop parameters

| Name | Value | Unit | Source |
|------|-------|------|--------|
| `in_RevBed_Tom` | 8,800 | $ / bed / season | Case givens — tomato revenue per bed |
| `in_RevBed_Car` | 2,094 | $ / bed / season | Case givens — carrot revenue per bed |
| `in_RevBed_Mes` | 2,700 | $ / bed / season | Case givens — mesclun revenue per bed |
| `in_HrsWkBed_Tom` | 2.5 | hours / week / bed | Case givens — tomato field hours |
| `in_HrsWkBed_Car` | 2.5 / 3 | hours / week / bed | Case givens. **Enter as `= in_HrsWkBed_Tom / 3`, not 0.833** — see §1.5 |
| `in_HrsWkBed_Mes` | 2.5 / 2 | hours / week / bed | Case givens. **Enter as `= in_HrsWkBed_Tom / 2`, not 1.25** |
| `in_Fert_Tom` | 880 | $ / bed | Case givens — tomato fertilizer |
| `in_Fert_Car` | 440 | $ / bed | Case givens — carrot fertilizer |
| `in_Fert_Mes` | 880 | $ / bed | Case givens — mesclun fertilizer |
| `in_Dim_Tom` | 0.1000 | decimal rate | Case givens — tomato diminishing-returns rate |
| `in_Dim_Car` | 0.0250 | decimal rate | Case givens — carrot diminishing-returns rate |
| `in_Dim_Mes` | 0.0125 | decimal rate | Case givens — mesclun diminishing-returns rate |
| `in_BedCap_Tom` | 20 | beds | Case givens — tomato bed cap |
| `in_BedCap_Car` | 20 | beds | Case givens — carrot bed cap |
| `in_BedCap_Mes` | 30 | beds | Case givens — mesclun bed cap |

Price is taken as given — the farmer is a price taker — so revenue per bed is a
constant, not a function of `q`.

### 1.3 Labor

| Name | Value | Unit | Source |
|------|-------|------|--------|
| `in_OwnerPay` | 50,000 | $ / season | Case givens — owner pay |
| `in_OwnerHrsPaid` | 1,440 | hours | Case givens — 40 hrs/wk × 36 wks |
| `in_OwnerFieldShare` | 0.50 | decimal | Case givens — share of paid hours in the field |
| `in_TempPay` | 25,000 | $ / season / worker | Case givens — temp pay |
| `in_TempHrsEach` | 1,440 | hours / worker | Case givens — 40 × 36, all in the field |
| `in_TempMax` | 4 | workers | Case givens — maximum temps |

### 1.4 Derived — cost structure block, same sheet

Computed, never typed. These four cells are the entire cost structure.

| Name | Formula | Value |
|------|---------|-------|
| `in_OwnerFieldHrs` | `= in_OwnerHrsPaid * in_OwnerFieldShare` | 720 hours |
| `rate_Owner` | `= in_OwnerPay / in_OwnerHrsPaid` | $34.7222…/hr |
| `rate_Temp` | `= in_TempPay / in_TempHrsEach` | $17.3611…/hr |
| `cap_TempHours` | `= in_TempMax * in_TempHrsEach` | 5,760 hours |

The three cost layers, labeled on the sheet beside those cells:

| Layer | Behavior | Named total |
|-------|----------|-------------|
| Fixed | $20,000, constant in every mix | `calc_FixedCost` |
| Variable per bed | Fertilizer, linear in beds | `calc_FertCost` |
| Variable per hour, two-tier | Field labor. Hours compound in `q`; first 720 hours at `rate_Owner`, every hour beyond at `rate_Temp` | `calc_LaborCost` |

Only the third layer creates rising marginal cost. Fertilizer is flat per bed and fixed
cost is constant, so **the optimal mix is invariant to `in_FixedCost`** — it moves the
profit level, not the decision. Keep it in the profit line; never in an MC schedule.

### 1.5 Entry rules — three traps that each break the check figure

1. **Carrot and mesclun hours are entered as expressions.** The panel shows `2.5 ÷ 3`
   and `2.5 ÷ 2`. Typing `0.833` instead of `= in_HrsWkBed_Tom / 3` moves profit at the
   published mix by **+$6.83**.
2. **Wage rates are derived, never typed.** The panel quotes $34.72 and $17.36 as
   displays. Typing those two-decimal figures moves profit by **+$6.67**.
3. **Nothing is rounded on entry.** Rounding is a display format only.

Each of these still produces a plausible-looking answer near $42,762, which is why §5
tests for them directly rather than trusting the total.

---

## 2. Structure

Five sheets, left to right in build order. Inputs flow one direction only: no sheet
references a sheet to its right.

| Sheet | Contents | Why it exists |
|-------|----------|---------------|
| `1_Inputs` | §1.1–1.3 inputs, §1.4 derived rates and cost structure | The only sheet with typed numbers. Input cells shaded, everything else locked. |
| `2_Schedules` | One block per crop, rows `q = 1 … cap`: cumulative hours, marginal hours, cumulative cost, marginal cost, revenue, plant/stop flag | The bed-by-bed MC schedule the case asks to see, and the sheet the answer is read off |
| `3_Optimize` | Regions A–E of §4 | Decisions, objective, constraints, Solver setup, P = MC readout |
| `4_Results` | §6 outputs and the one required chart | Reporting only; no calculation originates here |
| `5_Checks` | §5 acceptance tests, one row each, PASS or FAIL | Proves the build before anyone trusts the number |

The two named functions live in Name Manager, not on a sheet, so they need no space.

### 2.1 Two marginal-cost measures — keep them separately named

`in_OwnerFieldHrs` is a **single pool shared across all three crops**, so labor cost is
not separable by crop and the three schedules cannot be summed. Carry both measures
under distinct names:

- **`sch_MC_*` — standalone marginal cost.** Crop planted alone, rate switching at 720
  hours. The series the case asks to chart, and the one showing the drop at tomato bed 6.
- **`calc_IncNext_*` / `calc_IncLast_*` — incremental cost at the chosen mix.** The
  change in total cost from one more, or one fewer, bed of that crop given the other two
  decisions.

They diverge for any bed whose marginal hours sit inside the owner pool standalone but
outside it in the mix — all the early carrot and mesclun beds. **At the published optimum
they coincide for all three crops, to the cent**, because the owner pool is already
exhausted in both views, so every marginal hour prices at `rate_Temp` either way. That
coincidence is a property of this optimum, not a general result. Check C5 tests the
optimum against `calc_IncNext_*`, which is correct everywhere, and check C7.4 confirms
the two agree here.

### 2.2 The two-tier rate, and why marginal cost falls once

`rate_Owner` is **$34.72/hr** and `rate_Temp` is **$17.36/hr** — the owner's time costs
twice a temp's. The case spends the owner's 720 field hours first, so the expensive tier
goes before the cheap one. The model must show the consequence rather than hide it:
**marginal cost rises within each tier but falls once, at the crossover.** For tomatoes
planted alone, standalone MC climbs to **$7,660.86** at bed 5, then drops to **$4,906.28**
at bed 6, because bed 5 is where cumulative hours cross 720 (724.73 hours).

Note that `in_OwnerFieldHrs × rate_Owner` = 720 × $34.7222 = **exactly $25,000**, half of
`in_OwnerPay`. Only the field half of the owner's pay enters the crop decision; the other
$25,000 covers non-field paid time and is not charged against the planting mix. This
follows the case's construction and is stated here rather than left implicit.

---

## 3. Calculation logic

Named-range notation throughout. No formula in the workbook may contain a cell address.

### 3.1 Rates

Defined in §1.4. Nothing else derives a rate.

### 3.2 The two named functions — Name Manager

```
fn_Hours = LAMBDA(q, hrsWkBed, dim,
    q * hrsWkBed * in_WeeksSeason * (1 + dim) ^ q )

fn_LaborCost = LAMBDA(h,
      MIN(h, in_OwnerFieldHrs) * rate_Owner
    + MAX(0, h - in_OwnerFieldHrs) * rate_Temp )
```

These are the only two functions in the workbook. If the build must avoid `LAMBDA`,
write the same expressions inline using the named inputs — identical behavior, but each
rule then exists in more than one place, so prefer the named form.

### 3.3 Schedules — `2_Schedules`

Spine `sch_q` is the integer column 1 … `in_BedCap_*`. Tomato shown; carrots and mesclun
are identical with their own inputs. Six columns per crop, no more.

```
sch_Hours_Tom   = fn_Hours(sch_q, in_HrsWkBed_Tom, in_Dim_Tom)
sch_MgHours_Tom = sch_Hours_Tom - IF(sch_q = 1, 0, PRIOR(sch_Hours_Tom))
sch_CumCost_Tom = fn_LaborCost(sch_Hours_Tom) + in_Fert_Tom * sch_q
sch_MC_Tom      = sch_CumCost_Tom - IF(sch_q = 1, 0, PRIOR(sch_CumCost_Tom))
sch_Plant_Tom   = IF(in_RevBed_Tom >= sch_MC_Tom, "PLANT", "STOP")
```

`PRIOR` denotes the preceding row of the same named column. Implement as a relative
reference inside the named column — the one permitted exception to the no-addresses rule.
It must not reach outside its column.

### 3.4 Profit for any mix — `3_Optimize`

```
calc_Hours_Tom = fn_Hours(dec_Beds_Tom, in_HrsWkBed_Tom, in_Dim_Tom)
calc_Hours_Car = fn_Hours(dec_Beds_Car, in_HrsWkBed_Car, in_Dim_Car)
calc_Hours_Mes = fn_Hours(dec_Beds_Mes, in_HrsWkBed_Mes, in_Dim_Mes)

calc_HoursTotal     = calc_Hours_Tom + calc_Hours_Car + calc_Hours_Mes
calc_OwnerHoursUsed = MIN(calc_HoursTotal, in_OwnerFieldHrs)
calc_TempHoursUsed  = MAX(0, calc_HoursTotal - in_OwnerFieldHrs)
calc_TempWorkers    = calc_TempHoursUsed / in_TempHrsEach

calc_Revenue   = dec_Beds_Tom * in_RevBed_Tom
               + dec_Beds_Car * in_RevBed_Car
               + dec_Beds_Mes * in_RevBed_Mes

calc_FertCost  = dec_Beds_Tom * in_Fert_Tom
               + dec_Beds_Car * in_Fert_Car
               + dec_Beds_Mes * in_Fert_Mes

calc_LaborCost = fn_LaborCost(calc_HoursTotal)
calc_FixedCost = in_FixedCost
calc_TotalCost = calc_LaborCost + calc_FertCost + calc_FixedCost
calc_Profit    = calc_Revenue - calc_TotalCost
calc_BedsUsed  = dec_Beds_Tom + dec_Beds_Car + dec_Beds_Mes
```

### 3.5 Incremental cost at the chosen mix — `3_Optimize`

One line per direction per crop; tomato shown.

```
calc_IncNext_Tom = fn_LaborCost(calc_HoursTotal - calc_Hours_Tom
                     + fn_Hours(dec_Beds_Tom + 1, in_HrsWkBed_Tom, in_Dim_Tom))
                 - calc_LaborCost + in_Fert_Tom

calc_IncLast_Tom = calc_LaborCost
                 - fn_LaborCost(calc_HoursTotal - calc_Hours_Tom
                     + fn_Hours(dec_Beds_Tom - 1, in_HrsWkBed_Tom, in_Dim_Tom))
                 + in_Fert_Tom
```

The optimum satisfies, for every crop, either `calc_IncNext_* > in_RevBed_*` (economic
stop) or that crop sitting at its bed cap (capacity stop).

---

## 4. The Solver region — `3_Optimize`

Five labeled regions, top to bottom, sized to fit one screen so the whole optimization is
visible without scrolling. Nothing else goes on this sheet.

### Region A — Decision cells

Three cells, named, shaded as inputs, integer-formatted, bordered, labeled
`DECISIONS — Solver changes these`. They are the **only** cells Solver may change.

| Cell name | Start | Bounds |
|-----------|-------|--------|
| `dec_Beds_Tom` | 0 | integer, 0 … `in_BedCap_Tom` |
| `dec_Beds_Car` | 0 | integer, 0 … `in_BedCap_Car` |
| `dec_Beds_Mes` | 0 | integer, 0 … `in_BedCap_Mes` |

### Region B — Objective

One cell, `calc_Profit`, labeled `OBJECTIVE — maximize`, currency format, shaded
distinctly from the decisions. Directly beneath it, the profit build as a visible stack:
revenue, less labor, less fertilizer, less fixed, equals profit. The objective must
decompose on screen without opening another sheet.

### Region C — Constraint panel

One row per constraint. Columns: **Constraint | Used | Limit | Slack | Status**, where
`Status` is `IF(slack < 0, "VIOLATED", IF(slack = 0, "BINDING", "slack"))`. This panel is
what makes a binding constraint legible without opening the Solver dialog.

| Constraint | Used | Limit |
|------------|------|-------|
| Total beds | `calc_BedsUsed` | `in_BedsTotal` |
| Tomato beds | `dec_Beds_Tom` | `in_BedCap_Tom` |
| Carrot beds | `dec_Beds_Car` | `in_BedCap_Car` |
| Mesclun beds | `dec_Beds_Mes` | `in_BedCap_Mes` |
| Temp hours | `calc_TempHoursUsed` | `cap_TempHours` |
| Temp workers | `calc_TempWorkers` | `in_TempMax` |

### Region D — Solver parameter block

A static table reproducing exactly what goes into the Solver dialog, so the setup is
auditable from the sheet and rebuildable if the saved model is lost.

| Solver field | Entry |
|--------------|-------|
| Set Objective | `calc_Profit` |
| To | Max |
| By Changing Variable Cells | `dec_Beds_Tom`, `dec_Beds_Car`, `dec_Beds_Mes` |
| Subject to | `dec_Beds_Tom <= in_BedCap_Tom` |
| | `dec_Beds_Car <= in_BedCap_Car` |
| | `dec_Beds_Mes <= in_BedCap_Mes` |
| | `calc_BedsUsed <= in_BedsTotal` |
| | `calc_TempHoursUsed <= cap_TempHours` |
| | `dec_Beds_Tom`, `dec_Beds_Car`, `dec_Beds_Mes` = integer |
| Make Unconstrained Variables Non-Negative | checked |
| Solving Method | **Evolutionary**, or GRG Nonlinear with Multistart |
| Convergence | 0.0001 |
| Mutation / Population Size | 0.075 / 100 |
| Max Time without Improvement | 60 s |

**Simplex LP is not valid.** The objective contains `(1 + dim) ^ q`, so this is a
non-linear integer program. Simplex will either refuse the model or return a wrong answer
that looks clean.

Three cells beneath the table record every run: engine used, starting values, resulting
profit. A non-linear integer search is not guaranteed to reach the global optimum from an
arbitrary start, so the run does not certify itself — checks C1 and C7.3 do.

### Region E — P = MC readout

Two rows per crop, showing the economics of the answer directly on the optimization sheet.

| Crop | Last bed planted | Next bed | Revenue | Stop reason |
|------|-----------------|----------|---------|-------------|
| Tomato | `calc_IncLast_Tom` | `calc_IncNext_Tom` | `in_RevBed_Tom` | `economic` or `at cap` |
| Carrot | `calc_IncLast_Car` | `calc_IncNext_Car` | `in_RevBed_Car` | `economic` or `at cap` |
| Mesclun | `calc_IncLast_Mes` | `calc_IncNext_Mes` | `in_RevBed_Mes` | `economic` or `at cap` |

---

## 5. Validation rules — acceptance criteria

Seven groups on `5_Checks`, each sub-row returning PASS or FAIL. The workbook is not
finished until every row passes. All figures were computed from the §1 inputs.

### C1 — The published answer

| | Test | Criterion |
|---|------|-----------|
| C1.1 | Published profit | At 10 / 20 / 30, `calc_Profit` = **$42,761.66**, the published **$42,762** to the dollar. Test `ROUND(calc_Profit, 0) = 42762` |
| C1.2 | Solver reproduces it | Solver returns 10 / 20 / 30 |
| C1.3 | Optimum is local-best | No ± 1-bed neighbour of the optimum scores higher. Exhaustive enumeration of all 10,416 feasible mixes confirms it is also the global optimum |

### C2 — Profit decomposition at the published mix

| | Component | Criterion |
|---|-----------|-----------|
| C2.1 | Revenue | $210,880 |
| C2.2 | Fertilizer | $44,000 |
| C2.3 | Labor | $104,118.34 |
| C2.4 | Fixed | $20,000 |
| C2.5 | Total cost | $164,118.34 |
| C2.6 | Owner tier | 720.00 hours costing exactly $25,000.00 |
| C2.7 | Temp tier | 4,557.216 hours costing $79,118.34 |

### C3 — The labor formula

Spot-checks against the worked example. All in hours.

| | Crop and bed | Cumulative | Marginal |
|---|------------|-----------:|---------:|
| C3.1 | Tomato, bed 1 | 99.00 | 99.00 |
| C3.2 | Tomato, bed 2 | 217.80 | 118.80 |
| C3.3 | Tomato, bed 3 | 359.37 | 141.57 |
| C3.4 | Tomato, bed 10 | 2,334.37 | 424.43 |
| C3.5 | Tomato, bed 11 | 2,824.59 | 490.22 |
| C3.6 | Carrot, bed 20 | 983.170 | — |
| C3.7 | Mesclun, bed 30 | 1,959.678 | — |
| C3.8 | Total at the optimum | 5,277.216 | — |

### C4 — The rate break

| | Test | Criterion |
|---|------|-----------|
| C4.1 | Break located | Cumulative tomato hours first exceed 720 at `sch_q` = 5 (724.73) |
| C4.2 | MC peaks before it | `sch_MC_Tom` at bed 5 = $7,660.86 |
| C4.3 | MC drops at it | `sch_MC_Tom` at bed 6 = $4,906.28 — a fall of $2,754.58, the only decrease permitted anywhere in the series |
| C4.4 | Monotonic otherwise | `sch_MC_*` strictly increases at every other step, all three crops |

### C5 — Which constraints bind, and why

The economics of the answer. A model can hit $42,762 and still get this wrong.

| | Test | Criterion |
|---|------|-----------|
| C5.1 | Tomato stops on economics | Interior at 10 of 20. `calc_IncLast_Tom` = $8,248.59 < $8,800 ≤ `calc_IncNext_Tom` = $9,390.72 |
| C5.2 | Carrot stops on capacity | At its cap of 20, with `calc_IncNext_Car` = $1,741.51 still **below** revenue of $2,094 — the farm would plant more if allowed |
| C5.3 | Mesclun stops on capacity | At its cap of 30, with `calc_IncNext_Mes` = $2,453.53 still **below** revenue of $2,700 |
| C5.4 | Total beds is slack | 60 of 64. A model that drives this to 64 is wrong |
| C5.5 | Temp cap is slack | 4,557.216 of 5,760 hours; 3.165 of 4 workers |
| C5.6 | Owner pool is the binding resource | `calc_OwnerHoursUsed` = `in_OwnerFieldHrs` exactly |

### C6 — The `q = 1` hand calculation

Tomatoes, one bed, nothing else planted.

```
hours(1) = 1 × in_HrsWkBed_Tom × in_WeeksSeason × (1 + in_Dim_Tom) ^ 1
         = 1 × 2.5 × 36 × 1.10
         = 90 × 1.10
         = 99.00 hours

rate_Owner = in_OwnerPay / in_OwnerHrsPaid = 50,000 / 1,440 = $34.722222… per hour

99.00 <= in_OwnerFieldHrs (720), so every hour is charged at rate_Owner and no
temp labor is used.

labor(1) = 99.00 × 50,000 / 1,440 = 4,950,000 / 1,440 = $3,437.50   exactly
fert(1)  = in_Fert_Tom                                 =   $880.00
sch_MC_Tom at sch_q = 1                                = $4,317.50

in_RevBed_Tom $8,800 > $4,317.50  ->  PLANT

contribution of bed 1    = 8,800 - 4,317.50            = $4,482.50
calc_Profit at 1 / 0 / 0 = 4,482.50 - in_FixedCost     = -$15,517.50
```

**Acceptance:** `sch_Hours_Tom` = 99.00, `sch_MC_Tom` = $4,317.50, and `calc_Profit` at
1 / 0 / 0 = −$15,517.50, each to the cent. The negative profit is correct — one bed cannot
carry $20,000 of fixed cost — and it confirms fixed cost sits in the profit line and not
in any MC schedule.

### C7 — Workbook hygiene

| | Test | Criterion |
|---|------|-----------|
| C7.1 | No error cells | `COUNTIF` over every used range for `#DIV/0!`, `#VALUE!`, `#REF!`, `#NAME?`, `#N/A`, `#NUM!`, `#NULL!` returns 0 |
| C7.2 | Every calculated cell contains a formula | Non-formula cells outside the `1_Inputs` input block = 0, tested with `ISFORMULA` over each used range |
| C7.3 | Fixed cost does not move the optimum | Setting `in_FixedCost` to 0 leaves the mix at 10 / 20 / 30 and raises profit by exactly $20,000 |
| C7.4 | Both MC measures agree here | At the optimum, `INDEX(sch_MC_Tom, dec_Beds_Tom)` = `calc_IncLast_Tom`, and likewise for carrots and mesclun — the §2.1 coincidence, tested rather than assumed |
| C7.5 | Inputs entered unrounded | `in_HrsWkBed_Car × 3` = `in_HrsWkBed_Tom` and `in_HrsWkBed_Mes × 2` = `in_HrsWkBed_Tom`, both exactly — catches the §1.5 traps |
| C7.6 | No hardcoded numbers in formulas | No formula outside `1_Inputs` contains a literal other than 0, 1, or a row index |
| C7.7 | No cell addresses in named logic | Every formula on `3_Optimize` and `4_Results` references named ranges only |
| C7.8 | Integer beds | Each `dec_Beds_*` equals its own `ROUND(…, 0)` |

---

## 6. Outputs

Reported on `4_Results`. Each is a named cell; none performs original calculation.

| Name | Reports | Unit | At the optimum |
|------|---------|------|---------------:|
| `out_Beds_Tom` | Optimal tomato beds | beds | 10 |
| `out_Beds_Car` | Optimal carrot beds | beds | 20 |
| `out_Beds_Mes` | Optimal mesclun beds | beds | 30 |
| `out_BedsUsed` | Total beds planted, against 64 | beds | 60 |
| `out_Profit` | Maximum season profit | $ | 42,761.66 |
| `out_Revenue` | Total season revenue | $ | 210,880 |
| `out_LaborCost` | Total field labor cost | $ | 104,118.34 |
| `out_FertCost` | Total fertilizer cost | $ | 44,000 |
| `out_FixedCost` | Fixed cost | $ | 20,000 |
| `out_TotalCost` | Total cost | $ | 164,118.34 |
| `out_HoursTotal` | Total field hours required | hours | 5,277.216 |
| `out_OwnerHours` | Hours at `rate_Owner` | hours | 720.000 |
| `out_TempHours` | Hours at `rate_Temp` | hours | 4,557.216 |
| `out_TempWorkers` | Temp workers implied | workers | 3.165 |
| `out_MCLast_Tom` / `out_MCNext_Tom` | Incremental cost, last and next tomato bed | $ / bed | 8,248.59 / 9,390.72 |
| `out_MCLast_Car` / `out_MCNext_Car` | Same, carrots | $ / bed | 1,688.95 / 1,741.51 |
| `out_MCLast_Mes` / `out_MCNext_Mes` | Same, mesclun | $ / bed | 2,420.10 / 2,453.53 |
| `out_StopReason_Tom` | Why tomatoes stop | text | economic |
| `out_StopReason_Car` | Why carrots stop | text | at cap |
| `out_StopReason_Mes` | Why mesclun stops | text | at cap |
| `out_BindingConstraints` | Constraints at their limit | text | carrot cap, mesclun cap, owner field hours |
| `out_ChecksPassed` | §5 sub-rows passing, over total | count | 37 / 37 |

The "At the optimum" column states the expected result. Every one of those cells must
compute; none may be typed.

### 6.1 Required chart

One chart: `sch_MC_Tom`, `sch_MC_Car`, `sch_MC_Mes` against `sch_q`, with three
horizontal reference lines at `in_RevBed_Tom`, `in_RevBed_Car` and `in_RevBed_Mes`. Each
crop's economic optimum is where its MC series crosses its own revenue line. Two features
must be visible:

1. The tomato series crossing $8,800 between beds 10 and 11.
2. The tomato series stepping **down** between beds 5 and 6, where the owner's 720 hours
   run out and the cheaper temp rate takes over.

The carrot and mesclun series never reach their revenue lines within their caps. That is
the picture of C5.2 and C5.3, and the reason those two crops are capacity-constrained
rather than economically stopped.
