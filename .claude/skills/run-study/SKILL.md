---
name: run-study
description: Stage 5 of the research pipeline — carry out the planned study on a vault project in a Jupyter notebook: download the chosen dataset, check it, clean it, run the pre-planned method and save figures and numbers, explaining each step as it goes; moves heavy GPU work to a Google Colab notebook. Use when a project in projects/ has a chosen dataset and method and the user says to run the analysis, do the experiment, "реализирај истражување", or "run the study".
user-invocable: true
---

# Run the study

Whiteboard stage 5 — Реализирај истражување. Read `CLAUDE.md` first. Input: a project
at `stage: 4`. If dataset or method is missing, stop and point to
`/pick-dataset-method`. Output: `notebooks/analysis.ipynb`, results in `results/`,
`stage: 5`.

**Explain as you go.** Run the whole study without waiting for approval at each step,
but tell the user what each step does and why, in plain words, as it happens — the
user is learning the method. Stop only when something goes wrong or a decision
belongs to them.

**The plan in `project.md` is the contract.** Run the method, α, effect size and
decision rule written there. If the data forces a change (assumption fails, column
missing), stop, explain, propose the fallback, and log the deviation once the user
agrees. Never try other tests until one comes out significant.

## 1. Environment

- Python 3.12 (the interpreter named in CLAUDE.md), venv at
  `projects/<slug>/.venv`, with `ipykernel` registered so the notebook uses it.
  Check what's installed before installing anything.
- Minimal dependencies: pandas, numpy, scipy, statsmodels, scikit-learn, matplotlib,
  jupyter. Write `notebooks/requirements.txt` with pinned versions.

## 2. Local or Colab?

Local laptop by default (GTX 1650 4 GB VRAM, 16 GB RAM). Use Google Colab when the
chosen method needs more — e.g. fine-tuning a model that doesn't fit 4 GB, training
that would take hours on CPU, or data larger than ~5 GB. In that case:

- write `notebooks/analysis_colab.ipynb`: first cell installs requirements and
  downloads the data (or mounts Google Drive); everything else matches the local
  notebook;
- tell the user how to open it in Colab (upload the file at colab.research.google.com,
  Runtime → Change runtime type → T4 GPU) and to bring back the `results/` files;
- keep data loading, cleaning and descriptives local so they can be checked here.

## 3. Notebook structure — `notebooks/analysis.ipynb`

A markdown cell before every code cell explains what it does and why. Sections:

1. **Setup** — imports, random seed, paths.
2. **Get the data** — download into `data/raw/` (if it needs a login, ask the user to
   download it and say where to put it). Never edit raw files.
3. **Check the data** — shape, columns, types, missing values, ranges. Compare with
   what the dataset page promised. **If the dataset turns out unsuitable** (needed
   variable missing, far fewer rows, unusable quality), stop, explain, and offer the
   next dataset from **Backup datasets** in `project.md`.
4. **Clean** — every step explained; save to `data/clean/`. Note rows before/after
   and what was dropped and why.
5. **Descriptive statistics** of every variable used → `results/descriptives.csv`.
6. **Assumption checks** for the method (normality, equal variance, expected counts,
   multicollinearity, class balance…).
7. **The test / model** exactly as planned. For ML: baseline first, same CV folds,
   preprocessing fitted inside each fold only (no leakage).
8. **Effect size + 95% CI**, not just a p-value.
9. **Figures** (1–3) that show the answer → `results/fig_*.png`, labelled axes and units.
10. **Numbers** — every number the paper will quote → `results/numbers.json`.
11. **Answer** — a markdown cell: the result in plain words, and whether it supports
    H1 by the decision rule.

## 4. Check before reporting

- Restart the kernel and run all cells once; the numbers must match.
- Sanity-check: sample sizes add up, effect direction makes sense.
- Add `data/raw/`, `data/clean/` to `.gitignore` if larger than ~50 MB.

## 5. Update the vault

`project.md`: `stage: 5`; Log entry with what ran, where (local/Colab), and any
deviation from the plan or dataset switch. Tell the user the headline result and hand
over to `/document-results`.
