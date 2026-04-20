# 项目说明

## 项目目标
本项目用于调研与测评 ClawHub 平台上和学术论文写作、文献综述、论文总结、学术研究辅助相关的 skills，并将结果沉淀为可汇报的文档材料。

当前重点不是泛泛查看功能，而是判断这些 skills 在真实科研工作流中的实际价值，尤其是：
- 是否适合文献综述
- 是否适合论文搜索与资料整理
- 是否适合单篇论文总结
- 是否适合论文写作与综述成文

## 语言与沟通
- 始终使用中文交流。
- 对外输出、过程记录、结论说明优先使用中文。
- 技术术语、命令、文件名、skill slug 保持原文。

## 工具与环境约定
- 当前系统中的 Python 由 `uv` 管理。
- 涉及 Python 运行、依赖安装、测试执行时，优先使用 `uv`。
- 使用 ClawHub CLI 进行 skill 搜索、下载与 inspect。
- 飞书文档用于最终汇报与阶段性沉淀。

## 当前目录规范
本项目的 skill 测评目录统一放在：

```text
evaluations/<skill-slug>/
  skill/
  run/
```

说明：
- `skill/`：通过 `clawhub install <slug>` 下载的 skill 原始内容
- `run/`：该 skill 的测评任务、过程记录、输出结果和结论

## 当前待测 skills
- literature-review
- academic-research-hub
- research-paper-writer
- paper-summarize-academic
- paper-writing-workflow
- daily-paper-digest
- zeelin-academic-paper
- thesis-helper
- paper-summary

## 测评原则
- 不再采用“一刀切全测写论文”的方式。
- 当前已确认：按 skill 类型分任务进行评测。
- 当前认可的初步分组：
  - 搜索 / 检索型：literature-review、academic-research-hub、daily-paper-digest
  - 单篇总结 / 解析型：paper-summary、paper-summarize-academic
  - 写作 / 工作流型：research-paper-writer、paper-writing-workflow、zeelin-academic-paper、thesis-helper
- 后续需先为不同类型的 skill 设计合适的统一测试任务，再开始具体测评。

## 项目记录要求
- Memory.md 作为本项目的持续记忆与进度板。
- 每次关键阶段推进后，都要更新 Memory.md 中的当前进度。
- 需要确保 Memory.md 中的状态与实际项目进展一致。

## Git 约定
- 当前项目已初始化本地 git 仓库。
- 提交信息使用中文。
- 在没有用户明确要求前，不主动 push 到远端。

## 汇报要求
- 调研与测评结果最终需要沉淀到飞书文档。
- 每个 skill 后续应尽量形成可复用的本地记录材料，便于汇总、对比和写汇报。
