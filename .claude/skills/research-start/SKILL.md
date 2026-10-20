---
name: research-start
description: Stages 1–3 of the research pipeline — interview the user about their interests, find research gaps in the literature around them, suggest 5 topics, then pin down the problem, the scientific question and a testable hypothesis backed by 2–4 real papers, and create the project folder in the vault. Use when the user wants to start a new research project or seminar paper, asks for topic ideas ("predlagaj temi", "what could I research"), has a rough question to turn into a research question/hypothesis, or says "утврди проблем / научно прашање / хипотеза".
user-invocable: true
---

# Research start — interests → gap → problem → question → hypothesis

Covers whiteboard stages 1–3. Read `CLAUDE.md` at the vault root first; its folder,
link and ground rules apply here. Everything is written in English. The output is a
`projects/<slug>/project.md` at `stage: 3` with a question and hypothesis the user has
signed off on, and 2–4 supporting papers saved as source notes.

## 1. Interview the user about their interests

Ask questions (`AskUserQuestion`, 2–4 at a time, a second round if answers are vague)
until you can name a narrow area. Cover:

- Which fields/subjects interest them (and why — what caught their attention).
- Concrete things they've built, read or wondered about recently.
- Skills they want to use or learn (e.g. ML, statistics, NLP, data viz).
- Constraints from the professor: course, deadline, length, required method if any.
- How much time they can spend.

If they arrive with a question already, still ask enough to know the surrounding
interest — the gap search in step 2 runs on that.

## 2. Find research gaps around those interests

For the area(s) from step 1, search the literature (WebSearch; Google Scholar, arXiv,
Semantic Scholar, PubMed, recent surveys/reviews) and look for gaps:

- "future work" / "limitations" sections of recent papers and surveys;
- questions studied elsewhere but not for a given region, population, period or
  data type (e.g. done for the US, never for North Macedonia / the Balkans);
- methods applied to one dataset but not tested on another;
- conflicting results between papers;
- new public data that no paper has analysed yet.

Only report a gap if a source you actually opened supports it; give the link.

## 3. Suggest 5 topics — short table

Each topic is built on one gap from step 2 and must be doable by a student alone, on
a laptop, in a few weeks. Before listing a topic, confirm by WebSearch that **a public
dataset exists** for it.

| # | Topic | Gap it fills (link) | Candidate question | Dataset (link) | Difficulty |
|---|---|---|---|---|---|

One line per cell. Mark one as recommended and say why in one sentence. Let the user
pick, mix, or ask for another round.

## 4. Problem (stage 1 — Утврди проблем)

3–5 sentences: what is unknown or going wrong, who it affects, why it matters, and
the gap from step 2. A problem is not a question yet.

## 5. Research question (stage 2 — Утврди научно прашање)

Turn the problem into **one** question that is:

- **answerable with the data** — every variable it mentions exists (or can be derived);
- **specific** — names population, variables and measure;
- of one stated type — descriptive, comparative, relational, predictive, or causal
  (warn that causal is hard without an experiment; suggest relational instead).

Offer 2–3 phrasings, recommend one. Search for prior work answering it and tell the
user what exists before they commit.

## 6. Hypothesis (stage 3 — Утврди хипотеза)

- **H1** — the expected answer, directional where possible, falsifiable.
- **H0** — the null it is tested against.
- **Supported if** — which result would support H1 (refined in stage 4).
- **Why** — one or two sentences on why H1 is expected, citing the papers below.

## 7. Supporting papers ("читај / chat") — always

Find **2–4 real papers** that support the problem, the gap and the hypothesis.
Every one must be opened (WebFetch) and have a DOI, arXiv id or working URL.

- Give the user a list: title, authors, year, **link**, one line on why it's relevant.
- Save each as a source note following the `/add-source` format (source note only).

## 8. Write it into the vault

After the user confirms question and hypothesis:

1. Pick a short kebab-case slug (`skopje-pm10-temperature`).
2. Create `projects/<slug>/` with `data/`, `notebooks/`, `results/`, `paper/`.
3. Create `project.md` from `templates/project.md`: `stage: 3`, `question`,
   `hypothesis`, and the Problem / Research gap / Research question / Hypothesis
   sections, linking the supporting papers' source notes.
4. List the candidate dataset(s) under "Dataset & method" — not chosen yet.
5. Append to the Log: date, what was decided, prior work found.

End by telling the user the next step: `/pick-dataset-method`.
