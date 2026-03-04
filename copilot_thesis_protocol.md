# Copilot 写作协议（NJU njuthesis｜硕士论文）

> 这份文档是给 Copilot / 代码助手“反复附加”的**写作协议**。它的目标不是一次性把论文写完，而是让每次增量生成都**可控、可审、可持续迭代**。

---

## 0. 一句话定位（North Star）

本硕士论文聚焦于：**面向大规模硬件设计的高精度静态时序分析（STA，含 CPPR），提出并实现一种以 *pre-slack* 引导搜索的 *CPPR-after-search* 方法，在保证 soundness / 可控精度前提下显著加速关键路径分析。**

> 所有章节、段落、图表、实验组织都应围绕这句话展开；若出现偏离，应在 `thinking/` 中记录“偏离原因与修正计划”。

---

## 1. 资料优先级与冲突裁决

可用资料位于仓库内：

- `materials/paper.pdf`：研究生阶段主要工作（**最高优先级**）
- `materials/bachelor-thesis.docx`：本科论文（可复用表述/图表/实现细节，但**不得覆盖 paper 的结论/定义**）
- `materials/paper-references.bib`：paper 的参考文献库（**只作为素材库**）
- `examples/ZJC.pdf`：同校论文格式/章数尺度参考（只参考“框架与规模”，**不参考内容**）
- `njuthesis-userguide.pdf`：njuthesis 模板与编译规范（遵循其说明）

**冲突裁决规则：**
1. 与 `paper.pdf` 冲突 → 以 `paper.pdf` 为准。
2. 与 njuthesis 规范冲突 → 以 njuthesis 为准（例如编译引擎、格式、封面字段等）。
3. 与 ZJC 结构冲突 → 以“本课题叙事完整性”为准，ZJC 仅作为“尺度”参考。

---

## 2. 工程化输出方式（强制）

### 2.1 章节拆文件：`main.tex` 只负责编排（强制）
- **禁止**把正文都堆在 `main.tex`。
- 推荐结构（如目录不存在则创建）：

```
chapters/
  chap01_intro.tex
  chap02_background_related.tex
  chap03_modeling_observations.tex
  chap04_method_flashtimer.tex
  chap05_system_implementation.tex
  chap06_experiments_conclusion.tex
refs/
  refs.bib
thinking/
images/
```

`main.tex` 只做：
- 模板/宏包配置
- 元数据（作者、学号、导师等）
- `\include{chapters/...}` 或 `\input{...}` 的编排
- 参考文献与附录入口

### 2.2 增量改动要“可 review”（强制）
每次 Copilot 输出应遵循：
- 只改指定范围（例如“只改标题+摘要+第1章”）
- 追加内容时尽量**不重排**已完成章节（避免大 diff）
- 新增文件必须写明路径与目的
- 若生成新内容需要素材（图/引用/数据），必须使用占位符机制（见第 4 节）

---

## 3. 推荐论文结构（对齐 NJU 习惯，章数≈6）

> 结构可在后续迭代中微调，但应保持“背景→建模→方法→实现→实验”的叙事闭环。

1. **绪论**  
   背景与意义、问题定义、现有方法瓶颈、论文贡献、组织结构。
2. **背景与相关工作**  
   STA/CPPR 基础；方法谱系：Before Search / During Search / After Search 等分类；对比讨论。
3. **问题建模与关键观察**  
   pre-slack / post-slack / pessimism 定义；为什么分布/相关性可用；关键符号与假设。
4. **FlashTimer 方法**  
   CPPR-after-search 两阶段框架；终止条件；正确性（soundness）与复杂度要点；可调精度-性能权衡。
5. **系统实现与工程优化**  
   关键数据结构、并行/缓存、预处理；RMQ/LCA 等工程化细节；可复现性说明。
6. **实验评估与总结展望**  
   数据集（Tau contest 等）、对比基线、速度/精度、可扩展性；威胁与局限；未来工作。

---

## 4. 图片与引用的占位符机制（强制，禁止“编造”）

### 4.1 图片规则
- 优先使用 **PDF**（矢量、打印友好）；次选 PNG。
- 尽可能复用现有图片（来自 `materials/`、本科论文、paper 附图），统一放到 `images/`。
- 若需要新增图片但当前没有素材：
  1) 复制根目录的 `dummy-image.png`  
  2) 重命名到预期路径，例如：`images/flowchart_for_buzz.png`  
  3) 在对应章节附近写 **TODO 说明**：图片要表达什么、关键元素、建议来源/生成方式。

示例（正文中）：
```tex
\begin{figure}[htbp]
  \centering
  \includegraphics[width=0.9\linewidth]{images/flowchart_for_buzz.png}
  \caption{TODO：这里需要一张展示 FlashTimer 两阶段流程（pre-slack 引导 + CPPR-after-search 计算）的流程图。}
  \label{fig:flashtimer_flow}
\end{figure}
```

### 4.2 引用规则（最重要）
- **禁止**凭空生成论文条目、作者、年份、期刊。
- `materials/paper-references.bib` 仅作素材库：需要引用时，把必要条目**拷贝**到 `refs/refs.bib`。
- 若需要新增引用但暂时没找到条目：添加一个 `@misc{TODO_...}` 占位条目，并在 `note` 字段写清楚“我需要引用什么”。之后我会去 Google Scholar 补齐。

示例（占位 bib）：
```bibtex
@misc{TODO_cppr_survey,
  title  = {TODO: CPPR / common path pessimism removal 经典综述或代表性工作},
  note   = {关键词：CPPR, common path pessimism removal, static timing analysis; 需要一篇高引用综述或早期奠基论文。},
  year   = {TODO},
}
```

---

## 5. `thinking/` 目录：中间过程的“产物规范”（强制）

`thinking/` 不做杂物堆，只允许两类文件：

### 5.1 Decision Log（决策记录）
- 文件名：`thinking/decision_log.md`
- 用于记录：结构调整原因、术语选择、删改原因、与 paper 冲突的处理等。

### 5.2 Content Map（内容映射）
- 文件名：`thinking/content_map.md`
- 目标：建立从 `paper.pdf` → thesis 章节的映射，以及本科论文可复用段落/图表索引。
- 每次新增章节时同步更新映射。

建议模板（可直接复制到文件）：
```md
## Mapping: paper -> thesis

- paper §X.Y “...” -> thesis chap04 §4.2 “...” (状态：草稿/完成)
- bachelor §A.B “...” -> thesis chap02 §2.3 “...” (可复用：定义/背景/实现细节)

## Open TODOs
- TODO: 需要补一张“pre-slack 分布示意图” (fig:pre_slack_dist)
- TODO: 需要补引用：CPPR survey (bib:TODO_cppr_survey)
```

---

## 6. 编译与工具链约定（强制）

- njuthesis **不支持 pdftex**；默认使用 **XeLaTeX 或 LuaLaTeX**。
- 建议采用 `latexmk` 编译，**必须**遵循工作区 `.vscode/settings.json` 的参数。尤其是需要设置输出目录 `-outdir=./out`，**严禁将编译中间文件散落到根目录**。

手动执行参考命令：
```bash
latexmk -synctex=1 -interaction=nonstopmode -file-line-error -xelatex -outdir=./out main.tex
```

如果需要清理编译文件：
```bash
latexmk -c -outdir=./out main.tex
```

---

## 7. 质量闸门（每次生成必须满足）

每次 Copilot 生成后，必须自检并满足：

1. **内容体量**：生成的各章节应具备硕士论文应有的字数与深度，不得过于单薄（可参照 `examples/ZJC.pdf` 的篇幅尺度进行扩充）。
2. **不引入虚构引用**：所有 bib 要么来自拷贝，要么 `TODO_` 占位。
3. **不引入未定义缩写**：首次出现 STA/CPPR/LCA/RMQ 等必须定义。
4. **不擅自改动已完成章节结构**：除非明确要求，并在 `thinking/decision_log.md` 写原因。
5. **图表/公式可追溯**：新增图必须来自 `images/`；新增公式需在文中解释符号。
6. **与 paper 一致**：关键定义、结论、数值结果优先从 `paper.pdf` 取；若不确定，用 TODO 标记而不是编造。

---

## 8. 推荐的迭代交互模式（给 Copilot 的短 Prompt 模板）

你每次只需给 Copilot 一小段短 prompt，并附加本协议。例如：

- 生成标题/摘要/第 1 章：
  > “请遵循 `copilot_thesis_protocol.md`。基于 `materials/paper.pdf` 拟定中文/英文题名、中文摘要、英文摘要，并生成 `chapters/chap01_intro.tex` 的初稿。不要编造引用；需要引用请用 TODO bib 占位。不要改动其它章节。”

- 生成第 4 章方法：
  > “请遵循协议。根据 `paper.pdf` 的方法部分，生成 `chapters/chap04_method_flashtimer.tex`。必须包含两阶段框架、终止条件、正确性/复杂度要点；需要流程图则按占位符机制创建 dummy image。”

- 生成 refs：
  > “请遵循协议。从 `materials/paper-references.bib` 中挑选本章真正用到的条目，拷贝到 `refs/refs.bib`，并确保 `main.tex` 指向 `refs/refs.bib`。”

---

## 9. 命名与风格约定（建议）

- 图文件：`images/<topic>_<purpose>.pdf/png`（例如 `pre_slack_distribution.pdf`）
- 标签：`fig:...` / `tab:...` / `eq:...` 统一风格
- 章节内小节：优先“问题→方法→讨论→小结”的顺序
- 数学符号：尽量与 paper 一致；不一致必须说明并记录到 decision log

---

## 10. 本协议的更新规则

- 若要修改论文结构/命名规则/占位符规范，应先在 `thinking/decision_log.md` 记录原因，再更新本文件。
- 任何变更都应以“降低后续迭代成本”为导向。
