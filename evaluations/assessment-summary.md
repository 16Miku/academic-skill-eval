# 评估结果总表

## 文档目的

本文件用于统一汇总本项目中所有 academic skills 的测试效果、适用情况、商业化适配判断，以及各 skill 测试记录文件的索引。

本项目的核心目标不是单纯查看 skill 功能，而是筛选出：

- 哪些 skill 适合论文写作
- 哪些 skill 适合文献综述
- 哪些 skill 适合论文总结
- 哪些 skill 适合作为未来云端 Agent 服务的能力组件进行商业化落地

因此，本文件中的评估不只关注“能不能运行”，还关注：

- 实际任务效果
- 输出质量
- 稳定性与工程可接入性
- 是否适合封装为商业化能力模块

---

## 当前阶段性分层

### 继续观察
- `literature-review`
- `academic-research-hub`
- `paper-writing-workflow`

### 待测
- `paper-summary`
- `paper-summarize-academic`
- `research-paper-writer`
- `zeelin-academic-paper`
- `thesis-helper`

### 暂不推荐
- `daily-paper-digest`

---

## 总表

| skill | 类型 | 当前状态 | 当前结论 | 适合做什么 | 不适合做什么 | 商业化适配判断 | 备注 |
|---|---|---|---|---|---|---|---|
| literature-review | 搜索 / 检索型 | 已完成首轮测试 | 一般 | 文献检索、摘要获取、metadata 查询 | 直接生成高质量综述成品 | 中 | 能检索真实论文，但依赖和编码问题明显，仍需人工筛选 |
| academic-research-hub | 搜索 / 检索型 | 已完成首轮测试 | 一般 | 按来源分别检索论文、导出结构化结果 | 自动聚合综述资料、稳定完成多源整合 | 中 | 更像 academic search toolbox，Semantic Scholar 本轮失败 |
| daily-paper-digest | 搜索 / 检索型（偏速递） | 已完成首轮测试 | 不推荐 | 每日论文播报、研究热点订阅、信息流聚合（理论上） | 稳定支撑综述导向专题检索、稳定完成每日速递 | 低 | 本轮实际表现说明当前不好用：默认主链路与昨天窗口补测均返回空结果 |
| paper-summary | 单篇总结 / 解析型 | 待测 | 待定 | 待测 | 待测 | 待定 | 待执行首轮测试 |
| paper-summarize-academic | 单篇总结 / 解析型 | 待测 | 待定 | 待测 | 待测 | 待定 | 待执行首轮测试 |
| research-paper-writer | 写作 / 工作流型 | 待测 | 待定 | 待测 | 待测 | 待定 | 待执行首轮测试 |
| paper-writing-workflow | 写作 / 工作流型 | 已完成首轮测试 | 一般 | 论文写作流程拆解、通用大纲生成、作为上层 workflow / 模板框架组织写作步骤 | 直接自动产出高质量论文初稿、稳定串联依赖 skill 完成端到端写作 | 中低 | 更像 workflow 脚手架；联动补测显示关键依赖 `research-paper-writer` 当前仍是 stub |
| zeelin-academic-paper | 写作 / 工作流型 | 待测 | 待定 | 待测 | 待测 | 待定 | 待执行首轮测试 |
| thesis-helper | 写作 / 工作流型 | 待测 | 待定 | 待测 | 待测 | 待定 | 待执行首轮测试 |

---

## 已测 skill 简要结论

### 1. literature-review
- 类型：搜索 / 检索型
- 当前结论：一般
- 适用场景：文献综述前期资料搜集、找论文、看摘要、拿 DOI / PDF 线索
- 主要优点：支持 multi-source academic search，返回字段较完整，details 能拿到更丰富 metadata
- 主要问题：首次运行依赖缺失，Windows 编码环境下会触发 `UnicodeEncodeError`，多源结果仍需人工筛选
- 商业化判断：**中**
  - 适合作为“检索辅助能力模块”观察保留
  - 暂不适合作为高质量综述生成的核心能力模块

### 2. academic-research-hub
- 类型：搜索 / 检索型
- 当前结论：一般
- 适用场景：按来源手动检索学术论文、输出结构化结果、作为 research toolbox 使用
- 主要优点：自带 `requirements.txt`，source 维度清晰，命令结构明确，支持多种输出格式
- 主要问题：Semantic Scholar 本轮失败，不支持自动多源聚合和去重，PubMed 结果偏离当前主题较多
- 商业化判断：**中**
  - 更适合作为底层检索工具模块
  - 不适合作为文献综述任务中的核心检索整合器

### 3. daily-paper-digest
- 类型：搜索 / 检索型（偏每日聚合 / 速递）
- 当前结论：不推荐
- 适用场景：理论上适合每日论文播报、研究热点订阅、面向聊天应用的学术信息流推送
- 主要优点：输出格式友好，支持默认配置和 LLM 定向配置，arXiv 专题搜索子能力可运行
- 主要问题：依赖声明冲突导致无法直接安装，默认主链路为空结果，补测昨天窗口后 arXiv 仍为 0，HuggingFace 抓取脆弱，专题搜索相关性不足
- 商业化判断：**低**
  - 当前实际表现说明这个 skill 不好用
  - 不建议作为重点候选继续投入精力

---

### 4. paper-writing-workflow
- 类型：写作 / 工作流型
- 当前结论：一般
- 适用场景：论文写作流程拆解、通用大纲生成、作为 workflow / 模板框架组织多个写作步骤
- 主要优点：6 阶段流程清晰，模板与检查项较全，可生成 Markdown 大纲文件
- 主要问题：依赖 skill 生态不完整，主脚本不会自动编排调用，联动补测显示关键依赖 `research-paper-writer` 仍是 stub，无法真正生成论文初稿
- 商业化判断：**中低**
  - 可作为“论文写作编排层 / 模板层”继续观察
  - 暂不适合作为高质量论文生成核心模块

---

## 文件索引

### literature-review
- 测试任务：`evaluations/literature-review/run/task.md`
- 原始输出：`evaluations/literature-review/run/raw-output.md`
- 运行记录：`evaluations/literature-review/run/notes.md`
- 综合结论：`evaluations/literature-review/run/conclusion.md`

### academic-research-hub
- 测试任务：`evaluations/academic-research-hub/run/task.md`
- 原始输出：`evaluations/academic-research-hub/run/raw-output.md`
- 运行记录：`evaluations/academic-research-hub/run/notes.md`
- 综合结论：`evaluations/academic-research-hub/run/conclusion.md`

### daily-paper-digest
- 测试任务：`evaluations/daily-paper-digest/run/task.md`
- 原始输出：`evaluations/daily-paper-digest/run/raw-output.md`
- 运行记录：`evaluations/daily-paper-digest/run/notes.md`
- 综合结论：`evaluations/daily-paper-digest/run/conclusion.md`

### paper-summary
- 测试任务：`evaluations/paper-summary/run/task.md`
- 原始输出：`evaluations/paper-summary/run/raw-output.md`
- 运行记录：`evaluations/paper-summary/run/notes.md`
- 综合结论：`evaluations/paper-summary/run/conclusion.md`

### paper-summarize-academic
- 测试任务：`evaluations/paper-summarize-academic/run/task.md`
- 原始输出：`evaluations/paper-summarize-academic/run/raw-output.md`
- 运行记录：`evaluations/paper-summarize-academic/run/notes.md`
- 综合结论：`evaluations/paper-summarize-academic/run/conclusion.md`

### research-paper-writer
- 测试任务：`evaluations/research-paper-writer/run/task.md`
- 原始输出：`evaluations/research-paper-writer/run/raw-output.md`
- 运行记录：`evaluations/research-paper-writer/run/notes.md`
- 综合结论：`evaluations/research-paper-writer/run/conclusion.md`

### paper-writing-workflow
- 测试任务：`evaluations/paper-writing-workflow/run/task.md`
- 原始输出：`evaluations/paper-writing-workflow/run/raw-output.md`
- 运行记录：`evaluations/paper-writing-workflow/run/notes.md`
- 综合结论：`evaluations/paper-writing-workflow/run/conclusion.md`

### zeelin-academic-paper
- 测试任务：`evaluations/zeelin-academic-paper/run/task.md`
- 原始输出：`evaluations/zeelin-academic-paper/run/raw-output.md`
- 运行记录：`evaluations/zeelin-academic-paper/run/notes.md`
- 综合结论：`evaluations/zeelin-academic-paper/run/conclusion.md`

### thesis-helper
- 测试任务：`evaluations/thesis-helper/run/task.md`
- 原始输出：`evaluations/thesis-helper/run/raw-output.md`
- 运行记录：`evaluations/thesis-helper/run/notes.md`
- 综合结论：`evaluations/thesis-helper/run/conclusion.md`

---

## 当前阶段建议

### 当前优先级 1
开始单篇总结 / 解析型：
- `paper-summary`
- `paper-summarize-academic`

目标：验证哪些 skill 真的能稳定完成论文总结，而不是只改写摘要。

### 当前优先级 2
继续写作 / 工作流型：
- `research-paper-writer`
- `zeelin-academic-paper`
- `thesis-helper`

目标：筛选哪些 skill 真正能支撑综述草稿写作，哪些只是模板或工作流包装。

### 当前优先级 3
汇总已测写作 / 工作流型的横向判断：
- `paper-writing-workflow`

目标：明确它更适合作为编排层 / 模板层，还是后续应降级观察优先级。

---

## 商业化视角下的当前判断

截至目前，已完成测试的四个 skill 中，`daily-paper-digest` 已可以明确排除；`literature-review` 与 `academic-research-hub` 仍更接近“检索辅助 / toolbox”方向；`paper-writing-workflow` 则证明了工作流编排思路存在一定价值，但目前仍不足以作为高质量论文生成核心模块投入。

### 当前更接近可用的方向
- **检索辅助型能力模块**：可继续观察 `literature-review`
- **底层 research toolbox 模块**：可继续观察 `academic-research-hub`
- **写作编排 / 模板层模块**：可继续观察 `paper-writing-workflow`

### 当前仍不足的地方
- 稳定性仍不够
- 开箱即用程度不足
- 需要较多人工后处理
- 对“高质量论文综述产出”的支持还不够强

因此，后续重点应该放在：
1. 是否能找到真正强的总结型 / 写作型 skill
2. 是否能组合多个 skill 形成更适合商业化的工作流
3. 最终不是只挑单个 skill，而是识别哪些能力适合被拆成云端 Agent 服务模块
