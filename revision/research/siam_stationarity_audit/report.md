# Verdict

**C1：Supported with qualification。** 在广义非凸 primal–dual splitting、ADMM 和 augmented-Lagrangian 文献中，stationary/KKT 条件、显式 primal–dual operator 的零点，以及 \(\varepsilon\)-stationarity 都是正式理论目标。对狭义非凸 PDHG，已核验的代表性工作足以支持“已有若干代表性研究”，不足以支持“已有大量工作”。

**C2：Supported。** 核心定理通常保证原问题驻点、KKT 点、operator root 或 \(\varepsilon\)-KKT residual；全局最优且零对偶间隙的 primal–dual pair 不是这些收敛定理的普遍终点，也未被单独作为必要前提证明。

**C3：Not supported as written。** 文献明确区分 KKT system、primal–dual operator inclusion、augmented-Lagrangian criticality 与原目标的 limiting-stationarity。不能把它们统一改写为“标准做法是研究标量 Lagrangian function 的 critical point”。更稳妥的说法是：**非凸 primal–dual 分析通常研究 stationarity/KKT 条件或明确给出的 primal–dual operator 零点。**

“大量工作”建议替换为 **“a substantial body of work, including representative results in SIOPT and SIIMS”**；若只讨论 PDHG，则用 **“several representative works”**。

# Scope distinction

1. **狭义 PDHG/primal–dual splitting。** Clason–Mazurenko–Valkonen 允许非线性算子 \(K\)，在约束资格条件下以 \(0\in H(\hat x,\hat y)\) 为 critical-point system；Theorem 4.1 证明迭代弱收敛到 \(H^{-1}(0)\)。Möllenhoff 等在 SIIMS 中定义 \(0\in\partial E(u)\) 为 critical point，但主定理要求非凸项的 semiconvexity 被强凸项补偿；真正总体非凸情形只给出附加条件下的 subsequential result。因此，PDHG 证据应写“若干代表性工作”，不能把凸 PDHG 文献混入。

2. **广义 primal–dual/ADMM/augmented Lagrangian。** Li–Pong、Hong–Luo–Razaviyayn、Yang–Pong–Chen 和 Zhu–Ng 都显式使用 multiplier/dual variable，并把 convergence target 写成 stationary/KKT 或 \(\varepsilon\)-stationary residual。Boț–Csetnek–Nguyen 进一步说明：KKT 点只有在额外的 injectivity 或 convexity 条件下，才可等价为原问题的 critical point。这一组文献支持广义方法论依据，不等同于狭义 PDHG 证据。

# Evidence table

| Paper | Journal and year | Class | Algorithm family | Source of nonconvexity | Exact convergence target | Definition/equation | Main theorem | Original/transformed formulation | Zero-duality-gap role | Supports for appeal | Limitation |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Clason, Mazurenko, Valkonen, “Acceleration and Global Convergence of a First-Order Primal-Dual Method for Nonconvex Problems” | SIOPT 29(1), 2019, 933–963, DOI 10.1137/18M1170194 | A | nonlinear PDHG | nonlinear \(K\) | root of \(H\), critical point; Theorem 4.1 weak convergence | Eq. (2.1), PDF pp. 3–4 | Thm. 4.1, PDF pp. 18–20 | original \(P\) and saddle \(S\), with operator system | not part of theorem; no separate zero-gap result | direct strict-PDHG precedent for stationarity/operator-zero analysis | local assumptions, constraint qualification, and operator-root formulation |
| Li, Pong, “Global Convergence of Splitting Methods for Nonconvex Composite Optimization” | SIOPT 25(4), 2015, 2434–2460, DOI 10.1137/140998135 | A | proximal ADMM | nonconvex \(h\), \(P\), and composite \(P\circ M\) | stationary point of original model | Eq. (4), PDF p. 4 | Thm. 1, PDF pp. 6–7; Thm. 3, p. 13 | ADMM split \(y=Mx\), then original model | not assumed or certified as global zero-gap pair | direct ADMM precedent for stationarity of original problem | surjective \(M\), sufficiently large penalty, cluster-point/boundedness assumptions |
| Hong, Luo, Razaviyayn, “Convergence Analysis of ADMM for a Family of Nonconvex Problems” | SIOPT 26(1), 2016, 337–364, DOI 10.1137/140990309 | A | ADMM | possibly nonconvex smooth block functions \(g_k\), constrained model | stationary solution; proximal-gradient residual | problem (1.1), PDF pp. 1–2 | Thm. 2.4, PDF p. 9; Thm. 2.5, p. 13 | consensus/sharing reformulation | paper explicitly says nonconvexity makes primal/dual gap estimates difficult; uses augmented-Lagrangian decrease | strong evidence that stationarity, not global optimality, is the operative target | special consensus/sharing structure and large penalties |
| Yang, Pong, Chen, “ADMM for a Class of Nonconvex and Nonsmooth Problems…” | SIIMS 10(1), 2017, 74–110, DOI 10.1137/15M1027528 | A | three-block ADMM | nonconvex, nonsmooth, non-Lipschitz terms | stationary point of original model | Eq. (2.2), PDF pp. 5–6 | Thm. 4.3, PDF p. 13; Thm. 4.4, p. 16 | original model (1.1), split model (3.1) | not a stated target or assumption | direct SIIMS imaging precedent; cluster-point and whole-sequence stationarity | boundedness, penalty threshold, KL condition for whole sequence |
| Zhu, Ng, “An Inexact Majorized Proximal ADMM for Diffusion Tensors” | SIIMS 17(3), 2024, 1795–1819, DOI 10.1137/23M1607015 | A | inexact majorized proximal ADMM | nonconvex nonsmooth tensor model | \(\varepsilon\)-stationary solution with multiplier; \(O(\varepsilon^{-4})\) iterations | Def. 2.1, journal p. 1800, Eqs. (2.5a–c) | Thm. 2.10, journal p. 1807 | approximate penalized problem (2.6), residual transferred to (2.3) | no zero-gap theorem | recent SIIMS complexity example with explicit stationarity residual | model-specific assumptions and inexact-subproblem conditions |
| Boț, Csetnek, Nguyen, “A Proximal Minimization Algorithm for Structured Nonconvex and Nonsmooth Problems” | SIOPT 29(2), 2019, 1300–1328, DOI 10.1137/18M1190689 | A | full splitting with augmented Lagrangian and multiplier | \(F,G,H\) need not be convex | KKT point; criticality of original problem only under extra conditions | problem (1.1), Remark 6, PDF p. 15 | Thm. 9, PDF pp. 15–16; Thm. 11 | split problem with \(z=Ax\) | no global zero-gap target | sharp evidence that KKT/operator stationarity and original criticality are distinct | injectivity/convexity needed for equivalence |

**B/C 分类。** Möllenhoff–Strekalovskiy–Moeller–Cremers（SIIMS 8, 2015, DOI 10.1137/140976601）定义 critical point（Def. 2.4, PDF p. 7），但主定理的强凸补偿使总体能量强凸（Thm. 2.8, p. 9）；真正总体非凸时仅有附加条件下的 Prop. 2.12（p. 14），故为 B。Chen–Cui–Han [12] 是 weakly convex + 两个 strongly convex block 的特殊三块 ADMM；Thm. 3.7、4.4、5.4 目标是 augmented-Lagrangian criticality，支持有限。Driggs 等 [20] 是 stochastic PALM，无 dual/multiplier，属于 B。Pock–Sabach [45] 的 iPALM 是 alternating minimization，无 dual，属于 B。Chambolle 等 [9]、Chouzenoux 等 [13]、Esser–Zhang–Chan [22] 处理凸 primal–dual/PHDG，属于 C，不能证明当前非凸陈述。

# Manuscript-reference audit

- **[12] Chen–Cui–Han：B，部分支持。** 有非凸 weakly-convex ADMM 与 criticality/rate，但结构依赖两 strongly-convex blocks，不支持一般非凸 primal–dual 或 PDHG 的“普遍性”。
- **[15] Clason–Mazurenko–Valkonen：A，直接支持。** Eq. (2.1) 是 primal–dual operator root；Thm. 4.1 的目标是 \(H^{-1}(0)\)，不是 scalar Lagrangian 的 limiting-subdifferential criticality。
- **[20] Driggs 等：B。** 非凸 nonsmooth stochastic PALM 的 stationarity/convergence 依据，但没有 dual/multiplier 变量。
- **[45] Pock–Sabach：B。** 非凸 nonsmooth critical-point 先例，但不是 primal–dual 证据。
- **[9] Chambolle 等：C。** stochastic PDHG 的凸结果；不能支持非凸 stationarity 陈述。
- **[13] Chouzenoux 等：C。** convex primal–dual under adjoint mismatch；与当前非凸性不对应。
- **[22] Esser–Zhang–Chan：C。** convex first-order primal–dual framework；不能作为非凸证据。

# Editorial-board author cross-reference

- Radu Ioan Boț：SIOPT Editor-in-Chief；Boț–Csetnek–Nguyen 2019 的 Thm. 9/Remark 6 提供 KKT 与原问题 criticality 的条件性区分。
- Guoyin Li：SIOPT Associate Editor；Li–Pong 2015 是直接 ADMM-stationarity 证据。
- Ting Kei Pong：SIOPT Associate Editor；Li–Pong 2015 与 Yang–Pong–Chen 2017 是核心方法论证据。
- Meisam Razaviyayn：SIOPT Associate Editor；Hong–Luo–Razaviyayn 2016 是非凸 ADMM stationarity 证据。
- Michael K. Ng：SIIMS Associate Editor；Zhu–Ng 2024 是近期 SIIMS \(\varepsilon\)-stationarity/复杂度实例。

编委身份只用于定位文献，不代表期刊政策或期刊背书。用户提到的“2026 年 Guoyin 和 Ng 的文章”无法唯一对应到一篇同时满足本任务范围的 SIOPT/SIIMS 文献；检索到的 2026 年 Ng 相关 SIIMS 非凸成像论文未核实为 primal–dual/ADMM stationarity 论文，因此没有纳入核心证据。若指向特定题目，应在后续提供题目或 DOI。

# Recommended wording

**版本 1：申诉信。**

> In nonconvex primal–dual and splitting optimization, a standard theoretical target is a stationarity or KKT condition, or the zero of an explicitly defined primal–dual operator. Representative results in SIOPT and SIIMS establish such guarantees for nonlinear PDHG, ADMM, and augmented-Lagrangian variants under their stated assumptions. These theorems generally do not target a globally optimal zero-duality-gap primal–dual pair, and they do not identify every stationarity notion with a critical point of the scalar Lagrangian. For DualHash, the remaining task is therefore to prove the stationarity-transfer relation between the transformed primal–dual system and the original W-regularized objective.

**版本 2：Related Work。**

> Representative nonconvex PDHG, ADMM, and augmented-Lagrangian analyses use stationarity, KKT conditions, primal–dual operator zeros, or \(\varepsilon\)-stationarity as their convergence targets. These notions are formulation-specific and should not be conflated with global zero-duality-gap optimality or with scalar-Lagrangian criticality without an additional equivalence proof.

# What the literature does not establish for DualHash

1. 先例不能自动证明 DualHash 的 transformed stationary point 对应原始 W-regularized objective 的 stationary point。
2. 先例不能替代新增的 stationarity-transfer proposition 及其 contact/Fenchel–Young 条件。
3. 先例不能证明 DualHash 自身的 stochastic complexity proof 正确。

# Core references

最终核心集为 5 篇：2 篇 SIOPT 的非凸 ADMM/PDHG，2 篇 SIIMS 的非凸 ADMM，另加 1 篇 SIIMS 的近期 \(\varepsilon\)-stationarity complexity 结果。核心 BibTeX 见 `core_references.bib`。

