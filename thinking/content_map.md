## Mapping: paper -> thesis

- bachelor abstracts -> thesis abstract (状态：完成初步迭代)
- bachelor §1.x "绪论" -> thesis chap01 "绪论" (状态：完成初步迭代，明确了以 pre-slack 引导的 CPPR-after-search 北极星目标)
- bachelor / paper related work -> thesis chap02 "背景与相关工作" (状态：完成初步迭代，展开了 OCV 模型和 CPPR 各大流派对比)
- paper intro/formulation -> thesis chap03 "问题建模与关键观察" (状态：完成初步迭代，实现了三大核心指标的形式化以及基于散点分布的强相关性逻辑导出)
- paper method -> thesis chap04 "FlashTimer 方法" (状态：完成初步迭代，非常充分地论证了两阶段架构、严格单调序列约束下的终止判定定理证明以及性能平衡机制)

## 后续章节生成计划 (Chapter Generation Plan)
根据 `copilot_thesis_protocol.md` 和已生成的绪论，后续共有 5 章，计划如下：

### 第2章：背景与相关工作
- **目标文件**：`chapters/chap02_background_related.tex`
- **主要内容**：
  - STA与CPPR基础概念、快速-慢速时序模型（OCV）。
  - 方法谱系分类：详细探讨 CPPR-before-search、CPPR-during-search、CPPR-after-search 的演进机制与代表性工作。
  - 对比分析现有方法在大规模硬件分析中的扩展性瓶颈。

### 第3章：问题建模与关键观察
- **目标文件**：`chapters/chap03_modeling_observations.tex`
- **主要内容**：
  - 问题形式化定义与符号说明（图模型、到达时间、要求时间等）。
  - 核心定义提取：`pre-slack`（不带CPPR的裕度）、`post-slack`（真实裕度）、`pessimism`（悲观值）。
  - 关键观察（来自 paper）：揭示 pre-slack 与 post-slack 之间的强相关性及分布特征，论证“用 pre-slack 引导搜索”的合理性。

### 第4章：FlashTimer 方法
- **目标文件**：`chapters/chap04_method_flashtimer.tex`
- **主要内容**：
  - 整体算法框架（CPPR-after-search 两阶段架构）。
  - 阶段一：基于预搜索的候选路径生成（前置引导）。
  - 阶段二：精确评估与极速悲观消除、终止条件设计。
  - 理论分析：正确性（soundness）边界论证、时间复杂度分析，以及可调的精度-性能权衡机制。

### 第5章：系统实现与工程优化
- **目标文件**：`chapters/chap05_system_implementation.tex`
- **主要内容**：
  - 底层数据结构设计、图预处理。
  - 核心加速技术：基于树的 LCA (最近公共祖先) 与 RMQ (区间最值查询) 工程化实现，用于极速识别公共路径节点。
  - 高性能工程优化：缓存命中优化、高并发多线程调度机制。

### 第6章：实验评估与总结展望
- **目标文件**：`chapters/chap06_experiments_conclusion.tex`
- **主要内容**：
  - 实验环境、对比基线算法及数据集介绍（如 IEEE Tau contest 真实脱敏电路）。
  - 核心评估指标：计算速度提速比、精度误差分析（trade-off）、扩展性评估（应对千万级边数据的表现）。
  - 威胁与局限性探讨，以及未来工作的展望（结论与结语）。

## Open TODOs
- TODO: `refs/refs.bib` 的提取与交叉引用（目前第一章和第二章使用了多个 `\cite{TODO_...}` 占位符如 `TODO_uitimer`, `TODO_noveltimer`, `TODO_ocv_model` 等，需后续补充）。
- TODO: `images/cppr_clock_tree_example.png` 当前使用了占位空文件，需从 paper 或本科论文中提取一张展示时钟树早晚路径导致悲观现象的示意图覆盖它。
- TODO: `images/slack_correlation_scatter.png` 需要替换为真正的散点图分布测试结果。
- TODO: `images/flashtimer_flowchart.png` 当前使用了占位空文件，需补充具体的阶段引导流图。
- TODO: 撰写第五章《系统实现与工程优化》（解决高并发、LCA优化树、代码可复现设计）。
