# DualHash 文献调研资料（面向 SIAM 转投与审稿痛点）

## 0. 本文档的用途

这份文档不是回信草稿，而是给你后续改稿直接使用的“问题导向型”调研底稿。组织逻辑完全对齐你当前的四个审稿痛点：

1. 深度哈希里近年来与 Transformer 相关的代表性工作，以及它们是否提供了真正可比的理论保证。
2. 审稿人关于 `ELU smoothness` 与 `ReLU backbone` 的质疑，在优化期刊语境下如何回应。
3. 2024--2025 年与“减小量化误差/离散化误差”最相关的工作梳理。
4. 基线补充建议，尤其是 `GreedyHash`、`CSQ`、Transformer-based 方法，以及 reviewer 提到的 `DAHNet`、`FTH` 的核验情况。

同时，我把“适合 SIAM 口味的引用策略”单独放在最后，避免全文继续维持 TIP 风格的 benchmark 叙事。

---

## 1. 先给结论：你这篇工作的“理论定位”目前仍然有很强的独特性

### 1.1 当前检索到的主流深度哈希文献，几乎都没有给出你这种层级的优化复杂度保证

从已检索到的单模态深度哈希文献看，主流工作基本都集中在以下三类：

- 改 backbone 或表征结构：如 CNN、Hybrid CNN+Transformer、ViT、Mamba。
- 改监督方式：pairwise、triplet、center/proxy、fine-grained saliency、semantic centers。
- 改量化/二值化机制：straight-through、center similarity、proxy quantization、bit balance、semantic preservation。

但这些工作通常只给出以下层面的“理论”：

- 哈希中心构造的几何解释。
- 某个损失项的动机或启发式分析。
- 某个子模块的性质说明。
- 强经验实验，而非端到端优化复杂度分析。

就这次检索范围内的**单模态深度图像哈希**而言，我没有找到一篇在你所研究的这条线上，已经给出类似

- 非渐近收敛分析，
- 随机一阶 oracle/sample complexity，
- 多块变量 primal-dual 结构下的 \(\mathcal O(\varepsilon^{-4})/\mathcal O(\varepsilon^{-3})\)

这样的完整理论保证的已发表代表作。

### 1.2 需要谨慎区分“有理论陈述”和“有优化收敛保证”

这个区分非常重要，因为 reviewer 很容易把这两件事混在一起。

检索中确实看到一些哈希相关工作提到 “theoretical guarantee” 或 “strict theoretical guarantee”，但它们通常不是你这个层面的优化保证。例如：

- `Semantic-Aware Adversarial Training for Reliable Deep Hashing Retrieval`（2023）中的 “strict theoretical guarantee” 是针对其语义代表特征学习/对抗训练构造的局部理论陈述，不是针对单模态深度哈希主目标的端到端复杂度分析。
- 更早一些的 cross-modal/binary reconstruction 方向，也有“preservation guarantee” 或表示学习层面的理论说明，但并不等于对深度哈希训练算法本身给出复杂度保证。

因此，你在 related work 里最稳妥的说法应该是：

> 现有深度哈希工作虽然包含若干理论动机、几何解释或子模块性质分析，但在单模态深度图像哈希中，针对含 W-type 量化正则的端到端训练过程给出非渐近随机复杂度保证的工作仍非常罕见；据我们调研，尚缺少与 DualHash 在理论层级上直接可比的结果。

这句话比“完全没有理论工作”更稳，也更容易过审。

---

## 2. 痛点一：Transformer 相关深度哈希文献梳理

这一部分建议你在论文里分成两层写：

- 第一层：**一般单模态深度哈希**中的 Transformer / hybrid backbone。
- 第二层：**fine-grained / remote sensing / specialized retrieval** 中的 Transformer hashing。

这样 reviewer 会看到你“确实查过”，同时你又不必把所有任务混成同一类直接 baseline。

### 2.1 与你最相关的 Transformer / hybrid 单模态图像哈希

#### A. `HybridHash: Hybrid Convolutional and Self-Attention Deep Hashing for Image Retrieval`

- 年份/来源：ICMR 2024
- 核心点：用 hybrid convolution + self-attention backbone 做单模态图像哈希，强调兼顾局部与全局信息。
- 与你的关系：这是 reviewer 所期待看到的“至少一种 Transformer/hybrid hashing baseline”的非常合适候选。
- 理论层面：我没有看到它提供类似你这种优化复杂度保证。
- 建议用途：可进 related work，也很适合作为补实验 baseline。

#### B. `Content-adaptive perception mixer visual transformer for hashing image retrieval (CAPMH)`

- 年份/来源：Expert Systems with Applications, 2025
- 核心点：明确把 Transformer-based hashing 与 CNN-based hashing 做对比，主打多尺度信息融合与全局语义建模。
- 与你的关系：它属于 2025 的“新架构类”方法，适合用来回应“baseline 太旧”。
- 理论层面：偏模型设计与实验验证，不是复杂度分析文章。
- 建议用途：可列入“2025 代表性新方法”。

#### C. `Multi-proxy deep hashing for image retrieval (MPDH)`

- 年份/来源：ACM ICMR Workshop, 2024
- 核心点：提出 `proxy quantization strategy`，明确针对 continuous feature 与 binary code 的不一致问题。
- 与你的关系：虽然不一定是最强 baseline，但它非常适合放在“最新减小量化误差策略”这一脉络中。
- 理论层面：未见可比的收敛复杂度结果。

### 2.2 Fine-grained / 专项任务中的 Transformer hashing

这些工作不一定与你的单模态通用哈希设置完全一致，但非常适合放 related work，用来说明 Transformer 确实已经进入 hashing retrieval：

#### D. `Rethinking Vision Transformer for Large-Scale Fine-Grained Image Retrieval (EET)`

- 年份/来源：arXiv 2025
- 核心点：ViT + token pruning + discriminative transfer，用于大规模 fine-grained image retrieval。
- 与你的关系：它不是最直接的通用单模态 baseline，但它能很好回应 reviewer 关于“为什么没有 Transformer-based hashing 对比”的质疑。
- 适合写法：作为 Transformer hashing 的前沿进展引用，而不是与你的主表直接并列，除非你愿意切到 fine-grained 数据集。

#### E. `A Semantically Guided Deep Supervised Hashing Model for Multi-Label Remote Sensing Image Retrieval (SGDSH)`

- 年份/来源：Remote Sensing, 2025
- 核心点：基于改进 Swin Transformer 的多标签遥感图像哈希。
- 与你的关系：说明多标签哈希场景里也已经出现 Transformer-based 方法。
- 适合写法：放“specialized Transformer hashing” 小段落，不建议直接拿来做主表 baseline。

### 2.3 这一部分可以怎么写进论文

建议 related work 里的判断语气是：

- Transformer 或 hybrid backbone 已经开始进入深度哈希与细粒度哈希检索。
- 这些工作主要提升的是表征能力、细粒度判别性或多尺度建模。
- 但它们依然主要采用经验型训练目标，尚未解决含非凸量化正则的端到端复杂度分析问题。

这会把你的工作和这些工作拉开：

- 他们强在 `representation/backbone`
- 你强在 `optimization/theory`

---

## 3. 痛点二：关于 ELU smoothness 与 ReLU backbone 的优化文献解释

这是你现在最需要“说人话但又学术稳”的部分。

### 3.1 reviewer 的质疑本质上是对的

你文中的 `Assumption (ii)` 要求 \(F(x,B)\) 对 \((x,B)\) 是 \(L_F\)-smooth。

这在使用 ELU 时相对自然，因为 ELU 至少是连续可导的，便于直接套 descent lemma、Lyapunov 下降和 oracle complexity 分析。

但标准 ReLU 的问题在于：

- ReLU 在 0 点不可导；
- 整个网络目标因此不再是全局 \(C^1\) smooth；
- 所以你目前这套 `smooth finite-sum + primal-dual Lyapunov` 理论，**严格地说并不能直接覆盖标准 ReLU 预训练骨干**。

这一点不能硬扛，最好承认。

### 3.2 但这并不意味着 ReLU 下算法就“不成立”或“完全没理论”

优化文献里其实有一条很重要的替代路线：**不要求全局光滑，而改用 nonsmooth / tame / weakly convex 分析框架**。

#### 关键文献脉络

#### A. `Stochastic Subgradient Method Converges on Tame Functions`

- Davis, Drusvyatskiy, Kakade, Lee
- Foundations of Computational Mathematics, 2019
- 关键信息：对 semialgebraic / Whitney-stratifiable / tame 类函数，随机次梯度法的极限点收敛到一阶驻点。
- 对你最重要的含义：
  现代神经网络常见构件属于“tame”类，ReLU 网络就在这条分析线上能被处理。

这篇文献非常适合你拿来回应 reviewer：

> 虽然 ReLU 破坏了本文主定理所依赖的全局光滑性假设，但从更一般的 nonsmooth optimization 视角看，ReLU 网络仍属于可分析的 tame/Whitney-stratifiable 类，随机次梯度型训练在更弱意义下仍可获得驻点收敛保证。

#### B. `Stochastic Model-Based Minimization of Weakly Convex Functions`

- Davis, Drusvyatskiy
- SIAM Journal on Optimization, 2019
- 关键信息：对 weakly convex 问题建立了 stochastic model-based / proximal-type 方法的复杂度结果。
- 对你的意义：
  它给你一个非常好的“转折句”：
  不是所有深度学习优化都必须在全局 smooth 框架下分析，很多与神经网络相关的问题已经转向 weakly convex / Moreau envelope 视角。

#### C. `Graphical Convergence of Subgradients in Nonconvex Optimization and Learning`

- Davis, Drusvyatskiy
- Mathematics of Operations Research, 2022
- 关键信息：针对 weakly convex learning 问题的样本近似与 subgradient 稳定性做了更细致分析。
- 对你的意义：
  可以支持这样一句话：
  “即使光滑条件不严格成立，学习问题仍可通过 nonsmooth/weakly convex 框架研究其稳定性与驻点结构。”

#### D. `Convergence of a Stochastic Gradient Method with Momentum for Non-Smooth Non-Convex Optimization`

- Mai, Johansson
- ICML 2020
- 关键信息：带 momentum 的随机梯度法也可以在 nonsmooth, nonconvex 设置下分析。
- 对你的意义：
  这篇很适合连接你的 `DualHash-StoM`。
  你可以据此说：即使不采用 ELU，带 momentum 的随机方法在更广义 nonsmooth 非凸框架下也已有理论先例。

#### E. `Optimization Methods for Large-Scale Machine Learning`

- Bottou, Curtis, Nocedal
- SIAM Review, 2018
- 关键信息：这是偏总论性的优化综述，明确把现代机器学习中的非光滑结构纳入讨论语境。
- 对你的意义：
  非常适合放 related work 或 discussion，帮助把你的假设放回“优化理论为清晰复杂度常需额外正则性假设”的大背景。

### 3.3 你可以怎样在文中解释 ELU / ReLU

我建议你在正文里明确分三层表述：

#### 第一层：承认理论覆盖范围

> 为了得到显式的非渐近复杂度界，本文在 Assumption (ii) 中采用了 \(L_F\)-smooth 假设，并在实验中使用 ELU 以匹配该理论框架。

#### 第二层：解释 ReLU 为什么仍有实践合理性

> 在实践中，标准 ReLU 网络虽然不满足全局 \(C^1\) 光滑性，但其目标函数具有分片线性/分片光滑结构，训练过程通常在几乎处处可微的区域内进行，因此基于自动微分的随机优化仍然表现稳定。

#### 第三层：把 reviewer 的质疑接到优化文献

> 从更一般的 nonsmooth optimization 文献看，ReLU 网络可纳入 tame / weakly convex / Whitney-stratifiable 框架，在该框架下，随机次梯度型方法能够获得驻点收敛保证；但要把本文当前的 primal-dual oracle complexity 原样推广到标准 ReLU 架构，仍需新的技术分析。

这段话的好处是：

- 不否认 reviewer；
- 不自毁理论；
- 同时显示你知道更广的优化文献；
- 还自然引出“ReLU robustness experiment 作为补充实验”的必要性。

### 3.4 对你论文最有帮助的精炼结论

一句最适合写进 discussion 的话是：

> ELU 在本文中主要承担“便于建立显式复杂度界”的理论角色，而不是方法可用性的必要条件；对于标准 ReLU 架构，现有 nonsmooth stochastic optimization 文献表明其训练仍可在更弱的驻点意义下被分析，但要获得本文同阶的多块 primal-dual 复杂度结果仍是开放问题。

这句话我建议你后面直接拿去改文。

---

## 4. 痛点三：2024--2025 年“减小量化误差”相关工作梳理

这一部分 reviewer 的真实意思不是“你一定要追所有新 paper”，而是：

- 你必须证明你知道 2024--2025 的量化/离散化方向在做什么；
- 你的方法不是只赢了老 baseline。

因此最好的组织方式不是简单列 paper，而是按“量化误差怎么处理”来梳理。

### 4.1 这一方向的主流思路

近两年与量化误差相关的深度哈希工作，大致可以分为四类：

#### A. 通过 hash centers / proxies 降低离散化不一致

- `CSQ`（CVPR 2020）是这一脉络的经典代表；
- 2024 以后继续出现 proxy/semantic center/center-preserving 的变体。

代表：

- `Multi-proxy deep hashing for image retrieval (MPDH)`, 2024
  - 明确提出 `proxy quantization strategy`
  - 目标就是减小 continuous feature 与 binary code 之间的不一致

- `Deep Hashing with Semantic Hash Centers for Image Retrieval (SHC)`, 2025 preprint
  - 核心思想是让 hash center 反映类间语义关系
  - 更偏 center-based 结构创新

#### B. 通过 neighborhood / tuple / graph 结构减少语义损失并间接压低量化误差

代表：

- `Deep Neighborhood Structure-Preserving Hashing (DNSH)`, 2024
  - 通过 adaptive margin quadruplet loss + quadruple regularization
  - 明确引入 regularization 以减少 binary-like embedding 与 hash code 之间的量化误差

#### C. 通过更强 backbone 提高 binary-like representation 的质量

代表：

- `HybridHash`, 2024
- `CAPMH`, 2025
- `EET`, 2025

这类方法未必显式写“quantization error minimization”，但它们通过更强的表征学习，把送入 hashing layer 的连续特征做得更干净，因此经常伴随更好的二值化结果。

#### D. 通过“保留语义信息”来对抗 hash transformation 中的信息丢失

代表：

- `Semantic Preservation-Based Hash Code Generation for fine-grained image retrieval (SPBH)`, 2025
  - 明确指出很多方法只关注 feature extraction，而忽略从 continuous feature 到 binary code 的语义损失
  - 提出 intermediate features 与 adaptive category voting mask 来缓解哈希变换中的信息丢失

### 4.2 这些文献和你的关系

你的工作和这些方法的差异应该这样定位：

- 这些方法大多在**设计更好的离散化目标、中心结构、代理约束或表征 backbone**；
- 你的工作则在**优化层面**解决：
  - W-type regularization 的直接优化难题，
  - 非线性复合形式 \(h(D_i(x))\) 的 proximal 不可解问题，
  - 并给出显式非渐近复杂度保证。

换句话说，你不是在“发明另一个 quantization loss”，而是在“给一类已有且有效的 quantization regularization 一个真正可分析的优化框架”。

这句话要在 related work 里写清楚，否则 reviewer 会把你和所有新哈希模型放在同一个比较维度上。

### 4.3 这一部分推荐加入 related work 的写法

建议写成两句：

> Recent deep hashing methods continue to improve quantization quality through proxy/center-based supervision, neighborhood-preserving regularization, and stronger Transformer-style backbones. Representative examples include CSQ, MPDH, DNSH, HybridHash, CAPMH, and related semantic-preserving hashing frameworks.

> These methods mainly enhance representation learning or discretization heuristics, whereas our focus is different: we study how to optimize W-type quantization regularization itself under a nonlinear compositional structure and provide explicit stochastic complexity guarantees.

---

## 5. 痛点四：你现在最该补的 baseline 到底是什么

### 5.1 必补，且优先级最高

这两个 reviewer 点名得很准，确实应该补：

#### A. `GreedyHash`

- 年份/来源：NeurIPS 2018
- 价值：经典离散优化/straight-through 风格代表方法。
- reviewer 意图：不是因为它最新，而是因为它是 deep hashing benchmark 中的“ established strong baseline ”。
- 理论层面：有算法推导与解释，但不是你这个层级的复杂度保证。

#### B. `CSQ`

- 年份/来源：CVPR 2020
- 价值：center similarity / hash center 这条线里的代表方法，非常常见。
- reviewer 意图：你不包含它，会显得 baseline 不完整。
- 理论层面：主要是中心构造与损失设计，不是复杂度分析。

### 5.2 2024--2025 中最值得优先考虑补实验的

如果你只能补 2--3 个新 baseline，我建议按下面顺序：

#### 第一梯队：最值得补

- `HybridHash` (ICMR 2024)
  - 原因：单模态、图像检索、hybrid self-attention，最能回应 “没有 Transformer baseline”。

- `CAPMH` (ESWA 2025)
  - 原因：2025、Transformer-based、直接面向 hashing image retrieval。

- `MPDH` (2024)
  - 原因：直接强调 `proxy quantization strategy`，很适合回应“量化误差方向的最新进展”。

#### 第二梯队：可作为文献讨论，不一定适合主表

- `EET` (2025)
  - 更偏 fine-grained FGIR，不一定和你现有数据集完全同设定。

- `SGDSH` (2025)
  - 更偏遥感多标签 retrieval，适合做 related work，不一定适合直接主表对比。

### 5.3 关于 reviewer 提到的 `DAHNet` 和 `FTH`

这一点要非常谨慎。

在这次检索里：

- `DAHNet` 我没有从可信的一手来源中稳定核实到一篇“TKDE 2024、单模态通用深度哈希、与你同设定可直接主表对比”的论文条目。
- 检索结果里出现的 `DAHNet` 更多与 fine-grained retrieval 或甚至完全无关领域共享这个缩写。
- `FTH` 也存在强歧义；当前检索到的一个 `FTH` 实际上是较早的 `Fine-Tune Hashing` 缩写，并不是 reviewer 所说的 “ACM MM 2025 新方法”的可靠对应。

因此这里的建议非常明确：

> 在没有拿到准确 DOI、论文标题和官方代码库之前，不要把 `DAHNet` 或 `FTH` 直接写进正文，更不要盲目加入参考文献。

更稳妥的做法是：

- 正文先补 `GreedyHash + CSQ + HybridHash`
- 再加 1 个你能可靠跑通并能核实出处的新方法

这样已经足够显著改善 baseline 的时效性问题。

---

## 6. 适合 SIAM 口味的文献组织方式

你转 SIAM 后，related work 不应该再写成“图像检索算法大比武”，而应该写成下面三条线：

### 6.1 深度哈希中的量化/离散化难题

这一段放：

- DSH
- DHN
- HashNet
- GreedyHash
- CSQ
- OrthoHash
- MDSHC
- 2024--2025 的 HybridHash / MPDH / CAPMH

强调：

- 他们解决的是 representation、discretization heuristic、hash center、proxy 或 backbone；
- 但没有触及你这里的“非线性复合 W-type regularization + complexity guarantee”。

### 6.2 非凸、非光滑、复合优化与 primal-dual / stochastic methods

这一段放：

- stochastic/variance-reduced nonconvex optimization
- weakly convex / tame / nonsmooth stochastic subgradient
- primal-dual / ADMM / augmented Lagrangian
- SIOPT / MOOR / FOCM / COAP 这些优化文献

强调：

- 你的理论根基在这里；
- 这才是 SIAM 审稿人最看重的 related work。

### 6.3 ReLU/ELU 的理论讨论作为 discussion 而非主贡献

不要把这部分写成 defensive rebuttal 风格，而要写成：

- smooth activation 使显式复杂度分析更透明；
- ReLU 则可放入更广义 nonsmooth/tame 框架；
- 将 DualHash 的多块 primal-dual 复杂度推广到标准 ReLU 是自然但非平凡的后续工作。

这样会显得你不是“被 reviewer 逼着补锅”，而是在主动界定理论边界。

---

## 7. 你下一版 related work 最值得增加的几句话

下面这些是这次调研后最值得落进正文的核心判断。

### 7.1 关于“目前是否已有真正可比的理论保证”

> Although several deep hashing papers provide geometric intuition, center-construction analysis, or theoretical motivation for specific modules, we did not find a directly comparable non-asymptotic stochastic complexity result for end-to-end single-modal deep hashing with W-type quantization regularization.

### 7.2 关于 Transformer 进展

> Recent hashing research has started to incorporate hybrid CNN-Transformer or pure Transformer backbones, e.g., HybridHash and subsequent ViT-based retrieval frameworks, showing that stronger representation learning is becoming an important trend in modern hashing systems.

### 7.3 关于 ELU / ReLU

> The ELU activation is adopted in this paper to match the smoothness assumptions required by our explicit complexity analysis. For standard ReLU backbones, the smoothness condition no longer holds globally; however, existing nonsmooth stochastic optimization literature suggests that such models remain analyzable under weaker tame/weakly-convex frameworks, albeit without the present oracle complexity guarantee.

### 7.4 关于你与最新工作之间的差异

> Our contribution is complementary to recent architecture-driven advances: instead of designing a new hash loss or backbone, we provide a stochastic primal-dual optimization framework for a widely used class of nonconvex quantization regularizers and establish explicit complexity bounds.

---

## 8. 当前最建议补到参考文献里的文献清单

### 8.1 强烈建议加入正文和实验对比

1. Greedy Hash: Towards Fast Optimization for Accurate Hash Coding in CNN. NeurIPS 2018.
2. Central Similarity Quantization for Efficient Image and Video Retrieval. CVPR 2020.
3. HybridHash: Hybrid Convolutional and Self-Attention Deep Hashing for Image Retrieval. ICMR 2024.

### 8.2 强烈建议加入 related work，但是否进主表看算力

4. Multi-proxy Deep Hashing for Image Retrieval. ACM ICMR Workshop 2024.
5. Deep Neighborhood Structure-Preserving Hashing for Large-Scale Image Retrieval. IEEE TMM 2024.
6. Content-adaptive Perception Mixer Visual Transformer for Hashing Image Retrieval. Expert Systems with Applications 2025.

### 8.3 建议加入 discussion / Transformer 进展综述

7. Rethinking Vision Transformer for Large-Scale Fine-Grained Image Retrieval. arXiv 2025.
8. A Semantically Guided Deep Supervised Hashing Model for Multi-Label Remote Sensing Image Retrieval. Remote Sensing 2025.

### 8.4 强烈建议加入理论 related work

9. Davis, Drusvyatskiy, Kakade, Lee. Stochastic Subgradient Method Converges on Tame Functions. Foundations of Computational Mathematics, 2019.
10. Davis, Drusvyatskiy. Stochastic Model-Based Minimization of Weakly Convex Functions. SIAM Journal on Optimization, 2019.
11. Davis, Drusvyatskiy. Graphical Convergence of Subgradients in Nonconvex Optimization and Learning. Mathematics of Operations Research, 2022.
12. Bottou, Curtis, Nocedal. Optimization Methods for Large-Scale Machine Learning. SIAM Review, 2018.
13. Mai, Johansson. Convergence of a Stochastic Gradient Method with Momentum for Non-Smooth Non-Convex Optimization. ICML 2020.

---

## 9. 关于“按编委口味引用”的说明

当前项目文件中没有看到你已经锁定的那两本 SIAM 期刊及具体编委姓名，因此这一版没法做到“按某个编委个人论文定制”。

所以我先按**期刊风格**给建议：

### 如果目标更偏 `SIAM Journal on Optimization (SIOPT)`

优先强化：

- stochastic nonconvex optimization
- nonsmooth / weakly convex / tame geometry
- primal-dual / augmented Lagrangian / operator splitting
- complexity analysis

### 如果目标更偏 `SIAM Journal on Imaging Sciences (SIIMS)`

优先强化：

- 成像/检索任务背景
- 量化误差与表示学习之间的关系
- retrieval experiments 的完整性
- 但仍保留优化理论亮点

### 如果目标更偏 `SIAM Journal on Mathematics of Data Science (SIMODS)`

优先强化：

- 数据科学问题建模
- 统计/优化/表示学习之间的结合
- 数学化的问题定义与算法分析
- 适度实验，而非纯 CV benchmark 风格

如果你后面把具体编委姓名发给我，我可以继续把这份文档升级成：

- “按编委拆分的可引用论文清单”
- “最适合放引言/related work/discussion 的具体位置建议”

---

## 10. 参考来源（本次调研用到的主要检索结果）

以下链接是本次整理时主要参考的公开来源：

- GreedyHash, NeurIPS 2018:
  https://proceedings.neurips.cc/paper/2018/hash/13f3cf8c531952d72e5847c4183e6910-Abstract.html

- CSQ, CVPR 2020:
  https://openaccess.thecvf.com/content_CVPR_2020/html/Yuan_Central_Similarity_Quantization_for_Efficient_Image_and_Video_Retrieval_CVPR_2020_paper.html

- HybridHash, arXiv / ICMR 2024:
  https://arxiv.org/abs/2405.07524

- MPDH, 2024:
  https://pure.qub.ac.uk/en/publications/multi-proxy-deep-hashing-for-image-retrieval/

- DNSH, TMM 2024:
  https://colab.ws/articles/10.1109%2Ftmm.2023.3289765

- CAPMH, 2025:
  https://www.sciencedirect.com/science/article/abs/pii/S0957417425018482

- SPBH, 2025:
  https://www.sciencedirect.com/science/article/pii/S0957417425002908

- EET / Rethinking ViT for FGIR, 2025:
  https://arxiv.org/abs/2504.16691

- SGDSH, 2025:
  https://www.mdpi.com/2072-4292/17/5/838

- SAAT, 2023:
  https://arxiv.org/abs/2310.14637

- Stochastic Subgradient Method Converges on Tame Functions:
  https://arxiv.org/abs/1804.07795
  https://optimization-online.org/2018/05/6635/

- Stochastic Model-Based Minimization of Weakly Convex Functions:
  https://arxiv.org/abs/1803.06523
  https://optimization-online.org/2018/03/6545/

- Graphical Convergence of Subgradients in Nonconvex Optimization and Learning:
  https://arxiv.org/abs/1810.07590

- SIAM prize spotlight on weakly convex optimization:
  https://www.siam.org/publications/siam-news/articles/june-prize-spotlight-2023/

- Optimization Methods for Large-Scale Machine Learning:
  https://users.iems.northwestern.edu/~nocedal/PDFfiles/SIAMREVIEW_optML.pdf

- Momentum in nonsmooth nonconvex optimization:
  https://proceedings.mlr.press/v119/mai20b.html

---

## 11. 最后一句最实用的判断

如果你现在只做最小充分补强，那么最划算的动作不是盲目追很多 2025 新方法，而是：

1. 文献上把 `GreedyHash + CSQ + HybridHash + CAPMH/MPDH` 讲清楚；
2. 理论上把 `ELU smoothness` 与 `ReLU nonsmooth/tame optimization` 的关系讲清楚；
3. 实验上至少补 `GreedyHash + CSQ + 1 个 Transformer/hybrid baseline`。

这样最能同时解决审稿痛点和 SIAM 转投定位问题。
