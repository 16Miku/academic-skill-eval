# 测试任务

## 基本信息
- skill：zeelin-academic-paper
- 类型：写作 / 工作流型
- 测试轮次：第 1 轮
- 测试日期：2026-04-21
- 执行方式：采用与 `literature-review` 联动的测试方式，只使用该 skill 的说明与模板推进写作

## 固定主题
Agent Systems for LLMs

## 测试目标
验证该 skill 在接入前序 `literature-review` 检索得到的真实文献线索后，是否能够明显提升论文写作结果，真正围绕文献写出可继续加工的中文论文初稿，而不只是模板化扩写。

## 联动输入来源
来自 `evaluations/literature-review/run/raw-output.md:16` 之后的已测结果，优先使用已检索到的真实论文线索作为输入材料。

### 固定参考文献线索
1. A survey on LLM-based multi-agent systems: workflow, infrastructure, and challenges
   - DOI: 10.1007/s44336-024-00009-2
   - Year: 2024
2. LLM-Based Multi-Agent Systems for Software Engineering: Literature Review, Vision, and the Road Ahead
   - DOI: 10.1145/3712003
   - Year: 2025
3. ALI-Agent: Assessing LLMs' Alignment with Human Values via Agent-based Evaluation
   - DOI: 10.52202/079017-3142
   - Year: 2024
4. Program Code Generation: Single LLMs vs. Multi-Agent Systems
   - DOI: 10.1109/icnlp65360.2025.11108400
   - Year: 2025

### 固定研究背景 / 综述材料
大语言模型正在从单轮问答工具演变为具备规划、工具调用、任务分解、记忆保持、多 Agent 协作与自我反思能力的 Agent Systems。已有检索结果显示，当前研究至少覆盖：多 Agent 系统总体框架、面向软件工程的 multi-agent 应用、基于 agent 的对齐评测，以及单 LLM 与多 Agent 系统的能力对比。现在需要基于这些真实文献线索，生成一篇中文理工科论文初稿，围绕 Agent Systems for LLMs 的研究框架、能力结构、代表性路线与现实挑战展开。

## 测试模块

### 模块 A：联动适配性检查
目标：确认该 skill 是否天然适合接收“真实文献题目 + 综述背景”作为输入，并以此推进论文写作。

关注点：
1. 文档是否明确要求参考文献与背景输入
2. 文档是否承诺生成题目、大纲与各章节正文
3. 该 skill 是否更适合作为“文献后处理写作器”而不是独立检索器

### 模块 B：基于真实文献的题目与大纲生成
目标：验证它是否能利用前序检索结果，生成贴合文献线索的中文题目与五段式大纲。

任务：
1. 基于固定参考文献与背景生成中文论文题目
2. 生成五段式 JSON 大纲

关注点：
- 题目是否贴近 multi-agent / agent systems 主题
- 大纲是否体现真实研究方向，而不是泛泛 AI 套话
- 论证和建议部分是否和前序文献线索相关

### 模块 C：基于真实文献的正文生成能力
目标：验证它能否把真实文献线索转化为更像论文的正文，而不只是模板输出。

任务：
至少继续生成以下内容：
1. 引言
2. 综述或综述收束段
3. 论证部分（至少体现理论框架 / 研究问题与假设 / 研究设计）
4. 结论

关注点：
- 是否是连续成文的中文学术写作
- 是否比“空输入写作”更具体、更像基于文献展开
- 是否出现对代表工作、研究方向、挑战的真实呼应
- 是否仍然只是高密度模板套写

### 模块 D：联动增益判断
目标：判断 `literature-review` 的真实文献输入是否实际提高了该 skill 的产出质量。

关注点：
1. 参考文献是否真正被利用，而不只是摆设
2. 输出是否明显优于纯模板化写作
3. 是否出现虚构实验、虚构数据或不受文献支持的扩写
4. 是否适合未来作为“检索 + 写作”链路中的写作核心模块候选

## 可接受结果
- 至少输出：题目 + 大纲 + 若干关键章节正文
- 更理想结果：形成一篇结构完整、和真实文献线索明显相关的中文论文初稿

## 评测结论关注点
- `literature-review` 提供的真实文献是否能提升其论文写作质量
- 它是不是值得作为“文献检索后的论文生成器”继续观察
- 它的输出到底是可用草稿，还是带真实文献外壳的模板化文本
- 它是否值得纳入商业化候选能力池
