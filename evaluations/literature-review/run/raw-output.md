# 原始输出

## 执行命令 1：多源检索

```bash
PYTHONIOENCODING=utf-8 uv run --python .venv python scripts/lit_search.py search "Agent Systems for LLMs" --limit 5 --source all
```

## 结果摘要
- `total_before_dedupe`: 15
- `total_after_dedupe`: 15
- 返回来源覆盖：OpenAlex、Crossref、PubMed
- 本次 multi-source 调用成功返回结果，但未出现 Semantic Scholar 独立结果块；从最终输出看，至少多源聚合流程可运行

## 检索结果节选

### 1. OpenAlex
- **A survey on LLM-based multi-agent systems: workflow, infrastructure, and challenges**
  - DOI: `https://doi.org/10.1007/s44336-024-00009-2`
  - Year: 2024
  - CitationCount: 212
  - Authors: Xinyi Li, S. Wang, Siqi Zeng, Yu Wu, Yi Yang
  - Source: openalex

- **LLM-Based Multi-Agent Systems for Software Engineering: Literature Review, Vision, and the Road Ahead**
  - DOI: `https://doi.org/10.1145/3712003`
  - Year: 2025
  - CitationCount: 94
  - Authors: Junda He, Christoph Treude, David Lo
  - Source: openalex

### 2. Crossref
- **ALI-Agent: Assessing LLMs' Alignment with Human Values via Agent-based Evaluation**
  - DOI: `10.52202/079017-3142`
  - Year: 2024
  - Venue: Advances in Neural Information Processing Systems 37
  - Source: crossref

- **Program Code Generation: Single LLMs vs. Multi-Agent Systems**
  - DOI: `10.1109/icnlp65360.2025.11108400`
  - Year: 2025
  - Source: crossref

### 3. PubMed
- **Transforming oncology clinical trial matching through neuro-symbolic, multi-agent AI and an oncology-specific knowledge graph**
  - DOI: `10.1016/j.esmorw.2026.100706`
  - Year: 2026
  - Source: pubmed

- **A Multi-AI Agent Framework for Interactive Neurosurgical Education and Evaluation**
  - DOI: `10.1227/neuprac.0000000000000217`
  - Year: 2026
  - Source: pubmed

## 代表性命令 2：详情查询

```bash
PYTHONIOENCODING=utf-8 uv run --python .venv python scripts/lit_search.py details "DOI:10.1007/s44336-024-00009-2"
```

## details 结果摘要
- 标题：**A survey on LLM-based multi-agent systems: workflow, infrastructure, and challenges**
- DOI：`10.1007/s44336-024-00009-2`
- Year：2024
- Venue：Vicinagearth
- ReferenceCount：361
- CitationCount：408
- TL;DR：该论文系统综述了 LLM-based multi-agent systems，并总结出五个关键组成部分：profile、perception、self-action、mutual interaction、evolution。
- Open Access PDF：存在可访问 PDF 链接

## details 结果节选
```json
{
  "paperId": "fc8ce12d6186ddaa797e2b36d5e8eb7921425308",
  "externalIds": {
    "DOI": "10.1007/s44336-024-00009-2",
    "CorpusId": 273218743
  },
  "title": "A survey on LLM-based multi-agent systems: workflow, infrastructure, and challenges",
  "venue": "Vicinagearth",
  "year": 2024,
  "referenceCount": 361,
  "citationCount": 408,
  "tldr": {
    "model": "tldr@v2.0.0",
    "text": "A comprehensive survey of LLM-based multi-agent systems is presented, offering a systematic review of these studies, and a general structure encompassing five key components: profile, perception, self-action, mutual interaction, and evolution is synthesized."
  }
}
```
