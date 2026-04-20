# academic-writing-skills 阶段性结论

## 本轮结论

`academic-writing-skills` 更适合被定位为：

> 针对已有论文草稿的“审稿 + 检查 + 精修 + 修改路线图”工具套件

而不是：

> 从零生成整篇论文的写作 agent

## 运行效果概括

### 1. `paper-audit` 最有实用价值

本轮实际运行表明：

- `quick-audit` 能快速识别投稿风险、阻塞问题、引用错误和实验叙事问题。
- `deep-review` 能给出接近 reviewer 风格的结构化报告，并产出 revision roadmap。
- 输出不仅有打分，还有明确的优先级和修改方向。

适合场景：

- 投稿前体检
- 导师/合作者内部预审
- rebuttal 前自查
- 修订路线制定

### 2. `latex-paper-en` 适合英文论文精修

本轮多模块演示说明：

- 它适合按模块拆开处理已有论文问题。
- 尤其是 `experiment` 模块，对实验章节的诊断较有价值。
- `grammar / sentences / logic / experiment` 可以形成清晰的分模块反馈，而不是混成一段泛化建议。

适合场景：

- introduction 精修
- experiments 章节诊断
- related work 重构
- title / abstract / deai 优化

### 3. `typst-paper` 当前仓库样例不完整

本轮未能直接跑通 `typst-paper`，原因不是环境，而是仓库缺少可直接调用的 `.typ` 示例文件。

这意味着：

- skill 设计存在
- 但仓库自带演示材料不足
- 若后续要正式评测，需要自备最小 Typst 示例输入

## 对“写论文 skill”的评价

如果评价标准是：

### 能否从零写出完整论文？

- 不强
- 不是它的主要定位

### 能否显著提升已有论文质量？

- 有明显价值
- 尤其在投稿前检查与深度审稿模拟上表现突出

## 建议的后续动作

1. 继续测试 `latex-thesis-zh`，补足中文学位论文场景。
2. 把 `paper-audit` 作为重点候选 skill 单独纳入最终汇报。
3. 对 `typst-paper` 额外准备一个最小 `.typ` 文件，再做补测。

## 当前阶段判断

从真实科研工作流价值看，`academic-writing-skills`：

- **不适合**作为“从零写论文”的核心候选
- **适合**作为“已有论文后处理与投稿前审查”的强候选 skill
