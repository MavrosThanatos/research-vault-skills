---
name: pick-dataset-method
description: Stage 4 of the research pipeline — for a fixed research question and hypothesis, search for public datasets, rank them in a comparison table, and offer the method at three levels (easy / medium / ambitious) for the user to choose, keeping backup datasets in case the first turns out unsuitable. Use when the user has a research question and asks which dataset to use, which methodology/statistical test/model to apply, "odberi dataset", "najlesna metodologija", "избери метод", or runs it on a project in projects/.
user-invocable: true
---

# Pick dataset + method

Whiteboard stage 4 — Избери метод (научен). Read `CLAUDE.md` first. Input: a
`projects/<slug>/project.md` at stage 3 (if there isn't one, ask for the question and
hypothesis, or suggest `/research-start`). Output: the same note at `stage: 4` with one
dataset and one method chosen, and the runner-up datasets kept as backups.

## 1. Read the question

From `project.md` extract: question type (descriptive / comparative / relational /
predictive / causal), the variables needed (outcome + predictors/groups), the unit of
observation (person, day, city…), and what result would support H1.

## 2. Find candidate datasets

Search all of these, then keep the **4–6** best candidates:

- **Datasets used in the project's source papers** — prefer these where they fit,
  so results are comparable with prior work.
- Kaggle, UCI ML Repository, OpenML.
- Official statistics: World Bank, Eurostat, WHO, Our World in Data, MakStat
  (makstat.stat.gov.mk), data.gov.
- Hugging Face Datasets, Papers with Code.

**Verify from the dataset's page only** (WebFetch — no downloading at this stage).
For each candidate record: the variables the question needs and whether the page
lists them; number of rows/observations; time span and population; licence; size;
access (free download / login / request); known issues mentioned. If the page
doesn't state something, write "not stated" — don't guess. Never list a dataset whose
page you couldn't open.

## 3. Ranked comparison table

Rank by: fit to the question (has the needed variables) › enough data › access and
licence › size fits the laptop › comparability with the source papers.

| Rank | Dataset (link) | Fit to question | Rows / span | Licence & access | Size | Used in papers? | Notes |
|---|---|---|---|---|---|---|---|

Under the table: one sentence on why #1 is ranked first.

## 4. Method at three levels

For the top-ranked dataset, offer the method as three levels — all must be valid tests
of the hypothesis; they differ in effort and depth:

| Level | Method | What it tells you | Effort | Compute |
|---|---|---|---|---|
| Easy | … | … | ~hours | laptop CPU |
| Medium | … | … | ~days | laptop |
| Ambitious | … | … | ~week+ | laptop GPU or Colab |

Starting points by question type:

| Question type | Easy | Medium | Ambitious |
|---|---|---|---|
| Compare 2 groups (numeric) | Welch t-test / Mann–Whitney U | Regression with group + covariates | Mixed-effects model / Bayesian estimation |
| Compare 3+ groups | One-way ANOVA + Tukey / Kruskal–Wallis | Two-way ANOVA or ANCOVA | Mixed-effects model |
| Categorical association | Chi-square / Fisher's exact | Logistic regression | Multilevel logistic regression |
| Two numeric variables | Pearson / Spearman correlation | Multiple linear regression with controls | Non-linear (GAM) or causal-inference design |
| Trend over time | Linear trend / Mann–Kendall | Seasonal decomposition + regression | ARIMA / Prophet forecasting |
| Predict a number | Baseline vs linear regression, k-fold CV | Random forest / gradient boosting + feature importance | Neural network or tuned ensemble + SHAP |
| Predict a class | Majority baseline vs logistic regression, k-fold CV | Random forest / XGBoost + class weights | Fine-tuned pretrained model (text/images) |
| Causal | Association + controls (say it's not causal) | Matching / propensity scores | Difference-in-differences / regression discontinuity |

Check the assumptions each method needs against what the dataset page says (sample
size, data type, balance). Rules of thumb: ≥ 30 per group for t-test/ANOVA; ≥ 10–20
rows per predictor for regression; ≥ 5 expected per cell for chi-square.

For every level fix up front, so the result can't be tuned later:
significance level (α = 0.05 by default), effect size to report (Cohen's d, r, R²,
odds ratio, F1/AUC…), the decision rule for "H1 supported", and for ML the metric,
baseline, CV scheme and random seed.

Ask the user to pick a dataset and a level.

## 5. Write it into the vault

After the user chooses:

1. Create a source note for the chosen dataset in `sources/` (`type: dataset`, URL,
   licence, size, variables used, accessed date).
2. Update `project.md`: `stage: 4`, `dataset: "[[<dataset note>]]"`,
   `method: "<name> (easy|medium|ambitious)"`, and fill "Dataset & method" with
   variables, planned preprocessing, method, α, effect size, decision rule.
3. Add a **Backup datasets** section to `project.md`: the rest of the ranked table
   with links and notes, so `/run-study` can switch if the chosen one turns out
   unsuitable once downloaded.
4. Write `data/README.md`: source URL, licence, how to download, accessed date.
5. Append to the Log.

Next step: `/run-study`.
