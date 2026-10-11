# W-type 文献最小更新方案

日期：2026-10-11。用户已选择“最小更新”；本文档供正文实施前审查。

## 目标与定位

以深度哈希为应用主线，突出非光滑 W-type 正则与非线性网络输出复合所产生的优化问题。通过 BinaryRelax、ProxQuant 和 PARQ 建立与量化优化文献的简短联系。BinaryRelax 在 SIIMS 发表，是学术定位的直接依据；本文的非渐近复杂度分析与条件性的驻点对应关系构成差异。

## 已完成的设计任务

- 已查看现有 Related Work、第三章定义与例子、结论、BibTeX、Git 状态与近期提交。
- 已提供最小更新、完整交叉线、仅哈希更新三种方案；用户选择最小更新。
- 已核对出版信息、论文公式以及不同论文间的理论边界。
- 本任务讨论文献范围和数学定位，无需视觉伴侣。
- 本文档已进行范围与一致性自检；正文实施等待用户审查。

## 文献证据

| 文献 | 核对结论 | 原始来源 |
| --- | --- | --- |
| BinaryRelax，SIIMS 2018，11(4):2205–2223 | Moreau envelope 得到到量化权重集合的平方距离；量化集合包含共享可调尺度，不能直接等同于本文固定二值集合的可分 W-type 正则 | https://epubs.siam.org/doi/10.1137/18M1166134 ; https://arxiv.org/pdf/1801.06313 |
| ProxQuant，ICLR 2019 | 第 3.1 节、式 (5)–(7) 给出二值权重的 W-shaped 正则及显式近端；平方版本同节讨论；Theorem 5.1 对正则可微性的要求须保留 | https://arxiv.org/pdf/1810.00861 ; https://arxiv.org/html/1810.00861v3 |
| PARQ，ICML 2025，PMLR 267:28044–28062 | 使用凸分片仿射正则及 aggregate proximal stochastic gradient；不能声称其正则直接等于本文的双共轭；原文二值示例端点之外采用无限斜率 | https://proceedings.mlr.press/v267/jin25e.html ; https://arxiv.org/pdf/2503.15748 |
| MambaHash，ICMR 2025，442–450 | 式 (18) 是输出与其 sign 的平方距离；可表述为平方二值惩罚；第 4.2 节使用 RMSProp | https://arxiv.org/pdf/2506.16353 |
| CRH，AAAI 2026，40(14):12018–12026 | 方法名称为 Center-Reassigned Hashing，缩写 CRH；式 (3) 是平方二值惩罚；Adam 与中心重新分配结合；多标签实验把量化权重设为零 | https://ojs.aaai.org/index.php/AAAI/article/view/38190 ; https://ojs.aaai.org/index.php/AAAI/article/download/38190/42152 ; https://github.com/iFamilyi/CRH |

额外交叉核对：SHC 的 arXiv 原文第 42 式为平方二值惩罚。尽管文字说沿用 CSQ，不能据此填成 log cosh。CSQ 原文第 4 式确实采用 log cosh。SHC 与 CSQ 本轮不增加表格行，保留已有引用。

来源：https://arxiv.org/html/2507.08404v1 ; https://arxiv.org/pdf/1908.00347 。

## 文件修改范围

### paper/sections/related_work.tex

保留现有 Optimization in Deep Hashing 段落的应用主线。将 MambaHash 的单独说明与 CRH 合并成近期仍使用平方二值惩罚的一句。去掉 squared W-type 正则全局 smooth 的不准确表述，因为它在零点仍不可微。

量化背景增加两句，工作差异用一句结束。候选文本：

> Related regularization-based approaches also arise in neural-network weight quantization: BinaryRelax penalizes squared distances to quantized weight sets, while ProxQuant uses W-shaped penalties with explicit proximal updates. More recently, PARQ develops convex piecewise-affine regularization for quantization-aware training. These methods regularize the weights directly; our focus is stochastic optimization with nonsmooth W-type penalties composed with nonlinear network outputs.

三句分别引用 BinaryRelax 与 ProxQuant、PARQ，以及本文自身的模型。若篇幅受限，把第三句并入现有段落的结尾。整体净新增英文正文控制在约 100 词内。

深度哈希比较表仅新增 MambaHash 和 CRH 两行。两行量化项均为平方二值惩罚；优化分别为 RMSProp 和 Adam + center reassignment。CRH 行或表下注注明多标签实验未启用该罚项。Type 区分样本代码的更新方式；中心重新分配在 Optimization 列说明，避免与样本二值代码更新混淆。Complexity 列表示驻点目标的非渐近复杂度，未报告对应分析的行使用现有破折号。

更新 multirow 的实际行数；将现有 DCH 至 DHN 的 5 行共用组计数纠正为 5。保持其余表结构与内容。

### paper/sections/problem_reformulation.tex

当前例子采用 enumerate，未采用 Used in 表格，因此只补例子后的引用。已有 ProxQuant 覆盖线性和平方例子，无需重复新增。平方例子补 CRH；BinaryRelax 只放 Related Work 的广义量化背景，PARQ 不放入非凸 W-type 例子。

保留 W1–W3、引理、证明及用户正在调整的结构。本文数学结构的比较在 Related Work 结束，不扩展到模型推导。

### paper/sections/conclusion.tex

把 binary neural networks 的扩展方向用语同步为 binary neural-network quantization，并引用 BinaryRelax 与已有 ProxQuant。保留未来研究语气，不宣称本文已经分析二值激活网络。

### paper/refers.bib

新增 BinaryRelax、PARQ、CRH 三条正式发表的 BibTeX。复用现有 ProxQuant、MambaHash、SHC、CSQ 条目，避免重复。

## 理论表述边界

本文 Proposition prop:transfer 给出接触集条件与原问题驻点的对应关系。条件是辅助变量逐项绝对值至少为一，不能写成每项恰好为正负一；也不能写成算法产生的所有 KKT 点自动满足接触集条件。

正则自身有显式近端算子，不能据此推出与非线性网络复合后的近端也容易计算。本文差异应落在非线性复合结构、非光滑正则与有明确假设的随机复杂度结果上。

## 实施后的验证

核对新增 citation keys 存在且无重复，失败时修正 BibTeX 与引用。编译最新 paper/main.tex，检测新表格语法、引用未定义和行宽溢出；出现问题时修正对应引用或表格布局。仅检查受影响表格页的视觉布局。此次文献更新不要求重跑实验或额外数学审计。

正文着色沿用项目约定。未获另行上传指令时，仅保留本地变更。
