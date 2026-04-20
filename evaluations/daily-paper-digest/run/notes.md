# 运行记录

## 基本记录
- 执行时间：2026-04-21
- 使用入口：`test.py`、`main.py`、`arxiv_fetcher.py`
- 是否严格只使用该 skill：是

## 环境准备
执行命令：
```bash
uv pip install --python .venv -r requirements.txt
```

安装结果：失败，原因是依赖声明冲突：
- `arxiv==2.1.0` 依赖 `feedparser==6.0.10`
- skill 自身 `requirements.txt` 又固定为 `feedparser==6.0.11`

后续为了继续验证运行链路，改为手动安装可解析版本组合：
```bash
uv pip install --python .venv arxiv==2.1.0 requests==2.31.0 beautifulsoup4==4.12.3 feedparser==6.0.10
```

其中 `lxml==5.1.0` 在当前 Python 3.14 / Windows 环境下构建失败，需要 Microsoft C++ Build Tools，但该 skill 实际运行并未依赖 `lxml` 主链路，因此测试得以继续。

## 实际执行模块
1. `test.py`：用于验证依赖导入、arXiv 模块、HuggingFace 模块、主模块初始化
2. `main.py`：验证默认 daily digest 主链路
3. `PaperDigest(config_path='config/sources_llm.json')`：验证 LLM 定向过滤链路
4. `ArxivFetcher.search_papers('Agent Systems for LLMs')`：验证专题搜索补充链路
5. 主题搜索结果再经过 LLM 关键词过滤：验证过滤是否提升相关性

## 过程记录
- 该 skill 的主定位更接近“每日论文速递 / 聚合输出”，而不是专题检索工具。
- `test.py` 能证明各模块可导入，且 arXiv 搜索子能力可工作。
- 默认 `main.py` 运行时，arXiv 最近一天结果为 0，HuggingFace 解析结果也为 0，因此最终输出为空日报。
- 补充按“昨天窗口”放宽测试后，arXiv 结果仍为 0，说明不能简单把空结果归因于“凌晨测试时点”。
- `sources_llm.json` 配置文件存在，说明作者考虑过 LLM 定向过滤场景。
- 但在抓取源本身返回 0 的情况下，过滤链路无法发挥作用。
- `search_papers('Agent Systems for LLMs')` 能返回 5 条结果，但主题精度较差，结果混入明显无关论文。

## 问题与异常
- `requirements.txt` 自身存在版本冲突，不能直接完整安装。
- `lxml==5.1.0` 在当前环境下构建失败。
- HuggingFace 抓取返回 0 篇，疑似页面结构变化或解析逻辑过脆。
- arXiv 默认 daily fetch 只取最近一天论文，在本次执行时返回 0；补测放宽到昨天窗口后仍返回 0，说明主链路问题不只是测试时点造成。
- `test.py` 对 HuggingFace 返回 0 篇的情况仍判定为“通过”，说明它更像烟雾测试，而不是严格有效性测试。
- 专题搜索能力存在，但 query 相关性控制较弱，不足以胜任高质量综述资料搜集。

## 主观观察
- 这个 skill 更像“学术信息流 / 每日播报脚本”，不是综述导向检索器。
- 如果上游源工作正常，它的输出格式对聊天应用推送比较友好。
- 但当前实现对外部页面结构与时间窗口依赖很强，稳定性偏弱。
- 它在商业化上更适合观察为“研究热点订阅模块”，不适合作为文献综述主检索模块。
