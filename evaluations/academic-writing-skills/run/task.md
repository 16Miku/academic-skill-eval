# academic-writing-skills 测评任务

## 测评对象

- 仓库：`https://github.com/bahayonghang/academic-writing-skills`
- 本地路径：`evaluations/academic-writing-skills/`
- 类型：Academic Writing Skills 套件，包含多个 Claude Code skill

## 本轮目标

按照该仓库的 skill 说明，构建必要运行环境，并用“真实使用方式”展示其核心能力。

重点观察：

1. 是否能对已有论文进行投稿前检查。
2. 是否能模拟审稿人给出深度审查意见。
3. 是否能对英文 LaTeX 论文进行多模块写作诊断。
4. 是否能覆盖 Typst / 中文学位论文等扩展场景。
5. 产出是否适合作为论文写作工作流中的实际成果。

## 测试输入

本轮优先使用仓库自带 fixture：

- `academic-writing-skills/paper-audit/evals/fixtures/quick_audit_fixture.tex`
- `academic-writing-skills/paper-audit/evals/fixtures/deep_review_fixture.tex`
- `tests/fixtures/paper_audit/sample_paper.tex`

## 演示方式

按如下结构记录：

1. 用户自然语言请求。
2. skill 路由到哪个模块或模式。
3. 实际运行命令。
4. 输出摘要。
5. 对运行效果的评价。
