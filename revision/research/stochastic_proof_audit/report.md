# 随机证明局部核验与修正

核验范围：原稿 (C.7)、Lemma D.1、(D.7)、(D.15) 及其对 Theorems 5.2–5.3 的影响。完整推导写入 revision.tex 的内部 Appendices C–D。此记录不宣称原稿所有理论问题均已解决。

## 已确认结果

1. **(4.8) 的批次下标**：用户确认两个梯度均使用当前批次。已将 methodology.tex 中活动公式的旧点梯度批次从 J_{k-1} 修正为 J_k。这是用户确认的符号笔误；关于误写公式退化为普通批次梯度的观察不适用于确认后的算法。
2. **(C.7) 的条件期望**：先用历史条件下的无偏性与方差界，再对逐路径 Lyapunov 不等式取全期望，保留正文 (5.15)。Ψ_m(Q^k) 包含下一轮状态，不能视作当前采样前信息流可测。
3. **Lemma D.1 的缺项**：当前批次修正后，联合均方光滑性给出 Delta_x^{k+1}+Delta_B^{k+1}。模型中 grad_x F_i 确实依赖 b_i，因此后者没有自动消失的依据。引理应引用联合均方光滑性假设。
4. **(D.7) 的放缩**：无需对有不定符号的差分项乘系数后比较。直接给误差递推加正权重，再与 Lyapunov 下降式相加，可得到修正式。
5. **(D.2)/(D.9)/(D.15) 的关联笔误**：(D.2) 删除来自 StoM 的多余项 -(C4-K6)Delta_x^k；(D.9) 使用 StoRM 的 S3；x 梯度恒等式中的误差符号为负，但平方范数上界不变；(D.15) 第二个分母删除 S4。

## 备选充分修正：随 T 取值的 tau

固定 horizon T，令 delta=1/6，eta_T=1/(16 L_F T^{1/3})，tau_T=eta_T/2，rho_T=16 eta_T² L_F²/b，b>=1 固定，b1=ceil(c_b T^{1/3})。令 q_k=E||e^k||²、V_k=EΨ_k+(eta_T/rho_T)q_k。

两个增量项均被控制后，有

V_{k+1}-V_k <= -(C_B/2)E Delta_B^{k+1} - C_B E Delta_B^k - [1/(4 eta_T)]E Delta_x^{k+1} - (eta_T/2)q_k + 2 eta_T rho_T sigma²/b.

令 M_T=max{2S1/C_B,S2/C_B,4 eta_T S3,4/eta_T}=O(T^{1/3})。若存在统一有界的期望初始 gap Delta_T，则

mean stationarity <= M_T/T [Delta_T + eta_T sigma²/(rho_T b1)] + 2 M_T eta_T rho_T sigma²/b = O(T^{-2/3}).

因此，在上述条件下，SFO 复杂度阶为 O(epsilon^{-3})。独立审阅者 review_storm_recursion 与 verify_repaired_bound 均核验了递推、正性、吸收系数及阶数。第二位仅收到自包含修正定理进行独立复核，未依赖第一位的审阅结论。

## Appendix C 备选方案的假设与适用范围

- 联合均方光滑性、无偏有限方差、各轮新批次独立。内部推导明确采用批次内独立有放回抽样。
- 所用期望有限；期望 Lyapunov 下界与初始 gap 对 T 一致。原回复的“几乎处处有界”不能单独推出此条件；正文已明确这是需要陈述的条件，内部附录给出一个充分的统一有界性条件。
- 首个分析状态须有有效的上一轮 B/对偶更新，且梯度估计使用新的初始大批次。内部附录给出固定 x 的 B/对偶预更新加重新编号的具体方案。
- Appendix C 备选方案将 tau 从原文独立于 T 变为 Θ(T^{-1/3})；Appendix D 新增另一条保留固定 tau 的充分修正。两条方案均须明确各自附加条件。
- 原固定 tau、任意初始化的版本尚未由这些证明建立；Appendix D 证明了显式块驻点初始化下的固定 tau 结果。不能从上界残留项推断算法真实误差存在同阶下界。

## 原稿落地状态

- 已写入：revision.tex Comment 3 现在采用保留固定 tau 的简短方案；Appendix D 为其详细证明，Appendix C 保留另一条充分方案。
- 已修正：paper/sections/methodology.tex 中用户确认的当前批次下标。
- paper/sections/convergence.tex 与 supplementaryB.tex 的原定理和旧证明尚未整体替换；当前交付是申诉回应与可复核修正方案，非全文理论修订完成。
- 验证：revision.tex 经 latexmk 编译，完整结果见 revision.pdf。数学复核针对新充分条件下的修正结果，不构成对原稿整体证明的通过判定。


## 最新修正：保留固定 tau 的结构证明

用户要求优先核验原步长范围。重新利用实际二次 B 子问题，而非仅以全局联合光滑界处理 B 增量，可保留修正后的原范围

0 < tau < (sqrt(30)-2)/(13 L_F), tau 与 T 无关。

令 m=gamma/n、r=tau m<2/7、beta_k=B^k-B^{k-1}、w_k=tau(Lambda^k-Lambda^{k-1})-beta_k。利用分量 proximal 映射的 secant slope d∈[0,1]，精确增量动力学为

A_d=[[-r,-1],[r(1-2d),1-d]], f_d=[r,r(2d-1)]。

公共二次型 P_r=[[r,r],[r,1]] 给出 H_{k+1}-H_k <= -r/8||(beta_k,w_k)||²+73r||D(x^{k+1})-D(x^k)||²。因此

Delta_B^{k+1} <= (12/r)(H_k-H_{k+1}) + G Delta_x^{k+1}, G=1753 L_D²。

与误差递推和 Lyapunov 下降组合，W_k=EΨ_k+(eta_T/rho_T)(q_k+48L_F²/(br) E H_k) 满足显式下降式。保持原 eta_T=eta0/(tilde L_F T^{1/3})、rho_T=8κ eta0²/T^{2/3}，只需明确数值系数范围：κ>=max{1,2L_F²(1+G)/(b tilde L_F²)}、eta0<=min{1/2,1/(4sqrtκ)}。

充分初始化为 B1=prox_{h**/m}(D(x1))、Lambda1=m(D(x1)-B1)、B0=B1、Lambda0=Lambda1，再以新的初始大批次计算 D1。该初始化保证 H1=0，满足两项前轮更新恒等式，且对 W 正则项有逐分量显式公式。因此，只要原有期望 gap 条件成立，平均误差 O(T^{-2/3}) 与 SFO O(epsilon^{-3}) 成立，tau 无需依赖 T。

### 适用范围的精确区分

- 这是固定 tau 的充分证明，不是对“只修笔误即可恢复所有原条件”的确认。
- 新增：明确的块驻点初始化、迭代映射 Lipschitz 条件的使用、参数常数范围及增量二次型论证。
- 保留：tau 的修正后原区间及与 T 无关的性质；eta、rho、b1、后续 batch 的原有阶数。
- 固定 tau 的常数可依赖 m=gamma/n、L_D 等问题参数；不声称其与 n 无关。
- 一般初始化时，本证明上界会多出 H1/(rho T) 类型项。它不是算法误差下界，不能据此断言所有任意初始化的固定 tau 方法必然更慢。
- 正式理论正文尚未整体替换；Comment 3 和内部附录已给出可复核的修订结果。


固定 tau 独立复核结果：review_storm_recursion 和 verify_repaired_bound 对 Appendix D 自包含推导均给出充分性通过。已落实 u_k 记号统一、期望 gap 的显式定义，并展开公共二次型的凸性步骤。公共矩阵、输入项常数、STORM 合并和速率阶均获核验；未声称任意初始化的必要性结论。
