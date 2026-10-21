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
| 回稿信 | 2026-10-21：已加入 AE 开头与四项主要修订摘要；两位审稿人的总评及 90 条分项意见均有回复，补入 25 条此前缺少的独立回复；活动正文无 `xxx`、`Page XX`。三处正文对应问题在回信中以红色 `Pending` 标记，见下方记录 |

2026-10-17 完成假设分组、§2 合并及量化误差定义；原有公式与命题标签保留，新增 `eq:quantization-error`。最新 `latexmk -pdf` 编译生成 PDF，无语法错误，仍有 `prop:original-certificate` 未定义引用；模型章、算法章和复杂度章编号分别为 §2、§3、§4。本次未进行完整数学复核。

## 下一步

1. 独立评议当前论文，确定最有价值的贡献与一项核心深化任务。完整任务指令见 `notes/prompts/gptpro-review-prompt.md`，方法入口为 `notes/skills/booming/SKILL.md` 和 `notes/skills/sparkling/SKILL.md`。
2. 对评议中发现的实质缺口，定位到具体命题、假设或证明步骤后处理；重点关联原问题、KKT 目标、有限精度保证与实验主张。
3. 2026-10-16 Booming 研讨确定初步科学问题（出口 A0）：`DualHash_A0_科学问题.md`。主线为「对偶化换来光滑随机速率、付出量化误差」；「对偶相对拆分加 $\operatorname{prox}_h$ 多换来什么」待验证。
4. 正文内容稳定后，更新对应审稿回复、主要变化摘要与修订摘录，编译并核对最终编号、页码和标色。

## 2026-10-20 修订记录与待办

- **单位标注（实验室自主课题要求）**：作者信息中崔春风老师的 `\thanks` 已加上 “State Key Laboratory of Virtual Reality Technology and Systems, Beihang University”（`paper/ex_shared.tex`，2026-10-21）。终稿、回稿信首页及后续成果（含 arXiv、slides、致谢/基金说明）提交前都要核对这一单位是否保留。

- 第 5 节（复杂度）结构调整：正文保留 Dual increment bound、两个 Lyapunov 函数及其充分下降引理、势函数下界段落和两个复杂度定理；One-iteration progress（\(\boldsymbol{B},\boldsymbol{\Lambda}\) 与两种 \(\boldsymbol{x}\)）、Auxiliary-block tracking、两个收敛率引理移入 `supplementaryB.tex` 各自证明小节开头，附录章节结构不变。正文新增或改写处标红：\(K_1\)–\(K_4\) 的定义句、指向移动引理的引用、StoM 定理中 \(L_\Delta, C_\sigma\) 的出处。备份为 `*.bak_move`。
- 第 5 节开头改为显式定义 \(L_F\)，有界性推导留在附录 B.1；code-length remark 已删除。
- §3.3 重写为“优化误差 + 结构误差”两项叙述；引理 `lem:smooth-lower-bound` 改为 (a)(b)(c) 三条，(c) 分一般情形与 \(\ell_1\) 情形；定理 Moreover 给出 \(Q_1\) 版本与 \(L=L_0\)；remark 环境已去掉。
- 正则项分层：W 类管结构性结论，\(\ell_1\) 管算法、复杂度与实验；§2 一句话说明范围并定义 compact dual domain；§6.4 开头一句连接 \(Q_1\) 与结构误差；表 1 精简正则列。
- **待办（编译后统一处理）**：回信中所有引理、定理、公式编号需重核。第 5 节引理移入附录后，“Lemma 5.x (now Lemma 5.y)” 应改为 “now Lemma C.x”，定理编号可能变化；附录公式编号也需核对。回信 R1 “Length and organization” 回复仍为空，可写入第 5 节精简；回信尚未提及推论 `cor:original-stationarity`。

## 2026-10-21 回稿信统一

- 已同步 `revision/response_letter.tex` 与 `revision/revision_refs.bib`。AE 称呼为 Professor Chouzenoux；90 个审稿意见原文块与本轮修改前完全一致。总评及分项均有对应回复，历史注释内容保留。
- 编号和大部分页码通过 `xr` 引用 `paper/main.aux`。先编译 `paper/main.tex`，再编译回信。已核对量化误差条件、罚模型关系、初始化、常数、引用及删除或迁移段落的回复。
- 格式：已核对回复使用原有黑色答复、蓝色修订摘录、绿色位置；三处未完成的正文对应问题标红：§2 的 ℓ₁ 限定与 §5 的 compact-dual-domain 范围不一致；Related Work 的变量提升公式仍未编号、符号未就地定义；全局随机 oracle 假设与有界 B 区域上的光滑性论证尚需统一。红色说明位于 W 类范围、变量提升、光滑性三条回复中。
- 验证：`latexmk -pdf -interaction=nonstopmode -halt-on-error response_letter.tex` 成功，生成 28 页 PDF；无未定义引用、文献或活动占位符，仍有四处不足 2.4pt 的轻微行溢出。本轮未修改论文正文，也未开展新实验或完整证明审计。

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

- 2026-10-18 将 StoRM 的辅助块跟踪结论提到 `convergence.tex`，新增 `lem:auxiliary-tracking`，并将附录对应部分改为 `Proof of \Cref{lem:auxiliary-tracking}`；加权误差累积证明改为引用正文引理。`latexmk -pdf main.tex` 通过，未发现新增未定义引用或 LaTeX 错误；常数 12 和 146 的完整展开证明仍待补充。

- 2026-10-18 在 `prop:original-certificate` 后新增 `rem:conjugate-minorant-role`，说明共轭下界保留乘子的一阶项、将乘子依赖集中到 `L_0`，以及结构误差对固定罚参数 `\gamma` 的无关性；引用 `nocedalWright2006`。本次未编译。

- 2026-10-18 按最新写作要求更新引言：在现有 “In this paper…” 段后加入计算与意义两个科学问题及回答，恢复四条贡献并将新增内容标为红色；将复杂度表述改为 “matches the order of the best-known lower bound”。同时修正 `convergence.tex` 中未闭合的 `\blue{...}`，`latexmk -pdf main.tex` 通过，未发现新增未定义引用或编译错误。

- 2026-10-18 将正文及回稿信中的 `entrywise` 统一改为 `componentwise`，覆盖 dual proximal 更新、量化误差等式、符号函数和辅助块跟踪证明，并标注替换处。

- 2026-10-18 将原目标证书中的光滑常数记号简化为 $L_s(\|\bLmbd\|)$，保留其对乘子范数的依赖。

- 2026-10-19 按讨论版本修订 Introduction：明确核心困难与 W-type 正则项的采用情况，调整标量 profile 和随机复合优化相关表述，恢复 Q1 的 affirmatively 回答；补充 DualHash 的 Fenchel 对偶更新机制、紧致对偶域下的复杂度范围，以及原目标近驻点证书和实验量化误差结果。删除 `optimality.tex` §3.3 开头重复的证书预览。本轮按要求未编译。

- 2026-10-19 修复 `paper/main.tex` 摘要中的双反斜杠转义错误。`\\red`、`\\blue`、`\\bz` 等内容曾被 LaTeX 解释为换行加普通文本，导致 PDF 摘要出现裸露的 `red`、`blue` 和宏名。改为正确的单反斜杠命令后，强制 `latexmk -g -pdf -interaction=nonstopmode -halt-on-error main.tex` 编译通过，未发现未定义引用或 LaTeX 致命错误。

- 2026-10-20 标色核对任务状态：对照基准为固定提交版文本导出 `paper/DualHash(2).md`；范围从当前稿 Related Work 到旧稿 §3.1 对应内容。已有标色调整，但尚未完成该范围的逐处最终核对，不得据此声称全部审稿意见均已落实或标色完整。

- 本项任务的修改约束：不得改动用户已经修改的正文内容，包括文字、公式、变量、论证与内容顺序；只允许调整颜色标记（如 `\\blue`、`\\red` 或图中颜色参数）。仅移动位置且内容不变的文字不标色。若发现正文或审稿意见尚有问题，只记录并报告，不直接修改正文。`old_version/` 是小改版工作材料，不作为当前大改版与提交版的对照基准。

- 2026-10-20 核对 Related Work 至旧稿 §3.1 标色：对照 `paper/DualHash(2).md` 和两份审稿附件、回稿信完成范围内检查。仅微调 `related_work.tex` 与 `problem_reformulation.tex` 的颜色标记：补上新增共轭定义、表格说明和非凸分裂文献引用的颜色；撤掉与旧式相同的下降引理公式及模型中保留公式主体的整块蓝色；保留图中发生变化的变量标签和阈值标签标蓝，未改曲线和刻度保持原色。没有修改论文语义正文。

- 本次发现的待处理项：`related_work.tex` 中约束提升式仍使用 `\\notag`，正文也未定义 `f(\\bw)`,`h(\\bw)`,`\\bc(\\bw)`；但 `revision/response_letter.tex` 声称该式编号为 (1.2) 并已给出定义。这是正文与回稿信不一致，按用户要求仅报告，未改正文。另有 Reviewer 1 关于精简或移置常规预备知识的建议；当前稿将预备知识并入模型章，但仍保留 proximal mapping 性质和下降引理等陈述，是否足以回应需单独判断。


## 2026-10-21 本轮标色核对

- 按用户最新范围，对照固定提交版 `paper/DualHash(2).pdf` 的文本导出，完成 `numerical_experiments.tex` 从 §6.2 到第六章末尾的精细标蓝：保留内容为黑色，实质变化按词句标蓝；整句新增的原目标结构误差说明保留整句蓝色。去除该范围的红色命令和未变化图号的蓝色。去除颜色包装后，文字、公式和表格数据与本轮标色前一致。
- 第五章末尾原目标复杂度保证小节按用户要求统一为蓝色，内容不变。第五章两条随机梯度假设本轮保持原文；其适用域量词与开头迭代区域说明的关系仍待作者决定。
- 前序最小修订已补充期望意义下的近驻点限定、初始化及部分文字标色；修复了写入颜色命令时产生的转义错误。量化误差图中的 DTSH 16-bit mAP 先前已按主表对齐至 0.6590，本轮仅将该点及数值的修订标记由红色换成蓝色；此对齐不代表已核验原始实验记录。
- 回稿信仍在核对阶段，尚未写入本轮更新，不能视为已完成。一般 W 类的量化误差界范围、实验敏感度百分比等审阅待办亦未在本轮标色中处理。
- 验证：独立目录 `/tmp/dualhash-response-work-20261021/paper` 的 `latexmk -pdf -interaction=nonstopmode -halt-on-error` 编译通过，无未定义引用、未定义命令或重复标签警告；保留已有排版警告，已更新 `paper/main.pdf`。

- 2026-10-21 按用户后续指定恢复提交版文字：§6.4 第（1）项末句、第（2）项正文、第（3）项及其高位/低位两段，以及 §6.5 参数敏感度解释段。恢复内容不标色；保留未被要求恢复的参数网格与其修订标色。此记录覆盖前条相应段落的标色状态。

- 2026-10-21 用户明确收紧实验修订范围：除实质性错误外保留原稿，不因措辞风格重写结果分析。已恢复 §6.2、§6.3 的原稿结果叙述及黑色显示；§6.3 首句按固定提交版指向 Tables 6.1、6.2，使用实际表格标签。表格数值与图片保持本轮恢复前的状态。

- 2026-10-21 按用户最新指令：恢复 §6.4 第（1）项原稿标题与正文，撤销该项的改写标色；按审稿人要求将 `helps produce` 写为 `helps \blue{to} produce`。图 6.2 的四个子图说明和两处纵轴标签统一为 P@topK、P@r=2，修订指标文字标蓝；仅覆盖标签区域，底层图像数据保持相同。ELU 原文按用户要求保留，不将其表述问题归为审稿人意见。
