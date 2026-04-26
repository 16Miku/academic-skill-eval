# 项目记忆

## 稳定信息
- 本项目用于调研与测评 ClawHub 上和学术论文写作、文献综述、论文总结相关的 skills。
- 当前采用的测评目录结构为：`evaluations/<skill-slug>/{skill,run}`。
- 当前待测的 9 个 skill：
  - literature-review
  - academic-research-hub
  - research-paper-writer
  - paper-summarize-academic
  - paper-writing-workflow
  - daily-paper-digest
  - zeelin-academic-paper
  - thesis-helper
  - paper-summary
- 当前已确认采用"按 skill 类型分任务"的测评方式，而不是所有 skill 都统一测试写论文。
- 当前认可的初步分组：
  - 搜索 / 检索型：literature-review、academic-research-hub、daily-paper-digest
  - 单篇总结 / 解析型：paper-summary、paper-summarize-academic
  - 写作 / 工作流型：research-paper-writer、paper-writing-workflow、zeelin-academic-paper、thesis-helper
- `paper-writing-workflow` 子目录中安装的 `research-paper-writer` 与项目待测 skill `research-paper-writer` 是同一个 skill。
- `research-paper-writer` 当前实际实现为 stub，只有成功消息与输入回显，不能实际生成论文正文。
- 本项目要求始终用中文交流。
- 本环境中 Python 由 `uv` 管理，涉及 Python 优先使用 `uv`。
- git 提交信息使用中文。

## 当前进度

### 已完成
- 已完成论文相关 skills 的初步调研。
- 已创建飞书调研文档并写入初版内容。
- 已初始化本地 git 仓库。
- 已创建 `evaluations/` 目录结构。
- 已为 9 个待测 skill 创建独立目录。
- 已通过 ClawHub CLI 下载全部 9 个 skill 到各自的 `skill/` 目录。
- 已确认后续测评采用方案 A：全量建目录，统一框架，后续逐个执行。
- 已确认目录名直接使用 skill slug。
- 已确认不再用统一写论文任务测所有 skill，而是按 skill 类型分任务测评。
- 已确认当前的 skill 分组方案。
- 已创建项目级 `CLAUDE.md`。
- 已创建项目 `Memory.md`。
- 已创建项目根目录 `README.md`。
- 已为各 skill 的 `run/` 目录创建统一模板文件。
- 已完成 `literature-review` 的首轮功能测试。
- 已完成 `academic-research-hub` 的首轮功能测试。
- 已完成 `daily-paper-digest` 的首轮功能测试。
- 已完成 `paper-writing-workflow` 的首轮测试与联动补测。
- 已完成 `zeelin-academic-paper` 与 `literature-review` 联动的首轮写作补测，并已产出完整论文纯文本。
- 已完成 `thesis-helper` 的完整脚本流程测试，确认其更适合作为论文辅助工具箱。
- 已确认 `research-paper-writer` 当前为 stub，无法实际写出论文正文，并已同步更新总表判断。
- 已完成 `academic-writing-skills` 的首轮测试，确认其更适合作为论文后处理 / 投稿前审查 / reviewer 模拟工具套件。
- 已完善 `.gitignore`（新增 `.claude/`、`__pycache__/`、`.venv/`、LaTeX 编译中间文件）。
- 已规划、创建并发布 `arxiv-paper-writer` skill 子项目，用于把 `papers/agent-survey` 的 arXiv 论文写作实战流程沉淀为可复用 Claude Code skill；当前已推送到 GitHub 独立仓库 `https://github.com/16Miku/arxiv-paper-writer-skill.git`，父项目已通过 Git submodule 接入，并已发布到 ClawHub：`https://clawhub.ai/16miku/arxiv-paper-writer`（版本：`arxiv-paper-writer@0.1.0`，发布 ID：`k97ekrsn8p00174pmzjf826pfd85gr5s`）。已按方案 B 选择性迁移 `papers/agent-survey` 可复用资产到 skill，包括完整 9 章节 survey 模板、39 条真实 BibTeX 示例、TikZ/表格模板、实践指南、Linux TeX Live 指南和流程图资产。

### 已完成：arXiv 论文实战（2026-04-21 ~ 2026-04-23）
- **主题**：AI Agent 发展历程综述
- **产出**：
  - 15 页 PDF（`papers/agent-survey/output/agent-survey.pdf`）
  - 39 篇真实引用（`papers/agent-survey/references.bib`）
  - 2 幅 TikZ 图（Agent 架构图 + 发展时间线）+ 2 个表格
  - 9 个章节 + Abstract + Keywords
- **配套文档**：
  - `papers/agent-survey/PRACTICE_GUIDE.md`（~1000 行完整实践指南，含 uv 安装步骤）
  - `papers/agent-survey/PLAN.md`（执行计划）
  - `papers/agent-survey/lit_search_llm_agents.json`（文献检索中间数据）
- **核心结论**：CC 直接写 LaTeX 可行，不依赖任何 paper-writing skill，免费工具链足够。
- **Git / 发布记录**：
  - `2213315` 完成用 CC 写出 arXiv 规格论文的首次实战
  - `fe2c225` 补充 PRACTICE_GUIDE.md 中 uv 安装步骤文档
  - `8358bb2` 完善 .gitignore 并提交遗漏的中间产物
  - `arxiv-paper-writer` 已发布为独立 skill：GitHub `https://github.com/16Miku/arxiv-paper-writer-skill.git`，ClawHub `https://clawhub.ai/16miku/arxiv-paper-writer`（版本：`arxiv-paper-writer@0.1.0`，发布 ID：`k97ekrsn8p00174pmzjf826pfd85gr5s`）
  - 已将 `papers/agent-survey` 成功实战中的核心可复用资产按方案 B 模板化迁移到 `skills/arxiv-paper-writer/`，并在独立 skill 仓库提交为 `29bbb64`。

### 进行中
- 正在逐个执行各 skill 的实际测评，并沉淀测试记录与综合结论。

### 待完成
- 开始单篇总结 / 解析型 skill（`paper-summary`、`paper-summarize-academic`）的首轮测试。
- 汇总各类型 skill 的阶段性横向对比结论。
- 持续更新飞书汇报文档。

## 实战经验：用 CC 写 arXiv 论文

### 成功经验
1. **CC 直接写 LaTeX 可行**：不需要依赖 skill，CC 可以直接生成高质量 LaTeX 源码
2. **免费工具链足够**：
   - 文献检索：BrightData search_engine（免费）+ 已有知识构建 BibTeX
   - 编译：本地 MiKTeX（pdflatex + bibtex）
   - 图表：TikZ 直接在 LaTeX 中绘制
3. **分步策略有效**：
   - Step 1: 初始化项目 + 模板
   - Step 2: 构建 BibTeX 文献库
   - Step 3: 写论文骨架和内容
   - Step 4: 生成图表
   - Step 5: 编译产出 PDF
4. **LaTeX 模板复用**：`latex-document-skill` 的 `academic-paper.tex` 模板质量很高，直接可用

### 遇到的问题与解决
1. **Semantic Scholar API 限流**：改用 BrightData + 已有知识构建文献库
2. **WebSearch API 错误**：改用 BrightData search_engine
3. **MiKTeX 宏包缺失**：配置 AutoInstall（`initexmf --set-config-value="[MPM]AutoInstall=yes"`）+ 手动安装关键宏包
4. **MiKTeX 编译卡住**：AutoInstall 配置后解决，不再弹窗等待确认
5. **newtxmath/amssymb \Bbbk 冲突**：添加 `\let\Bbbk\relax` 解决
6. **literature-review skill 脚本退出码 49**：跳过，直接用其他方式检索文献

### 核心发现
- **Skill 不是必需的**：对于论文写作，CC 的核心能力（理解需求、生成 LaTeX、引用文献）已经足够
- **Skill 的价值在于自动化**：如果有稳定的文献检索 API、自动化的图表生成、端到端的编译流程，skill 能提高效率
- **当前 skill 生态的问题**：
  - 依赖付费 API（PARALLEL_API_KEY、OPENROUTER_API_KEY）
  - 脚本稳定性不足（literature-review 退出码 49）
  - 端到端流程不完整（缺少从内容到 PDF 的自动化）

## 备注
- 后续每推进一个关键阶段，都需要同步更新本文件中的"当前进度"。
