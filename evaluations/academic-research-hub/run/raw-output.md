# 原始输出

## 执行命令 1：arXiv 检索

```bash
PYTHONIOENCODING=utf-8 uv run --python .venv python scripts/research.py arxiv "Agent Systems for LLMs" --max-results 5 --format json
```

## arXiv 结果摘要
- 返回数量：5
- 命令执行成功
- 返回内容中包含部分与主题相关的 survey / human-agent collaboration 论文，但也混入了偏离主题的 multi-agent / RL / HCI 内容

### arXiv 结果节选
- **LLM-Based Human-Agent Collaboration and Interaction Systems: A Survey**
  - arXiv ID: `2505.00753v4`
  - Published: 2025-05-01
  - Categories: `cs.CL`, `cs.LG`
  - 相关性：中高

- **A Survey of Multi-Agent Deep Reinforcement Learning with Communication**
  - arXiv ID: `2203.08975v2`
  - Published: 2022-03-16
  - 相关性：一般，偏 multi-agent / RL，不是典型 LLM agent 综述

## 执行命令 2：Semantic Scholar 检索

```bash
PYTHONIOENCODING=utf-8 uv run --python .venv python scripts/research.py semantic "Agent Systems for LLMs" --max-results 5 --format json
```

## Semantic Scholar 结果摘要
- 命令执行未成功
- 返回空数组 `[]`
- stderr 报错：
  `Error searching Semantic Scholar: RetryError[<Future ... raised ConnectionRefusedError>]`

## 执行命令 3：PubMed 检索

```bash
PYTHONIOENCODING=utf-8 uv run --python .venv python scripts/research.py pubmed "Agent Systems for LLMs" --max-results 5 --format json
```

## PubMed 结果摘要
- 返回数量：5
- 命令执行成功
- 检索结果大多是医学 / 临床 / 教育应用场景中的 multi-agent AI 论文，和通用 LLM agent systems 综述主题存在偏差

### PubMed 结果节选
- **Transforming oncology clinical trial matching through neuro-symbolic, multi-agent AI and an oncology-specific knowledge graph**
  - PMID: `42004487`
  - DOI: `10.1016/j.esmorw.2026.100706`
  - 相关性：一般，偏肿瘤临床试验应用

- **A Multi-AI Agent Framework for Interactive Neurosurgical Education and Evaluation**
  - PMID: `41982325`
  - DOI: `10.1227/neuprac.0000000000000217`
  - 相关性：一般，偏神经外科教育场景
