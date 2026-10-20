---
name: add-source
description: Save a paper, PDF, book, dataset or web page into the research vault as a source note — verified metadata, link to the PDF/URL, short summary and author's intention. Does not extract claims; the user does that themselves into topic notes. Use when the user gives a DOI, arXiv link, URL or PDF to save, says "add this paper", "save this source", or wants to find papers on a topic to add.
user-invocable: true
---

# Add a source

Read `CLAUDE.md` first. This skill only creates **source notes**. Extracting claims
into topic notes is the user's own work (Source → Topic → Argument): don't write to
`notes/` or `arguments/` from this skill.

## 1. Get and verify the source

- DOI / arXiv / URL: fetch it (WebFetch; for arXiv the abs page). Get authors, year,
  title, venue, DOI. If it can't be opened, stop and say so — never fill metadata from
  memory.
- PDF on disk: copy it into `support/` as `firstauthorYEAR.pdf` and read it.
- Topic search: WebSearch, propose 3–5 papers (title, year, link, one line each) and
  let the user pick which to add.
- If a source note already exists (check `sources/` filenames and `aliases`), update
  it instead of creating a duplicate.

## 2. Source note — `sources/Author Year.md`

Filename: first author's surname + year (`Vanderwel 2020`); add `a`/`b` on clashes.
Fill `templates/source.md`:

- Frontmatter: `type` (paper/book/dataset/web), `authors` (as `[[Name]]` links), `year`,
  `title`, `venue` (as `[[Journal]]`), `doi`, `url`, `pdf`, `accessed`,
  `aliases: [Author Year]`.
- `# [[support/x.pdf|Title]]` — the title linking to the PDF (or `[Title](url)` if
  there's no PDF).
- `> [!summary]- Summary (AI)` — collapsed callout, 2–3 sentences, marked as AI-written.
- **Author's intention** — why the paper was written, one or two sentences.
- For a dataset: what it contains, rows/span, licence, variables.

Keep it short — the source note is an anchor to the original, not a substitute for
reading it.

## 3. Link to a project

If the source was added for a project, link it from that project's `project.md` and
append a Log line. Give the user the link to the paper in the reply.
