# DualHash 当前项目状态

更新日期：2026-10-18。本文档是当前进度入口；数学内容以 `paper/` 最新源码为准。

## 目标与阶段

- 目标期刊：SIAM Journal on Imaging Sciences（SIIMS）。编辑给予强修订再提交机会，来信见 `../project/editor-correspondence.md`。
- 当前处于论文与回稿信的实质修订阶段。近期工作集中于模型与驻点关系、复杂度分析、初始化、势函数下界及附录组织。
- 本轮准备通过 Booming、Sparkling 对当前成果作独立研究评议。评价 Taste、Insight、Importance、Novelty、Solid、Vision，回稿信完成程度不进入研究评分。

## 当前成果位置

| 内容 | 当前来源与状态 |
|---|---|
| 论文主入口 | `paper/main.tex` 与 `paper/ex_shared.tex`；标题已调整为 W-Type Regularized Deep Hashing 的随机原始—对偶算法 |
| 模型与最优性关系 | §2 `paper/sections/problem_reformulation.tex` 合并模型与预备知识，包含 W 类定义与结构性质、原目标 P0、量化误差 Q、罚模型 P 和基本假设；通用定义保留在 `preliminary.tex`；Q 使用到二值集合的逐元素 ℓ₁ 距离，列出 ℓ₁ W 正则的精确等式及一般 W 类的零值等价；最优性证明见 `supplementaryB.tex` 中 `app:optimality-properties` |
| 算法及采样 | §3 `paper/sections/methodology.tex`；Recursive Momentum 小节已注释通用 StoRM 介绍，保留 Cutkosky 等人的引用并直接给出实例化估计量；采样与 §4 复杂度章节联合说明 |
| 理论条件与复杂度 | `problem_reformulation.tex` 模型后列 Basic regularity，W 类讨论保留共轭有效域条件；`convergence.tex` 列 Stochastic first-order oracle，方差条件限定于已有迭代有界区域，StoRM 的 Mean-square smoothness 独立保留；相关引用已统一 |
| 理论证明 | `paper/sections/supplementaryB.tex`；含共同估计、两种算法的势函数下界与复杂度证明 |
| 实验 | `paper/sections/numerical_experiments.tex`、`supplementaryA.tex` 与图片目录；本次同步未开展新实验 |
| 回稿信 | `revision/response_letter.tex` 已有多项实质回复，仍含 summary 占位、`xxx` 和 `Page XX`；这些是交付待办，不参与研究质量评分 |

2026-10-17 完成假设分组、§2 合并及量化误差定义；原有公式与命题标签保留，新增 `eq:quantization-error`。最新 `latexmk -pdf` 编译生成 PDF，无语法错误，仍有 `prop:original-certificate` 未定义引用；模型章、算法章和复杂度章编号分别为 §2、§3、§4。本次未进行完整数学复核。

## 下一步

1. 独立评议当前论文，确定最有价值的贡献与一项核心深化任务。完整任务指令见 `notes/prompts/gptpro-review-prompt.md`，方法入口为 `notes/skills/booming/SKILL.md` 和 `notes/skills/sparkling/SKILL.md`。
2. 对评议中发现的实质缺口，定位到具体命题、假设或证明步骤后处理；重点关联原问题、KKT 目标、有限精度保证与实验主张。
3. 2026-10-16 Booming 研讨确定初步科学问题（出口 A0）：`DualHash_A0_科学问题.md`。主线为「对偶化换来光滑随机速率、付出量化误差」；「对偶相对拆分加 $\operatorname{prox}_h$ 多换来什么」待验证。
4. 正文内容稳定后，更新对应审稿回复、主要变化摘要与修订摘录，编译并核对最终编号、页码和标色。

## 历史材料的使用

- `revision/research/stochastic_proof_audit/report.md` 记录早期修正方案，含 `revision.tex` 及“正文尚未替换”等历史状态；当前正文已存在新的初始化、下界和证明章节，研究时直接读取最新源码。
- `revision/research/siam_stationarity_audit/report.md` 是文献与理论目标核查记录，按具体问题取证。
- `docs/superpowers/specs/2026-10-11-wtype-literature-minimal-design.md` 是历史设计方案，实施状态由当前正文确认。
- `revision/appeal_letter.tex`、`boundedness_responses_v2.tex` 与 `revision/materal/` 保留推导和交流背景。
- 固定提交版是 `paper/DualHash(2).pdf`；`revision/supplementary/0_DualHash.pdf` 与它的字节内容不同，分别保留。

## 项目维护

- `AGENTS.md` 维护共用项目指令；`.claude/CLAUDE.md` 导入共用指令，避免两套正文重复维护。
- `notes/skills/` 来自用户指定的 `3_DDLALM/overleaf/notes/skills/`，保留参考材料、许可证和来源说明。
- GitHub 主分支为 `origin/master`；保留 `paper-overleaf/` 用于 Overleaf 同步，主仓库忽略本地独立工作副本。
- 2026-10-16 按用户指示删除重复图片、临时截图、编译辅助文件和无独有修改的 Claude 工作副本；两张旧图与旧网页幻灯片归档到 `archive/`。详情见 `file-inventory.md`。编译辅助文件将在下次编译时生成。

- 2026-10-17 按用户指定原文加入 `rem:wtype-roles` 三句 Remark，并在 W 类表格说明后补充非弱凸结论；旧条件说明注释与必要性反例当前已不存在，复杂度章未修改。PDF 已生成；`prop:original-certificate` 尚未定义，`Ass: basic regularity` 已恢复，当前仅证书命题引用待处理。

- 2026-10-17 同步当前修订：统一 W-type 术语；预备知识中通用函数用 `r`、光滑函数用 `g` 与 `L_g`，模型保留 `f`；W 类小节整体标蓝并清理重复标色，表格设为 `[!t]`。保留用户对引言与模型叙述的修改。

- 2026-10-17 注释通用 StoRM 介绍，消除其目标函数与 W 类标量函数的记号冲突；移除通用估计量的正文引用。编译生成 PDF，无新增引用警告，仍有 `prop:original-certificate` 未定义引用。

- 2026-10-17 按清单命名原目标 `J_0`、整理 §2 小节与 W 类条件说明、迁移记号复用约定、精简重复文字、将已注释 StoRM 通用块的目标符号换为 `\Theta`，并加入指定引言段落及贡献说明；逐项检索与 `latexmk -pdf main.tex` 验收通过，无 LaTeX 错误或重复标签，仅待补 `prop:original-certificate`、`cor:original-stationarity` 两个标签。

- 2026-10-17 将 W 类表格后的适用范围段替换为 `compact dual domain` 子类定义，并在 StoM、StoRM 两个复杂度定理中显式加入该条件；W1–W3 的两句 Remark 与全 W 类的精确驻点对应保持不变。证书命题尚待添加，届时采用该子类条件，并注明证明只用共轭域的有界包含关系。编译生成 PDF，无新增错误，仍待补 `prop:original-certificate`、`cor:original-stationarity`。

- 2026-10-17 调整 W 类计算性质的叙述：说明表中原正则的近端映射在原点集值且无连续单值选择，共轭近端映射单值且非扩张；移除算法框架中“凸性使近端更易计算”的比较。保留逐元素近端公式及标签，并在算法框架引用；明确共轭函数有界用于建立 Lyapunov 函数的一致下界。

- 2026-10-17 将模型部分的量化误差记为 $Q_1$，正式定义为网络输出与逐元素二值化结果的 entrywise ℓ₁ 距离，保留 `eq:quantization-error` 标签；以行内一句给出表中 p=1 与 p=2 的对应关系，矩阵平方误差使用 Frobenius 范数。Notation 明确 sgn(0)=1；表中双变量 Q 与附录迭代元组 $\mathcal{Q}^k$ 保持原记号。

- 2026-10-17 按 Notations 与 Reviewer 2 的范数记号意见，统一模型、复杂度正文及附录中的矩阵 Frobenius 范数为无下标写法；保留 ℓ₁ 范数下标与光滑常数 $L_F$，不改变数学条件。

- 2026-10-17 按用户要求将 §3 整理为四节：3.1 合并 min–max 构造与 KKT 条件；3.2 集中零间隙、间隙反例及精确驻点对应；3.3 框架与 3.4 实现保持原内容。完整 `rem:kkt-choice` 及其反例保留在 3.1 正文，例子改为自足表述；所有原标签保留，新增 `Sec: original optimality relations`。本次结构调整及必要衔接修改按用户最新要求统一标蓝。

- 2026-10-18 加入原目标近驻点证书与两条 Remark，保留示意公式及其说明，补齐共轭下界、证书、表格常数与乘子界的附录证明。NAS 允许零容差，范数省略 F 下标。Algorithmic Framework 提升为独立章节，随机更新实现归入该章。

- 2026-10-18 两个算法统一采用 `eq:proximal-initialization` 的近端初始化，去除初始网络输出须在立方体内的要求；保留一般输出界，tanh 输出下退化为原初始化。同步复杂度章块驻点说明、附录初始次梯度来源、初始辅助变量范数界和 H1=0 的理由；乘子界明确使用 compact dual domain。核对首步恒等式与初始势函数估计的依赖。独立目录 `/tmp/dualhash-proximal-init-build` 中 latexmk 编译通过，无未定义引用；既有排版警告仍在。本次仅处理初始化，辅助块跟踪引理完整推导、全章 compact dual domain 约定、批量上限、均方光滑假设及 transfer 证明指代等待办保持待处理。

- 2026-10-18 按用户要求去除第三章 3.2 起的整段标蓝，恢复 3.2 标签并为 3.3 补充标签；算法两种实现提升为 4.2、4.3，实验中的算法章节引用改指 Algorithmic Framework。复杂度章新增 5.4（一个原目标近驻点推论及一个码长 Remark），不标蓝；将量化误差阈值表述为充分条件，注明码长比较固定 n、lambda 和光滑常数。实验补充 Q1/(nd) 口径及码长 Remark 引用。独立目录 `/tmp/dualhash-section-sync-build` 编译通过，无未定义引用；仅保留已有附录溢出警告。

- 2026-10-18 按用户确认删除正文 `rem:kkt-choice` 及 ADMM/ALM 比较、反例与次微分包含关系论证；在 KKT 映射后以两句说明原变量极小化、对偶变量极大化及对偶方向的凸次微分。检索正文与回稿信无该 Remark 标签引用；同步修正邻接过渡段中 J0 所属问题的引用为 P0。

- 2026-10-18 按最新要求将 KKT 映射后的说明调整为“对偶极大化条件—ADMM/ALM 对比—原点反例—采用 KKT 映射”，保留为正文段落。反例在 R² 上给出，次微分计算只涉及有限函数；未恢复有效域边界上的次微分包含关系论证。

- 2026-10-18 合并 3.3 过渡段：区分罚模型的精确驻点关系与原目标证书，说明有限罚参数下 B 与网络输出可能不一致；零残差、零量化误差下的精确驻点结论作为已有证书特例。小节标题引用由 P 修正为 P0，未新增定理。

- 2026-10-18 近驻点定义已改为正式 definition 环境；将残差定义移至本节首次使用处并补上 Q1 符号。定义引导改引 Tian、Zhou、So (ICML 2022), Definition 2.5，注明本文用极限次微分替代其 Clarke 次微分；保留原定义与证明。Goldstein (1977)、Zhang 等 (2020) 的 BibTeX 条目保留，但不作为本文 nearby-point 定义的直接出处。

- 2026-10-18 将 `prop:original-certificate` 改为优化误差与结构误差的加法分离形式：在 `methodology.tex` 中拆分 `(a)` 的 `\delta`、`\varepsilon`，保留 `(b)` 的 `L_0`；在 `convergence.tex` 中保留推论期望界并简化证明；在 `supplementaryB.tex` 中补充 `\sqrt{a+b}\le\sqrt a+\sqrt b` 与近驻点单调性推导。`revision/response_letter.tex` 未引用该命题。`latexmk -pdf main.tex` 已通过，未发现未定义引用或新增 LaTeX 错误；保留既有排版警告。
