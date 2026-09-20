# AGENTS.md

Instructions for AI agents (Claude Code, Codex, and any other tool that reads this convention)
working in this repository. `CLAUDE.md` is a one-line pointer here — this file is canonical.

This is the portfolio repository of **Isaiah Charles Stewart**, Honolulu, HI.

## My Field

I work in **corporate finance — financial planning and analysis (FP&A)**: annual budgeting,
long-range forecasting, driver-based modeling, capital planning and business cases (NPV, ROI,
sensitivity analysis), variance and benchmarking analysis, and executive reporting. My experience
spans aviation, commercial real estate, and education/endowment finance. I am a Global MBA student
at the Shidler College of Business, University of Hawaiʻi at Mānoa.

Read `RESUME.md` for the full background. Assume a finance-literate author: you do not need to
explain NPV, IRR, contribution margin, or a three-statement build to me.

## Data That Must Never Be Pasted Into a Model

Nothing below goes into a prompt, a file in this repository, or any AI tool — not in full, not in
excerpt, not "anonymized" by deleting a name. If a task seems to need it, stop and ask me.

**From Kamehameha Schools (current employer):**
- Student, applicant, scholarship, and financial-aid records, or anything identifying an
  individual learner or beneficiary — FERPA-protected in every form
- Endowment and trust data: portfolio holdings, manager-level returns, spending-policy
  calculations, and any non-public trust or `ʻāina`/asset financials
- Donor records and gift terms
- Internal program budgets, actuals, forecasts, and board-reporting packages
- Employee records: names, IDs, salaries, headcount rosters, performance information

**From prior employers (Hawaiian Airlines, American Airlines, HomePro, Step Up Technology):**
- Route-level and flight-level profitability, network and schedule plans, and competitive
  benchmarking built from internal data
- Union and workforce data: wage rates, bargaining positions, employment and demographic files
- Vendor and supplier terms: negotiated rates, invoice terms, contract language, retrofit and
  capital project schedules
- Client commercial real estate project economics and deal terms
- Material non-public information of any public company, including pre-release results and
  internal forecasts

**Across all of it:**
- Raw extracts from Hyperion/EPM, SAP, or any internal system
- Personal identifiers of any kind: SSNs, employee or student IDs, home addresses, personal emails
- Credentials, API keys, and internal URLs or hostnames

For coursework and anything published here, use **public filings, synthetic figures, or numbers I
have explicitly cleared**. When a model needs realistic inputs, build them from public sources and
label them as such in the file.

## How I Want Things Explained

- **Answer first.** Lead with the recommendation or the number, then the drivers behind it. I
  should be able to stop reading after two sentences and still know what you concluded.
- **Show the assumptions.** Name every rate, growth factor, and allocation basis you used, and
  where it came from. An unsourced number is worse than no number.
- **Units, currency, and period on every figure.** "$4.2M FY27 operating expense" — not "4.2".
- **Plain language over jargon**, but do not over-explain finance fundamentals to me. Define
  anything specific to a tool, a library, or a dataset on first use.
- **Say what would change the answer.** Which two or three inputs move the result most, and by
  how much.
- **Tables beat paragraphs** for more than three numbers.
- **Never invent a figure.** If you do not have a value, say so or leave a marked placeholder —
  do not supply a plausible-looking number, not even as an example.
- **Flag disagreement plainly.** If my framing or an assumption looks wrong, say so once, clearly,
  and then do the work as asked.
- In spreadsheets, **calculated cells are formulas, never hardcoded values**. Only raw source data
  may be typed as a literal.

## What You May Not Write

**You may not write my briefs, analyses, memos, or reflections.** Those are mine — the thinking is
the point of the work, and a portfolio of your prose is worth nothing.

You may: check my math, pressure-test an assumption, build and debug models and code, format and
structure files, gather and cite public data, point out what I have left out, and ask the questions
that make my draft sharper. Where the line is unclear, ask before writing prose.

## Repository Structure

- `README.md` — profile bio. Mine to write.
- `RESUME.md` — current resume.
- `AGENTS.md` / `CLAUDE.md` — these instructions; `CLAUDE.md` only points here.
- `prompt-log.md` — running log of AI sessions (see the standing rule below).
- `capabilities/` — one folder per capability, each with `README.md`, `spec.md`, and the model file.
- `docs/briefs/` — documents written **before** work begins: framing, scope, the question.
- `docs/decisions/` — recommendations written **after** the work: what was decided and why.
- `data/` — source data, kept separate from anything derived.
- `analysis/figures/` — charts and exhibits produced by the analyses.

Do not create directories named after a course, a semester, or a week. Work is organized by
capability and by artifact type, so it stays legible to someone who has never seen a syllabus.

## Naming

<!-- PLACEHOLDER — THIS IS NOT THE BASELINE TEXT.
     The baseline at https://adamwstauffer.github.io/ai-lms/ai-conventions.html could not be
     reached from the session that created this file (blocked by network egress policy), so its
     Naming section could not be copied word for word as required.
     ACTION: replace everything between these two comment markers with the baseline's Naming
     section, verbatim. The rules below are a stand-in derived from this repository's own
     structure — they carry no authority until that swap is made. -->

- Directories and files use lowercase kebab-case: `capabilities/scenario-modeling/`,
  `docs/briefs/fleet-capex-brief.md`.
- Documents in `docs/briefs/` and `docs/decisions/` are dated first: `YYYY-MM-DD-<slug>.md`.
- Capability folders are named for the capability, not the assignment that produced them.
- Model files carry the capability name: `capabilities/<capability>/model.xlsx`.
- No course codes, semesters, week numbers, or personal names in any path.

<!-- END PLACEHOLDER -->

## Prompt Log

At the end of every session that changed a file, append one entry to
prompt-log.md: the date, what I asked, what you produced, what was wrong and
how it was caught. Never backfill earlier sessions and never edit a past entry.
