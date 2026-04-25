# arxiv-paper-writer Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a reusable `arxiv-paper-writer` skill subproject that captures the successful `papers/agent-survey` workflow and can later become an independent Git repository/submodule.

**Architecture:** Create a self-contained skill directory under `skills/arxiv-paper-writer/` with `SKILL.md` as the primary workflow brain, plus bundled `templates/`, `references/`, `scripts/`, and `evals/`. The first version is instruction-led rather than script-heavy, but it includes deterministic helper scripts for validation so the skill can evolve toward stronger automation.

**Tech Stack:** Claude Code skill format, Markdown, LaTeX, BibTeX, Bash-compatible shell commands, optional Python via `uv` for validation scripts.

---

## File Structure

Create a future-independent skill subtree:

```text
skills/arxiv-paper-writer/
├── SKILL.md
├── README.md
├── templates/
│   ├── arxiv_survey_main.tex
│   └── references.bib
├── references/
│   ├── workflow.md
│   ├── latex_environment.md
│   ├── bibliography.md
│   ├── figures_and_tables.md
│   └── quality_review.md
├── scripts/
│   ├── check_skill_structure.py
│   ├── check_bibtex.py
│   └── check_latex_log.py
└── evals/
    └── evals.json
```

Responsibilities:

- `SKILL.md`: Triggering metadata and concise end-to-end workflow instructions.
- `README.md`: Human-facing explanation for future standalone repository use.
- `templates/arxiv_survey_main.tex`: Minimal arXiv-style survey paper template based on the current agent-survey success case, generalized for other topics.
- `templates/references.bib`: Tiny valid BibTeX seed file used for smoke tests.
- `references/workflow.md`: Detailed six-step paper workflow.
- `references/latex_environment.md`: Windows MiKTeX and Linux TeX Live setup guidance.
- `references/bibliography.md`: Literature search and BibTeX construction rules.
- `references/figures_and_tables.md`: TikZ/table design guidance.
- `references/quality_review.md`: Final paper quality checklist.
- `scripts/check_skill_structure.py`: Validates required skill files exist.
- `scripts/check_bibtex.py`: Performs lightweight BibTeX sanity checks.
- `scripts/check_latex_log.py`: Scans LaTeX logs for common fatal issues.
- `evals/evals.json`: Initial skill test prompts.

---

## Task 1: Create Skill Directory Skeleton

**Files:**
- Create: `skills/arxiv-paper-writer/`
- Create: `skills/arxiv-paper-writer/templates/`
- Create: `skills/arxiv-paper-writer/references/`
- Create: `skills/arxiv-paper-writer/scripts/`
- Create: `skills/arxiv-paper-writer/evals/`

- [x] **Step 1: Create directories**

Run:

```bash
mkdir -p skills/arxiv-paper-writer/{templates,references,scripts,evals}
```

Expected: command exits with status 0.

- [x] **Step 2: Verify directory skeleton**

Run:

```bash
python - <<'PY'
from pathlib import Path
base = Path('skills/arxiv-paper-writer')
required = ['templates', 'references', 'scripts', 'evals']
missing = [name for name in required if not (base / name).is_dir()]
if missing:
    raise SystemExit(f'Missing directories: {missing}')
print('skill directory skeleton ok')
PY
```

Expected output:

```text
skill directory skeleton ok
```

---

## Task 2: Write SKILL.md

**Files:**
- Create: `skills/arxiv-paper-writer/SKILL.md`

- [x] **Step 1: Create SKILL.md with metadata and workflow**

Write this exact file:

```markdown
---
name: arxiv-paper-writer
description: Use this skill whenever the user wants Claude Code to write, scaffold, compile, debug, or review an arXiv-style academic paper, especially survey papers with LaTeX, BibTeX citations, TikZ figures, tables, and PDF output. This skill should trigger for requests like writing a full paper, creating an arXiv paper project, turning a research topic into a LaTeX manuscript, reproducing the Paper-Write-Skill-Test agent-survey workflow, or setting up a Windows/Linux Claude Code paper-writing loop.
---

# arxiv-paper-writer

This skill guides Claude Code through an end-to-end arXiv-style paper workflow: plan the paper, initialize a LaTeX project, build a BibTeX library, write sections incrementally, create figures/tables, compile, debug, and perform a final quality review.

## Core principle

Treat paper writing as an engineering loop, not a one-shot generation task. Work in small verifiable stages: plan, scaffold, cite, write, compile, inspect, repair, and review.

## When starting a paper project

1. Clarify topic, paper type, target length, audience, and whether the user wants a survey, methods paper, benchmark paper, position paper, or tutorial.
2. Create or reuse a paper directory with `main.tex`, `references.bib`, `figures/`, `sections/`, and `output/`.
3. Use `templates/arxiv_survey_main.tex` as the default template for survey papers.
4. Read `references/workflow.md` for the full staged process.
5. Read only the reference files needed for the current phase.

## Workflow phases

### Phase 1: Plan

Create a concrete paper plan before writing prose. Include title candidates, scope, contribution claims, section outline, target references, expected figures/tables, and verification commands.

### Phase 2: Scaffold

Create a compilable LaTeX skeleton first. The first milestone is a PDF that compiles even before the paper is complete.

### Phase 3: Build bibliography

Construct `references.bib` before heavy writing. Prefer real, verifiable papers. Use stable BibTeX keys. Avoid invented citations. If uncertain about a reference, mark it for verification rather than fabricating metadata.

### Phase 4: Write sections incrementally

Write one or two sections at a time. After each substantial section, compile or at least check citations and LaTeX syntax. Write Abstract last.

### Phase 5: Create figures and tables

Prefer TikZ and LaTeX tables for reproducible academic artifacts. Keep figures information-dense and directly connected to claims in the text.

### Phase 6: Compile and debug

Use `latexmk` on Linux when available. On Windows MiKTeX, use `pdflatex`, `bibtex`, `pdflatex`, `pdflatex`. Read `.log` files before guessing fixes.

### Phase 7: Final review

Check PDF generation, citation completeness, undefined references, figure/table placement, academic tone, contribution clarity, and arXiv compatibility.

## Environment guidance

- For Windows MiKTeX details, read `references/latex_environment.md`.
- For Linux TeX Live details, read `references/latex_environment.md`.
- For BibTeX construction, read `references/bibliography.md`.
- For figure and table design, read `references/figures_and_tables.md`.
- For final review, read `references/quality_review.md`.

## Output style

When writing project files, edit the actual `.tex`, `.bib`, and documentation files. Do not merely describe what should be written. When explaining progress to the user, summarize briefly in Chinese and reference exact file paths.
```

- [x] **Step 2: Verify frontmatter exists**

Run:

```bash
python - <<'PY'
from pathlib import Path
p = Path('skills/arxiv-paper-writer/SKILL.md')
text = p.read_text(encoding='utf-8')
assert text.startswith('---\nname: arxiv-paper-writer\n'), 'missing skill frontmatter name'
assert 'description:' in text, 'missing description'
print('SKILL.md frontmatter ok')
PY
```

Expected output:

```text
SKILL.md frontmatter ok
```

---

## Task 3: Add LaTeX and BibTeX Templates

**Files:**
- Create: `skills/arxiv-paper-writer/templates/arxiv_survey_main.tex`
- Create: `skills/arxiv-paper-writer/templates/references.bib`

- [x] **Step 1: Create the arXiv survey LaTeX template**

Write `skills/arxiv-paper-writer/templates/arxiv_survey_main.tex` with this content:

```latex
\pdfoutput=1
\documentclass[11pt,a4paper]{article}

% Encoding and fonts
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{newtxtext}
\usepackage{newtxmath}

% Layout and typography
\usepackage[a4paper,margin=1in]{geometry}
\usepackage{microtype}
\usepackage{setspace}
\onehalfspacing

% Math
\usepackage{mathtools}
\usepackage{amssymb}
\let\Bbbk\relax

% Figures and tables
\usepackage{graphicx}
\usepackage[font=small,labelfont=bf,format=hang]{caption}
\usepackage{subcaption}
\usepackage{booktabs}
\usepackage{array}
\usepackage{multirow}
\usepackage{tabularx}
\usepackage{enumitem}
\setlist{nosep}

% Colors and TikZ
\usepackage{xcolor}
\definecolor{linkblue}{RGB}{0,51,153}
\usepackage{tikz}
\usetikzlibrary{shapes.geometric,arrows.meta,positioning,fit,calc}

% Citations and links
\usepackage[numbers,sort&compress]{natbib}
\usepackage{hyperref}
\hypersetup{
  colorlinks=true,
  linkcolor=linkblue,
  citecolor=linkblue,
  urlcolor=linkblue,
  pdftitle={PAPER_TITLE},
  pdfsubject={PAPER_SUBJECT},
  pdfkeywords={PAPER_KEYWORDS},
  bookmarks=true,
  bookmarksopen=true
}
\usepackage[nameinlink,noabbrev]{cleveref}
\crefname{figure}{Fig.}{Figs.}
\crefname{table}{Table}{Tables}
\crefname{section}{Sec.}{Secs.}

\title{\textbf{PAPER_TITLE}}
\author{PAPER_AUTHOR}
\date{PAPER_DATE}

\begin{document}
\maketitle

\begin{abstract}
ABSTRACT_TEXT
\end{abstract}

\vspace{0.5em}
\noindent\textbf{Keywords:} PAPER_KEYWORDS

\section{Introduction}
\label{sec:introduction}

Introduce the research area, motivation, scope, and contribution claims. Cite real work such as \citet{vaswani2017attention}.

\section{Background and Definitions}
\label{sec:background}

Define the core concepts and terminology used throughout the paper.

\section{Main Survey Body}
\label{sec:survey-body}

Organize the literature by historical stages, technical themes, or methodological categories.

\section{Challenges and Future Directions}
\label{sec:challenges}

Discuss limitations, open problems, safety concerns, evaluation gaps, and promising future work.

\section{Conclusion}
\label{sec:conclusion}

Summarize the paper and restate the main takeaways.

\bibliographystyle{plainnat}
\bibliography{references}

\end{document}
```

- [x] **Step 2: Create the seed BibTeX file**

Write `skills/arxiv-paper-writer/templates/references.bib` with this content:

```bibtex
@inproceedings{vaswani2017attention,
  title={Attention Is All You Need},
  author={Vaswani, Ashish and Shazeer, Noam and Parmar, Niki and Uszkoreit, Jakob and Jones, Llion and Gomez, Aidan N. and Kaiser, Lukasz and Polosukhin, Illia},
  booktitle={Advances in Neural Information Processing Systems},
  volume={30},
  year={2017}
}
```

- [x] **Step 3: Verify templates contain required markers**

Run:

```bash
python - <<'PY'
from pathlib import Path
tex = Path('skills/arxiv-paper-writer/templates/arxiv_survey_main.tex').read_text(encoding='utf-8')
bib = Path('skills/arxiv-paper-writer/templates/references.bib').read_text(encoding='utf-8')
for marker in ['PAPER_TITLE', 'PAPER_AUTHOR', 'ABSTRACT_TEXT', '\\bibliography{references}']:
    assert marker in tex, f'missing marker: {marker}'
assert '@inproceedings{vaswani2017attention' in bib, 'missing seed BibTeX entry'
print('templates ok')
PY
```

Expected output:

```text
templates ok
```

---

## Task 4: Add Core Workflow Reference

**Files:**
- Create: `skills/arxiv-paper-writer/references/workflow.md`

- [x] **Step 1: Create workflow reference**

Write `skills/arxiv-paper-writer/references/workflow.md` with this content:

```markdown
# arXiv Paper Workflow

Use this reference when the user asks to create a new paper project or reproduce the full Claude Code paper-writing loop.

## Stage 1: Define the paper

Clarify:

- paper type: survey, benchmark, methods, position, tutorial, or report
- topic and scope
- intended reader
- target length
- expected contribution claims
- required output: `.tex`, `.bib`, `.pdf`, figures, tables, or documentation

Produce a short plan before writing files.

## Stage 2: Initialize the project

Create this structure unless the user gives a different layout:

```text
paper-project/
├── main.tex
├── references.bib
├── figures/
├── sections/
└── output/
```

Copy `templates/arxiv_survey_main.tex` to `main.tex` and replace placeholders. Copy `templates/references.bib` to `references.bib` as a smoke-test bibliography.

## Stage 3: Build the bibliography

Before writing long sections, build a reference set:

- 15-25 core references for a short paper
- 35-60 references for a survey paper
- stable BibTeX keys such as `vaswani2017attention`
- no invented metadata
- uncertain entries marked for verification

## Stage 4: Write the paper incrementally

Write in this order:

1. title and outline
2. Introduction
3. Background
4. main body sections
5. challenges/future work
6. conclusion
7. abstract last

After each major section, compile or perform static checks.

## Stage 5: Add figures and tables

Prefer reproducible LaTeX-native artifacts:

- TikZ architecture diagram
- TikZ timeline
- `booktabs` comparison table
- benchmark or taxonomy table

Every figure/table should support a claim in the text.

## Stage 6: Compile and repair

Linux preferred command:

```bash
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```

Output directory variant:

```bash
latexmk -pdf -interaction=nonstopmode -halt-on-error -outdir=output main.tex
```

Windows MiKTeX fallback:

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

If compilation fails, read the `.log` file and fix the source. Do not guess blindly.

## Stage 7: Review

Check:

- PDF exists
- bibliography appears
- no undefined citations
- no undefined references
- figures and tables render
- contribution claims are clear
- related work coverage is credible
- limitations are explicit
- arXiv compatibility is preserved through `\pdfoutput=1`
```

- [x] **Step 2: Verify workflow reference exists**

Run:

```bash
python - <<'PY'
from pathlib import Path
p = Path('skills/arxiv-paper-writer/references/workflow.md')
text = p.read_text(encoding='utf-8')
for phrase in ['Stage 1: Define the paper', 'Stage 6: Compile and repair', 'latexmk -pdf']:
    assert phrase in text, f'missing phrase: {phrase}'
print('workflow reference ok')
PY
```

Expected output:

```text
workflow reference ok
```

---

## Task 5: Add Environment and Bibliography References

**Files:**
- Create: `skills/arxiv-paper-writer/references/latex_environment.md`
- Create: `skills/arxiv-paper-writer/references/bibliography.md`

- [x] **Step 1: Create LaTeX environment reference**

Write `skills/arxiv-paper-writer/references/latex_environment.md` with this content:

```markdown
# LaTeX Environment Reference

Use this reference when setting up or debugging local/cloud LaTeX compilation.

## Windows: MiKTeX

Recommended commands:

```bash
pdflatex --version
bibtex --version
```

Enable automatic package installation to avoid GUI prompts during Claude Code compile loops:

```bash
initexmf --set-config-value="[MPM]AutoInstall=yes"
initexmf --admin --set-config-value="[MPM]AutoInstall=yes"
```

Compile sequence:

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

## Linux: TeX Live

Recommended Ubuntu/Debian minimal install:

```bash
sudo apt update
sudo apt install -y \
  texlive-latex-base \
  texlive-latex-recommended \
  texlive-latex-extra \
  texlive-fonts-recommended \
  texlive-fonts-extra \
  texlive-pictures \
  texlive-bibtex-extra \
  latexmk \
  biber
```

Verify packages:

```bash
pdflatex --version
bibtex --version
latexmk --version
kpsewhich tikz.sty
kpsewhich newtxtext.sty
kpsewhich cleveref.sty
```

Preferred compile command:

```bash
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```

## Common issues

### Missing `.sty`

If LaTeX reports `File 'xxx.sty' not found`, run:

```bash
kpsewhich xxx.sty
```

Install the package group that contains it, or use full TeX Live if disk space is not constrained.

### `\Bbbk` conflict

If `newtxmath` and `amssymb` conflict, include this after loading `amssymb`:

```latex
\let\Bbbk\relax
```

### Undefined citations

Run BibTeX or use `latexmk`. Check that every `\citep{key}` or `\citet{key}` has a matching BibTeX key.
```

- [x] **Step 2: Create bibliography reference**

Write `skills/arxiv-paper-writer/references/bibliography.md` with this content:

```markdown
# Bibliography Reference

Use this reference when building or reviewing `references.bib`.

## Principles

- Prefer real, verifiable academic references.
- Do not invent titles, authors, venues, arXiv IDs, DOIs, or years.
- If metadata is uncertain, mark the entry for verification in a note outside the BibTeX entry.
- Use stable lowercase keys: `authorYYYYshorttitle`.
- Keep keys readable and consistent.

## Search strategy

For survey papers, split the topic into 5-8 literature clusters. For each cluster, identify:

- foundational works
- recent surveys
- representative systems
- benchmark/evaluation papers
- critique/safety/limitations papers

For the AI-agent-survey pattern, useful clusters were:

- symbolic agents and BDI
- reinforcement learning agents
- large language models
- LLM reasoning and planning
- tool use and memory
- agent frameworks
- multi-agent systems
- evaluation and safety

## Target counts

- Short paper: 15-25 references
- Standard survey: 35-60 references
- Large survey: 60+ references with stronger taxonomy and related-work coverage

## BibTeX entry shape

Article or arXiv paper:

```bibtex
@article{keyYYYYtopic,
  title={Full Title},
  author={Author, First and Second, Author},
  journal={Journal Name or arXiv preprint arXiv:XXXX.XXXXX},
  year={YYYY},
  doi={10.xxxx/example}
}
```

Conference paper:

```bibtex
@inproceedings{keyYYYYtopic,
  title={Full Title},
  author={Author, First and Second, Author},
  booktitle={Conference Name},
  pages={1--12},
  year={YYYY}
}
```

## Citation rules

Use `\citet{key}` when the author is part of the sentence:

```latex
\citet{vaswani2017attention} introduced the Transformer architecture.
```

Use `\citep{key}` when the citation supports a claim:

```latex
Transformers became foundational for modern language models \citep{vaswani2017attention}.
```

Use multiple citations when summarizing a research direction:

```latex
LLM agents combine reasoning, acting, and tool use \citep{yao2023react,shinn2024reflexion}.
```
```

- [x] **Step 3: Verify reference docs**

Run:

```bash
python - <<'PY'
from pathlib import Path
env = Path('skills/arxiv-paper-writer/references/latex_environment.md').read_text(encoding='utf-8')
bib = Path('skills/arxiv-paper-writer/references/bibliography.md').read_text(encoding='utf-8')
for phrase in ['MiKTeX', 'TeX Live', 'latexmk -pdf', '\\Bbbk']:
    assert phrase in env, f'missing environment phrase: {phrase}'
for phrase in ['Do not invent', 'authorYYYYshorttitle', '\\citet{key}', '\\citep{key}']:
    assert phrase in bib, f'missing bibliography phrase: {phrase}'
print('environment and bibliography references ok')
PY
```

Expected output:

```text
environment and bibliography references ok
```

---

## Task 6: Add Figure/Table and Quality Review References

**Files:**
- Create: `skills/arxiv-paper-writer/references/figures_and_tables.md`
- Create: `skills/arxiv-paper-writer/references/quality_review.md`

- [x] **Step 1: Create figure and table reference**

Write `skills/arxiv-paper-writer/references/figures_and_tables.md` with this content:

```markdown
# Figures and Tables Reference

Use this reference when creating reproducible paper visuals.

## Principles

- Prefer TikZ and LaTeX tables when the figure is conceptual, architectural, or taxonomic.
- Use external image files only when the paper needs plots, screenshots, or complex visual assets.
- Every figure/table should support a specific claim in the text.
- Keep captions explanatory enough that readers understand the artifact without reading the full section.

## Recommended survey artifacts

For arXiv-style survey papers, create 2-5 high-value artifacts:

1. timeline figure
2. architecture diagram
3. taxonomy table
4. framework/system comparison table
5. benchmark or limitation matrix

## TikZ architecture pattern

```latex
\begin{figure}[t]
\centering
\begin{tikzpicture}[
  node distance=1.5cm,
  box/.style={rectangle,rounded corners,draw=black,thick,align=center,minimum width=2.5cm,minimum height=0.8cm},
  arrow/.style={-{Latex[length=3mm]},thick}
]
\node[box] (input) {Input};
\node[box,right=of input] (reason) {Reasoning};
\node[box,right=of reason] (action) {Action};
\draw[arrow] (input) -- (reason);
\draw[arrow] (reason) -- (action);
\end{tikzpicture}
\caption{A minimal architecture diagram.}
\label{fig:architecture}
\end{figure}
```

## Booktabs table pattern

```latex
\begin{table}[t]
\centering
\caption{Comparison of representative systems.}
\label{tab:systems}
\begin{tabularx}{\linewidth}{l l l X}
\toprule
System & Year & Type & Key idea \\
\midrule
System A & 2023 & Framework & Short description. \\
System B & 2024 & Benchmark & Short description. \\
\bottomrule
\end{tabularx}
\end{table}
```
```

- [x] **Step 2: Create quality review reference**

Write `skills/arxiv-paper-writer/references/quality_review.md` with this content:

```markdown
# Quality Review Reference

Use this reference before calling a paper complete.

## Compile checks

- PDF exists.
- BibTeX bibliography appears.
- No fatal LaTeX errors remain.
- No undefined citations remain.
- No undefined references remain.
- Figures and tables render.
- Hyperlinks and cross-references work.

## Academic checks

- The paper has a clear scope and does not pretend to cover more than it covers.
- Contribution claims are explicit and defensible.
- Related work includes both foundational and recent papers.
- The structure has a coherent narrative, not just a list of summaries.
- Tables and figures add synthesis rather than decoration.
- Limitations and open problems are discussed explicitly.
- The abstract accurately summarizes the final paper and is written last.

## arXiv-style checks

- `\pdfoutput=1` is present.
- The paper can compile from source without hidden local dependencies.
- Figure paths are relative.
- Generated intermediate files are not required for compilation unless intentionally included.
- The bibliography file is included.

## Suggested final commands

Linux:

```bash
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```

Windows fallback:

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

Log scan:

```bash
grep -E "Undefined|undefined|Fatal|Emergency|Error" main.log || true
```
```

- [x] **Step 3: Verify review references**

Run:

```bash
python - <<'PY'
from pathlib import Path
fig = Path('skills/arxiv-paper-writer/references/figures_and_tables.md').read_text(encoding='utf-8')
rev = Path('skills/arxiv-paper-writer/references/quality_review.md').read_text(encoding='utf-8')
for phrase in ['TikZ', 'Booktabs table pattern', 'taxonomy table']:
    assert phrase in fig, f'missing figure phrase: {phrase}'
for phrase in ['Compile checks', 'Academic checks', 'arXiv-style checks', '\\pdfoutput=1']:
    assert phrase in rev, f'missing review phrase: {phrase}'
print('figure/table and quality references ok')
PY
```

Expected output:

```text
figure/table and quality references ok
```

---

## Task 7: Add Validation Scripts

**Files:**
- Create: `skills/arxiv-paper-writer/scripts/check_skill_structure.py`
- Create: `skills/arxiv-paper-writer/scripts/check_bibtex.py`
- Create: `skills/arxiv-paper-writer/scripts/check_latex_log.py`

- [x] **Step 1: Create skill structure checker**

Write `skills/arxiv-paper-writer/scripts/check_skill_structure.py` with this content:

```python
from pathlib import Path

REQUIRED_FILES = [
    "SKILL.md",
    "README.md",
    "templates/arxiv_survey_main.tex",
    "templates/references.bib",
    "references/workflow.md",
    "references/latex_environment.md",
    "references/bibliography.md",
    "references/figures_and_tables.md",
    "references/quality_review.md",
    "evals/evals.json",
]


def main() -> int:
    base = Path(__file__).resolve().parents[1]
    missing = [path for path in REQUIRED_FILES if not (base / path).exists()]
    if missing:
        print("Missing required files:")
        for path in missing:
            print(f"- {path}")
        return 1

    skill_md = (base / "SKILL.md").read_text(encoding="utf-8")
    if not skill_md.startswith("---\nname: arxiv-paper-writer\n"):
        print("SKILL.md frontmatter is missing the expected name")
        return 1
    if "description:" not in skill_md:
        print("SKILL.md frontmatter is missing description")
        return 1

    print("arxiv-paper-writer skill structure ok")
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

- [x] **Step 2: Create BibTeX checker**

Write `skills/arxiv-paper-writer/scripts/check_bibtex.py` with this content:

```python
import re
import sys
from pathlib import Path

ENTRY_RE = re.compile(r"@\w+\s*\{\s*([^,]+),", re.MULTILINE)
REQUIRED_FIELDS = ["title", "author", "year"]


def main() -> int:
    if len(sys.argv) != 2:
        print("Usage: python check_bibtex.py <references.bib>")
        return 2

    path = Path(sys.argv[1])
    if not path.exists():
        print(f"BibTeX file not found: {path}")
        return 1

    text = path.read_text(encoding="utf-8")
    entries = ENTRY_RE.findall(text)
    if not entries:
        print("No BibTeX entries found")
        return 1

    duplicate_keys = sorted({key for key in entries if entries.count(key) > 1})
    if duplicate_keys:
        print("Duplicate BibTeX keys:")
        for key in duplicate_keys:
            print(f"- {key}")
        return 1

    lower_text = text.lower()
    missing_fields = [field for field in REQUIRED_FIELDS if f"{field}={{" not in lower_text and f"{field} = {{" not in lower_text]
    if missing_fields:
        print(f"Missing common required fields somewhere in file: {missing_fields}")
        return 1

    print(f"BibTeX sanity check ok: {len(entries)} entries")
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

- [x] **Step 3: Create LaTeX log checker**

Write `skills/arxiv-paper-writer/scripts/check_latex_log.py` with this content:

```python
import re
import sys
from pathlib import Path

ERROR_PATTERNS = [
    re.compile(r"! LaTeX Error:"),
    re.compile(r"! Emergency stop"),
    re.compile(r"Fatal error occurred"),
    re.compile(r"Citation .* undefined", re.IGNORECASE),
    re.compile(r"Reference .* undefined", re.IGNORECASE),
]


def main() -> int:
    if len(sys.argv) != 2:
        print("Usage: python check_latex_log.py <main.log>")
        return 2

    path = Path(sys.argv[1])
    if not path.exists():
        print(f"LaTeX log not found: {path}")
        return 1

    lines = path.read_text(encoding="utf-8", errors="replace").splitlines()
    matches = []
    for index, line in enumerate(lines, start=1):
        if any(pattern.search(line) for pattern in ERROR_PATTERNS):
            matches.append((index, line.strip()))

    if matches:
        print("LaTeX log issues found:")
        for index, line in matches[:50]:
            print(f"{index}: {line}")
        return 1

    print("LaTeX log sanity check ok")
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

- [x] **Step 4: Run syntax checks for scripts**

Run:

```bash
uv run python -m py_compile \
  skills/arxiv-paper-writer/scripts/check_skill_structure.py \
  skills/arxiv-paper-writer/scripts/check_bibtex.py \
  skills/arxiv-paper-writer/scripts/check_latex_log.py
```

Expected: command exits with status 0.

---

## Task 8: Add Initial Skill Evals

**Files:**
- Create: `skills/arxiv-paper-writer/evals/evals.json`

- [x] **Step 1: Create eval prompts**

Write `skills/arxiv-paper-writer/evals/evals.json` with this content:

```json
{
  "skill_name": "arxiv-paper-writer",
  "evals": [
    {
      "id": 1,
      "prompt": "我想用 Claude Code 写一篇 10-15 页 arXiv 风格的英文综述论文，主题是 LLM-based agents 的发展历程。请创建论文项目结构、LaTeX 主文件、BibTeX 文件，并说明后续写作与编译步骤。",
      "expected_output": "Creates or proposes a complete LaTeX paper project structure, uses an arXiv-compatible template, initializes references.bib, and gives a staged compile/write/debug workflow.",
      "files": []
    },
    {
      "id": 2,
      "prompt": "我的 main.tex 编译失败，日志里有 undefined citation 和 newtxmath/amssymb 的 Bbbk 冲突。请帮我按论文工程闭环定位并修复问题。",
      "expected_output": "Reads or asks for relevant log/source files, identifies undefined citations and Bbbk conflict, applies targeted LaTeX/BibTeX fixes, and recommends rerunning BibTeX/latexmk.",
      "files": []
    },
    {
      "id": 3,
      "prompt": "我已经有一篇初稿 PDF 和 LaTeX 源码，想判断它距离高质量 arXiv survey paper 还差什么。请按学术质量、引用、结构、图表和编译可复现性给我审查清单。",
      "expected_output": "Provides a structured review checklist covering academic contribution, literature coverage, narrative, figures/tables, citations, LaTeX reproducibility, and arXiv readiness.",
      "files": []
    }
  ]
}
```

- [x] **Step 2: Validate eval JSON**

Run:

```bash
uv run python - <<'PY'
import json
from pathlib import Path
p = Path('skills/arxiv-paper-writer/evals/evals.json')
data = json.loads(p.read_text(encoding='utf-8'))
assert data['skill_name'] == 'arxiv-paper-writer'
assert len(data['evals']) == 3
for item in data['evals']:
    assert item['prompt']
    assert item['expected_output']
    assert isinstance(item['files'], list)
print('evals json ok')
PY
```

Expected output:

```text
evals json ok
```

---

## Task 9: Add Human-Facing README

**Files:**
- Create: `skills/arxiv-paper-writer/README.md`

- [x] **Step 1: Create README**

Write `skills/arxiv-paper-writer/README.md` with this content:

```markdown
# arxiv-paper-writer

`arxiv-paper-writer` is a Claude Code skill for creating, compiling, debugging, and reviewing arXiv-style academic papers, especially survey papers written in LaTeX with BibTeX citations, TikZ figures, and reproducible PDF output.

This skill was extracted from the successful `papers/agent-survey` experiment in the Paper-Write-Skill-Test project, where Claude Code produced a 15-page AI Agent survey paper with real references, TikZ figures, tables, and a compiled PDF.

## What this skill does

It guides Claude Code through a staged paper engineering loop:

```text
plan → scaffold → bibliography → section writing → figures/tables → compile → debug → review
```

The skill is designed to avoid one-shot paper generation. It instead encourages small verified steps and repeated compile/debug cycles.

## Directory layout

```text
arxiv-paper-writer/
├── SKILL.md
├── README.md
├── templates/
├── references/
├── scripts/
└── evals/
```

## Main capabilities

- Create an arXiv-style LaTeX paper project.
- Build and maintain a BibTeX bibliography.
- Write survey sections incrementally.
- Generate TikZ figures and LaTeX tables.
- Compile with Linux TeX Live or Windows MiKTeX.
- Diagnose common LaTeX and BibTeX errors.
- Review academic quality and arXiv readiness.

## Recommended usage

Example user prompts:

```text
用 Claude Code 帮我写一篇 10-15 页 arXiv 风格的英文综述论文，主题是 LLM agents。
```

```text
请把这个研究主题初始化成一个 LaTeX 论文项目，并创建 main.tex 和 references.bib。
```

```text
我的论文编译失败了，请读取 main.log 并修复 undefined citation 和 LaTeX 宏包冲突。
```

## Validation scripts

Check skill structure:

```bash
uv run python scripts/check_skill_structure.py
```

Check BibTeX:

```bash
uv run python scripts/check_bibtex.py path/to/references.bib
```

Check LaTeX log:

```bash
uv run python scripts/check_latex_log.py path/to/main.log
```

## Future standalone repository

This directory is intentionally self-contained so it can later be moved into an independent Git repository and attached back to the parent project as a git submodule.
```

- [x] **Step 2: Verify README content**

Run:

```bash
python - <<'PY'
from pathlib import Path
text = Path('skills/arxiv-paper-writer/README.md').read_text(encoding='utf-8')
for phrase in ['arxiv-paper-writer', 'papers/agent-survey', 'plan → scaffold', 'Future standalone repository']:
    assert phrase in text, f'missing phrase: {phrase}'
print('skill README ok')
PY
```

Expected output:

```text
skill README ok
```

---

## Task 10: Run End-to-End Local Validation

**Files:**
- Uses all files under `skills/arxiv-paper-writer/`

- [x] **Step 1: Run structure checker**

Run:

```bash
uv run python skills/arxiv-paper-writer/scripts/check_skill_structure.py
```

Expected output:

```text
arxiv-paper-writer skill structure ok
```

- [x] **Step 2: Run BibTeX checker on seed bibliography**

Run:

```bash
uv run python skills/arxiv-paper-writer/scripts/check_bibtex.py skills/arxiv-paper-writer/templates/references.bib
```

Expected output starts with:

```text
BibTeX sanity check ok:
```

- [x] **Step 3: Create a temporary clean LaTeX log and test log checker**

Run:

```bash
printf 'This is a clean LaTeX log.\nNo fatal issues here.\n' > skills/arxiv-paper-writer/.tmp-clean.log
uv run python skills/arxiv-paper-writer/scripts/check_latex_log.py skills/arxiv-paper-writer/.tmp-clean.log
rm skills/arxiv-paper-writer/.tmp-clean.log
```

Expected output:

```text
LaTeX log sanity check ok
```

- [x] **Step 4: Create a temporary failing LaTeX log and verify checker fails**

Run:

```bash
printf '! LaTeX Error: File `missing.sty` not found.\n' > skills/arxiv-paper-writer/.tmp-bad.log
uv run python skills/arxiv-paper-writer/scripts/check_latex_log.py skills/arxiv-paper-writer/.tmp-bad.log; test $? -eq 1
rm skills/arxiv-paper-writer/.tmp-bad.log
```

Expected: command exits with status 0 because the checker correctly returned status 1 before `test $? -eq 1`.

- [x] **Step 5: Validate eval file**

Run:

```bash
uv run python - <<'PY'
import json
from pathlib import Path
path = Path('skills/arxiv-paper-writer/evals/evals.json')
data = json.loads(path.read_text(encoding='utf-8'))
assert data['skill_name'] == 'arxiv-paper-writer'
assert len(data['evals']) == 3
print('local validation ok')
PY
```

Expected output:

```text
local validation ok
```

---

## Task 11: Prepare for Future Independent Repository/Submodule

**Files:**
- Create: `skills/arxiv-paper-writer/.gitignore`
- Create: `skills/arxiv-paper-writer/SUBMODULE_NOTES.md`

- [x] **Step 1: Create skill-local .gitignore**

Write `skills/arxiv-paper-writer/.gitignore` with this content:

```gitignore
# Python
__pycache__/
*.pyc
.venv/

# Temporary validation files
.tmp-*.log

# LaTeX intermediate files
*.aux
*.bbl
*.blg
*.log
*.out
*.fls
*.fdb_latexmk
*.synctex.gz
*.toc
*.lof
*.lot

# Generated PDFs are project outputs, not skill source
output/
```

- [x] **Step 2: Create submodule notes**

Write `skills/arxiv-paper-writer/SUBMODULE_NOTES.md` with this content:

```markdown
# Submodule Notes

This skill directory is designed to become an independent repository later.

## Current phase

During initial development, keep the skill as a normal directory under the parent project:

```text
skills/arxiv-paper-writer/
```

This avoids prematurely creating a nested Git repository before the skill structure stabilizes.

## Future migration path

When ready to publish independently:

1. Create a new empty remote repository, for example `arxiv-paper-writer-skill`.
2. Copy or move `skills/arxiv-paper-writer/` into a clean standalone directory.
3. Initialize Git in that standalone directory.
4. Commit the skill files.
5. Push to the new remote.
6. Remove the normal directory from the parent repository.
7. Add it back as a submodule.

Example commands after the independent repository exists:

```bash
git rm -r skills/arxiv-paper-writer
git submodule add <REMOTE_URL> skills/arxiv-paper-writer
git commit -m "将 arxiv-paper-writer skill 接入为独立 submodule"
```

Do not run these commands until the remote repository exists and the user confirms the migration.
```

- [x] **Step 3: Verify submodule preparation files**

Run:

```bash
python - <<'PY'
from pathlib import Path
ignore = Path('skills/arxiv-paper-writer/.gitignore').read_text(encoding='utf-8')
notes = Path('skills/arxiv-paper-writer/SUBMODULE_NOTES.md').read_text(encoding='utf-8')
assert '*.aux' in ignore
assert 'git submodule add' in notes
assert 'Do not run these commands until' in notes
print('submodule preparation ok')
PY
```

Expected output:

```text
submodule preparation ok
```

---

## Task 12: Update Parent Project Documentation

**Files:**
- Modify: `README.md`
- Modify: `Memory.md`

- [x] **Step 1: Add skill subproject note to README**

In `README.md`, add this subsection under the existing project output or directory section:

```markdown
## 新增论文写作 skill 子项目

项目新增计划中的 `arxiv-paper-writer` skill 子项目：

```text
skills/arxiv-paper-writer/
```

该 skill 用于沉淀 `papers/agent-survey` 实战中已经跑通的 Claude Code 写 arXiv 规格论文流程，包括 LaTeX 项目初始化、BibTeX 构建、分章节写作、TikZ 图表、编译排错和最终质量审查。

当前阶段先作为普通目录开发，待结构稳定并创建独立远程仓库后，再迁移为 Git submodule。
```

- [x] **Step 2: Add progress note to Memory.md**

In `Memory.md`, add this entry under current progress or completed work after implementation succeeds:

```markdown
- 已规划并创建 `skills/arxiv-paper-writer/` skill 子项目，用于把 `papers/agent-survey` 的 arXiv 论文写作实战流程沉淀为可复用 Claude Code skill；当前先作为普通目录开发，后续可迁移为独立仓库并作为 submodule 接入。
```

- [x] **Step 3: Verify documentation mentions the skill**

Run:

```bash
python - <<'PY'
from pathlib import Path
readme = Path('README.md').read_text(encoding='utf-8')
memory = Path('Memory.md').read_text(encoding='utf-8')
assert 'skills/arxiv-paper-writer/' in readme
assert 'skills/arxiv-paper-writer/' in memory
print('parent documentation ok')
PY
```

Expected output:

```text
parent documentation ok
```

---

## Task 13: Final Plan and Skill Validation

**Files:**
- Uses: `skills/arxiv-paper-writer/**`
- Uses: `docs/superpowers/plans/2026-04-26-arxiv-paper-writer-skill.md`

- [x] **Step 1: Check for required skill files**

Run:

```bash
uv run python skills/arxiv-paper-writer/scripts/check_skill_structure.py
```

Expected output:

```text
arxiv-paper-writer skill structure ok
```

- [x] **Step 2: Check the plan has no unresolved placeholders**

Run:

```bash
uv run python - <<'PY'
from pathlib import Path
text = Path('docs/superpowers/plans/2026-04-26-arxiv-paper-writer-skill.md').read_text(encoding='utf-8')
for forbidden in ['TB' + 'D', 'TO' + 'DO', 'implement ' + 'later', 'fill in ' + 'details']:
    assert forbidden not in text, f'plan contains unresolved placeholder: {forbidden}'
print('plan placeholder scan ok')
PY
```

Expected output:

```text
plan placeholder scan ok
```

- [x] **Step 3: Check skill resource cross-references**

Run:

```bash
uv run python - <<'PY'
from pathlib import Path
base = Path('skills/arxiv-paper-writer')
skill = (base / 'SKILL.md').read_text(encoding='utf-8')
for rel in [
    'templates/arxiv_survey_main.tex',
    'references/workflow.md',
    'references/latex_environment.md',
    'references/bibliography.md',
    'references/figures_and_tables.md',
    'references/quality_review.md',
]:
    assert rel in skill or rel.startswith('templates/'), f'SKILL.md does not mention {rel}'
    assert (base / rel).exists(), f'missing referenced file: {rel}'
print('skill cross-reference check ok')
PY
```

Expected output:

```text
skill cross-reference check ok
```

---

## Task 14: Commit the Skill Subproject

**Files:**
- Add: `skills/arxiv-paper-writer/**`
- Add: `docs/superpowers/plans/2026-04-26-arxiv-paper-writer-skill.md`
- Modify: `README.md`
- Modify: `Memory.md`
- Modify: `CLAUDE.md`

- [x] **Step 1: Inspect working tree**

Run:

```bash
git status --short
```

Expected: shows the new skill files, plan file, and documentation updates. It may also show unrelated files; do not stage unrelated files.

- [x] **Step 2: Stage only relevant files**

Run:

```bash
git add \
  CLAUDE.md \
  README.md \
  Memory.md \
  docs/superpowers/plans/2026-04-26-arxiv-paper-writer-skill.md \
  skills/arxiv-paper-writer
```

Expected: command exits with status 0.

- [x] **Step 3: Review staged diff**

Run:

```bash
git diff --cached --stat
git diff --cached -- CLAUDE.md README.md Memory.md docs/superpowers/plans/2026-04-26-arxiv-paper-writer-skill.md skills/arxiv-paper-writer
```

Expected: diff includes only the new skill, plan, and intended documentation updates.

- [ ] **Step 4: Commit**

Run:

```bash
git commit -m "$(cat <<'EOF'
新增 arXiv 论文写作 skill 子项目计划与实现

创建 arxiv-paper-writer skill 子项目，用于沉淀已跑通的 Claude Code 写 arXiv 规格论文流程，并补充模板、参考文档、检查脚本和评测用例，方便后续迁移为独立仓库和 submodule。

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
EOF
)"
```

Expected: commit succeeds.

---

## Self-Review Checklist

- [ ] Spec coverage: the plan creates the skill, templates, references, scripts, evals, README, submodule notes, parent docs, validation, and commit.
- [x] Placeholder scan: the plan contains no unresolved placeholder markers listed in Task 13 Step 2.
- [ ] Scope check: the first version is a self-contained normal directory, not a premature nested Git repo or submodule migration.
- [ ] Type/path consistency: all file paths use `skills/arxiv-paper-writer/` consistently.
- [ ] User constraint: documentation writing is split into small chunks; future implementation should also write large documents in small file edits.

---

## Execution Options

Plan complete. Two execution options:

1. **Subagent-Driven (recommended)**: dispatch a fresh subagent per task, review between tasks, and iterate quickly.
2. **Inline Execution**: execute this plan in the current session with checkpoints after major groups of tasks.

For this specific task, inline execution is also reasonable because the files are mostly deterministic Markdown, LaTeX templates, JSON, and small Python scripts. Use subagents later when running skill evals and comparing outputs.
