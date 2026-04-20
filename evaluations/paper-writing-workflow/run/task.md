# 测试任务

## 基本信息
- skill：paper-writing-workflow
- 类型：写作 / 工作流型
- 测试轮次：第 1 轮
- 测试日期：2026-04-21
- 执行方式：安装该 skill 声明的全部依赖 skills，并按 workflow 方式完成联动测试

## 固定主题
Agent Systems for LLMs

## 测试设计说明
`paper-writing-workflow` 不是单体写作 skill，而是一个整合多个论文写作相关 skills 的 workflow orchestrator。因此本轮测试不再只看“能否单独输出 800–1200 字正文”，而是评估：在依赖 skills 补齐后，它是否能真正推进论文写作流程，并形成有价值的中间与最终产出。

## 依赖范围
### Required skills
- scientific-writing
- academic-writing
- research-paper-writer
- citation-management

### Optional skills
- literature-search-workflow
- paper-parse
- scientific-schematics
- scientific-visualization
- hypothesis-generation

## 测试模块 A：依赖安装与可执行性
### 任务目标
验证主 skill 及其声明依赖是否能通过 ClawHub CLI 安装，并具备后续 workflow 联动测试的基础。

### 关注点
1. 所有声明依赖是否真实存在
2. 是否能成功安装
3. 是否有结构异常、缺少文件、依赖冲突等问题
4. 这套 workflow 的前提是否成立

## 测试模块 B：主 workflow 编排能力
### 任务目标
围绕主题 `Agent Systems for LLMs`，验证主 skill 是否能输出清晰的 6 阶段写作流程与下一步执行建议。

### 关注点
1. 是否输出完整 6 阶段
2. 阶段顺序是否合理
3. 是否明确关联相应依赖 skills
4. 更像真实 workflow 还是仅仅列出清单

## 测试模块 C：大纲生成能力
### 任务目标
验证主 skill 自身最明确的可执行产出：论文大纲生成。

### 关注点
1. 是否成功生成大纲文件
2. 大纲是否结构清晰
3. 是否适合作为 `Agent Systems for LLMs` 论文 / 综述的骨架
4. 是否只是泛化模板而缺少主题贴合度

## 测试模块 D：依赖联动后的写作推进能力
### 任务目标
在依赖 skills 就位后，尽量按 workflow 顺序推进写作任务，验证是否能形成有价值的联动产出。

### 关注点
1. 各阶段是否有实质产出
2. 是否能形成：文献线索 / 大纲 / 部分正文 / 图表计划 / 检查结果
3. 是否真的推进论文写作，而不只是给建议
4. 各 skill 之间是否容易衔接

## 最终评估要求
除常规“可用 / 一般 / 不推荐”外，还需单独判断：
1. 它是否是一个真正可执行的论文写作 workflow skill
2. 它是否适合作为论文写作主能力模块
3. 它更适合作为：核心 workflow / 编排辅助模块 / 模板型能力 / 暂不推荐
