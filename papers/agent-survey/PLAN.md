# Context

用户需求发生了根本转变：不再是测评 skill，而是**用 CC 实战写出一篇 arXiv 水平的 Agent 发展历程综述论文**。Skill 只是辅助手段之一，核心是 CC 直接写 LaTeX，遇到问题就解决问题。

论文主题：**AI Agent 发展历程综述**（从早期符号 AI 到 LLM-based Agent 到多智能体系统）

目标产出：一篇 10-15 页的英文综述论文，LaTeX 排版，PDF 输出，包含真实引用、图表、标准学术结构。

# Recommended Approach

## 总体策略

**CC 直接写 LaTeX + 免费工具链辅助**，不依赖付费 API Key。

- 文献检索：CC 自带 WebSearch + BrightData MCP + Semantic Scholar API（免费）
- 内容生成：CC 直接写 LaTeX 源码，分章节逐步生成
- 图表：CC 用 Python matplotlib/tikz 生成
- 编译：本地 MiKTeX（pdflatex/xelatex 已确认可用）
- 模板：复用 `latex-document-skill` 的 `academic-paper.tex`

## 工作目录

```
A:/study/AI/LLM/Skill-Test/Paper-Write-Skill-Test/
└── papers/agent-survey/          # 论文项目目录
    ├── main.tex                  # 主 LaTeX 文件
    ├── references.bib            # BibTeX 引用库
    ├── figures/                  # 图表
    ├── sections/                 # 分章节 .tex 文件（可选，如果单文件太长）
    └── output/                   # 编译产出（PDF 等）
```

## 写作流程（6 步）

### Step 1：项目初始化
- 创建 `papers/agent-survey/` 目录结构
- 从 `latex-document-skill/assets/templates/academic-paper.tex` 复制模板
- 定制模板：标题、作者、摘要占位、章节骨架
- 创建空 `references.bib`
- 编译验证模板可用

### Step 2：文献检索与 BibTeX 构建
**主要工具：literature-review skill（多源检索 + BibTeX 导出）**

检索策略：
- 用 literature-review skill 搜索关键主题：
  - "large language model agents"
  - "autonomous agents survey"
  - "ReAct prompting"
  - "multi-agent systems LLM"
  - "agent frameworks AutoGPT LangChain"
- 数据源：Semantic Scholar + OpenAlex + Crossref + PubMed（自动去重）
- 导出格式：BibTeX
- 补充检索：WebSearch / BrightData 查找最新论文（2024-2025）
- 目标：收集 40-60 篇真实论文的 BibTeX 条目，包含完整元数据（DOI/volume/pages）

### Step 3：论文骨架（LaTeX skeleton）
在 `main.tex` 中建立完整章节结构：
1. Abstract
2. Introduction（Agent 的定义、重要性、综述范围）
3. Background（Agent 形式化定义、核心组件：感知/推理/行动/记忆）
4. Early Foundations（符号 AI Agent、BDI、强化学习 Agent）
5. LLM-Based Agents（Prompting 策略、记忆与规划、工具使用）
6. Agent Frameworks（AutoGPT、LangChain、CrewAI 等）
7. Multi-Agent Systems（协作、通信、集体智能）
8. Applications（代码生成、科研、自动化）
9. Challenges and Future Directions（可靠性、安全、评估）
10. Conclusion

每个章节先写 TODO 注释说明要点和预计引用。

### Step 4：分章节内容生成
**核心策略：每次生成 1-2 个章节，边写边引用。**

对每个章节：
1. 先用 WebSearch 补充该章节特定文献
2. CC 直接写 LaTeX 内容（800-2000 词/章节）
3. 使用 `\cite{}` 引用真实论文
4. 同步更新 `references.bib`
5. 编译验证无错误

章节生成顺序：Introduction → Background → Early Foundations → LLM-Based Agents → Frameworks → Multi-Agent → Applications → Challenges → Conclusion → Abstract（最后写）

### Step 5：图表生成与集成
计划生成 3-5 个图表：
- Fig 1：Agent 发展时间线（Python matplotlib 或 TikZ）
- Fig 2：LLM-based Agent 架构图（TikZ）
- Fig 3：主要 Agent 框架对比表（LaTeX table）
- Fig 4：多智能体协作模式图（TikZ）
- 可选 Fig 5：论文引用趋势图（matplotlib）

### Step 6：编译、审查、迭代
- `pdflatex` + `bibtex` 编译
- 检查：页数、引用完整性、图表位置、格式规范
- 迭代修复问题
- 产出最终 PDF

## 关键文件

| 文件 | 用途 |
|------|------|
| `latex-document-skill/assets/templates/academic-paper.tex` | LaTeX 模板 |
| `latex-document-skill/scripts/compile_latex.sh` | 编译脚本 |
| `latex-document-skill/scripts/fetch_bibtex.sh` | BibTeX 获取 |
| `latex-document-skill/scripts/latex_wordcount.sh` | 字数统计 |

## Verification

1. Step 1 完成后：`pdflatex main.tex` 编译通过，产出空白模板 PDF
2. Step 2 完成后：`references.bib` 包含 40+ 条目，每条有 DOI
3. Step 3 完成后：骨架编译通过，目录结构完整
4. Step 4 每个章节完成后：编译通过，无 undefined citation 警告
5. Step 5 完成后：图表正确显示在 PDF 中
6. Step 6 完成后：最终 PDF 10-15 页，格式规范，可提交 arXiv
