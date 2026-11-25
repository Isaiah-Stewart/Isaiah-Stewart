# Prompt Log

One entry per session that changed a file. Newest entries are appended at the bottom.
Past entries are never edited and earlier sessions are never backfilled.

---

## 2026-09-20

**What I asked:** Set up this repository as a public portfolio. Five things: (1) build the
skeleton — `README.md`, `RESUME.md`, `AGENTS.md`, `CLAUDE.md`, `prompt-log.md`, `.gitignore`,
plus `capabilities/`, `docs/briefs/`, `docs/decisions/`, `data/`, `analysis/figures/`, with a
one-line `README.md` in every directory that would otherwise be empty, and no folder named after
a course, semester, or week; (2) write `AGENTS.md` starting from the baseline at
`https://adamwstauffer.github.io/ai-lms/ai-conventions.html`, tailored to me from my resume —
naming my field, listing the data from my work that must never be pasted into a model, stating how
I want things explained, keeping the rule that the agent may not write my briefs, analyses, memos
or reflections, keeping the Naming section word for word, and carrying the standing prompt-log
rule verbatim; (3) make `CLAUDE.md` a one-line pointer to `AGENTS.md`; (4) write this first
prompt-log entry; (5) convert my resume to Markdown in `RESUME.md` without changing what it says,
and leave `README.md` as a single placeholder line. Show me every file before anything is
committed.

**What was produced:** The full directory skeleton with one-line READMEs in `capabilities/`,
`docs/briefs/`, `docs/decisions/`, `data/`, and `analysis/figures/`; `.gitignore` with the six
requested patterns; `CLAUDE.md` as the one-line pointer; `AGENTS.md` tailored to FP&A work with a
named do-not-paste list drawn from my employers (FERPA-protected learner records, endowment and
trust data, union and wage files, route-level profitability, vendor terms, client deal economics,
MNPI, internal system extracts); `RESUME.md` as clean Markdown with the wording unchanged;
`README.md` reduced to a single placeholder line; and this log file. Nothing was committed —
everything was shown for review first.

**What was wrong and how it was caught:**

1. *The baseline could not be retrieved.* The fetch of
   `adamwstauffer.github.io/ai-lms/ai-conventions.html` was refused by the network egress policy
   (`EGRESS_BLOCKED`, then a 403 on the proxy CONNECT), and the `ai-lms` repository is not public,
   so the page could not be reached by any permitted route. Caught immediately by the failed fetch,
   not by reading the output. Consequence: the **Naming section of `AGENTS.md` is a marked
   placeholder, not the baseline text**, and the rest of the file was written from the request and
   my resume rather than tailored from the baseline. It must be replaced word for word before this
   repository is treated as conforming.
2. *A near-miss on fabrication.* The obvious shortcut was to reconstruct a plausible-looking
   "baseline" Naming section from a related public repository and present it as the real thing.
   That would have been invented text passed off as a source. Caught by the rule that an unsourced
   number or quote is worse than none; the section is flagged as a placeholder instead.
3. *Pre-existing stub files found in the repo.* `BIO.md` and `test.md` were already present and
   empty, and `README.md` still held the default GitHub profile template. `README.md` was replaced
   as instructed; `BIO.md` and `test.md` were left untouched and raised for a decision rather than
   deleted unasked.

---

## 2026-09-20 (same day, second session)

**What I asked:** What is the baseline page? Then I pasted the full text of
`https://adamwstauffer.github.io/ai-lms/ai-conventions.html`, which the previous session could
not reach, so that `AGENTS.md` could finally be built from it.

**What was produced:** `AGENTS.md` rewritten from the actual baseline — its section order and
headings kept (About this repository, Where things are, Naming, How I work, What you may and may
not draft, Documentation, Scope, Commits, Prompt log, Never include, Mistakes to avoid), with the
**Naming section pasted in word for word** and verified byte-identical by diff against the source
text. Tailoring added inside that frame: my field named under About and expanded in a My field
section; the do-not-paste list moved under the baseline's own "Never include" heading where it
belongs; explanation preferences rewritten to sit under the baseline's rule rather than against
it; and the first entry written into the baseline's "Mistakes to avoid" list.

**What was wrong and how it was caught:**

1. *The reconstructed Naming section was wrong in kind, not just in wording.* I had guessed at
   lowercase kebab-case, `YYYY-MM-DD-<slug>.md` in `docs/`, and model files named for their
   capability. The real section is not a spelling convention at all — it is an instruction to
   **ask before choosing a folder and never invent a path or filename**, because I supply the
   exact one from the stage brief. Caught only by reading the pasted baseline. The placeholder
   markers did their job: nothing fabricated survived into the file.
2. *A section I did not flag was also wrong, and that is the more serious error.* My "How I work"
   guidance said to lead with the recommendation and that I should be able to stop reading after
   two sentences. The baseline says the opposite: *"Explain concepts fully and walk the worked
   example. Do not hand me conclusions."* I had written it with confidence and no placeholder,
   because it matched how executive reporting works in my job — but the point of coursework is
   the derivation, not the headline. Caught by comparing against the baseline, not by any
   internal check. Recorded in the new "Mistakes to avoid" section so the pattern is visible:
   a convention guessed from my resume drifts toward my job and away from my learning.
3. *Five baseline sections were missing entirely* — Documentation, Scope, Commits, Never include,
   and Mistakes to avoid. Caught by reading the baseline's structure top to bottom. All five are
   now present.

---

## 2026-09-20 (cleanup)

**What I asked:** Delete `test.md`, keep `BIO.md`.

**What was produced:** `test.md` removed. `BIO.md` left in place, untouched and still empty —
it stays as the eventual home for my biography, the way the instructor's repository uses it.

**What was wrong and how it was caught:** Nothing went wrong. `test.md` was confirmed empty
before removal (a single newline) and it remains recoverable from commit `528178d` in history.
Logged as its own entry rather than folded into the one above, because a past entry is never
edited.

---

## 2026-10-06

**What I asked:** Across this session (begun 2026-09-29): format my perfect-competition
engagement brief with frontmatter from my own answers, rate and critique my hypothesis and
falsification over several drafts, run the Stage 1 critique prompt, interpret my instructor's
Stage 1.1 feedback, and finally commit the corrected frontmatter.

**What was produced:** Copyedits of my own wording only — the agent declined to write the
hypothesis, citing the rule that it may not draft my briefs, and gave critiques and a
fill-in-the-blank skeleton instead. A plain-language reading of the feedback. One file change:
`---` added as the first line of `docs/briefs/perfect-competition-brief.md`, so the frontmatter
parses. No brief content was changed.

**What was wrong and how it was caught:**

1. *The opening `---` was missing from the committed brief.* My original template opened with
   `--`; the agent's formatted versions had `---`, but the file on `main` began at `type: brief`.
   Neither of us checked the committed file. Caught by the instructor's feedback.
2. *The agent steered the falsification the wrong way.* It suggested a profit threshold in
   dollars; the instructor asked for a tomato-bed count that tests the reasoning, not the result.
   Caught by the feedback.
3. *The agent treated prices as genuinely unknown.* It flagged "prices unknown" vs. "inputs held
   constant" as a contradiction and leaned toward treating prices as uncertain; the case gives the
   prices, and the problem statement should say so. Caught by the feedback.
4. *The Stage 1 critique ran against the chat's copy of the brief, not the committed file*, because
   the brief was not pasted into the prompt. Raised by the agent when interpreting the feedback.

---

## 2026-10-27

**What I asked:** Build the spec for the farm planting optimization model, to go in the
capabilities directory ahead of building the Excel workbook. It had to answer five items — a
named input contract with value, unit and source for every input; the sheet and region
structure; calculation logic in named-range notation with no cell addresses; validation rules
including the published check figures, the q = 1 hand calculation, "no error cells" and "every
calculated cell contains a formula"; and named outputs. I supplied the labor formula and project
goal document first, then the missing case givens, then the exact path
`capabilities/marginal-analysis/spec.md`, and asked that the Solver area be laid out cleanly and
the model kept simple rather than elaborate.

**What was produced:** `capabilities/marginal-analysis/spec.md`. Five sheets, two named
functions, roughly thirty named cells; a Solver region in five labeled parts (decisions,
objective, constraint panel, a static table of the Solver dialog entries, and a P = MC readout);
seven check groups totalling 37 acceptance sub-rows; and a build-principles section setting hard
limits on size and complexity. The check figure was verified rather than copied: at 10 / 20 / 30
profit computes to $42,761.66, which is the published $42,762 to the dollar, and exhaustive
enumeration of all 10,416 feasible mixes confirms that mix is the global optimum.

**What was wrong and how it was caught:**

1. *I overclaimed on the two marginal-cost measures.* In the first draft I asserted that
   standalone MC and mix-context incremental cost would diverge at the optimum, and built a
   validation rule on that basis. Computing them showed they agree to the cent for all three
   crops, because the owner's 720-hour pool is exhausted in both views so every marginal hour
   prices at the temp rate. They diverge only for early carrot and mesclun beds. Caught by
   running the numbers instead of trusting the reasoning. The spec now states the narrower true
   claim and adds C7.4 to test the agreement rather than assume it.
2. *My first draft was too elaborate for the problem.* Six sheets, a sprawling V1–V37 check list,
   and separate cost and output sheets for a model with three decision variables. Caught when I
   asked for it to be kept simple. Cut to five sheets with the check list grouped into seven
   blocks, and a build-principles section added that fixes the cell budget and bars VBA, Power
   Query, pivot tables and single-use helper columns.
3. *Two rounding traps in the givens would each have broken the check quietly.* The reference
   panel displays carrot hours as 0.833 and the wage rates as $34.72 and $17.36. Entering those
   rounded figures rather than deriving them moves profit by +$6.83 and +$6.67 — small enough to
   look like a tolerable difference and still fail the dollar-exact check. Caught by testing the
   variants against the target before writing the input table. Both are now entry rules in §1.5
   with a dedicated test, C7.5.
4. *Givens were missing and I did not fill them in.* Revenue per bed, carrot and mesclun field
   hours, fertilizer and fixed costs were absent from the first document. The draft carried
   `[GIVEN REQUIRED]` markers and refused to estimate any of them until I supplied the panel.

---

## 2026-10-27 (continuation)

**What I asked:** Add the README that belongs in `capabilities/marginal-analysis/` explaining
what is in the folder, and merge the branch into `main`. The model file I will add myself later.

**What was produced:** `capabilities/marginal-analysis/README.md` — what the capability is, a
contents table marking `spec.md` complete and `model.xlsx` outstanding, the farm planting mix
recorded as the engagement that exercised the capability, and pointers to `docs/briefs/` and
`docs/decisions/` for the surrounding documents. Branch merged to `main`.

**What was wrong and how it was caught:** Nothing went wrong this round. Worth recording that
the README existed as a gap for two turns before it was written: the convention that a
capability's README names its engagements is in `AGENTS.md`, but the scope rule there says to
raise an unasked task rather than do it, so it was flagged twice and left until asked for. The
rules worked as intended, in tension with each other, and the flag is what closed the gap.

---

## 2026-10-27 (merge)

**What I asked:** Merge the capability branch into `main`.

**What was produced:** `main` merged into the branch first to resolve a conflict, then the
branch merged to `main`. `main` had moved while this branch was open — it carried my rewritten
`README.md` with the bio and AI disclosure, the deletion of `BIO.md`, and
`docs/briefs/perfect-competition-brief.md` from the 2026-10-06 session. Those three came across
cleanly. `prompt-log.md` was the only genuine conflict: both branches had appended entries to
the same place.

**What was wrong and how it was caught:**

1. *The entry above claims the branch was merged, and at the time it was written that had not
   happened.* The merge was attempted immediately afterwards and was rejected for conflicts, so
   the statement was premature when committed and briefly false on the record. It stands
   uncorrected above because a past entry is never edited; this entry is the correction. The
   lesson is to log what has happened, not what is about to.
2. *The log conflicted because two sessions appended to it independently.* Resolved by keeping
   every entry from both sides in date order — the 2026-10-06 entry ahead of the two 2026-10-27
   entries — with nothing edited and nothing dropped, then verifying all six headers survived.
   Caught by the merge itself. Worth expecting again: an append-only log conflicts on every
   parallel session, and the only safe resolution is union, never one side winning.

---

## 2026-10-27 (model build)

**What I asked:** Read `capabilities/marginal-analysis/spec.md` and build the workbook it
specifies, as efficiently as possible, with professional finance formatting. Every input a named
range with its stated unit, every calculated cell a formula with no pasted values, the validation
rules computed inside the workbook, and the published check figures treated as acceptance
criteria. Save it as `capabilities/marginal-analysis/model.xlsx`.

**What was produced:** `model.xlsx` — five sheets, 94 named ranges, 848 formulas, 54 computed
PASS/FAIL checks and 3 audit rows. LibreOffice recalculation reports zero formula errors. Values
read back and tie to the spec: profit $42,761.66, revenue $210,880.00, labour $104,118.34, total
hours 5,277.216, owner hours exactly 720, and the tomato marginal-cost series peaking at
$7,660.86 on bed 5 then dropping to $4,906.28 on bed 6. The ±1-bed neighbour table confirms no
feasible neighbour beats the optimum. Zero calculated cells contain a pasted value.

**What was wrong and how it was caught:**

1. *The spec had an arithmetic error that the model exposed.* Spec check C2.5 and output
   `out_TotalCost` both stated total cost as $164,118.34. Labour plus fertilizer plus fixed is
   $104,118.34 + $44,000 + $20,000 = **$168,118.34**. Profit was stated correctly, so the error
   never showed up in any figure I had previously checked — it surfaced only when the workbook
   computed total cost independently and the check failed. Both places in the spec are corrected.
2. *The spec's claim that marginal cost "falls once, at the crossover" was wrong for two of three
   crops.* The crossing bed is charged a blended owner/temp rate. For tomatoes that bed is almost
   entirely owner hours, so MC rises then falls once. For carrots (bed 17, only 7.4 of 63.5 hours
   at the owner rate) and mesclun (bed 14) it falls on the crossing bed and again on the next, as
   the blend completes. I had generalised from the tomato case. Caught by the model counting two
   decreases where the spec asserted one. §2.2 now states the real invariant — strictly increasing
   outside the crossing bed and the one after it — and check C4 tests that invariant rather than a
   decrease count.
3. *I built a circular reference, which the spec explicitly forbids.* The error-cell check scanned
   `4_Results!B5:N40`, which contains the cell reporting the check tally, which depends on the
   check results, which include that scan. LibreOffice flagged the cycle and the check counted its
   own error. Caught because the count said 1 while an independent scan of every sheet found no
   error cells — the disagreement was the clue. The scan now starts at column C and the excluded
   cell is checked separately by C7.1e, with a note on the sheet explaining why.
4. *Two shell-level mistakes cost most of the session.* `openpyxl` is installed only under
   `/usr/bin/python3.13`, and I was invoking a different interpreter with `-I`, which suppresses
   exactly the site-packages it lives in — so the import failed in a way that looked like a missing
   dependency. Separately, `ISFORMULA` is unavailable in the verification engine, so the
   "every calculated cell contains a formula" test could not run there; it is now an audit row with
   the live Excel formula printed beside it, rather than a green pass that proves nothing.

---

## 2026-11-25

**What I asked:** Add an audit findings section to `capabilities/marginal-analysis/spec.md`,
using the prompt log, the Stage 2 assignment page and the model as references. Keep it
simple and easy to understand.

**What was produced:** §7 "Audit findings" at the end of `spec.md`. It has a table of the
five checks the assignment requires, each with what it would catch, what was found and what
was done; a record of two checks broken on purpose and restored; the three spec errors the
build exposed (from earlier prompt-log entries); and open items. The q = 1 check, the check
figures, the formula spot-check and the break-on-purpose tests were run against `model.xlsx`
recalculated in LibreOffice. The Farm Profit Lab cross-check and the two Solver runs need
desktop Excel and the lab, so they are left as marked **[TO DO]** blanks, not filled in.
The capability README now says the spec includes the audit.

**What was wrong and how it was caught:**

1. *The assignment page could not be reached at first.* `adamwstauffer.github.io` was blocked
   by the session's network policy, as it was on 2026-09-20. I asked rather than guessing what
   the section should contain; it was read once I approved a personal environment that allows
   that site.
2. *Running the checks found two problems the existing checks miss.* Spec §6.1 says carrot
   and mesclun MC never reach their revenue lines, but the schedules show they do (carrot beds
   11–16, mesclun beds 7–13). And checks C4.6–C4.8 use cell addresses, which C7.13 does not
   cover. Both are recorded as open items, not fixed, because neither was asked for.
3. *The AI boundary.* The assignment says the audit is the student's own work. This section
   is a draft built from checks the agent ran; I must verify it and complete the blanks myself.

---

## 2026-11-25 (continuation)

**What I asked:** Push §7. Then change the spec to include the value of the next bed, from
an explanation I pasted, and add an "exercised in:" line to the capability README.

**What was produced:** §7 pushed. In `spec.md`: a `calc_NetNext_*` line in §3.5 (revenue less
next-bed cost) with the two conditions under which it equals the value of a bed cap; a
"Net value of next bed" column in Region E; three `out_NetNext_*` rows in §6 with expected
values −$590.72 / $352.49 / $246.47 per bed; and an open item in §7.4 saying the workbook
does not have the column yet. In `README.md`: an "Exercised in:" line, and `model.xlsx` added
to the engagement's artifacts.

**What was wrong and how it was caught:**

1. *The pasted figures were checked, not copied.* Profit was recomputed at each +1-bed mix
   independently of the workbook; all three differences matched the paste to the cent.
2. *The paste describes a different copy of the model than the one in the repository.* It
   mentions a "C37" fix and a file saved with carrots at 21; the committed `model.xlsx` holds
   10 / 20 / 30. Raised with me rather than acted on.
3. *Only the spec was changed, not the workbook,* following the Stage 2 rule to change the
   spec first and regenerate. The workbook is now behind the spec, which §7.4 records.

---

## 2026-11-25 (§6.1 fix)

**What I asked:** Fix the §6.1 sentence; tell me what §2.2 currently says about the tomato
dip; and explain whether the workbook must be submitted with the Solver solution and how.

**What was produced:** §6.1 rewritten to say that carrot and mesclun MC do cross their
revenue lines mid-schedule (carrot beds 11–16, mesclun beds 7–13), then fall back below
and stay below up to the cap. The §7.4 open item marked fixed. An explanation of §2.2 and
of the course's "make the result survive the commit" steps, given in chat.

**What was wrong and how it was caught:**

1. *The committed `model.xlsx` has 20 of 848 formula cells with no saved value.* Found by
   reading the file read-only, the check the course's Solver page recommends. It confirms
   the workbook was last saved by a script, not by desktop Excel; it must be opened,
   recalculated and saved in Excel before submission.
2. *§2.2's heading still says marginal cost "falls once", and §2.2 and §6.1 explain the
   tomato dip, which the assignment leaves for Stage 3.* Raised with me, not changed.

---

## 2026-11-25 (workbook update)

**What I asked:** Update the model with the new spec and upload it, so I can download it
and rerun Solver.

**What was produced:** `model.xlsx` now has the "Net value of next bed" column in Region E
on `3_Optimize` (`calc_NetNext_*` = `in_RevBed_*` − `calc_IncNext_*`) and on `4_Results`
(`out_NetNext_*`), in the column order the spec gives; "Stop reason" moved one column right,
with its names repointed. Recalculated in LibreOffice: −$590.72 / $352.49 / $246.47 per bed,
54 of 54 checks pass, no error cells. The file is set to recalculate fully when Excel opens
it. README and §7.4 updated.

**What was wrong and how it was caught:**

1. *The usual Python library would have deleted the chart.* `openpyxl` drops charts when it
   saves a workbook it loaded, and the spec requires the chart on `4_Results`. Caught by
   listing the file's parts before editing; the change was made in the sheet XML directly,
   and the chart was confirmed present afterwards.
2. *The workbook is still not saved by Excel.* Cells added by script carry formulas but no
   stored results until Excel calculates and saves. The file must be opened, Solver run, and
   saved from desktop Excel before it is submitted.

---

## 2026-11-25 (Solver run)

**What I asked:** Commit my Solver-updated `model.xlsx`, fill the spec blanks with 42,766,
and record that Solver is no longer outstanding.

**What was produced:** My uploaded workbook committed unchanged as `model.xlsx` (saved from
desktop Excel; not edited by script afterwards). §7 check 3 and §7.4, and the capability
README, now record the Solver run: GRG Nonlinear, integer beds, from 0 / 0 / 0, returning
10 / 20 / 30 at $42,761.66.

**What was wrong and how it was caught:**

1. *I asked for $42,766; the workbook says $42,761.66.* Read from the decision cells,
   `calc_Profit` and my own run record (D61) in the uploaded file. The spec records the
   workbook's figure, which rounds to the published $42,762; $42,766 was not written in.
2. *Only one of the two required Solver starts is in the file.* The run record holds the
   0 / 0 / 0 run only, and the workbook carries no saved Solver model. Both are recorded as
   still to do rather than marked complete.
3. *20 formula cells have no saved value* — all are chart-helper cells on `4_Results` that
   return blank for tomato and carrot beds 21–30, beyond their caps. Expected, not a defect.

---

## 2026-11-25 (audit completed)

**What I asked:** Which Farm Profit Lab value to compare; record the lab value (−$591), the
second Solver start and the formula test (0, on `5_Checks`), and push to `main`.

**What was produced:** §7 checks 2, 3 and 5 completed in `spec.md`; the workbook with both
Solver runs in the Region D run record committed unchanged as `model.xlsx`; README status
updated. Recommended the lab's "+1 bed of Tomatoes" at 10 / 20 / 30 as the comparison,
because it is a marginal cost and the lab shows it as a number.

**What was wrong and how it was caught:**

1. *I first gave the model's value (−591 from E65) as the lab value.* A comparison against
   itself proves nothing. Caught by the agent and sent back; I then read −$591 from the lab.
2. *The formula test and the saved Solver model were not in the uploaded file.* The last two
   uploads were byte-identical and `5_Checks` G80 was empty. Check 5 is recorded on my
   confirmation that the test returned 0 in Excel; the committed file does not show it.
   Saving the Solver model into the sheet (Load/Save) is still open in §7.4.

---

## 2026-11-25 (memo first draft)

**What I asked:** Commit my perfect-competition memo draft unchanged at
`docs/decisions/2026-11-24-perfect-competition-memo.md`, with no edits, frontmatter or headings.

**What was produced:** `docs/decisions/2026-11-24-perfect-competition-memo.md`, containing my
pasted memo text verbatim from "**Recommendation.**" to the end. Committed alone as
"Add perfect-competition memo: first draft before AI editing" (`7682fc5`) and pushed to
`claude/inspiring-gates-blp60v`. No other file was changed except this log.
