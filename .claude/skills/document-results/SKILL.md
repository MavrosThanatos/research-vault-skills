---
name: document-results
description: Stage 6 of the research pipeline — turn a finished analysis into documented results, a results note in the vault (numbers, figures, hypothesis decision, limitations, how to reproduce) plus a half-page plain-language summary for non-experts. Use when a project's study has been run and the user wants the results written up or documented, "документирај резултати", "dokumentacija", or before writing the paper.
user-invocable: true
---

# Document the results

Whiteboard stage 6 — Документирај резултати. Read `CLAUDE.md` first. Input: a project
at `stage: 5` with `results/numbers.json`, figures and `notebooks/analysis.ipynb`.
Output: `results/results.md`, `results/summary.md`, `stage: 6`. All in English.

Every number comes from `results/numbers.json` — never retype or round from memory.

## 1. `results/results.md` — the full record

1. **Summary** — 3 sentences: question, what was found, does it support H1.
2. **Data** — dataset (link the source note), N after cleaning, period/population,
   cleaning steps (from the notebook and Log). Mention a dataset switch if one happened.
3. **Method** — as run, including any logged deviations from the plan.
4. **Results** — reported fully: test statistic, df, p-value, effect size with 95% CI
   (e.g. "t(212) = −3.41, p < .001, d = 0.47 [0.20, 0.74]"); for ML: metric mean ± SD
   across folds vs baseline. Embed figures with `![[fig_x.png]]` and a caption each.
5. **Hypothesis decision** — apply the decision rule from `project.md` literally:
   H1 supported / not supported. "Not supported" is a valid result; say so plainly.
6. **Interpretation** — what it means, kept separate from the numbers; compare with
   the papers linked in the project (link their source notes).
7. **Limitations** — sample, measurement, confounders, generalisability,
   association ≠ causation where relevant.
8. **Reproduce** — open `notebooks/analysis.ipynb`, Restart & Run All; Python version;
   requirements file; Colab notebook if used.

## 2. `results/summary.md` — plain-language summary

Half a page for someone with no statistics background (a friend, or the professor
skimming): what question was asked and why it matters, what data was used, what was
found (in words, one or two key numbers at most, no test names), what it does *not*
show, and one sentence on what could be done next. No jargon; if a term is
unavoidable, explain it in the same sentence. Link it from `project.md`.

## 3. Update state

`project.md`: `stage: 6`; link both files; Log entry. If the work raised new
questions, list them under "Future work" in `project.md` (the user may later turn them
into argument notes). Next: `/write-paper`.
