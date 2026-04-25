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
