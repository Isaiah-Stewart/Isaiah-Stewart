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
