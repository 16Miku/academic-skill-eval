# academic-writing-skills 运行过程记录

## 环境构建

已完成：

- `uv sync --extra dev`
- 安装 `Typst 0.14.2`
- 安装 `MiKTeX 25.12`
- 验证可用：
  - `latexmk 4.88`
  - `bibtex`
  - `biber 2.21`
  - `chktex 1.7.9`

说明：

- MiKTeX 安装后未自动进入当前 shell 的 PATH。
- 本轮通过临时加入路径 `C:\Users\文文\AppData\Local\Programs\MiKTeX\miktex\bin\x64` 完成 LaTeX 工具链验证。
- MiKTeX 报告了“尚未检查更新”的提示，但不影响当前运行。

## 真实使用方式演示设计

本轮选择 3 类代表性场景：

1. `paper-audit / quick-audit`
2. `paper-audit / deep-review`
3. `latex-paper-en / multi-module sequence`
4. `typst-paper / format`（尝试）

原因：

- `paper-audit` 最能体现投稿前检查与审稿价值。
- `latex-paper-en` 最能体现对已有英文论文的多模块写作诊断。
- `typst-paper` 用于验证其实际可演示程度。

## 演示结果概览

### 1. quick-audit

用户请求示例：

> 帮我快速检查这篇论文有没有明显投稿风险。

实际命令：

```bash
uv run python -B academic-writing-skills/paper-audit/scripts/audit.py academic-writing-skills/paper-audit/evals/fixtures/quick_audit_fixture.tex --mode quick-audit
```

结果：成功执行，但命令返回码为 `1`，因为审查结果判定为存在阻塞问题，不代表脚本异常。

### 2. deep-review

用户请求示例：

> 请模拟审稿人深度审查这篇论文，并给我 revision roadmap。

实际命令：

```bash
uv run python -B academic-writing-skills/paper-audit/scripts/audit.py academic-writing-skills/paper-audit/evals/fixtures/deep_review_fixture.tex --mode deep-review --scholar-eval
```

结果：成功执行，生成完整 deep review 产物。

### 3. latex-paper-en 多模块检查

用户请求示例：

> 检查 introduction 的语法、长句和逻辑，再检查 results 的实验叙述问题。

实际命令：

```bash
uv run python -B academic-writing-skills/latex-paper-en/scripts/analyze_grammar.py tests/fixtures/paper_audit/sample_paper.tex --section introduction
uv run python -B academic-writing-skills/latex-paper-en/scripts/analyze_sentences.py tests/fixtures/paper_audit/sample_paper.tex --section introduction
uv run python -B academic-writing-skills/latex-paper-en/scripts/analyze_logic.py tests/fixtures/paper_audit/sample_paper.tex --section introduction
uv run python -B academic-writing-skills/latex-paper-en/scripts/analyze_experiment.py tests/fixtures/paper_audit/sample_paper.tex --section results
```

结果：成功执行。

### 4. typst-paper

用户请求示例：

> 按 IEEE 风格检查这个 Typst paper 的格式。

实际命令：

```bash
uv run python -B academic-writing-skills/typst-paper/scripts/check_format.py academic-writing-skills/typst-paper/examples/sample.typ --venue ieee
```

结果：失败，原因不是环境，而是仓库中没有该示例文件：

```text
[ERROR] File not found: academic-writing-skills/typst-paper/examples/sample.typ
```

附加确认：仓库中当前不存在 `.typ` 文件。
