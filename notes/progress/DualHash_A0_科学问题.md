# 最紧凸松弛的取舍：DualHash 的研究背景、动机与科学问题

**记录日期：** 2026-10-16　**来源：** Booming 研讨，候选 1 定稿（第 8 轮）　**状态：** 出口 A0 已修订，并附出口 A 判断

**用户决定（第 8 轮）：**
- 主线按「最紧凸松弛：换来 Lipschitz 更新，付出量化误差」来写，并在正文中与直接路线（拆分后对 $\mathbf B$ 用 $\operatorname{prox}_h$）正面对比。
- 哈希反例「立方体内不施加二值化力，仍得到最低量化误差」暂不写。

**第 9 轮核对结论（取代下文关于「Lipschitz 更新」的表述）：**
- StoRM 的跟踪不等式 $\Delta_{\mathbf B}^{k+1}\le\frac{12}{r}(H_k-H_{k+1})+G\,\Delta_{\mathbf x}^{k+1}$（`supplementaryB.tex` 707–716 行）用到的是 $\operatorname{prox}_{\tau^{-1}h^*}$ 的逐分量单调性与 1-Lipschitz 性，也就是 $h^*$ 的凸性。
- 误差递推里 $\Delta_{\mathbf B}^{k+1}$ 的权重是 $\eta_T/\rho_T\sim T^{1/3}$，而 $\mathbf B$ 块的下降系数 $C_{\mathbf B}$ 是常数，吸收不了这一项。跟踪不等式把它换成 $\Delta_{\mathbf x}^{k+1}$，后者的下降系数是 $1/(4\eta_T)\sim T^{1/3}$，再加上一个可以望远镜求和的项，才把 $\mathcal O(\varepsilon^{-3})$ 闭合。
- 结论：凸松弛换来的就是复杂度结果，「Lipschitz 更新」是它背后的机制，算不上另一项独立的收益。
- 直接路线：$\operatorname{prox}_{\tau h}$ 在 $0$ 处不连续，这个跟踪不等式不成立。固定 $\tau$ 时现有证明无法推到直接路线；换一种参数选择能否恢复，仍是开放问题。
- StoM 的 $\mathcal O(\varepsilon^{-4})$ 不需要跟踪不等式。$h^*$ 的凸性只在 B、Λ 一步进展里用到（`supplementaryB.tex` 174–176 行）。

- **图表核对：** `results_images/sensitivity_2datasets.pdf` 两个坐标轴实际是 $\eta\in\{10^{-5},10^{-3},10^{-2},1\}$ 和 $\gamma\in\{0.3,3,30,100\}$；而图题和正文（`numerical_experiments.tex` 214–227 行）写的是 $\eta$ 与 $\lambda$。论文里没有任何关于 $\lambda$ 的实验证据，这与 $\boldsymbol\Lambda\equiv0$ 一致。

## 1. 研究背景（C1、C2）

**对象。**

- **哈希目标：** $P_0(\mathbf x)=f(\mathbf x)+h(\mathcal D(\mathbf x))$。
  - $f$ 是光滑的语义损失。
  - $h$ 是非凸非光滑的 W 型正则（`problem_reformulation.tex` 41–187 行）。
  - $\mathcal D$ 满足 Ass (ii) 的光滑性，取值有界：$\lvert\mathcal D_{ij}\rvert\le1$。理论不依赖 tanh。
- **拆分问题 $P$：** 引入辅助码 $\mathbf B$，罚参数 $m=\gamma/n$。
- **处理 $h$ 的两条路线：**
  - 直接路线：对 $\mathbf B$ 用闭式的 $\operatorname{prox}_h$。
  - 对偶路线：通过共轭重构 $\mathcal L$（`methodology.tex` 179–201 行）。等价于把 $h$ 换成它的凸包络 $h^{**}$，也就是最紧的凸松弛。对 W 类有 $h^{**}\equiv0$ 于 $[-1,1]^{n\times d}$（`lem:wtype-conj`）。

**已有结果。**

- **复合问题 $h\circ\mathcal D$。** 已有理论覆盖凸或弱凸的情形（引言 70 行所引文献）。$h$ 非凸时，$h\circ\mathcal D$ 不一定弱凸。
- **非凸非光滑正则的随机近端梯度。** Xu–Jin–Yang（NeurIPS 2019，arXiv 1902.07672）证明：只要 prox 好算，单块在线设定下就有 $\mathcal O(\varepsilon^{-4})$ 与 $\mathcal O(\varepsilon^{-3})$，与凸正则同阶。
- **W 正则的 prox 是闭式的**（ProxQuant；引言 63 行）。
- **非光滑部分只能通过次梯度访问时**（Li–Pan，arXiv 2608.03246）：对弱凸 Lipschitz 函数，确定性方法需要 $\Omega(\varepsilon^{-4})$。这一条只作定位参照。
- **量化误差越低，检索不一定越好。** 这一经验观察已见于 arXiv 1710.11445，以及 DualHash 公开稿（arXiv 2510.18218）。

**为什么现在可以做。**

- 递归动量方差缩减在单块问题上给出不依赖大批量的 $\mathcal O(\varepsilon^{-3})$。
- 非凸 prox 的随机复杂度已经清楚（Xu–Jin–Yang）。因此两条路线的比较可以集中在速率以外的性质上。

## 2. 研究动机（C3、C5）

**看到了什么。**

- **用户本人的切入点：** 输出端的非光滑，可以通过对偶挪进一个 prox 友好的块。
- **研讨中推出的定位：** 拆分以后，两条路线都 prox 友好。对偶多做的一件事，是用最紧的凸松弛 $h^{**}$ 替换 $h$。
  - 收益：辅助块的更新对输出是 Lipschitz 的。
  - 代价：W 正则在立方体内部的作用消失，这部分损失由量化误差计量，而且这个代价是紧的。

**支撑这一判断的计算。**

1. **更新的连续性。**
   - DualHash 的辅助块更新为 $\mathbf B^{k+1}=(1-\tau m)\mathbf B^k+\tau m\,\mathcal D(\mathbf x^{k+1})-\tau\boldsymbol\Lambda^k$，对输出是 Lipschitz 的，常数为 $\tau m$。
   - 对偶步用的 $\operatorname{prox}_{\tau^{-1}h^*}$ 是非扩张的，所以整步更新对所有参数都是 Lipschitz 的。
   - 直接路线做一步近端梯度 $\mathbf B^{k+1}=\operatorname{prox}_{\tau h}(\cdot)$，自变量穿过 $0$ 时，更新跳变 $2\tau\lambda$。

2. **代价：证书。** 由输出饱和不等式 $\lVert\nabla\mathcal D_{ij}\rVert^2\le2L(1-\lvert\mathcal D_{ij}\rvert)$ 可得

$$
\lVert\nabla P_0(\mathbf x)\rVert\le\sqrt{1+(1+4m^2)G_{\mathcal D}^2}\;r_{\mathcal K}+\lambda\sqrt{2nd\,L}\,\sqrt{q_1(\mathbf x)},\qquad q_1(\mathbf x)=\bigl\lVert\mathbf 1-\lvert\mathcal D(\mathbf x)\rvert\bigr\rVert_1 .
$$

3. **代价是紧的。** 取 $n=d=1$、$f\equiv0$、$\mathcal D(x)=\tanh x$。
   - 对任意 $x$，$(x,\tanh x,0)$ 都是 KKT 点。
   - $P_0(x)=\lambda(1-\lvert\tanh x\rvert)$ 没有驻点。
   - $\lambda q(x)\le\lvert P_0'(x)\rvert\le2\lambda q(x)$。

4. **与直接路线对比。**
   - 在 $P$ 的驻点上，凡是 $\lvert\mathcal D_{ij}\rvert\le1-\lambda/m$ 的分量，都有 $m(\mathcal D_{ij}-\mathbf B_{ij})=-\lambda\operatorname{sgn}(\mathcal D_{ij})$，正好等于 W 项的梯度，于是被精确抵消。
   - 只有离 ±1 不到 $\lambda/m$ 的分量留下误差，每个不超过 $\lambda\sqrt{2L\lambda/m}$。
   - 因此直接路线到 $P_0$ 的差距更小，代价是辅助块更新不连续。

**对领域的意义。** 现有工作对非凸正则做共轭重构时，一般不交代损失了什么。本文给出一笔完整的账：
- 保留了什么：接触集（`prop:transfer`）。
- 失去了什么：量化误差，并且是紧的。
- 和直接路线相比，换来了什么：Lipschitz 更新。

对哈希读者，量化误差多了一层优化上的含义：它衡量凸松弛的保证与哈希目标之间的差距。

**切入点来源。** 它挑战默认前提「Fenchel 重构对原问题无损」，这只在凸情形成立。切入点来自优化与成像领域的原始—对偶分裂。

## 3. 科学问题（C4）

**主问题。** 对输出有界、光滑、带非凸 W 正则的哈希目标，用最紧的凸松弛处理辅助码：

1. 相对直接路线，它换来了什么？
2. 相对哈希目标，它付出了什么？这个代价能否精确计量、是否紧？

**已知与未知。**

| 情形 | 结论 | 依据 |
|---|---|---|
| KKT 复杂度 | StoM $\mathcal O(\varepsilon^{-4})$，StoRM $\mathcal O(\varepsilon^{-3})$ | 已证（`convergence.tex` 195、286 行）；假设的适用域与批量上限待修 |
| 精确转移 | KKT 点是 $P$ 的驻点，当且仅当所有 $\lvert b_{ij}\rvert\ge1$ | 已证（`prop:transfer`） |
| 更新的 Lipschitz 性，及直接路线的跳变 | 对偶路线常数为 $\tau m$ 与 $2m$；直接路线跳变 $2\tau\lambda$ | 已推导，待写入 |
| 有界输出下的证书 | $\lVert\nabla P_0\rVert\le C r_{\mathcal K}+\lambda\sqrt{2ndL}\sqrt{q_1}$ | 已推导，待写入 |
| 代价的紧性 | 标量 tanh 实例 | 已推导，待写入 |
| 直接路线到 $P_0$ 的差距 | 只来自离 ±1 不到 $\lambda/m$ 的分量 | 已推导（精确驻点情形），待写入 |
| 直接路线在两块随机设定下的速率 | **未知** | 文中写明不在本文范围 |
| 量化误差能否被理论控制 | **未知** | 复杂度定理不控制 $q$ |

**主难点。** 「Lipschitz 更新」这一收益，在分析上起不起作用：StoRM 的辅助块跟踪引理（`supplementaryB.tex` 674–733 行）是否依赖它。如果依赖，收益就从「性质」升格为「速率的前提」。

## 4. 风险与第一步

**风险。**

1. **审稿人认为「Lipschitz 更新」这一收益偏弱。** 应对：如实写成结构性质，不声称训练效果更好。如果跟踪引理确实依赖它，就把它写成分析上的前提。
2. **正面对比会暴露直接路线差距更小。** 应对：这是取舍本身的内容，按「换来什么、付出什么」写清楚，方法的定位就是可信的。

**第一步（一到两周）。**

1. 核对跟踪引理对辅助块更新 Lipschitz 性的依赖。
2. 在 4.2 节写入四项：证书命题、紧性实例、Lipschitz 与跳变的对比命题、直接路线的差距 Remark。
3. 按问题段改写引言。

**补证清单。**

- 跟踪引理是否依赖 Lipschitz 性。
- 证书常数按 $h$ 的归一化核对。
- 直接路线的差距推广到近似驻点。
- Li–Pan 正文中的下界条件。
- 假设适用域的修改，逐处核对。

## 出口 A：选题判断

- **判断：投入**（限于本轮强修订）。
- **我看到了什么：** 拆分后两条路线都 prox 友好。对偶等于把 $h$ 换成最紧的凸松弛：换来 Lipschitz 更新，付出量化误差，并且这个代价在最小实例上是紧的。
- **为什么现在可以做：** 非凸 prox 的随机复杂度已经清楚（Xu–Jin–Yang 2019），两条路线的比较可以集中在连续性和到原问题的差距上。
- **重要性落点：** 对优化读者是一笔完整的对偶化账目；对哈希读者，是量化误差在优化上的含义。属于甜点区的下沿：内行会认可，但未必惊讶。
- **扎实性（按 SIIMS 计价）：** 证书、紧性实例和对比命题都要写成完整的命题与证明；复杂度定理需要修好假设的适用域与批量上限。
- **最可能的失败方式：** 审稿人认为对偶的收益只是连续性，不足以支撑「新算法」的定位。
- **最小下一步：** 核对跟踪引理，并写出 4.2 节的四项新内容。
- **已用轮数 / 候选数：** 8 / 10，2 / 2。
