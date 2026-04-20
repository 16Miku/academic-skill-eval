# 运行记录

## 基本记录
- 执行时间：2026-04-21
- 使用入口：`scripts/lit_search.py`
- 是否严格只使用该 skill：是

## 实际执行命令
1. 初次尝试：
   ```bash
   uv run python scripts/lit_search.py search "Agent Systems for LLMs" --limit 5 --source all
   ```
   结果：失败，缺少 `requests`

2. 环境准备：
   ```bash
   uv venv .venv
   uv pip install --python .venv requests
   ```

3. 第二次尝试：
   ```bash
   uv run --python .venv python scripts/lit_search.py search "Agent Systems for LLMs" --limit 5 --source all
   ```
   结果：失败，终端输出触发 `UnicodeEncodeError`（gbk 编码无法输出部分字符）

4. 修正编码后重试：
   ```bash
   PYTHONIOENCODING=utf-8 uv run --python .venv python scripts/lit_search.py search "Agent Systems for LLMs" --limit 5 --source all
   ```
   结果：成功

5. 详情查询：
   ```bash
   PYTHONIOENCODING=utf-8 uv run --python .venv python scripts/lit_search.py details "DOI:10.1007/s44336-024-00009-2"
   ```
   结果：成功

## 过程记录
- 该 skill 提供的是独立 Python 脚本入口，而不是自动注入依赖的完整项目环境。
- 当前目录没有 `pyproject.toml`，因此不能直接用 `uv add`。
- 最终采用本地 `.venv` + `uv pip install --python .venv requests` 的方式满足依赖。
- Windows 控制台默认 gbk 编码导致 JSON 输出中个别特殊字符无法打印，需要显式设置 `PYTHONIOENCODING=utf-8`。

## 问题与异常
- 缺少基础依赖 `requests`
- 默认终端编码导致 `UnicodeEncodeError`
- multi-source 输出中未看到 Semantic Scholar 单独命中的结果，需谨慎看待“all”来源的完整性

## 主观观察
- skill 的命令行功能是可以运行起来的，但对环境有一定要求，不是开箱即用。
- OpenAlex 返回的结果与主题高度相关，质量较高。
- Crossref 与 PubMed 混入了一些边缘相关或偏应用场景结果，需要人工筛选。
- details 能拿到更完整的元数据、TL;DR 和 PDF 链接，对后续综述整理有帮助。
