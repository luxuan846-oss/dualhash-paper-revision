# 回稿信写作规范（SIIMS 强修订）

更新日期：2026-10-25（同日补充 R2 §2.3–2.4 的回答原则）；2026-10-27 补充按意见类型定回法与篇幅。本规范由 Reviewer 1 的逐条修订讨论整理而成，适用于 `revision/response_letter.tex` 的全部回复，后续 Reviewer 2 与首页、目录、总述按此执行。参照材料：MoSSP rebuttal（`2_MoSSP/review/rebuttal_prograss.md`，被接收）与 DualHash NeurIPS 2025 rebuttal（`revision/materal/DualHash_ ... _ OpenReview.pdf`，被拒）。

## 〇、导师格式要求（2026-10-26，崔老师批注，优先于下文）

参照高莹、Ng、崔老师的 SIIMS 回信（Blind HSI–MSI fusion）：

- 结构：标题块 “Response to the editors and referees' reports on 〈题目〉 by 〈作者〉” → 一段 “Dear editors and referees:” 和主要变化 → “Response to referee \#k” → 审稿人总评原文斜体（不单独回复）→ “Comments:” → Q1、Q2…（斜体原话，悬挂式 **Response:**）→ 结尾致谢与参考文献。源码用 `qalist` 环境：`\item[Q1.] {\itshape …}` 与 `\item[Response:] …`。
- 不设目录，不自拟章节标题，不按主题重组。保持审稿人原顺序；审稿人自己的小节标题照搬（R2：General comments，1 Abstract/Introduction … 10 Appendix D）。
- 回信内部引用写 “our response to Q2”，不用 “Section X” 或链接。
- 首页与主要变化写清哪些是提交版已有的（提交版 (3.1)、(P)、(D)、(4.2)、Lemma 5.2、两个复杂度结论、实验），不要让人觉得都是新的；小改动不说重（例：“slightly adjusted the assumptions”）。
- 单条回复直接回答问题，不重复主要变化里已写的内容；不写夸赞复述（“both reviewers found the experiments comprehensive”）或 “While we agree that the convexity of the conjugate is standard … what is actually novel” 这类开头。
- 引用我们自己的研究问题 (RQ1)(RQ2) 时，写明是我们提出的，避免误认为审稿人的问题。

## 一、定位：期刊回信，不是会议 rebuttal

| 项目 | 会议 rebuttal | 本次期刊回信 |
|---|---|---|
| 修改状态 | we will add | we have added，并给出位置与原文 |
| 核对者 | 审稿人打分后一般不再审终稿 | 同一批审稿人逐条对照新稿 |
| 覆盖范围 | 只答关键问题 | 每条意见都答，含错别字 |
| 目标 | 说服提分 | 让 AE 与审稿人确认问题已解决 |
| future work | 可放开写 | 只用于解释性陈述或明确开放方向 |

本稿曾被判 “cannot be considered established as written”，可信度优先于说服力。任何一句说过头，整封信都会被打折扣。

## 二、颜色约定

- **黑色正文**：我们对修改和贡献的解释。
- **位置**：指向稿件的位置，如 Section 3、Theorem 3.5、页码，含页码的括注整体用 `\blue{(...)}`。用 `\mref`、`\meq` 生成编号，`\mpl{X}` 生成 “p. 11, line 309”，`\mplr{a}{b}` 生成页行范围，不手写编号。
- **蓝色 `\blue{}` / `bluequote`**：逐字摘录修订稿原文，必须与源码一致（宏写法差异可接受，措辞和公式差异不可）。为回信重新整理的公式（如三个问题并列）不标蓝。
- **回信内部引用**：写 “our response to Q2 (of Referee \#1)”，不用链接与回信章节号（2026-10-26 导师要求）。
- **下划线 `\mk{}`**：蓝色引文中局部修改的那一处（对应稿件源码里的 `\blue{…}`）加下划线；整段新写的引文不加。只用于 Reviewer 2（Reviewer 1 的引文都是新结果）。
- **红色**：回信中不用。回复正文不再用 `\chg{}` 标红（2026-10-25 起 `\chg` 已定义为普通黑色），新写的文字直接写黑色。
- 审稿人原话一律斜体；我们的回复正体。

## 三、每条回复的结构

1. **首句作答；致谢只在第一条和关键意见处出现**。需要致谢时点明这条意见带来的收获，并与答案写在同一处，如 “Thank you for this key question, which led us to … : yes, …”。避免只写 “We thank the reviewer for this comment.”
2. **在审稿人首次提出的位置完整回答一次**。后面重复的意见用一两句加整句链接指回去。同一结果不讲两遍以上。
3. **先给结论，再给依据**。先一句话说明结论，再接蓝色引文；引文后最多一句解释。
4. **引原文要少而轻**。关键定义、例子、命题（例：Def 2.2、Ex 3.2、Prop 3.3、Def 3.4、Thm 3.5）可引原文，配一句引出、一句解释。证明类回复按审稿人的每一点配一处修订原文（含公式块），不零散摘引多个片段；与问题无关的公式不贴。原文前后只加一两句解释。
5. **证明位置**：结果简单、审稿人未质疑的，只给结果位置；审稿人质疑过正确性的（R2 各引理、Thm 3.5），保留证明位置。
6. **认错只说一次**。真错误明确承认；原稿已有但没写透的，写成 “already showed …, but it did not develop this relation further”，不要写 “did not show”。
7. 推导只放回信时，说明理由：“Since this is a standard calculation, the manuscript cites it rather than reproducing the derivation; for completeness, …”

### 补充：回答原则（2026-10-25，R2 §2.3–2.4 修订后确定）

1. **先正面回答审稿人问的问题**。他没问的内容不写（例：D.2 删去 $H_k$ 与 $b_1$ 的说明）。
2. **借回答澄清我们的工作**：每条回答顺带说明修订后的论证实际怎样进行。
3. **采纳的建议**：说明它改进了稿件，如 “we thank the reviewer for this simplification”。
4. **顺着审稿人思路多做的改进**：写出主动、自主的姿态，感谢之后说明我们做得更多，如 “Following this suggestion, we also rechecked the entire StoRM analysis and corrected two further errors”“Rather than only restoring this factor, we restructured the proof”“Building on them, we reorganized …”“Taking this opportunity, we …”。
5. **未照做的建议**（如 R2 Lemma C.2 的顺序、$\beta_k$ 写法）：首句说明没有照做，再给我们的处理方式和理由，不能沉默略过。审稿人给了改写或推荐文献而我们没有照用，同样要交代原因。
6. **先答后证（总分）**：每条回复的首句直接回答审稿人问的那件事——是非题答 Yes 或 No，问是否笔误就答是或否，建议答采纳与否；理由、修改和位置放在后面。不用 “We agree that …”“We also thank the reviewer for noting …”“For your convenience”“We now answer …” 这类不带信息的铺垫句。
7. **写错就直说**：例 “We also apologize for an error in the submitted (4.8): … was written with $\mathcal{J}_{k-1}$, whereas …”，不附加辩解或实现层面的说明。
8. **审稿人的计算**：若他按印刷稿算得正确，明说 “with this printed coefficient the reviewer's value … is correct”，再给改正后的结论。
9. **只有删除的改动**（稿件中无蓝字）：一句文字说明加位置即可。
10. **纯指路条目**（如 “See my comments relative to Appendix C”）：一行指到对应回复，如 “We address this comment in our responses to Q76 and Q77 below.”（2026-10-27 按原报告顺序重排后，每条意见都保留。）
11. **证明类回复的形状**：一句开头（直接说明改了什么、结论是否变化）→ 修订稿原文（含公式块）→ 中间一句解释 → 下一段原文或结论。能用一个公式块说清的，不用文字转述。
12. **分 item，不堆大段**：一条意见含多点时用 [i]/[ii] 列出，每点两三句；避免多段散文，也避免一整段堆在一起。每个 item 的加粗开头是一句完整的话，概括这一点的结论（参照 R1 四项贡献的写法），不写 “Scope of the table.”“DualHash-StoM.” 这类名词标签；正文不重复这句话。
13. **交叉引用看篇幅**：被引用的回复内容多，用整句链接指过去；只有一两行，就把那一两行直接搬进当前回复，不写 “see our response in Section X”。
14. **尊重体现在直接回答和措辞上**：称审稿人为 you（“Following your suggestion”），不用 “the reviewer”；致谢只放在第一条意见和关键处，审稿人的表扬也要回应；避开 standard 这类显得对方不懂的词。致谢之后紧接答案，不让致谢占据首句的位置，如把 “We thank the reviewer for this practical question. This requirement can be met as follows.” 改为 “Yes, this requirement can be met for every $\varepsilon>0$ by taking …”。

### 补充：按意见类型定回法与篇幅（2026-10-27）

目标是便利审稿人、不啰嗦：审稿人用不上的内容不写；需要他核对的内容（改后的式子、位置）直接摆出来。按类型分配篇幅的做法参照高莹 SIIMS 回信（`revision/materal/revision_temp/Response_to_referees_siims.pdf`）；她在不同审稿人处整段复制同一回复、每条都写 insightful，这两点不学。

| 类型 | 审稿人要核对的 | 回法 |
|---|---|---|
| 指出错误或论证缺口 | 改对了没有，结论还成不成立 | 承认一次 → 改后的式子 → 一句影响（如 “the complexity order is unchanged”）→ 位置 |
| 提问 | 答案 | 第一句就回答，再给一两句理由；做不到的直说 |
| 建议 | 采纳了没有 | 采纳的写改了什么、在哪；没采纳的给一句理由 |
| 记号、笔误 | 改了没有 | 一句话加位置；不单写致谢句，用 “As suggested, we have …” 接住审稿人 |

- 新写的回复，同一内容只完整写一次：前面回复已经引过的公式不再整段引用，写 “displayed in our response to Q21”；只有一两行的内容直接搬过来（如 R2 Q39 补上 Example 的数值）。已写进 Summary of the Main Changes 的贡献，单条回复不再复述。
- **改已有回复以小改为主**（2026-10-27 用户要求）：补一句影响，删一两句与问题无关的话；已有的段落、引文和分点不整段删除，也不重写。

## 四、语气与措辞

### 主语
- 讲我们做了什么，用 **We** 开头：We establish / We give / We write。避免 “A relationship holds …” 这种论文口吻。
- 讲结果说了什么，用结果作主语：Theorem 3.5 shows that …

### 对话口吻
- 把关键句放回一对一的对话，称 you：“In response to your question, …” “Following your suggestion, …”；不用 “the reviewer”“For the reviewer's convenience”。
- **不用审稿人的评价词夸自己**：rigorous、novel、significant、first 留给审稿人。用 “the requested stationarity relationship” 代替 “a rigorous stationarity relationship”。
- **借用审稿人的原话描述贡献**：如 R1 的 “what is actually novel”“meaningful guarantees”“the relationship between the primal–dual reformulation and the original objective”。

### 肯定与留余地
- **数学事实写肯定**：if and only if、等式、已证的界、exact criterion。
- **对文献、新颖性、上界含义留余地**：do not directly apply；to our knowledge；often；alone may not；This motivates。
- 上界只能推出“可能”，不能推出“一定”：例如结构误差是 Thm 3.5 的上界项，写 “alone may not remove”，不写 “cannot remove”。
- 反例只说明“可能”：Ex 3.2 支持 “the gap can be positive”，不支持 “positive in general”。与稿件原话保持一致。
- 不处处留余地，分寸以“数学事实肯定、外部判断谨慎”为准。

### 常用句式
- 让步转折：“While we agree that X, …”
- 划定已有结果范围：“While DualHash builds on …, existing complexity results cover simpler settings, such as …, which … need not satisfy.”
- 澄清范围：“We clarify that … In fact, we only require …”
- 肯定前人：“X is a fundamental work … Our contribution lies in showing that …”
- 局限有出处：“… is unavoidable under our current analysis framework: comparable analyses [1, 2] carry the same dependence.”
- future work：“… is an interesting future direction, for which we believe Theorem 3.5 provides a natural criterion.”

### 避免 AI 腔
- 不连用三连形容词；不堆 crucial、notably、furthermore、It is worth noting that。
- 同一句式不在全文反复出现（如 10 处 “The X now reads”）。引改后短句可用 “now reads”；引定理用 “is reproduced below”“states”“becomes”。
- 小标题写成一句有信息量的结论，可接动名词结果状语（…, unifying …；…, providing …），不加 thereby。
- 能一句说清，不写一段。

## 五、术语统一

| 概念 | 统一写法 |
|---|---|
| 正则 | W-type regularizer(s)；方法写 W-type regularized deep hashing；具体一个写 $\ell_1$ W-regularizer |
| 对偶问题 | min–max problem，标签 (NC-L)；审稿人语境用 primal–dual reformulation |
| 原问题 | original problem (P0)；(P0) 定义前写 original problem / original hashing objective |
| 罚模型 | penalty model (P) |
| KKT | KKT point of $\mathcal{L}$；approximate KKT point；KKT residual $r_{\mathcal K}$ |
| 误差 | optimization error；structural error；quantization error $Q_1$ |
| 期望 | total expectation（不用 full expectation） |
| 投稿版 | submitted manuscript（不用 original manuscript） |
| 复杂度 | oracle complexity；SFO calls |
| 驻点关系 | stationarity relationship（不用 optimality relationship） |

编号：投稿版对象写死原编号（Lemma 5.2、Appendix C）；修订稿对象一律用 `\mref`。

问题标签在正文中写成 `Problem~(P0)`、`Problem~(P)`、`Problem~(NC-L)`，与稿件 “Problem~\eqref{…}” 的写法一致；不单独写 “(P)”。公式 `\tag` 与审稿人原话除外。

## 六、与稿件一致性的检查

- 每个 “we have …” 都能在源码找到对应改动；找不到就改回信或改稿件，不能把承诺写成事实。
- 蓝色引文逐字对照源码。
- 涉及范围的说法（W 类整体 vs $\ell_1$ vs compact dual domain）与稿件对应位置一致。
- 不写 first、novel 等无依据的自我评价。
- 有界性、光滑性等论证一律引证明（如 $R_B$ 引理），不用直觉说法（如 “B naturally stays near ±1”）。

## 七、NeurIPS 2025 的教训

- AC 拒稿理由：“not very innovative”。审稿人 Hm6N 要的是 problem-specific insight 和对 $\operatorname{sgn}(\cdot)$ 后二值码质量的保证。
- 修订稿的回应点：Thm 3.5 与量化误差 $Q_1(\mathbf{x})=\lVert\mathcal{D}(\mathbf{x})-\operatorname{sgn}(\mathcal{D}(\mathbf{x}))\rVert_1$ 把二值码质量和原问题驻点保证联系起来，这是 hashing 特有的洞见，应在贡献中前置。
- 当时减分的写法：多处 “first”；有界性靠直觉；对理论问题用实验表格代替。

## 八、协作流程

- 先写逐条回复，最后写首页总述与主要变化（不设目录）。
- 回复正文的改动直接写黑色，不再标红；稿件位置用绿色。
- Codex 与 Claude 不同时改 `response_letter.tex`，避免互相覆盖；IDE 开启保存即编译时，命令行编译可能抢占 aux 文件，失败后重编一次。
- 每次修改后先编译 `paper/main.tex`，再编译回信，确认无未定义引用。
- 回信参考文献直接用稿件的 `paper/refers.bib` 与 `siamplain` 样式（`\bibliography{../paper/refers}`），保证与正文格式一致；`revision_refs.bib` 不再使用。
- 待用户审阅的新改动用 `\rv{}`（红色）临时标出，确认后去掉。
- 稿件中审稿人未提及的部分（如实验），自己发现的小错误可以直接改正，但不标蓝也不标红。
- 不建备份副本（如 `/tmp/*_before_*.tex`），对比和回退用 `git diff`；临时日志当轮删除。

## 九、范例模版

以下原文逐字摘自两份 rebuttal，按用途分类。每段后注明可借鉴之处或需避免之处。期刊回信使用时，把 “we will” 改为 “we have”，并补上稿件位置。

### A. MoSSP（ICML，被接收）：可直接借鉴

**A1. 总开头：致谢 + 引用审稿人的肯定 + 预告结构**

> We sincerely thank Reviewer Vqbz for the thorough and constructive suggestions. We are greatly encouraged by the characterization of our framework as "a novel contribution" as well as the recognition of the importance of our problem. We also thank you for your kind remarks on the clarity and organization of the paper. Below, we address each concern point by point.

借鉴：审稿人的肯定用其原话加引号复述；“novel” 出自审稿人，不是我们自称。

**A2. 单条开头：致谢 + 这条意见带来的收获**

> We appreciate the valuable question regarding the theoretical assumptions, which allows us to clarify their roles and their practical reasonableness.

> We greatly appreciate the valuable comments, which allow us to clarify the positioning of our work and elaborate on our treatment of the $g$-subproblem.

> We thank the reviewer for this constructive suggestion, which allows us to highlight our contribution better.

借鉴：“which allows us to …” 把致谢落到具体收获上，避免空泛的 “Thanks for the comment”。

**A3. 让步转折 + 文献支撑：承认假设更强，说明这是该类方法的共同前提**

> We sincerely appreciate the valuable question, which helped us state our contributions more precisely and rigorously. While we agree that the improved $\mathcal{O}(\varepsilon^{-3})$ complexity result of Algo. 2 relies on the slightly stronger Assump. 3.1, this is precisely the structure exploited by variance reduction (VR) methods [2–4] to achieve improved complexity in nonconvex stochastic optimization. Without Assump. 3.1, [1] establishes an $\Omega(\varepsilon^{-4})$ lower bound for any stochastic first-order method, explaining why VR methods [2–4] generally require this condition to attain lower complexity results.

借鉴：先让步，再用下界和同类文献说明这一前提不可避免。让步后立即给依据。

**A4. 澄清范围：说明假设只需沿迭代成立**

> We appreciate this valuable comment. We clarify that Assump. 2.1 is stated globally for brevity in our paper and is standard in constrained stochastic optimization. In fact, we only require the boundedness along the iterate sequence.

借鉴：“We clarify that … In fact, we only require …” 把质疑的强假设收窄到实际所需。

**A5. 肯定前人，再说明自己的贡献落点**

> Hu et al. (2024) is a fundamental work and, to our knowledge, the first single-loop method for stochastic difference-of-weakly-convex (DWC) optimization via the DME idea. Our contribution lies in showing that, in the nonconvex constrained setting, the DME idea can be incorporated into a penalty framework. This is done while retaining the optimal oracle complexity in our manuscript.

借鉴：“first” 只用于称赞别人；自己的贡献用 “Our contribution lies in showing that …” 限定场景。

**A6. 局限有出处**

> While making $\delta$ explicit highlights the problem-structure-awareness of the bounds, this $\delta^{-2}$ dependence is theoretically unavoidable under our current analysis framework: comparable penalty-based analyses under similar CQ assumptions [1, 2] carry the same implicit dependence.

借鉴：承认局限，同时指出同类分析有相同依赖，局限不显得是我们独有的缺陷。

**A7. 设计选择的理由 + 未来方向**

> Simple implementation motivated our choice of quadratic penalty. While exact penalty methods are theoretically attractive, the penalty threshold is typically unknown a priori, and the nonsmoothness of standard exact penalty functions (e.g., $\ell_1$) introduces additional practical difficulties. […] We appreciate this constructive comment, which helps us identify important future directions.

**A8. future work 的正面写法**

> Extending to more general conditions, such as Hölder smoothness, is an interesting future direction that we believe our framework has the potential to accommodate.

> While our current framework leverages closed-form $\operatorname{prox}_g$, the analysis can indeed accommodate more general cases […]. We thank the reviewer for helping us uncover the practical extension potential of our framework.

期刊调整：只用于解释性陈述或明确开放的方向；若审稿人下一轮可能要求“请证明”，不用 “has the potential to accommodate”。

**A9. 收尾**

> We sincerely appreciate your positive feedback and are greatly encouraged by your willingness to raise the score. We will carefully incorporate the promised revisions into the final version. Thank you again for your time and support.

期刊调整：去掉提分相关措辞，改为 “We hope the revision addresses your concerns, and we would be glad to clarify any further questions.”

### B. DualHash（NeurIPS 2025，被拒）：结构可借鉴，措辞需改

AC 结论：“technically, it was seen as not very innovative, in an area (hashing) that has seen much work in the past”。审稿人 Hm6N 的最终意见：“correct and tedious for a niche problem, but ultimately generic”。

**B1. 回应“机械套用标准工具”**

> We sincerely appreciate this thoughtful feedback, which allows us to highlight our key contributions better. While building upon established optimization principles, our work makes several novel advances specific to deep hashing: […]
> (3) Complexity Novelty: While standard complexity bounds exist for simpler settings (one-block primal-dual or multi-block unconstrained problems), our work provides the first complete analysis for the challenging combination of stochastic, non-convex, non-smooth, multi-block primal-dual optimization through a novel Lyapunov function.

可借鉴：“While standard … exist for simpler settings (A or B), …” 的划界句式；括号点名已有情形；一串形容词概括难点。
需避免：“novel advances”“the first complete analysis”“a novel Lyapunov function”“To the best of our knowledge, this represents the first theoretically-guaranteed algorithm”。这些自我评价没有说服审稿人。

本次改写（R1 贡献 (4)）：

> While DualHash builds on primal–dual splitting and the momentum and STORM estimators, existing complexity results cover simpler settings, such as tractable proximal mappings or weakly convex compositions, which $h\circ\mathcal{D}$ need not satisfy. Our analysis handles this stochastic, nonconvex, nonsmooth, multi-block setting through the closed-form proximal mapping of $h^*$ and Lyapunov functions that control the possible increase of $\mathcal{L}$ under the dual update.

**B2. 有界性：直觉论证（需避免）**

> The dual variable λ remains inherently bounded due to the properties of the proximal operator for h* […]. The hash variable B naturally maintains values around ±1 due to its physical interpretation. For network parameters x, we employ weight decay to ensure objective function coercivity. Pretrained initialization provides additional stability.

问题：“naturally”“physical interpretation”“additional stability” 都不是证明。SIIMS 两位审稿人随后正是在有界性上判定论证不完整。本次一律引用证明：$\lVert\boldsymbol\Lambda^k\rVert\le\lambda\sqrt{nd}$ 与 $\lVert\mathbf{B}^k\rVert\le R_B$ 的引理。

**B3. 对理论问题用实验代替（需避免）**

> Although theoretical guarantees for thresholded binary codes remain challenging, our empirical results […] consistently show superior performance, with 15-20% lower quantization loss than baselines.

问题：审稿人问的是二值码质量的理论保证。本次改为理论回答：$Q_1(\mathbf{x})$ 是网络输出到 $\operatorname{sgn}(\mathcal{D}(\mathbf{x}))$ 的距离，Thm 3.5 由此把二值码质量与原问题驻点保证联系起来。

**B4. 解释为何不用替代方法：结构可借鉴**

> Let us clarify why our dual transformation approach was selected over ADMM or Bregman proximal methods for this specific problem: (1) ADMM is primarily designed for linearly constrained problems, whereas our formulation […] involves nonlinear constraints. […] (2) While Bregman proximal methods can yield closed-form solutions for many non-prox-friendly regularizations [9,10], their successful application requires careful, problem-specific design of the Bregman divergence.

可借鉴：逐个替代方法给出具体的不适用条件，并用 While 先肯定替代方法的优点。
需避免：“To our knowledge, no existing work has demonstrated …” 这类难以核实的否定性断言，改为说明已有结果覆盖的具体情形。

**B5. Author Final Remarks 结构：可借鉴**

> Core contributions (1) Modeling Innovation […] (2) Theoretical Novelty […] (3) Practical Effectiveness […] Main concerns (1) […] (2) […] (3) […] Revisions We will (1) […] (2) […] (3) […].

可借鉴：贡献、主要疑虑、修改三段式，适合首页 summary 与 cover letter 的组织。标题需改成一句话结论，不用 “Innovation”“Novelty” 这类自评词。

### C. 本次期刊回信的单条骨架

```latex
\item <审稿人原话，斜体>
\par\smallskip
\normalfont\upshape\responseLabel
<首句作答：Yes/No, … / We have adopted your suggestion: … / We have kept …, because …>
<（仅第一条或关键意见）Thank you for this <key question>, which <led to …>.>
<一句引出> (<Section/Theorem>, p.~\mpage{<label>}):
\begin{bluequote}
<修订稿原文，逐字>
\end{bluequote}
<最多一句解释，必要时一句留余地的延伸或 future direction>
```

重复的意见：

```latex
\responseLabel <一句结论>, as detailed in \hyperref[<label>]{our response in Section~\ref*{<label>}}.
```
