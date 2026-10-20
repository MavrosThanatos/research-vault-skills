---
name: write-paper
description: Stage 7 of the research pipeline — prepare a scientific paper (научен труд) for a vault project as a detailed IMRaD outline with paragraph starters and verified citations, for the user to write the prose themselves; default format IEEE LaTeX/PDF, switchable to other templates such as Springer. Use when the user wants to write the paper, seminar work or article for a project, "научен труд", "naucno spisanie", or change the paper's template/format.
user-invocable: true
---

# Write the paper — outline + paragraph starters

Whiteboard stage 7 — Научен труд. Read `CLAUDE.md` first. Input: a project at
`stage: 6` (`results/results.md` exists). Output: `paper/outline.md` and an
Overleaf-ready `paper/latex/` with the skeleton, `stage: 7`. Written in English.

**The user writes the prose.** This skill builds the structure, the argument and the
citations; it does not write finished paragraphs. Each paragraph gets a starter (the
first sentence or two) and bullet points of what it must say, with sources attached.

## 0. Ask the target — every project

`AskUserQuestion`: what is this paper for — course seminar paper, student conference,
or a real journal/conference (which one?). Also length limit / page count if known.

Format: **default IEEE (conference template, numbered citations) in LaTeX → PDF**,
unless the user or the target says otherwise. Supported switches, for example:

- **Springer** — LNCS (`llncs` class, for conference proceedings) or Springer Nature
  journal template (`sn-jnl`); citation style per template (`splncs04` for LNCS).
- **Elsevier** (`elsarticle`), **ACM** (`acmart`), **APA 7** author-year.
- A specific venue — fetch its author guidelines and template page, follow them
  (page limit, anonymisation, sections), and note the URL in the Log.

Record the chosen target and template in `project.md` (`target:`, `template:`).

## 1. `paper/outline.md` — built from the vault

For each section: its purpose in one line, then one entry per paragraph:

```
### 1. Introduction
**¶1 — the problem**
Starter: "Air pollution in Skopje regularly exceeds EU limits during winter …"
- point: PM10 exceeded the EU daily limit on X days in 2022 [[Source 2023]]
- point: health impact … [[Source 2021]]
```

| Section | Built from |
|---|---|
| Abstract | outline of 5 sentences: context, gap, method, result, implication |
| 1. Introduction | Problem + Research gap in `project.md`; ends with the question and H1 |
| 2. Related work | source notes linked from the project, grouped by theme; how this study differs |
| 3. Data and methods | "Dataset & method" + `results.md` Data/Method; enough to reproduce |
| 4. Results | `results.md` Results; numbers only from `results/numbers.json`; figures/tables with draft captions |
| 5. Discussion | interpretation + hypothesis decision + comparison with related work |
| 6. Limitations & future work | `results.md` Limitations + Future work |
| 7. Conclusion | 3 points: question, answer, why it matters |

Starters are a sentence or two at most — the rest is bullets.

## 2. Citation integrity

- Every citation must map to a source note in `sources/` with a DOI or URL. If a point
  needs a source the vault doesn't have, find and verify one (open it) and create its
  source note — or mark the point `[needs source]`. Never cite from memory.
- Build `paper/latex/references.bib` from the source notes' frontmatter (fetch the
  BibTeX from the DOI where possible: `https://doi.org/<doi>` with
  `Accept: application/x-bibtex`).

## 3. `paper/latex/` — the skeleton

- Download the official template for the chosen format (IEEE: IEEEtran; Springer:
  llncs or sn-jnl from Springer's site/CTAN), place it in `paper/latex/`.
- `main.tex` with section headings, the figures from `results/` already inserted with
  draft captions, tables of the key numbers filled from `numbers.json`, `\cite{}`
  keys in place, and `% TODO: write ¶ — see outline.md` comments where prose goes.
- Compile to PDF if a LaTeX engine is installed (check `pdflatex`/`latexmk` first);
  otherwise tell the user to upload `paper/latex/` as a zip to Overleaf.

Switching format later: rebuild `paper/latex/` from the new template, carrying over
the user's written text, `references.bib` and figures.

## 4. Review checklist (run when the user says the prose is written)

Every number matches `numbers.json`; every figure/table referenced in the text;
abstract states the result; no causal language for associational findings; every
`\cite` resolves; within the length limit; template rules followed. Report issues
by section — don't rewrite the user's text unasked.

`project.md`: `stage: 7`; Log entry with the paper path. Remind the user to check the
course's rules on AI assistance and disclose it if required.
