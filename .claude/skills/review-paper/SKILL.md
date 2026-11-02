---
name: review-paper
description: Peer-review a draft paper the way a venue would. Three simulated reviewers (methods & evidence, related work & novelty, clarity & format) plus a meta-review, written as a ranked list of pointers. It never edits the paper. Reads the LaTeX in a project's paper/latex/, the compiled PDF, or any external paper PDF. On re-runs it tracks which earlier points were fixed. Use when the user wants the paper reviewed, checked or critiqued, asks "what should I change", "рецензија", "recenzija", "провери го трудот", or has finished writing the prose after /write-paper.
user-invocable: true
---

# Review the paper: three reviewers + meta-review

Runs after stage 7 (`/write-paper`) and can be repeated while the user revises. Read
`CLAUDE.md` first. Written in English. **The reviewer never edits the paper.** The
`.tex`, `.bib` and figures are only read. All feedback goes to a review file as
pointers that the user acts on.

## 0. Find the paper and the context

**Vault project** (`projects/<slug>/`, usually `stage: 7`):
- Read `paper/latex/*.tex`, `references.bib`, `paper/outline.md`, `project.md`
  (question, hypothesis, `target`, `template`), `results/results.md`,
  `results/numbers.json`, and the source notes the project links to.
- The PDF: use `paper/latex/main.pdf` if it exists and is newer than the `.tex` files.
  Otherwise compile it if `latexmk`/`pdflatex` is installed. If neither works, ask the
  user to download the PDF from Overleaf into `paper/latex/`. If they don't, review
  from the LaTeX alone and say that layout and page count were not checked.
- Review files go to `paper/reviews/review-YYYY-MM-DD.md` (add `-2` and so on for
  more runs on the same day).

**External paper** (a PDF or LaTeX folder outside any project):
- Read it in full. There is no `numbers.json`, so the evidence checks use only the
  paper's own internal consistency.
- Ask the target venue if it isn't clear. Write the review to
  `support/reviews/<short-title>-YYYY-MM-DD.md`.

**Venue calibration.** Use `target` from `project.md`, or ask with `AskUserQuestion`:
course seminar, student conference, or a named conference/journal. For a named venue,
open its call for papers / author guidelines and its review form if it is public.
Use them for the page limit, anonymisation, required sections and the scoring scale,
and note the URL in the review. Default scale when nothing is given: 1 strong reject,
2 reject, 3 weak reject, 4 borderline, 5 weak accept, 6 accept, 7 strong accept.
Hold the paper to the bar of *that* venue. A seminar paper is not judged like a
journal paper.

**Previous reviews.** If earlier review files exist, read the latest one. Its numbered
points are passed to the reviewers in step 1.

## 1. Three reviewers, run in parallel

Spawn three `general-purpose` subagents in **one** message. Each gets: the paper paths,
the venue and its bar, the project context files from step 0, its own brief below,
the output format below, and the ground rules:
- Read the paper in full, not excerpts. Cite every point by section and by `.tex`
  file:line (or PDF page if there is no LaTeX).
- No invented references. Any paper it names must have been found and opened in this
  run, and come with a DOI, arXiv id or working URL.
- Don't rewrite the prose. Say what is wrong, why it matters and what kind of change
  would fix it. A short example phrasing is fine only if the point can't be explained
  without one.
- Be specific and honest. No generic praise and no padding. If something is fine,
  don't invent a problem.

**R1: Methods & evidence**
- Every number in the text, tables and captions matches `results/numbers.json` (list
  each mismatch with both values). Every figure and table is referenced in the text
  and supports what the text says about it.
- Claims stay within what the results show. No causal language for associational
  findings, no generalising beyond the dataset, effect sizes and uncertainty
  reported, not just p-values or a single run.
- The method is described well enough to reproduce: data, preprocessing, splits,
  hyperparameters, seeds, hardware, code availability.
- The hypothesis decision is stated and matches `results.md`. Negative or mixed
  results are reported honestly. Baselines and statistical tests are appropriate.
- Limitations and threats to validity: which are missing, which are understated.

**R2: Related work & novelty**
- Every `\cite` resolves to a `references.bib` entry. Every bib entry is real (open its
  DOI/URL), has correct metadata, and is cited for what it actually says. Check this
  against the source note and, if needed, the paper itself. Claims a source cites
  from someone else are not attributed to it.
- Search the web (WebSearch, extended mode for niche topics) for close prior work the
  paper misses, especially anything that weakens a novelty claim. List each one with
  its link, why it matters, and where in the paper it belongs. Don't create source
  notes. The user saves papers with `/add-source`.
- The gap → question → contribution chain in the Introduction is clear, and the
  contribution is stated as precisely as the evidence allows.
- How the paper positions itself against the closest work: is the difference stated
  and fair?

**R3: Clarity, structure & format**
- IMRaD flow. The abstract states the context, gap, method, main result *with a number*,
  and implication. The introduction ends with the question and hypothesis. The
  discussion answers the question. The conclusion adds nothing new.
- Paragraph-level logic: topic sentences, transitions, undefined terms and acronyms,
  inconsistent notation or naming.
- English: unclear or overlong sentences, wrong tense and register, overclaiming
  words ("prove", "significant" without a test, "novel"). Group repeated issues
  instead of listing every instance.
- Figures and tables: legible at print size, axis labels with units, captions that
  stand alone, colour-blind-safe, no wasted space (check in the PDF).
- Template and venue rules: page limit (from the PDF), anonymisation, required
  statements (data/code availability, AI-use disclosure if the venue or course asks),
  reference style, and LaTeX warnings (overfull boxes, `??` references) in the
  compile log.

**Output format for each reviewer** (returned as its final report):

```
## Reviewer N: <focus>
**Summary of the paper** (3–4 sentences, in the reviewer's own words)
**Strengths** (bullets, specific)
**Weaknesses**: numbered, each tagged [major] / [minor] / [nit]:
  N.k [major] <problem> (§3.2, main.tex:142)
       Why it matters: …  What would fix it: …
**Questions to the authors** (things a rebuttal would have to answer)
**Previous points** (re-runs only): each earlier point in this reviewer's area →
  resolved / partly / open, with one line of evidence
**Score**: x/7   **Confidence**: 1–5
```

## 2. Meta-review and the review file

Merge the three reports into the review file:

```
---
type: review
paper: <path>
venue: <target>
date: YYYY-MM-DD
previous: <previous review file or "">
recommendation: <accept | minor revision | major revision | reject>
---
# Review: <paper title>

## Meta-review
3–6 sentences: what the paper does, its main strength, the decisive problems,
recommendation for this venue.

## Fix these first
Ranked list of the 5–10 most important points across all reviewers, merged where they
overlap. Each has an id (M1, M2, …), location, problem, why, and kind of fix.
Majors first, ordered by impact per effort.

## Progress since last review   (re-runs only)
Table: earlier id | point | status (resolved / partly / open) | evidence

## Missing related work
Verified papers with links (from R2), each with where it belongs.

## Full reports
### Reviewer 1: Methods & evidence …
### Reviewer 2: Related work & novelty …
### Reviewer 3: Clarity, structure & format …
```

Before writing, check the merged report yourself. Drop any point that misreads the
paper (re-read the cited lines). Drop any reference you can't confirm was opened.
Remove duplicates. Keep the ids stable across runs: a point that is still open keeps
its old id.

## 3. Finish

- Tell the user the recommendation, the top 3–5 points, and the path to the review
  file. Give links for any papers mentioned.
- Vault project: append a Log entry to `project.md`
  (`YYYY-MM-DD — review-paper: <recommendation>, N major / M minor open, see paper/reviews/…`).
  `stage` stays 7.
- Offer the next step: fix the points and run `/review-paper` again, or save missing
  papers with `/add-source`.
