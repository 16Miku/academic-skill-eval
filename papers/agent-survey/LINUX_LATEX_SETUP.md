# Linux 云端 LaTeX 环境最小可行安装与验证方案

> 本文档用于验证：在 Linux 云端服务器上，Claude Code 是否能够稳定完成 arXiv 规格论文的 LaTeX 编写、编译、排错与 PDF 产出闭环。
>
> 目标不是一次性搭建最完整的科研写作平台，而是先搭建一个**最小可行环境**：能编译标准英文论文、能处理 BibTeX 引用、能渲染 TikZ 图表、能用 `latexmk` 自动完成多轮编译。

---

## 一、适用场景与验证目标

### 1.1 适用场景

本文档适用于以下场景：

- 在 Linux 云服务器上运行 Claude Code
- 希望让 Claude Code 直接编辑 `.tex` / `.bib` 文件
- 希望云端自动编译生成 PDF，而不是依赖 Windows 本地 MiKTeX
- 希望验证 arXiv 论文写作的最小工程闭环
- 希望后续将论文写作、编译、排错流程服务化或脚本化

### 1.2 最小可行目标

完成本文档后，服务器应具备以下能力：

| 能力 | 验证方式 |
|------|----------|
| LaTeX 基础编译 | `pdflatex main.tex` 成功生成 PDF |
| BibTeX 引用 | `bibtex main` 能处理 `.bib` 文献库 |
| TikZ 图表 | PDF 中能正常显示 TikZ 绘制的示意图 |
| 自动多轮编译 | `latexmk -pdf main.tex` 一条命令完成编译 |
| 论文级常用宏包 | `natbib`、`booktabs`、`hyperref`、`cleveref` 等可用 |
| Claude Code 闭环排错 | 编译失败后可读取 `.log` 并修复 `.tex` |

### 1.3 推荐工具链

Linux 云端推荐使用：

```text
TeX Live        # Linux 上最常见、最稳定的 LaTeX 发行版
latexmk         # 自动多轮编译工具
BibTeX          # 参考文献处理工具
TikZ / PGF      # LaTeX 内嵌图形绘制
Claude Code     # 编写、修改、编译、排错闭环
```

与 Windows 实践中的 MiKTeX 相比，Linux 上推荐 TeX Live，原因是：

- 更适合命令行和服务器环境
- 更适合无人值守编译
- 更适合 CI/CD 或自动化脚本
- 避免 GUI 弹窗式宏包安装流程

---

## 二、系统环境前置检查

### 2.1 确认系统版本

在服务器上执行：

```bash
uname -a
cat /etc/os-release
```

推荐优先验证以下系统：

- Ubuntu 22.04 / 24.04
- Debian 12
- 其他 Debian 系发行版

如果使用 CentOS、Rocky Linux、AlmaLinux，也可以安装 TeX Live，但包名和版本可能不同，建议优先用官方 TeX Live installer 或容器化方式。

### 2.2 确认基础命令可用

```bash
which bash
which curl
which git
which make
```

如果缺少基础工具，可先安装：

```bash
sudo apt update
sudo apt install -y curl git make ca-certificates
```

> 如果服务器没有 `sudo` 权限，需要让管理员预装 TeX Live，或改用用户目录安装 TinyTeX / TeX Live 官方 installer。

---

## 三、安装方案选择

### 3.1 推荐方案：apt 安装 TeX Live 子集

这是最适合最小验证的方案。优点是简单、稳定、可重复。

适合：

- Ubuntu / Debian 云服务器
- 只需要验证英文 arXiv 论文编译
- 不希望安装完整 TeX Live 的全部包

### 3.2 完整方案：安装 `texlive-full`

优点是宏包最全，后续最少遇到缺包问题。缺点是体积很大，安装时间长。

适合：

- 磁盘空间充足
- 希望减少后续排错
- 需要支持大量复杂模板

### 3.3 轻量方案：TinyTeX

优点是轻量，适合用户目录安装。缺点是遇到缺包时需要额外管理。

适合：

- 没有 sudo 权限
- 云服务器空间有限
- 愿意接受后续按需补包

本文档优先采用 **方案 3.1：apt 安装 TeX Live 子集** 作为最小可行路径。

---

## 四、Ubuntu / Debian 最小安装步骤

### 4.1 更新软件源

```bash
sudo apt update
```

### 4.2 安装最小可行 LaTeX 工具链

推荐先安装以下组合：

```bash
sudo apt install -y \
  texlive-latex-base \
  texlive-latex-recommended \
  texlive-latex-extra \
  texlive-fonts-recommended \
  texlive-fonts-extra \
  texlive-pictures \
  texlive-bibtex-extra \
  latexmk \
  biber
```

说明：

| 包 | 用途 |
|----|------|
| `texlive-latex-base` | LaTeX 基础能力 |
| `texlive-latex-recommended` | 常用推荐宏包 |
| `texlive-latex-extra` | `cleveref`、`enumitem`、`caption` 等扩展宏包 |
| `texlive-fonts-recommended` | 常见论文模板所需字体 |
| `texlive-fonts-extra` | `newtxtext`、`newtxmath` 等 Times 风格字体包 |
| `texlive-pictures` | TikZ / PGF 绘图能力 |
| `texlive-bibtex-extra` | BibTeX 样式与引用扩展 |
| `latexmk` | 自动多轮编译 |
| `biber` | BibLaTeX 备用后端，当前最小验证以 BibTeX 为主 |

> 如果磁盘空间充足，也可以直接使用：
>
> ```bash
> sudo apt install -y texlive-full latexmk biber
> ```
>
> 但 `texlive-full` 体积很大，不建议作为最小验证的默认方案。

### 4.3 验证命令是否安装成功

```bash
pdflatex --version
bibtex --version
latexmk --version
kpsewhich article.cls
kpsewhich tikz.sty
kpsewhich newtxtext.sty
kpsewhich cleveref.sty
```

期望结果：

- `pdflatex`、`bibtex`、`latexmk` 都能输出版本信息
- `kpsewhich` 能返回对应 `.cls` / `.sty` 文件路径
- 如果某个 `.sty` 没有路径输出，说明对应宏包缺失

---

## 五、创建最小验证项目

### 5.1 创建目录

```bash
mkdir -p ~/latex-cloud-test
cd ~/latex-cloud-test
```

建议验证目录结构如下：

```text
latex-cloud-test/
├── main.tex
├── references.bib
└── output/
```

创建输出目录：

```bash
mkdir -p output
```

### 5.2 创建最小 `main.tex`

新建 `main.tex`：

```latex
\pdfoutput=1
\documentclass[11pt,a4paper]{article}

\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{newtxtext}
\usepackage{newtxmath}
\usepackage[a4paper,margin=1in]{geometry}
\usepackage{microtype}
\usepackage{mathtools}
\usepackage{amssymb}
\let\Bbbk\relax

\usepackage{graphicx}
\usepackage[font=small,labelfont=bf,format=hang]{caption}
\usepackage{subcaption}
\usepackage{booktabs}
\usepackage{array}
\usepackage{multirow}
\usepackage{tabularx}
\usepackage{enumitem}
\usepackage{xcolor}

\usepackage{tikz}
\usetikzlibrary{shapes.geometric, arrows.meta, positioning, fit, calc}

\usepackage[numbers,sort&compress]{natbib}
\usepackage{hyperref}
\usepackage[nameinlink,noabbrev]{cleveref}

\hypersetup{
  colorlinks=true,
  linkcolor=blue,
  citecolor=blue,
  urlcolor=blue
}

\title{Minimal Linux Cloud LaTeX Verification for arXiv-Style Papers}
\author{Claude Code Environment Test}
\date{\today}

\begin{document}
\maketitle

\begin{abstract}
This document verifies that a Linux cloud environment can compile an arXiv-style LaTeX paper with citations, tables, cross references, hyperlinks, and TikZ figures.
\end{abstract}

\section{Introduction}

This is a minimal verification document for cloud-based LaTeX compilation. It tests references such as \citet{vaswani2017attention}, cross references such as \cref{fig:agent-loop}, and tables such as \cref{tab:toolchain}.

\section{A Minimal Agent Loop Figure}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[
  node distance=2.2cm,
  box/.style={rectangle, rounded corners, draw=black, thick, align=center, minimum width=2.4cm, minimum height=0.9cm},
  arrow/.style={-{Latex[length=3mm]}, thick}
]
\node[box] (observe) {Observe};
\node[box, right=of observe] (reason) {Reason};
\node[box, right=of reason] (act) {Act};
\node[box, below=of reason] (memory) {Memory};

\draw[arrow] (observe) -- (reason);
\draw[arrow] (reason) -- (act);
\draw[arrow] (reason) -- (memory);
\draw[arrow] (memory) -- (reason);
\draw[arrow] (act.south) |- (memory.east);
\end{tikzpicture}
\caption{A minimal observe-reason-act loop with memory.}
\label{fig:agent-loop}
\end{figure}

\section{Toolchain Table}

\begin{table}[htbp]
\centering
\caption{Minimal Linux LaTeX toolchain.}
\label{tab:toolchain}
\begin{tabularx}{\linewidth}{lllX}
\toprule
Tool & Required & Command & Purpose \\
\midrule
TeX Live & Yes & \texttt{pdflatex} & Compile LaTeX source to PDF \\
BibTeX & Yes & \texttt{bibtex} & Process bibliography entries \\
latexmk & Yes & \texttt{latexmk -pdf} & Automate multi-pass compilation \\
TikZ & Yes & \texttt{tikz.sty} & Draw vector figures inside LaTeX \\
\bottomrule
\end{tabularx}
\end{table}

\section{Conclusion}

If this document compiles successfully, the Linux cloud environment supports the minimal workflow needed for Claude Code to write and compile arXiv-style LaTeX papers.

\bibliographystyle{plainnat}
\bibliography{references}

\end{document}
```

### 5.3 创建最小 `references.bib`

新建 `references.bib`：

```bibtex
@inproceedings{vaswani2017attention,
  title={Attention Is All You Need},
  author={Vaswani, Ashish and Shazeer, Noam and Parmar, Niki and Uszkoreit, Jakob and Jones, Llion and Gomez, Aidan N. and Kaiser, Lukasz and Polosukhin, Illia},
  booktitle={Advances in Neural Information Processing Systems},
  volume={30},
  year={2017}
}
```

这个引用用于验证：

- `.bib` 文件可被读取
- `natbib` 可用
- `\citet{}` 命令可用
- 参考文献列表能正常生成

---

## 六、编译验证流程

### 6.1 手动多轮编译验证

先用最基础的方式验证：

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

验证结果：

```bash
ls -lh main.pdf
```

如果 `main.pdf` 存在，说明基础链路可用。

### 6.2 使用 `latexmk` 一键编译

清理中间文件后重新验证：

```bash
latexmk -C
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```

验证结果：

```bash
ls -lh main.pdf
```

推荐后续让 Claude Code 默认使用 `latexmk`，而不是手动执行多次 `pdflatex` / `bibtex`。

### 6.3 输出到 `output/` 目录

为了保持工作目录整洁，可以将中间文件和 PDF 输出到 `output/`：

```bash
latexmk -pdf \
  -interaction=nonstopmode \
  -halt-on-error \
  -outdir=output \
  main.tex
```

验证：

```bash
ls -lh output/main.pdf
```

> 注意：如果使用 `-outdir=output`，BibTeX 和交叉引用文件也会输出到该目录。对于复杂项目，需要确保图片路径和引用路径仍然正确。

---

## 七、成功标准

完成最小验证后，应满足以下标准：

| 检查项 | 成功标准 |
|--------|----------|
| PDF 生成 | `main.pdf` 或 `output/main.pdf` 存在 |
| 引用正常 | PDF 中出现 Vaswani et al. 引用和参考文献列表 |
| 图表正常 | PDF 中显示 TikZ agent loop 图 |
| 表格正常 | PDF 中显示 `booktabs` 风格表格 |
| 超链接正常 | 交叉引用和 citation 链接可点击 |
| 编译无致命错误 | `latexmk` 返回 exit code 0 |
| 无 undefined citation | 日志中不再出现 `Citation ... undefined` |
| 无 undefined reference | 日志中不再出现 `Reference ... undefined` |

可用以下命令快速检查日志：

```bash
grep -E "Undefined|undefined|Fatal|Emergency|Error" main.log || true
```

如果使用 `output/`：

```bash
grep -E "Undefined|undefined|Fatal|Emergency|Error" output/main.log || true
```

---

## 八、常见问题与解决方式

### 8.1 缺少 `.sty` 宏包

典型报错：

```text
LaTeX Error: File `xxx.sty' not found.
```

处理方式：

```bash
kpsewhich xxx.sty
```

如果没有输出，说明宏包未安装。优先补充：

```bash
sudo apt install -y texlive-latex-extra texlive-fonts-extra texlive-pictures
```

如果仍然缺失，可考虑安装完整版本：

```bash
sudo apt install -y texlive-full
```

### 8.2 `newtxtext.sty` 或 `newtxmath.sty` 缺失

处理方式：

```bash
sudo apt install -y texlive-fonts-extra
```

验证：

```bash
kpsewhich newtxtext.sty
kpsewhich newtxmath.sty
```

### 8.3 TikZ 不可用

典型报错：

```text
LaTeX Error: File `tikz.sty' not found.
```

处理方式：

```bash
sudo apt install -y texlive-pictures
```

验证：

```bash
kpsewhich tikz.sty
```

### 8.4 `\Bbbk` already defined 冲突

如果同时使用 `newtxmath` 和 `amssymb`，可能出现命令冲突。

处理方式是在 `\usepackage{amssymb}` 后加入：

```latex
\let\Bbbk\relax
```

或者调整宏包加载顺序。为了和当前 Windows 实战保持一致，推荐保留该行。

### 8.5 BibTeX 引用未生成

典型现象：

- PDF 中出现 `[?]`
- 日志中出现 `Citation ... undefined`
- 参考文献列表为空

处理方式：

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

或者直接使用：

```bash
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```

如果使用 `-outdir=output`，建议始终用 `latexmk` 管理，不要手动混用不同输出目录。

### 8.6 中文路径或空格路径导致问题

Linux 服务器上建议论文项目路径使用英文和短路径，例如：

```text
~/papers/agent-survey/
~/latex-cloud-test/
```

不建议使用：

```text
~/我的论文/Agent Survey Final Version/
```

原因是：

- 部分 LaTeX 工具对空格和非 ASCII 路径处理不稳定
- 自动化脚本更容易出错
- 日志排查更麻烦

### 8.7 没有 sudo 权限

如果没有 sudo 权限，有三种选择：

1. 让管理员安装 TeX Live
2. 使用 TinyTeX 安装到用户目录
3. 使用 Docker / devcontainer 固化环境

TinyTeX 适合最小验证，但后续遇到缺包时需要额外安装包，不如 TeX Live 子集稳定。

---

## 九、Claude Code 云端闭环工作流

Linux 云端环境搭好后，推荐让 Claude Code 按以下闭环工作：

```text
1. 修改 main.tex / references.bib
2. 执行 latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
3. 如果编译失败，读取 main.log
4. 根据错误修复 LaTeX 源码或 BibTeX
5. 再次编译
6. 编译通过后检查 PDF、引用、图表、页数
```

### 9.1 推荐给 Claude Code 的编译命令

普通项目：

```bash
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```

输出到 `output/`：

```bash
latexmk -pdf -interaction=nonstopmode -halt-on-error -outdir=output main.tex
```

清理中间文件：

```bash
latexmk -C
```

### 9.2 推荐的项目结构

```text
paper-project/
├── main.tex
├── references.bib
├── sections/
│   ├── introduction.tex
│   ├── background.tex
│   └── conclusion.tex
├── figures/
│   └── architecture.pdf
├── output/
└── README.md
```

对于早期验证，可以只使用单个 `main.tex`。当正文变长后，再拆分到 `sections/`。

### 9.3 推荐的 `.gitignore`

论文项目建议忽略 LaTeX 中间文件：

```gitignore
*.aux
*.bbl
*.blg
*.log
*.out
*.fls
*.fdb_latexmk
*.synctex.gz
*.toc
*.lof
*.lot
```

是否提交 `main.pdf` 取决于协作方式：

- 如果仓库用于成果归档，可以提交最终 PDF
- 如果仓库用于源码协作，可以只提交 `.tex` / `.bib`，PDF 由 CI 或本地生成

---

## 十、从 Windows/MiKTeX 实践迁移到 Linux/TeX Live

当前项目已有 Windows + MiKTeX 实践：

```text
papers/agent-survey/PRACTICE_GUIDE.md
```

迁移到 Linux 时，主要差异如下：

| 项目 | Windows 实践 | Linux 云端建议 |
|------|--------------|----------------|
| LaTeX 发行版 | MiKTeX | TeX Live |
| 缺包处理 | MiKTeX AutoInstall | apt 安装 TeX Live 包组 |
| 编译方式 | `pdflatex` + `bibtex` | 优先 `latexmk -pdf` |
| 图形界面 | 可能有安装弹窗 | 无 GUI，适合命令行自动化 |
| 路径问题 | Windows 路径长度与反斜杠 | 推荐短英文路径 |
| 云端协作 | 不适合直接复用本地 GUI | 适合脚本化、CI、服务化 |

### 10.1 可以直接复用的内容

可从 `PRACTICE_GUIDE.md` 复用：

- 论文目录结构
- LaTeX 宏包清单
- arXiv 模板定制要点
- BibTeX 构建方式
- 分章节写作策略
- TikZ 图表写法
- 编译排错思路

### 10.2 需要替换的内容

需要替换为 Linux 版本：

- MiKTeX 安装步骤 → TeX Live 安装步骤
- `initexmf` 自动安装配置 → `apt install` 包组安装
- Windows 路径 → Linux 路径
- 手动多轮编译 → 优先 `latexmk`

---

## 十一、最小验收清单

在 Linux 服务器上完成以下检查，即可认为最小环境可用：

```bash
pdflatex --version
bibtex --version
latexmk --version
kpsewhich tikz.sty
kpsewhich newtxtext.sty
kpsewhich cleveref.sty
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
ls -lh main.pdf
```

验收标准：

- 所有版本命令可执行
- 关键 `.sty` 文件可定位
- `latexmk` 编译成功
- PDF 中包含标题、摘要、引用、图、表、参考文献
- 日志无致命错误、无未定义引用

---

## 十二、后续增强方向

最小环境验证通过后，可以继续增强：

1. **增加 arXiv 模板验证**：使用真实 arXiv 风格论文模板编译完整稿件
2. **增加 CI 验证**：每次提交自动运行 `latexmk`
3. **增加 PDF 预览链路**：结合 VS Code Remote、WebDAV、对象存储或简单静态服务查看 PDF
4. **增加文献工具链**：加入 DOI 查询、BibTeX 去重、引用一致性检查
5. **增加质量检查**：加入拼写检查、断链检查、重复引用检查、overfull box 检查
6. **增加 Docker 镜像**：固化 TeX Live + latexmk + Claude Code 所需依赖，减少服务器差异

---

## 十三、结论

Linux 云端完全支持 Claude Code 论文写作所需的 LaTeX 编译环境。最小可行方案是：

```text
Ubuntu / Debian + TeX Live 子集 + latexmk + BibTeX + TikZ
```

只要最小验证文档能够通过 `latexmk -pdf` 编译，并正确生成包含引用、图表和参考文献的 PDF，就说明该云端环境已经具备支撑 arXiv 规格论文写作闭环的基础能力。

后续真正影响论文质量的关键不再是编译环境，而是：

- 文献覆盖是否完整
- 综述框架是否有原创性
- 论证是否有批判性分析
- 图表是否具备足够信息密度
- 人工审校是否足够严格



