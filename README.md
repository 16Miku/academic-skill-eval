# academic-skill-eval

## 项目简介

本项目用于调研与测评学术论文写作、文献综述、论文总结与学术研究辅助相关的 skills，当前首批评测对象主要来自 ClawHub。

项目关注的重点不是简单查看 skill 的功能介绍，而是尽可能在接近真实科研工作流的场景中验证：这些 skill 到底能不能真正用于文献检索、论文总结、文献综述与论文写作。

## 项目目标

本项目当前重点关注以下几个问题：

- 哪些 skill 适合做文献综述
- 哪些 skill 更适合论文搜索与资料整理
- 哪些 skill 适合单篇论文总结
- 哪些 skill 更适合论文写作与综述成文
- 不同类型的 skill 在真实任务中的可用性、稳定性与产出质量如何

## 当前纳入评测的 skills

当前已纳入评测范围的 9 个 skill 如下：

- literature-review
- academic-research-hub
- research-paper-writer
- paper-summarize-academic
- paper-writing-workflow
- daily-paper-digest
- zeelin-academic-paper
- thesis-helper
- paper-summary

## skill 分类与测评思路

经过初步调研后，当前已确认本项目**不采用“一刀切地让所有 skill 都执行同一种写论文任务”**的评测方式，而是按 skill 类型分别设计测试任务。

当前认可的初步分组如下：

- 搜索 / 检索型
  - literature-review
  - academic-research-hub
  - daily-paper-digest
- 单篇总结 / 解析型
  - paper-summary
  - paper-summarize-academic
- 写作 / 工作流型
  - research-paper-writer
  - paper-writing-workflow
  - zeelin-academic-paper
  - thesis-helper

后续将针对不同类型分别设计更合适的统一测试任务，再逐个开展正式测评。

## 目录结构

本项目当前采用如下目录结构：

```text
evaluations/<skill-slug>/
  skill/
  run/
```

说明：

- `skill/`：通过 ClawHub CLI 下载的 skill 原始内容
- `run/`：该 skill 的测评任务、过程记录、输出结果和结论材料

## 当前进度

目前已完成的基础工作包括：

- 已完成论文相关 skills 的首轮初步调研
- 已创建飞书调研文档并写入初版内容
- 已初始化本地 git 仓库
- 已创建 `evaluations/` 目录结构
- 已为 9 个待测 skill 创建独立目录
- 已通过 ClawHub CLI 下载全部 9 个 skill 到各自的 `skill/` 目录
- 已确认按 skill 类型分任务进行后续评测
- 已创建项目级 `CLAUDE.md` 与 `Memory.md`

## 后续计划

接下来将继续推进以下工作：

- 为不同类型的 skill 设计具体测试任务
- 为每个 skill 的 `run/` 目录准备统一模板文件
- 逐个调用 subagent 开始实际测评
- 汇总每个 skill 的测试结果与综合结论
- 持续更新飞书汇报文档

## 协作约定

- 项目过程记录、结论说明与沟通统一使用中文
- 本环境中的 Python 由 `uv` 管理，涉及 Python 时优先使用 `uv`
- skill 搜索、下载与 inspect 优先使用 ClawHub CLI
- 调研与测评结果后续会持续沉淀到飞书文档中
