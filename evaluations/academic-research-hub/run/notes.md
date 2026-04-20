# 运行记录

## 基本记录
- 执行时间：2026-04-21
- 使用入口：`scripts/research.py`
- 是否严格只使用该 skill：是

## 环境准备
执行命令：
```bash
uv venv .venv
uv pip install --python .venv -r scripts/requirements.txt
```

安装成功，主要依赖包括：
- arxiv
- semanticscholar
- biopython
- requests
- beautifulsoup4
- lxml

## 实际执行命令
1. arXiv：
```bash
PYTHONIOENCODING=utf-8 uv run --python .venv python scripts/research.py arxiv "Agent Systems for LLMs" --max-results 5 --format json
```
结果：成功

2. Semantic Scholar：
```bash
PYTHONIOENCODING=utf-8 uv run --python .venv python scripts/research.py semantic "Agent Systems for LLMs" --max-results 5 --format json
```
结果：失败，报 `ConnectionRefusedError` / `RetryError`

3. PubMed：
```bash
PYTHONIOENCODING=utf-8 uv run --python .venv python scripts/research.py pubmed "Agent Systems for LLMs" --max-results 5 --format json
```
结果：成功

## 过程记录
- 该 skill 明确采用“按 source 分别调用”的方式，而不是像 `literature-review` 那样内置 multi-source 聚合。
- `requirements.txt` 存在，因此这次依赖安装路径比上一个 skill 更清晰。
- arXiv 与 PubMed 主链路都能跑通。
- Semantic Scholar 在本轮测试中无法正常返回结果，是本次测试中的主要失败点。

## 问题与异常
- `research.py` 的 arXiv 逻辑出现 `DeprecationWarning`：`Search.results` 方法已被弃用
- Semantic Scholar 调用失败，返回 `RetryError` 和 `ConnectionRefusedError`
- PubMed 返回结果与“LLM Agent Systems”主题的相关性偏弱，容易检索到医学应用场景论文
- 该 skill 没有自动聚合多个来源结果，也没有自动去重或统一整合能力

## 主观观察
- 这个 skill 更像一个“多数据源检索工具箱”，不是一个面向文献综述整理的成品工作流。
- 对于需要分别访问 arXiv / PubMed / Semantic Scholar 的用户来说，命令接口比较明确。
- 但如果目标是快速围绕一个主题拿到高质量、聚合后的综述资料，它还需要大量人工筛选和后处理。
- 在本轮主题下，arXiv 的结果相对更有价值，PubMed 更像噪声源，Semantic Scholar 又未成功运行，因此整体表现一般。
