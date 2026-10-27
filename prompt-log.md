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
