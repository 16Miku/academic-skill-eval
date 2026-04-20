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
- 当前已确认采用“按 skill 类型分任务”的测评方式，而不是所有 skill 都统一测试写论文。
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
- 已创建本项目 `Memory.md`。
- 已创建项目根目录 `README.md`。
- 已为各 skill 的 `run/` 目录创建统一模板文件。
- 已完成 `literature-review` 的首轮功能测试。
- 已完成 `academic-research-hub` 的首轮功能测试。
- 已完成 `daily-paper-digest` 的首轮功能测试。
- 已完成 `paper-writing-workflow` 的首轮测试与联动补测。
- 已完成 `zeelin-academic-paper` 与 `literature-review` 联动的首轮写作补测，并已产出完整论文纯文本。
- 已完成 `thesis-helper` 的完整脚本流程测试，确认其更适合作为论文辅助工具箱。
- 已确认 `research-paper-writer` 当前为 stub，无法实际写出论文正文，并已同步更新总表判断。
- 已创建 `.gitignore` 并将 `.clawhub/` 加入忽略规则。
- 已完成首轮项目基础内容提交。
- 已完成 `academic-writing-skills` 的首轮测试，确认其更适合作为论文后处理 / 投稿前审查 / reviewer 模拟工具套件，不适合从零生成论文正文，也不适合把纯文本 / Markdown 直接转换成专业 LaTeX 论文。

### 进行中
- 正在逐个执行各 skill 的实际测评，并沉淀测试记录与综合结论。

### 待完成
- 开始单篇总结 / 解析型 skill（`paper-summary`、`paper-summarize-academic`）的首轮测试。
- 汇总各类型 skill 的阶段性横向对比结论。
- 持续更新飞书汇报文档。

## 备注
- 后续每推进一个关键阶段，都需要同步更新本文件中的“当前进度”。
