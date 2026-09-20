# AI conventions

## About this repository
The professional portfolio of Isaiah Charles Stewart — corporate finance and FP&A —
Global MBA candidate, Shidler College of Business, University of Hawaiʻi at Mānoa.
Canonical file: AGENTS.md. CLAUDE.md points here.

## Where things are
- capabilities/<capability>/  a capability, with its README, spec and model
- docs/briefs/          written BEFORE work: scope + hypothesis
- docs/decisions/       written AFTER work: recommendations
- analysis/             findings and figures
- data/                 sourced inputs, with provenance

No directory is ever named after a course, a semester, or a week.

## Naming
- The directory matters most. A file in the wrong folder may not be found
  at all. If you are not certain which folder a file belongs in, ask me
  before you write it — do not choose for me.
- Graded files use the exact filename the stage brief gives — lowercase,
  hyphens, no spaces. Some courses date-stamp (YYYY-MM-DD-lastname-slug.md);
  the stage page says so when they do.
- Slugs name the engagement, never the week, the course, or the assignment
  number.
- Never invent a path or a filename. I will give you the exact one.

## My field
Financial planning and analysis: annual budgeting, long-range forecasting,
driver-based modeling, capital planning and business cases (NPV, ROI, sensitivity
analysis), variance and benchmarking analysis, and executive reporting — across
aviation, commercial real estate, and education and endowment finance. See RESUME.md.

## How I work
- Explain concepts fully and walk the worked example. Do not hand me conclusions.
- I am fluent in corporate finance — you do not need to teach me NPV, IRR,
  contribution margin, or a three-statement build. Do walk the full derivation for
  anything new to me: an unfamiliar method, a library, a dataset, a statistical test.
- Show every assumption: each rate, growth factor, and allocation basis, and where it
  came from. An unsourced number is worse than no number.
- Units, currency, and period on every figure. "$4.2M FY27 operating expense" — not "4.2".
- Say which two or three inputs move the result most, and by how much.
- Critique my reasoning directly. I would rather be corrected than agreed with.
- When you are uncertain, say so and say what would resolve it.
- In spreadsheets, calculated cells are formulas, never hardcoded values. Only raw
  source data may be typed as a literal.

## What you may and may not draft
- You MAY explain, critique, debug, quiz me, and draft mechanical files.
- You MAY NOT write my briefs, analyses, memos, or reflections.
- Every statistic or figure you give me is a draft until I verify it against a source.
- Never invent a figure. If you do not have a value, say so or leave a marked
  placeholder — never supply a plausible-looking number, not even as an example.

## Never include
No credentials, no API keys, no personal data about anyone, no licensed or
copyrighted material. If I paste something that fits that description, stop and
tell me rather than committing it.

That rule is not abstract in my case. None of the following goes into a prompt, a file
here, or any AI tool — not in full, not in excerpt, not "anonymized" by deleting a name:

- **Kamehameha Schools.** Student, applicant, scholarship, and financial-aid records,
  or anything identifying an individual learner or beneficiary — FERPA-protected in
  every form. Endowment and trust data: holdings, manager-level returns, spending-policy
  calculations, non-public trust financials. Donor records and gift terms. Internal
  program budgets, actuals, forecasts, and board packages. Employee records.
- **Hawaiian Airlines, American Airlines.** Route- and flight-level profitability,
  network and schedule plans, and competitive benchmarking built from internal data.
  Union and workforce data: wage rates, bargaining positions, employment and demographic
  files. Vendor and supplier terms: negotiated rates, invoice terms, contract language,
  retrofit and capital project schedules.
- **HomePro, Step Up Technology.** Client project economics and deal terms.
- **Any public company.** Material non-public information, including pre-release results
  and internal forecasts.
- **Anywhere.** Raw extracts from Hyperion/EPM, SAP, or any internal system. Personal
  identifiers of any kind — SSNs, employee or student IDs, addresses, personal emails.

For coursework and anything published here, use public filings, synthetic figures, or
numbers I have explicitly cleared, and label which in the file.

## Documentation
When work changes, update the document that describes it in the same commit.
A capability's README names the engagements that exercised it — keep that current.

## Scope
Do the work I asked for. If you notice something worth doing that I did not ask
for, tell me instead of doing it.

## Commits
Descriptive messages: what changed and why. Never "update" or "stuff".

## Prompt log
At the end of every session that changed a file, append one entry to
prompt-log.md: the date, what I asked, what you produced, what was wrong and
how it was caught. Never backfill earlier sessions and never edit a past entry.

## Mistakes to avoid (append to this list)
Record errors here as they happen, so the same one does not repeat.
- 2026-09-20 — Wrote this file's "How I work" section as "answer first, lead with the
  recommendation, I should be able to stop reading after two sentences" while the
  baseline was unreachable. That is the opposite of the baseline's standing rule:
  explain fully, walk the worked example, do not hand me conclusions. Caught only when
  I pasted the baseline in. An agent guessing at a convention will guess toward the
  habits of my job, not the purpose of my coursework.
