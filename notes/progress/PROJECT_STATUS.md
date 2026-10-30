# DualHash 当前项目状态

更新日期：2026-10-27。本文档是当前进度入口；数学内容以 `paper/` 最新源码为准。

## 2026-10-30 回稿信标记方案调整
- 去掉全部 `\green` 与正文 `\newred`；含页码的位置括注整体用 `\blue{(...)}`（86 处）。Cover letter 后恢复一句颜色说明。题号 `Q[n]` 改为 `Qn`；回信内局部公式用 (R1.1)、(R1.2) 编号。
- **行号（已接入，可再调）**：稿件的 `review` 选项已带 lineno 行号；`main.tex` 重定义 `\label`，正文中的 `\label{X}` 同时记行号 `ln:X`（公式、图表内的标签无行号）。回信用 `\mpl{X}`（“p. 11, line 309”）与 `\mplr{a}{b}`（页行范围）；无行号时只显示页码。改稿后编译 `paper/main.tex`，再 `cp paper/main.aux revision/manuscript_labels.aux`。

## 交接：新对话从这里接手（2026-10-27）

当前任务是按导师 Cui 老师批注（参照高莹 SIIMS 回信格式）改 `revision/response_letter.tex`。格式规则见 `notes/progress/response_letter_style_guide.md` 第〇节；cover letter 的目的与三步逻辑见 memory `cover-letter-purpose`。

- **已完成**：cover letter（一页、无稿件编号、Cui 老师落款）；回信开头（Summary 四条 + Conventions，R1 另起一页）；R1 Q1–Q3；R2 按原报告顺序 Q1–Q88。详见下面两节 10-27 记录。
- **待用户审**：红色 `\newred` 文字，全部在 R2（Main Changes 与 R1 已无红字）——Q44、Q49、Q51、Q53、Q56 指路句；Q55 整段；第 5、9 节导言回复；Q50 的 “on Lemma~5.7”；“our response to Qx” 交叉引用（Q2、Q4、Q38、Q42、Q76）；结尾致谢段；以及 10-27 按类型小改新增的 7 处（见下节）。用户确认后去红。
- **未提交**：本地改动未 commit；Overleaf 上是 1f231c9 版（R2 重排之前）。commit 与再次推送 Overleaf 都等用户明说。
- **注意**：`revision/manuscript_labels.aux` 在工作区被删除（未提交的删除）；Overleaf 编译依赖这份快照，推送前用 `cp paper/main.aux revision/manuscript_labels.aux` 重建。
- **小问题**：余 2 处 <1.5pt 行溢出，可不处理。

## 2026-10-27 回信按意见类型小改（红色 `\newred` 待审）

- **原则**：按意见类型定回法与篇幅（规范第三节 2026-10-27 补充）；改已有回复以小改为主。先做过一版较大删减（删引文、整条重写），用户要求“不要删减太多、不要大改”后已全部恢复原文。
- **新增（红）**：R2 Q1 用 “As suggested, we” 替换单独致谢句；Q32 说明提交版 (4.5) 第二行（分析所用）正确、无估计受影响（已对照提交版附录 C）；Q39 [ii] 补 Example 的数值（$p^*=\tfrac12$，$d^*=0$），[iv] 与 Q38 重复的 coercivity 句改为指向 Q38；Q79 说明只改变 $L_\Delta$、$C_\sigma$；Q83、Q84 说明复杂度阶不变（指向 Q88）。
- **删去（各一句）**：Q17 “The subsequent sentence explains …”，Q19 “The smooth squared mismatch … is a separate term.”，Q64 “These settings give the plots more space …”。另修 Q24 引用式号的双括号。
- **验证**：编译通过，无未定义引用，31 页。

## 提交用 PDF 压缩

`paper/main.pdf` 约 8.9 MB，超过 SIIMS 5 MB 上限（实验图片为 1500 ppi）。最终编译后在 `paper/` 下运行以下命令生成提交版 `main_submission.pdf`（约 1.5 MB，图片降到 300 dpi，JPEG 质量 90）：

```bash
gs -q -dNOPAUSE -dBATCH -dSAFER -sDEVICE=pdfwrite -dCompatibilityLevel=1.6 -dEmbedAllFonts=true -dSubsetFonts=true -dDownsampleColorImages=true -dColorImageResolution=300 -dColorImageDownsampleThreshold=1.0 -dDownsampleGrayImages=true -dGrayImageResolution=300 -dDownsampleMonoImages=true -dMonoImageResolution=600 -dAutoFilterColorImages=false -dColorImageFilter=/DCTEncode -dJPEGQ=90 -sOutputFile=main_submission.pdf main.pdf
```

## 2026-10-27 Referee #2 按原报告顺序重排

- R2 共 88 条，按审稿人原报告顺序排列，标题照搬（General comments，1 Abstract/Introduction … 10 Appendix D），全文连续编号 Q1–Q88（R1 为 Q1–Q3，各审稿人单独编号）。删去自拟主题小节、各节 “General response”、总评回复和全部 `\hyperref`；回复正文逐段保留（脚本核对 0 缺失）。
- 新增文字标红 `\newred`：第 5 节 5 条 “See my comments relative to Appendix C/D” 的指路句（Q44、Q49、Q51、Q53、Q56）；Lemma 5.10 拆出为 Q55；第 9 节导言回复；交叉引用改为 “our response to Qx”；结尾致谢段（去掉落款）。
- 验证：编译通过，无未定义引用，31 页；余 3 处 <7pt 行溢出。

## 2026-10-27 Cover letter 与回信开头收紧（红色 `\newred` 待审）

- **已决（2026-10-27）**：cover letter 首句删去旧稿编号，改为 “of our manuscript”；旧编号只留在抬头 “Previous manuscript ID”。Cover letter 定稿结构：致谢 → 贡献同原稿（(i)–(iii)）→ “Following the decision letter and the referees' suggestions” 三条（加粗整句，先原有后补全）→ 改题、通讯作者 Chunfeng Cui、请求重新考虑；已去红。 **已定**：保留（与师姐的写法一致），无需修改。

- **Cover letter**（一页）：保留主干一句；三条按决定信顺序（关系 → W 类与重构定义 → 收敛分析），每条先写原稿已有、再写新增；删去 “encouraged … experimental section” 与 “mathematical significance” 两处复述；原第 4 条（RQ 重构）删去，Corollary 5.4 并入第 1 条；第 2 条补 “clarified the notation of the reformulation”。按用户要求，cover letter 不出现稿件编号（命题、假设、公式、问题标签均改为文字描述），编号只在回信中给出。
- **回信开头**（一页多，R1 接排不分页）：RQ 引文块改为一句说明（写明是我们提出的问题）；Summary 五条只保留结论句，另放回三句原话：共轭凸而 $h$ 非凸、标准对偶论证不直接适用；量化误差常被当作经验指标；已有复杂度结果覆盖更简单情形。Conventions 列表并为一句。
- **再调整**：Main Changes 恢复原四条（老师只要求去掉 “(Reviewer 1, Section 3)” 这类按回信章节指路的括注），并入 R1 general response 的三处要点各一次（共轭凸而 $h$ 非凸；$Q_1$ 连接二值码质量与驻点保证；已有复杂度结果覆盖更简单情形），恢复 RQ 引文块与 Conventions。`qalist` 改为标签段首接排（不再悬挂缩进），正文与其他部分同宽。
- **验证**：编译通过，无未定义引用，31 页。

## 2026-10-26 回稿信改为高莹回信格式（首页与 R1 已完成，R2 待重排）

- **备份**：本地 git tag `response-letter-before-advisor-format`（指向 115f86e，即导师批注所读版本）。回退：`git show response-letter-before-advisor-format:revision/response_letter.tex`。
- **格式**（导师批注，参照高莹–Ng–崔回信）：删去 Contents、行号、黄底 Response 框、紫色链接和自拟章节；页边距由 0.5in 改为 0.75in（1in 时加上 Response 悬挂缩进太窄）；新增 `qalist` 环境（Q 编号，悬挂式 **Response:**）。保留蓝色引文、绿色位置和下划线。
- **首页**：Cover Letter 只留期望 AE、改题说明和概括段；主要变化四条移到回信开头 “Dear editors and referees” 之后，先写 “keeps the framework of the submitted manuscript”。第 1 条写明 Prop 3.1 即提交版 Lemma 5.2；删去 (P0)/(P)/min–max “stated explicitly”；假设改为 “slightly adjusted”；RQ 写明是我们自己提出的问题。导师划掉的两句已删。
- **R1**：总评原文斜体放在开头，不单独回复（删去 “While we agree … standard” 与四条贡献）；按原报告顺序排为 Q1 关系、Q2 W 类定义、Q3 篇幅。Q1 补一句：(P0)、(P)、(D) 与 (4.2) 在提交版中已有。回信中 (NC-L) 直接写出，避免稿件 `\tag{\red{NC-L}}` 的红色漏进回信。Table 2.1 加宽到 1.1 倍行宽。
- **待办**：R2 按原报告顺序重排（General comments 加 1–10 节，共 88 条），编号与是否分 major/minor 待定；R2 在新页边距下有 6 处 overfull，重排时一并处理；结尾致谢段改为高莹格式。
- **验证**：`paper/main.tex` 与回信编译通过，无未定义引用；回信 31 页。

## 2026-10-26 按导师 1021 批注修改正文（改动标红待审）

- 稿件改动均用 `\red{}` 标出：摘要/结论 “move to the dual space” 改为 “reformulate … through its convex conjugate”；研究问题 (Q1)(Q2) 改为 (RQ1)(RQ2)，避免与量化误差 $Q_1$ 冲突；Theorem 3.5 的 $h\le cQ_1$ 比较移到定理后并限定为 Table 2.1 中的正则；“compact dual domain” 三处改为 $\operatorname{dom}\phi^*\subseteq[-\lambda,\lambda]$；$L_F$ 共用说明改写；StoM 定理补 $\lceil c_bT\rceil>n$ 指向 Remark 5.x(iii)（新增 `rem:complexity`）；SFO 精确计数改为 $Tb$ 与 $b_1+2(T-1)b$；§5 开头补 KKT 残差与增量的桥接句；光滑下界引理补动机句；“to a large extent, the gap” 收窄；$\gamma,\lambda$ 段改为 Remark；结论实验结论按表格收窄；实验部分 ELU、训练曲线、separability、“novel/stems from” 措辞降调；Figure 6.6 移到 Parameter Sensitivity 标题后；Table 1.1 移到引言（第 3 页引用、第 4 页出现）。
- 回信同步：(RQ1)(RQ2)、Theorem 3.5 引文、两处 compact dual domain、R1 中 “other W-type regularizers” 改为 Table 2.1 中的正则，均用 `\rv{}` 标红。
- 未处理：$\sigma$（方差与 prox 参数）和 $r$（一般函数与 $\tau\gamma/n$）的符号重用；导师批注 2、15 的 “for→with” 需要原 PDF 定位；Figures 6.3–6.5 排版。
- 验证：`paper/main.tex`（50 页）与回信均编译通过，无未定义引用；回信引文脚本比对无新差异。
- 2026-10-26 晚：Table 2.1 首列与 Algorithm 列居中，caption 缩为 “Representative W-type regularizers.”，符号说明移到表下，格式同 Table 1.1 的 **Notes**（scriptsize parbox）；回信 R1 第 2 节同步放出该表（标红待审）。回信按正文收窄范围：structural error 的 $Q_1$ 控制限于 Table 2.1，复杂度范围改为 $\operatorname{dom}\phi^*\subseteq[-\lambda,\lambda]$，$\sqrt{Q_1}$，single-valued prox，推论限 $\ell_1$；(D) 补入问题列表。均用 `\textcolor{red}` 标出。
- 2026-10-26 晚：上述回信红色改动核对语病后转定色（回复正文黑、Table 2.1 与 Corollary 引文蓝、位置绿）；首页第 4 项限定语前移以免 “a hashing-specific quantity” 误指 Table。回信 30 页。提交版原稿改名为 `paper/DualHash_submission.pdf`（及 `.md`）。
- 2026-10-26 晚：回信改为可在 Overleaf 独立编译——参考文献内联（不再跑 BibTeX）；稿件编号优先读 `../paper/main.aux`，否则读提交的快照 `revision/manuscript_labels.aux`。**正文改动后需刷新快照**：`cp paper/main.aux revision/manuscript_labels.aux`；新增引用需按回信末尾注释重新生成 bbl。Table 2.1 caption 改为加粗标签+句号（仅此表）。

## 2026-10-26 参考文献专名大小写核查

- 检查共享 `paper/refers.bib` 的 118 个条目，并核查主稿 69 条、回信 16 条实际参考文献；范围仅为标题专名、缩写和原有数学符号的大小写，不是完整文献真实性或引用论证审计。
- 修正 11 个条目、13 处词或符号：Cauchy、Hamming、Polyak、Greedy Hash、CNN、BinaryConnect、MambaHash、SDCAM、SpiderBoost、CT、$L_0$、$L_{1/2}$、L1--L2。使用花括号保护大小写，保持标题斜体；按用户后续要求撤去本轮蓝色标记，修正内容保持不变。普通词、作者、年份、文献键和引用关系不改。
- 主稿实际使用其中 4 个条目；回信现有 Lagrangian、Lipschitz、iPALM 等保护有效，无需新增同类修改。两者继续共用 `paper/refers.bib` 和 SIAM 样式，不修改停用的回信独立 bib。
- 验证：全库 BibTeX 输出的 13 处大小写检查通过；主稿与回信编译成功（50、30 个物理页），无未定义引用或文献。受影响参考文献页的专名、局部蓝色及斜体已检查；既有宏包/排版警告保留。

## 2026-10-26 回稿信全文按新标准过完

- **回信**：R1 与 R2 全部章节按 `notes/progress/response_letter_style_guide.md` 第三节补充原则过完（先回答问题、加粗总结句、引文少而轻、短内容直接贴不回引、不用 Yes/No 起句）。`\chg` 已定义为黑色，正文不再标红；蓝色引文中局部改动用 `\mk{}` 下划线（仅 R2）。纯指路条目已注释。
- **首页**：新增写给主编与 AE 的 Cover Letter 正文（四项，对应 D3–D6；落款 The authors of Submission M187198）。Response 开头改为致谢 + 引 (Q1)(Q2) 原文回应 D6；Summary 四项改为结论句并标注对应审稿人章节；Conventions 增加下划线说明。Contents 标题已转黑。
- **稿件改动**：`supplementaryB.tex` 恢复 StoRM Lemma 5.8（x-progress）的详细证明（标蓝）；(4.5) 的两个 $\tau^{-1}$ 与 (C.7) 后不等式中的 $\mathbb{E}$ 补标蓝色。
- **核对**：所有 R2 与 R1 蓝色引文已用脚本与现稿逐句比对，剩余差异仅为 `\mref`/`\Cref` 写法。0.1τ 一句提交版已有，回信改为 “as stated in”。
- **待定**：首页 “Cover Letter”“Response to Reviewers” 两个标题及 D6 编辑原话仍为红色；页码在最终编译后核对；Cochran 章节号未核。
- **验证**：`paper/main.tex` 与 `revision/response_letter.tex` 均编译通过，无未定义引用。

## 2026-10-25 附录证明局部精简

- 按“保留原证明结构与写法、不大改”的要求，仅精简 `supplementaryB.tex` 蓝色共轭证明中的三段叙述，以及新补的红色 `lem:auxiliary-tracking` 证明。未标蓝的原证明、命题条件、结论和标签不变。
- Tracking 去掉三处 Step 标题及临时记号 $\mathcal T$、$\mathbf q_\omega$、$p(r)$，保留紧凑矩阵估计所需的 $\mathbf M_\omega$、$\mathbf\Pi$ 和二维向量。初始化的 $k=0$ 依据、两个行列式下界及常数 146、12、1753 均保留。
- 独立复核确认本次修改前后数学等价；此结论仅针对本次差异，不是全稿正确性认证。此前发现的 `prop:transfer` 证明中 (b)–(c) 衔接及 SFO 精确计数问题未在本轮处理。
- 验证：`latexmk -cd -pdf -interaction=nonstopmode -halt-on-error paper/main.tex` 成功，PDF 为 49 页，无未定义引用或文献，未检出 `??`；修改页 32、44–45 已检查，无公式裁切或重叠。日志仍有宏包/书签警告、underfull hbox 及约 3.05pt 的 overfull vbox 提示。回稿信未修改或重新编译。

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

- 第 5 节（复杂度）结构调整：正文保留 Dual increment bound、两个 Lyapunov 函数及其充分下降引理、势函数下界段落和两个复杂度定理；One-iteration progress（\(\boldsymbol{B},\boldsymbol{\Lambda}\) 与两种 \(\boldsymbol{x}\)）、Auxiliary-block tracking、两个收敛率引理移入 `supplementaryB.tex` 各自证明小节开头，附录章节结构不变。正文新增或改写处标红：\(K_1\)–\(K_4\) 的定义句、指向移动引理的引用、StoM 定理中 \(L_\Delta, C_\sigma\) 的出处。
- 第 5 节开头改为显式定义 \(L_F\)，有界性推导留在附录 B.1；code-length remark 已删除。
- §3.3 重写为“优化误差 + 结构误差”两项叙述；引理 `lem:smooth-lower-bound` 改为 (a)(b)(c) 三条，(c) 分一般情形与 \(\ell_1\) 情形；定理 Moreover 给出 \(Q_1\) 版本与 \(L=L_0\)；remark 环境已去掉。
- 正则项分层：W 类管结构性结论，\(\ell_1\) 管算法、复杂度与实验；§2 一句话说明范围并定义 compact dual domain；§6.4 开头一句连接 \(Q_1\) 与结构误差；表 1 精简正则列。
- **待办（编译后统一处理）**：回信中所有引理、定理、公式编号需重核。第 5 节引理移入附录后，“Lemma 5.x (now Lemma 5.y)” 应改为 “now Lemma C.x”，定理编号可能变化；附录公式编号也需核对。回信 R1 “Length and organization” 回复仍为空，可写入第 5 节精简；回信尚未提及推论 `cor:original-stationarity`。

## 2026-10-21 回稿信统一

- 已同步 `revision/response_letter.tex` 与 `revision/revision_refs.bib`。AE 称呼为 Professor Chouzenoux；90 个审稿意见原文块与本轮修改前完全一致。总评及分项均有对应回复，历史注释内容保留。
- 编号和大部分页码通过 `xr` 引用 `paper/main.aux`。先编译 `paper/main.tex`，再编译回信。已核对量化误差条件、罚模型关系、初始化、常数、引用及删除或迁移段落的回复。
- 格式：已核对回复使用原有黑色答复、蓝色修订摘录、绿色位置；三处未完成的正文对应问题标红：§2 的 ℓ₁ 限定与 §5 的 compact-dual-domain 范围不一致；Related Work 的变量提升公式仍未编号、符号未就地定义；全局随机 oracle 假设与有界 B 区域上的光滑性论证尚需统一。红色说明位于 W 类范围、变量提升、光滑性三条回复中。
- 验证：`latexmk -pdf -interaction=nonstopmode -halt-on-error response_letter.tex` 成功，生成 28 页 PDF；无未定义引用、文献或活动占位符，仍有四处不足 2.4pt 的轻微行溢出。本轮未修改论文正文，也未开展新实验或完整证明审计。

## 2026-10-24 删除直接形式与指令拆分

- §2.2 删除直接哈希公式 `Eq:direct_deep_hashing_problem`，开头改为 “To reduce the quantization error introduced by continuous relaxation, ...”。§2.3 去掉 surrogate 引用并就地定义 $\ell_{s,i}$；引言 (P0) 补定义 $n$、$d$，$f_i$ 改称 smooth loss term。回信 R2 “(page 6) Give a number to the equation relative to deep hashing” 一条已改写为删除说明。
- 验证：`main.tex` 与 `response_letter.tex` 均用 latexmk 编译通过，无未定义引用。
- 指令拆分：`AGENTS.md`（Codex）与 `.claude/CLAUDE.md`（Claude）各自独立，内容同步，仅公式写法与全局规则优先级不同。
- **符号统一（2026-10-25，W3 已改）**：W3 的阈值与下降常数改为 $\theta$、$c_\theta$，只动 `problem_reformulation.tex` 的 (W3) 与 `supplementaryB.tex` 中 `lem:no-inner-stationary` 的证明；该证明里的增量 $h$ 改为 $\delta$。复杂度、回信和 slides 里的 batch 常数 $c_b$ 未改。prox 通用参数 $\sigma$ 与噪声方差 $\sigma^2$ 同形，仍未处理，优先级低。

## 2026-10-25 NC-L 命名与回信重写

### 稿件改动
- §3.1 min–max 问题 `Eq: minmax problem` 标签改为 (NC-L)，正文说明其具有 Xu–Jiang–Liu–So（MOR 2026，`xu2026rada`）研究的 nonconvex-linear minimax 结构；未写紧定义域条件。
- 附录 (C.2)：删去 “uniform sampling without replacement”，改为 “the sampling rule in Section 4”，并引用 Cochran《Sampling Techniques》第 2 章（`cochran1977sampling`，章号待核对书本）；方差推导只在回信给出。
- 两个 bib 文件已加入 `xu2026rada`、`cochran1977sampling`。

- Assumption 5.1 删去恒等式 $\frac1n\sum_i\nabla_{\bx}F_i=\nabla_{\bx}F$（由 $F$ 的定义自动成立），只保留方差界；label `Eq: finite-sum sampling identities` 无引用。

### 回信改动
- 首两页 Cover Letter 只列决定信、EiC 来信、申诉信原话；三处标题（Cover Letter、Contents、Response to Reviewers）标红，定稿时再改。
- **Reviewer 1 已完成**：顺序为 general assessment → Definition of the W-type regularizer → Relation → Length and organization。
  - general assessment：四条贡献，借用 R1 原话 “what is actually novel”“meaningful guarantees”；第 (3) 条加入 $Q_1$ 与 $\operatorname{sgn}(\mathcal D(x))$ 的联系；第 (4) 条用 “existing … cover simpler settings” 句式。
  - Relation：开头写明原稿 Lemma 5.2 已有零间隙结论但未展开；列出 (P0)、(P)、(NC-L)；直接引 Ex 3.2、Prop 3.3、Def 3.4、Thm 3.5 原文；间隙统一写 “can be positive”。
- Reviewer 2：general assessment 与 Stochastic formalism 已改；其余小节待改。
- 讨论期间的 `\chg{}` 红色已全部转为黑色，稿件位置转绿色。
- 内部跳转改为整句紫色链接（`\hyperref`）。

### 写作规范
- 新建 `notes/progress/response_letter_style_guide.md`：期刊与会议的区别、颜色、单条结构、语气与留余地、术语表、一致性检查、NeurIPS 教训、协作流程，以及 MoSSP 与 DualHash NeurIPS 两份 rebuttal 的逐字范例与批注。`AGENTS.md` 与 `.claude/CLAUDE.md` 均要求起草回信前先读。

### 待办（Reviewer 2 第 2 节 Theory and algorithms）
1. 与稿件不一致处：Lemma D.1 回复写 “complete proof”，稿件只有两步概要；Theorem 5.2 回复与附录第 690、961 行的 $rac1T\sumrac1{b_k}=rac1{c_bT}$ 应为 $\le$；算法 4.1、4.2 的 Output 未写 “independently of the mini-batches”；(5.17) 引文是证明中的平均形式；Appendix C 一条仍写 “uniform sampling without replacement”。
2. 重复：Appendix C 前四条与 Appendix D 首条 “See my comments” 各写一段，改为一句加链接；(P) 回复的整段模型引文只保留 $P(x,B)$ 与 (P)。
3. 稿件展示缺口：
   - **A1**：tracking 引理（`lem:auxiliary-tracking`，`supplementaryB.tex` 第 776–797 行）稿件只有结论与常数 146、12、1753；完整推导在 `revision/appeal_letter.tex` 第 1111–1202 行（已注释，结论已核对）。需搬入稿件，否则 Lemma D.2 回复 “由 $H_k$ 控制” 在稿件中无依据。
   - **A3**：Assumption 5.1 与 MSS 对所有 $B$ 成立，而 $F$ 只在 $\lVertB
Vert\le R_B$ 上光滑；MSS 复用符号 $L_F$。第 3 节对应回复仍有 Pending。
4. 语气：泛化的 “We thank the reviewer.” 与多处 “We apologize” 按规范统一；删去残留 $\lVert\cdot
Vert_F$ 下标。

### 验证
- `main.tex` 与 `response_letter.tex` 编译通过，无未定义引用。
- 协作注意：20:19 前后 `response_letter.tex` 曾被旧缓冲区覆盖一次，后续 Claude 与 Codex 不同时改该文件。

### 2026-10-25 下午：A1、A3、Remark 2.4 与 R2 摘录
- **A3**：Assumption 5.1、5.2 的定义域收窄为 $\|B\|\le R_B$；§5 开头写出 $R_B:=C_{\mathcal D}+n\lambda\sqrt{nd}/\gamma$（证明在附录 B.1）。原因：$\nabla_x F_i$ 含 $\gamma\nabla\mathcal D_i(x)^*b_i$，全空间方差界与均方光滑对非线性 $\mathcal D$ 一般不成立。5.2 后加一句 $L_F$ 共用符号说明。回信 Pending 已删。
- **A1**：tracking 引理（`lem:auxiliary-tracking`）完整证明已从 appeal letter D.2–D.3 搬入 `supplementaryB.tex`，记号改为 $U^k,V^k,S^k,\omega,M_\omega,q_\omega,\Pi,\mathcal T$；常数 146/12/1753 已逐步验算（依赖 $\Delta=\tfrac12\|\cdot\|^2$ 与初始化保证 $k=0$ 恒等式）。整段标红。
- **Remark 2.4（rem:coercivity）**：原式只给出 $x$ 方向增长，推不出强制性；已补 $B$ 方向：$J\ge\underline\ell-\frac{\gamma}{2n}C_{\mathcal D}^2+\frac\mu2\|x\|^2+\frac{\gamma}{4n}\|B\|^2$，标红。
- **Related Work**：提升式编号为 (1.2)（`Eq: the constrained reformulation`），旁边定义 $w,f,h,c$，标红。回信全文 Pending 清零。
- **回信**：R2 §3 起条目级 “We apologize” 改为 “We agree …”，去掉 4 处空泛 “We thank the reviewer.”；R2 §2 加入蓝色原文摘录（Remark (iii)、附录 B.1 光滑性、Remark 2.4、两个下界引理、(C.7) 步、Lemma C.2 recall 句）；第 972 行页码改为 `\mpage`。
- 剩余：Introduction 的硬写页码（回信 p.~1、p.~3）待最终编译统一；general response、Summary of Major Revisions、三个标题标红、Cochran 章节核对。

## 历史材料的使用

- `revision/research/stochastic_proof_audit/report.md` 记录早期修正方案，含 `revision.tex` 及“正文尚未替换”等历史状态；当前正文已存在新的初始化、下界和证明章节，研究时直接读取最新源码。
- `revision/research/siam_stationarity_audit/report.md` 是文献与理论目标核查记录，按具体问题取证。
- `docs/superpowers/specs/2026-10-11-wtype-literature-minimal-design.md` 是历史设计方案，实施状态由当前正文确认。
- `revision/appeal_letter.tex`、`boundedness_responses_v2.tex` 与 `revision/materal/` 保留推导和交流背景。
- 固定提交版是 `paper/DualHash_submission.pdf`；`revision/supplementary/0_DualHash.pdf` 与它的字节内容不同，分别保留。

## 项目维护

- `AGENTS.md` 维护共用项目指令；`.claude/CLAUDE.md` 导入共用指令，避免两套正文重复维护。
- `notes/skills/` 来自用户指定的 `3_DDLALM/overleaf/notes/skills/`，保留参考材料、许可证和来源说明。
- GitHub 主分支为 `origin/master`；保留 `paper-overleaf/` 用于 Overleaf 同步，主仓库忽略本地独立工作副本。
- 2026-10-16 按用户指示删除重复图片、临时截图、编译辅助文件和无独有修改的 Claude 工作副本。

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

- 2026-10-20 标色核对任务状态：对照基准为固定提交版文本导出 `paper/DualHash_submission.md`；范围从当前稿 Related Work 到旧稿 §3.1 对应内容。已有标色调整，但尚未完成该范围的逐处最终核对，不得据此声称全部审稿意见均已落实或标色完整。

- 本项任务的修改约束：不得改动用户已经修改的正文内容，包括文字、公式、变量、论证与内容顺序；只允许调整颜色标记（如 `\\blue`、`\\red` 或图中颜色参数）。仅移动位置且内容不变的文字不标色。若发现正文或审稿意见尚有问题，只记录并报告，不直接修改正文。

- 2026-10-20 核对 Related Work 至旧稿 §3.1 标色：对照 `paper/DualHash_submission.md` 和两份审稿附件、回稿信完成范围内检查。仅微调 `related_work.tex` 与 `problem_reformulation.tex` 的颜色标记：补上新增共轭定义、表格说明和非凸分裂文献引用的颜色；撤掉与旧式相同的下降引理公式及模型中保留公式主体的整块蓝色；保留图中发生变化的变量标签和阈值标签标蓝，未改曲线和刻度保持原色。没有修改论文语义正文。

- 本次发现的待处理项：`related_work.tex` 中约束提升式仍使用 `\\notag`，正文也未定义 `f(\\bw)`,`h(\\bw)`,`\\bc(\\bw)`；但 `revision/response_letter.tex` 声称该式编号为 (1.2) 并已给出定义。这是正文与回稿信不一致，按用户要求仅报告，未改正文。另有 Reviewer 1 关于精简或移置常规预备知识的建议；当前稿将预备知识并入模型章，但仍保留 proximal mapping 性质和下降引理等陈述，是否足以回应需单独判断。


## 2026-10-21 本轮标色核对

- 按用户最新范围，对照固定提交版 `paper/DualHash_submission.pdf` 的文本导出，完成 `numerical_experiments.tex` 从 §6.2 到第六章末尾的精细标蓝：保留内容为黑色，实质变化按词句标蓝；整句新增的原目标结构误差说明保留整句蓝色。去除该范围的红色命令和未变化图号的蓝色。去除颜色包装后，文字、公式和表格数据与本轮标色前一致。
- 第五章末尾原目标复杂度保证小节按用户要求统一为蓝色，内容不变。第五章两条随机梯度假设本轮保持原文；其适用域量词与开头迭代区域说明的关系仍待作者决定。
- 前序最小修订已补充期望意义下的近驻点限定、初始化及部分文字标色；修复了写入颜色命令时产生的转义错误。量化误差图中的 DTSH 16-bit mAP 先前已按主表对齐至 0.6590，本轮仅将该点及数值的修订标记由红色换成蓝色；此对齐不代表已核验原始实验记录。
- 回稿信仍在核对阶段，尚未写入本轮更新，不能视为已完成。一般 W 类的量化误差界范围、实验敏感度百分比等审阅待办亦未在本轮标色中处理。
- 验证：独立目录 `/tmp/dualhash-response-work-20261021/paper` 的 `latexmk -pdf -interaction=nonstopmode -halt-on-error` 编译通过，无未定义引用、未定义命令或重复标签警告；保留已有排版警告，已更新 `paper/main.pdf`。

- 2026-10-21 按用户后续指定恢复提交版文字：§6.4 第（1）项末句、第（2）项正文、第（3）项及其高位/低位两段，以及 §6.5 参数敏感度解释段。恢复内容不标色；保留未被要求恢复的参数网格与其修订标色。此记录覆盖前条相应段落的标色状态。

- 2026-10-21 用户明确收紧实验修订范围：除实质性错误外保留原稿，不因措辞风格重写结果分析。已恢复 §6.2、§6.3 的原稿结果叙述及黑色显示；§6.3 首句按固定提交版指向 Tables 6.1、6.2，使用实际表格标签。表格数值与图片保持本轮恢复前的状态。

- 2026-10-21 按用户最新指令：恢复 §6.4 第（1）项原稿标题与正文，撤销该项的改写标色；按审稿人要求将 `helps produce` 写为 `helps \blue{to} produce`。图 6.2 的四个子图说明和两处纵轴标签统一为 P@topK、P@r=2，修订指标文字标蓝；仅覆盖标签区域，底层图像数据保持相同。ELU 原文按用户要求保留，不将其表述问题归为审稿人意见。

- 2026-10-21 按用户最新批注完成局部恢复与去标色：摘要 competitive、数据集措辞、验证集说明、敏感性参数和实验附录修订保留为黑色；比较表撤下 DSARH、MambaHash、CRH 三行及其专属注释，相关文献讨论保留；恢复 §6.4 第（2）项原题和结论应用展望，展望中保留审稿人要求的拼写、标点修正。两张图撤销修订专用蓝色，DTSH 点恢复原曲线配色，数值仍为 0.6590；未核验原始实验记录。引言和结论实验总结以用户同期编辑为准，当前均未保留新增的数据集限定或 32-bit 例外。回稿信仅同步恢复展望对应的两条回复。正文及回稿信 latexmk 编译通过，无未定义引用或 LaTeX 错误；已有排版警告保留。

- 2026-10-25 审稿意见逐字核对修正：`revision/response_letter.tex` 补入 Reviewer 2 General comments 第 2 条及对应回复，现有 Reviewer 2 的 88 条意见均有对应条目；恢复三条意见中 9 处条件下标及 3 处标点原貌，修正分类标题和文献编号来源说明。按用户要求仅新增或修正部分标红；删除位置标红邻接保留字符。编译成功，生成 30 页 PDF，无未定义引用或 LaTeX 错误；5 处行溢出警告仍在，最大 9.12pt。本次未修改论文正文。
