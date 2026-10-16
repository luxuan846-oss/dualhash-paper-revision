# 请帮我提炼 DualHash 强修订稿的核心科学问题

**日期：** 2026-10-16　**目标期刊：** SIAM Journal on Imaging Sciences（SIIMS）　**状态：** 已获强修订再提交机会

**仓库：** [luxuan846-oss/dualhash-paper-revision](https://github.com/luxuan846-oss/dualhash-paper-revision)，分支 `master`

这份材料整理了我和另一个助手一整天的讨论。讨论中得到了一些新结果，也有不少走不通的方向。请在这些基础上，帮我提炼一个适合改文章用的核心科学问题，并给出相应的引言写法。

---

## 一、我要什么

1. **一个核心科学问题。** 要让读者有「还能这么想」或者「为什么我没想到」的感觉。不要求推翻整个领域，但不能是读者一眼就能猜到答案的那种。
2. **按这个问题改写的引言片段**，包括问题段和贡献列表，英文，SIIMS 风格。
3. **两类读者各自能带走的见解**：
   - 做哈希、成像的读者：我们对这个领域带来了什么新认识；
   - 做非凸、随机、分块、原始—对偶优化的读者：有什么可以迁移的理论见解。
4. 可以给两到三个候选，并说明你推荐哪一个、为什么。

---

## 二、硬约束（请严格遵守）

### 1. 叙事逻辑必须保持我原文的顺序

我的引言原本就按下面四步写（`paper/sections/introduction.tex`）：

1. 别人通常怎么做哈希，怎么处理这类优化问题。
2. 现有方法在这个问题上做不好，或者做不了。
3. **于是我们想到用共轭对偶来做，发现确实可行，并给出了复杂度结果。**
4. 随之而来的问题（这一步是本轮要强化的）。

**最重要的一条：共轭对偶 / 凸重构是我们提出的方法，是贡献本身。** 它不能写成 related work 里的已有工具，也不能写成「Fenchel duality offers a way around this obstacle」这种把它当现成手段的句子。叙事必须是「motivated by……，我们提出新算法」。上一个助手的草稿就犯了这个错误，读起来很死板。

### 2. 不要给审稿人递刀

这是投稿论文，不是技术报告。下面这些内容我们内部知道（见第五节），但**不要写进引言，也不要写进主要叙事**：

- 「凸化把 W 的尖点拍平」「凸包络在 $[-1,1]$ 上为零」这类听起来像自我否定的说法；
- 对偶变量在某些初始化下恒为零；
- 和「拆分后直接对 W 用近端映射」那条路线的正面比较；
- 任何「我们的方法其实没有用到 W」之类的暗示。

局限只在定理需要交代条件的地方，按常规写法处理。

### 3. 不用管的事

- **实验和保真度先不管。** 实验可以改，tanh 也可以不用，叙述可以调整，这些都不重要。
- **初始化可以改。** 现在的初始化 $\mathbf B^1=\mathcal D(\mathbf x^1)$、$\boldsymbol\Lambda^1=0$ 是为证明加上的，提交版里没有。
- **不要一直揪着漏洞去修补。** 修改机会宝贵，修补既修不完，也写不漂亮。重点是把文章写得漂亮。

### 4. 唯一的底线

**不写不成立的结论。** 第四节列出了几处原文措辞已经确定要改。

---

## 三、论文现状

### 模型

- 哈希目标（原文式 (P0)）：$\min_{\mathbf x}\ \frac1n\sum_i f_i(\mathbf x)+\sum_i h(\mathcal D_i(\mathbf x))$。
  - $f$ 是光滑的语义损失；
  - $h$ 是非凸、非光滑的 W 型正则，例如 $\lVert\lvert\mathbf z\rvert-\mathbf 1\rVert_1$；
  - $\mathcal D$ 是网络输出。
- 难点：
  - $h\circ\mathcal D$ 没有好算的 prox；
  - $h$ 非凸，所以 $h\circ\mathcal D$ 一般不是弱凸的，复合优化的已有方法用不上；
  - 已有的非凸 $h$ 结果只有渐近收敛（Bolte 2018），或者只在确定性设定下（Yao et al.、Zhang et al.）。

### 方法

- 引入辅助码 $\mathbf B$，得到罚问题 (P)。
- 用 Fenchel 共轭写成 min–max 问题 $\mathcal L$。
- 算法 DualHash 交替更新三块：$\mathbf x$ 做随机步，$\mathbf B$ 做梯度步，$\boldsymbol\Lambda$ 做 $h^*$ 的近端步（闭式，凸）。
- 两个实例：DualHash-StoM（动量）和 DualHash-StoRM（递归动量方差缩减）。

### 已有的理论结果

| 结果 | 位置 | 内容 |
|---|---|---|
| `lem:wtype-conj` | `problem_reformulation.tex` 70 行 | W 型正则的共轭和凸包络 |
| `prop:zerogap`、`ex:gap` | `methodology.tex` 234–251 行 | 零对偶间隙推出 KKT；非凸情形下对偶间隙可以为正 |
| `prop:transfer` | `methodology.tex` 265–274 行 | KKT 点是 (P) 的驻点，当且仅当所有 $\lvert b_{ij}\rvert\ge1$ |
| StoM 复杂度 | `convergence.tex` 195 行 | 找到 ε-KKT 点需要 $\mathcal O(\varepsilon^{-4})$ 次 SFO |
| StoRM 复杂度 | `convergence.tex` 286 行 | 找到 ε-KKT 点需要 $\mathcal O(\varepsilon^{-3})$ 次 SFO |
| 辅助块跟踪不等式 | `supplementaryB.tex` 707–716 行 | 见第四节 |

### 审稿意见要点

- **Reviewer 1：** 对偶 / 变换后的保证，对原来的非凸 W 目标到底有什么意义？
- **Reviewer 2：** 主要质疑证明的细节；明确肯定了实验部分。
- **编辑：** 允许强修订再提交。

---

## 四、今天讨论得到的新东西（可以用）

### 1. 凸性在复杂度证明里实打实起作用（技术核心）

- StoRM 的误差递推里，辅助块的变化 $\Delta_{\mathbf B}^{k+1}$ 前面的权重是 $\eta_T/\rho_T\sim T^{1/3}$。$\mathbf B$ 块自身的下降系数是常数，吸收不了这一项。
- 跟踪不等式

  $$
  \Delta_{\mathbf B}^{k+1}\le\frac{12}{r}\bigl(H_k-H_{k+1}\bigr)+G\,\Delta_{\mathbf x}^{k+1}
  $$

  把它换成 $\mathbf x$ 的变化。后者的下降系数是 $1/(4\eta_T)\sim T^{1/3}$，正好吸收得掉，递推在固定步长 $\tau$ 下闭合。
- 这一步用到 $\operatorname{prox}_{h^*}$ 的逐分量单调性和 1-Lipschitz 性，也就是 $h^*$ 的凸性。非凸的 prox 会跳变，这个论证就走不通。
- **结论：共轭带来的凸性，是随机多块方差缩减复杂度成立的原因。** 这一机制可以迁移到「随机主块 + 确定性辅助块」的一般结构。

### 2. 证书：量化误差控制原目标的驻点性（新命题，已推导，未写入）

- **输出饱和引理：** 若 $\mathcal D_{ij}$ 是 $L$-光滑的，且 $\lvert\mathcal D_{ij}\rvert\le1$，则 $\lVert\nabla\mathcal D_{ij}\rVert^2\le2L(1-\lvert\mathcal D_{ij}\rvert)$。证明只有一行：用下降引理。
- **证书：**

  $$
  \lVert\nabla P_0(\mathbf x)\rVert\le\sqrt{1+(1+4m^2)G_{\mathcal D}^2}\;r_{\mathcal K}+\lambda\sqrt{2nd\,L}\,\sqrt{\bigl\lVert\mathbf 1-\lvert\mathcal D(\mathbf x)\rvert\bigr\rVert_1}
  $$

  其中 $r_{\mathcal K}$ 是 KKT 残差，第二项是量化误差。
- **推论：** 与复杂度定理合起来，$\mathbb E\lVert\nabla P_0(\mathbf x^R)\rVert\le C\varepsilon+(\text{量化误差项})$。
- **可以讲的意义：** 哈希领域一直当作经验指标的量化误差，同时是原始非凸目标驻点性的证书。这正面回答了 R1。
- **注意：**
  - 这是充分条件，不是必要条件；
  - 常数要按论文的归一化重算；
  - 只能写「被量化误差控制」，不能写「等于」。

### 3. 原文里确定要改的措辞

- 引言 87 行的「stochastic critical points of Problem (P)」改为「ε-KKT points of the Lagrangian」。
- 「matching the best-known lower bound」改为「与下界的 ε 阶数相同」。
- 6.4 节里「对偶的凸性带来更低的量化误差」这种因果归因要改，可以改成用证书来解读。
- 敏感性分析图的坐标轴实际是 $\eta$ 和 $\gamma$，正文写成了 $\lambda$，要改。

---

## 五、内部知情、不要写进论文的事实

列出来只是为了让你避免说过头的话：

1. **凸包络 $h^{**}$ 在 $[-1,1]$ 上为零。** 因此当输出落在立方体里时，KKT 点等价于 $\mathbf B=\mathcal D(\mathbf x)$、$\boldsymbol\Lambda=0$、$\nabla f(\mathbf x)=0$，此时复杂度保证的只是语义损失的驻点。这个结论和初始化无关。
2. **一维例子。** 取 $f\equiv0$、$\mathcal D=\tanh$：整条实轴都是 KKT 点，而 $P_0$ 一个驻点都没有。
3. **对偶变量恒为零。** 在现在的初始化和理论步长下，$\boldsymbol\Lambda$ 恒为零。输出可以越出立方体（例如不用 tanh）时，对偶块才起作用。
4. **直接路线到 $P_0$ 的差距更小。** 拆分后直接用 $\operatorname{prox}_h$ 时，W 梯度在 $\lvert\mathcal D_{ij}\rvert\le1-\lambda/m$ 的分量上被精确抵消；代价是更新不连续，StoRM 的跟踪论证会失效。
5. **单块情形下凸性对速率不是必需的。** Xu–Jin–Yang（NeurIPS 2019）证明：非凸 prox 在单块情形下也有 $\mathcal O(\varepsilon^{-4})$ 和 $\mathcal O(\varepsilon^{-3})$。所以「凸性带来最优复杂度」只能限定在我们这种两块、固定步长的方差缩减情形里说。

---

## 六、讨论过、不采用的方向（附理由）

| 方向 | 不采用的理由 |
|---|---|
| 用「量化误差应该被最小化还是被计价」作主线 | 经验上的一半早有人说过（arXiv 1710.11445，2017）；提交版也已经写过低位宽下的取舍 |
| 用「拆分问题 (P) 与 $P_0$ 的残差差距」作主线 | 矛头指向我们自己的中间问题，会引来「为什么不直接解 (P)」 |
| 对照 Li–Pan（arXiv 2608.03246）的非光滑下界，讲「我们只付光滑问题的价格」 | 度量不同，只能用来定位，构不成裁决；最多放进一条 Remark |
| 「W 正则在立方体内部的二值化作用」 | 需要新算法和新实验，留作下一篇 |
| 「凸性带来 Lipschitz 更新」作为独立卖点 | 它只是复杂度成立的机制，不单列 |

---

## 七、上一个助手的草稿（供参考，我不满意）

问题段的草稿：

> Fenchel duality offers a way around this obstacle. Once the network output is decoupled by an auxiliary code, the W-type regularizer can be handled through its conjugate, and the resulting dual block is convex with closed-form proximal updates. But for a nonconvex regularizer, duality is not free: the duality gap can be strictly positive (Example~\ref{ex:gap}), so a method that solves the dual reformulation may, in principle, be solving a different problem. This leads to two questions. *Can stochastic primal–dual methods be guaranteed to work at all in this nonconvex, multi-block setting? And if so, does solving the reformulation actually solve the hashing problem?* We give an affirmative answer to both, and the second answer comes from an unexpected place. ... how far a KKT point is from stationarity for the original problem is controlled by the *quantization error*, the very quantity that deep hashing methods already monitor. A familiar heuristic diagnostic thus turns out to be a certificate of stationarity for the original nonconvex objective.

**我不满意的地方：**

1. 把共轭对偶写成了现成工具，和「我们提出新方法」的逻辑冲突；
2. 整体写法死板，「哇」的感觉不够。

---

## 八、请你读的文件

- `paper/sections/introduction.tex`（重点是 5–90 行，即现有引言和贡献列表）
- `paper/sections/methodology.tex`（179–292 行）
- `paper/sections/convergence.tex`
- `paper/sections/supplementaryB.tex`（707–716 行，跟踪不等式）
- `revision/supplementary/` 下的审稿意见
- `notes/progress/DualHash_A0_科学问题.md`（今天研讨的完整记录，其中的对比分析只供内部参考）

**请用中文回答。英文写作片段直接给出可以放进论文的版本。**
