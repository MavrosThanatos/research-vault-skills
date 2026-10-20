# research-vault-skills

[Claude Code](https://claude.com/claude-code) skills that walk a student through a
research project, from picking a problem to an Overleaf-ready paper, inside an
[Obsidian](https://obsidian.md) vault. The research pipeline and the long-term
knowledge base are the same set of Markdown files.

## The pipeline

| # | Stage | Skill | What it does |
|---|-------|-------|--------------|
| 1–3 | Problem, research question, hypothesis | `/research-start` | Interviews you about your interests, searches the literature for gaps, suggests 5 topics, then pins down the problem, question and a testable hypothesis backed by 2–4 real papers. Creates `projects/<slug>/`. |
| 4 | Dataset + method | `/pick-dataset-method` | Searches public datasets, ranks them in a comparison table, offers the method at three levels (easy / medium / ambitious) and keeps backup datasets. |
| 5 | Carry out the study | `/run-study` | Runs the pre-planned analysis in a reproducible Jupyter notebook (download, check, clean, analyse, save figures and numbers); moves heavy GPU work to Colab. |
| 6 | Document results | `/document-results` | Writes a results note (numbers, figures, hypothesis decision, limitations, how to reproduce) plus a half-page plain-language summary. |
| 7 | Write the paper | `/write-paper` | Builds a detailed IMRaD outline with paragraph starters and verified citations for you to write yourself. IEEE LaTeX by default, switchable (e.g. Springer LNCS). |
| any | Read | `/add-source` | Saves a paper, PDF, dataset or web page as a source note with verified metadata, a summary and the author's intention. |

Ground rules the skills follow:

- **No invented references.** Every paper, dataset or URL must be found and opened
  in the session, with a DOI, arXiv id or working URL.
- **Check before claiming novelty.** Prior work is searched and logged.
- **The student decides.** Skills propose options with trade-offs and a
  recommendation; you confirm every choice.
- **You write the thinking.** Skills never write claims into `notes/` or
  `arguments/`, and they outline the paper rather than writing its prose.

## Vault layout

```
sources/     one note per paper / book / dataset / web page ("Author Year.md")
notes/       topic notes: atomic one-sentence claims, each linked to its source
arguments/   your own questions, ideas, hypotheses
projects/<slug>/
  project.md   stage (1–7), question, hypothesis, dataset, method, log
  data/        raw data + README on how to re-download it
  notebooks/   analysis.ipynb (Restart & Run All reproduces everything)
  results/     figures, numbers.json, results.md, summary.md
  paper/       outline.md, latex/
support/     PDFs, images, attachments
templates/   note templates (included in this repo)
```

## Install

1. Copy `.claude/skills/` and `templates/` into the root of your Obsidian vault.
2. Add a `CLAUDE.md` at the vault root describing the conventions above and your
   hardware (the skills read it for the Python interpreter and GPU/RAM limits).
3. Open the vault folder in Claude Code and run `/research-start`.
