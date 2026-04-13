# DualHash 按编委方向定制的引用策略（基于“投稿期刊编委选择.docx”）

## 0. 这份补充说明解决什么问题

你最新的文件 [投稿期刊编委选择.docx](/Users/luxuan/Downloads/DualHash/投稿期刊编委选择.docx) 已经把目标期刊进一步收缩到了：

- `SIAM Journal on Scientific Computing (SISC)`
- `SIAM Journal on Imaging Sciences (SIIMS)`

因此，这份说明不再按我上一版的“期刊泛风格”来写，而是直接回答：

1. 如果主投 `SISC`，最该引用哪些**编委/潜在 AE/推荐审稿人**的文章？
2. 如果备投 `SIIMS`，最该引用哪些编辑方向最相关的文章？
3. 这些文章应该放在 `introduction / related work / methodology / discussion` 的哪里？
4. 哪些名字虽然在你文件里出现了，但其实**不适合为了投其所好而硬引**？

核心原则只有一条：

> 只引用那些与 `DualHash` 的“随机优化 + 原始-对偶 + 非凸/非光滑 + 收敛分析 + 深度学习/成像”真正有交集的文章。

---

## 1. 先修正总策略：你这版应该优先围绕 `SISC` 来组织理论引用

### 1.1 为什么

根据 SIAM 官方介绍，`SISC` 自 2024 年起新增了 `Machine Learning Methods for Scientific Computing` 分区，明确欢迎“为科学计算问题发展新的机器学习方法”的论文，而 `Lars Ruthotto` 是该方向的 section editor。  
来源：

- [SISC 新增 Scientific ML 分区的官方说明](https://www.siam.org/publications/siam-news/articles/new-sisc-section-on-scientific-machine-learning/)
- [Lars Ruthotto 主页，明确写有 SISC 机器学习方向 section editor](https://www.math.emory.edu/~lruthot/)

这对你非常重要，因为 `DualHash` 虽然是图像检索问题，但稿子的真正硬核部分并不是 CV benchmark，而是：

- 非凸复合优化建模；
- stochastic primal-dual 框架；
- nonsmooth regularization 的 dual reformulation；
- 非渐近复杂度分析。

这套叙事显然更接近 `SISC` 审稿口味。

### 1.2 对应的引用逻辑

如果主投 `SISC`，最该补的不是更多 CV 检索论文，而是把以下理论链条串完整：

- 随机复合非凸优化；
- primal-dual / ALM / ADMM / stochastic proximal point；
- nonsmooth / weakly convex / semismooth Newton；
- 深度学习优化与数值计算之间的桥梁。

---

## 2. 主投 SISC 时，最值得优先引用的“编委方向”文章

下面按“相关度”和“推荐插入位置”排序。

## 2.1 Lars Ruthotto：最值得在引言中出现

### 推荐文章 A

`Deep Neural Networks Motivated by Partial Differential Equations`  
Lars Ruthotto, Eldad Haber  
来源：

- [作者主页页面](https://math.emory.edu/~lruthot/publication/ruthotto-haber-2018/)
- [arXiv 版本](https://arxiv.org/abs/1804.04272)

### 为什么适合引

这篇不是 primal-dual 或哈希论文，但它对你很重要，因为它代表了 `SISC` 机器学习方向最自然的一条叙事：

- 深度网络不是纯经验工程；
- 可以从连续时间/数值分析/稳定性角度理解；
- 数值优化与网络训练之间存在结构化桥梁。

### 最适合放的位置

- `Introduction`：第一段或第二段，作为“深度学习训练值得用计算数学语言重新审视”的铺垫。
- `Conclusion/Discussion`：说明你的工作也属于“用优化理论重新刻画深度学习训练”的路线。

### 你可以借用的角度

不是说你和他们的方法相同，而是说：

> 类似于 PDE 视角为深度网络训练提供了稳定性与结构化解释，本文从 primal-dual 与 Fenchel 对偶角度为深度哈希中的量化正则训练提供了新的优化结构和复杂度分析。

---

## 2.2 Michael Ulbrich：最值得在理论 related work 中出现

### 推荐文章 B

`A Semismooth Newton Stochastic Proximal Point Algorithm with Variance Reduction`  
Andre Milzarek, Fabian Schaipp, Michael Ulbrich  
来源：

- [arXiv 页面](https://arxiv.org/abs/2204.00406)
- [dblp/SIOPT 2024 索引页](https://dblp.org/rec/journals/siamjo/MilzarekSU24)

### 为什么适合引

这篇和你的交集非常强：

- stochastic optimization；
- composite / weakly convex / nonsmooth；
- variance reduction；
- proximal point；
- 明确有收敛分析。

虽然它不是 primal-dual deep hashing，但在 `SISC/SIOPT` 语境下，它是非常好的“邻近理论工作”。

### 最适合放的位置

- `Related Work` 中 “Relation to nonconvex stochastic optimization” 小节；
- `Convergence analysis` 之前或结尾 remark；
- 也很适合在 rebuttal-like discussion 中用来说明：现代优化理论已经接受“深度学习相关目标不必都在光滑欧氏梯度框架下分析”。

### 最适合借的点

- 你可以借它来解释：`DualHash-StoRM` 并不是凭空出现的 heuristic，而是处在“stochastic variance-reduced methods for nonsmooth/nonconvex composite optimization”这条主线上。

---

## 2.3 Quoc Tran-Dinh：最值得作为“理论对照系”引用

你文件里说他不是编委但建议作为 reviewer，这个判断很对。对于正文而言，他的文献比“人情 AE”更值得实打实地引用。

### 推荐文章 C

`ProxSARAH: An Efficient Algorithmic Framework for Stochastic Composite Nonconvex Optimization`  
Pham, Nguyen, Phan, Quoc Tran-Dinh  
来源：

- [JMLR 正式页面](https://jmlr.org/beta/papers/v21/19-248.html)
- [arXiv 页面](https://arxiv.org/abs/1902.05679)

### 推荐文章 D

`Non-Stationary First-Order Primal-Dual Algorithms with Faster Convergence Rates`  
Quoc Tran-Dinh, Yuzixuan Zhu  
来源：

- [arXiv 页面](https://arxiv.org/abs/1903.05282)
- [Optimization Online PDF](https://optimization-online.org/wp-content/uploads/2019/04/7183.pdf)

### 为什么适合引

这两篇几乎就是 reviewer 最想看到你知道的东西：

- stochastic composite nonconvex optimization；
- primal-dual first-order methods；
- complexity rate；
- modern variance-reduced estimator。

### 最适合放的位置

- `Related Work`：这两篇应该是你现有 stochastic optimization 那一段里最值得出现的新支点。
- `Methodology`：如果你继续强调 `StoRM` 和 `StoM` 的现代优化背景，也可以引用。

### 借用方式

你不能说他们解决了你的问题，但可以说：

> 现有现代 stochastic composite/primal-dual 文献已在单块或更规则的问题上建立了复杂度理论，而 DualHash 的难点在于 W-type 非凸正则与 nonlinear network map 的复合结构。

这会显著提升 related work 的“优化味”。

---

## 2.4 Wotao Yin：最适合撑起 constrained / ALM / ADMM 这条线

### 推荐文章 E

`Global Convergence of ADMM in Nonconvex Nonsmooth Optimization`  
Yu Wang, Wotao Yin, Jinshan Zeng  
来源：

- [arXiv 页面](https://arxiv.org/abs/1511.06324)
- [UCLA technical report PDF](https://ww3.math.ucla.edu/camreport/cam15-62.pdf)

### 推荐文章 F

`Moreau Envelope Augmented Lagrangian Method for Nonconvex Optimization with Linear Constraints`  
Jinshan Zeng, Wotao Yin, Ding-Xuan Zhou  
来源：

- [arXiv 页面](https://arxiv.org/abs/2101.08519)

### 为什么适合引

这两篇对应你稿子里很关键的一条线：

- constrained reformulation；
- augmented Lagrangian / ADMM / primal-dual；
- nonconvex + nonsmooth；
- convergence under weaker assumptions。

### 最适合放的位置

- `Related Work` 中 primal-dual / ADMM 小节；
- `Discussion` 中“我们的框架与 ALM/ADMM 的关系”；
- `Conclusion` 中未来工作，如 memory cost / 更一般约束框架。

### 特别适合回答 reviewer 的地方

如果 reviewer 对你的 primal-dual reformulation 仍有怀疑，可以借这些文献说明：

> 将非凸非光滑问题转写为带辅助变量的约束优化问题，并在拉格朗日/原始-对偶框架下分析，是当前现代非凸优化中的主流路线之一。

---

## 2.5 Bin Dong：可以引，但不要作为最核心理论支柱

### 可选文章 G

`An image reconstruction framework based on deep neural network for electrical impedance tomography`  
来源：

- [IEEE Xplore 页面](https://ieeexplore.ieee.org/document/8296950)

### 为什么“可引但不应硬引”

Bin Dong 的确是 SIIMS/SciML 圈子里很重要的人，但你当前文章更偏“优化算法理论”，而不是“成像里 deep model 的 reconstruction framework”。

因此：

- 如果投 `SIIMS`，你可以在引言里轻描淡写地引用他，表示你知道 imaging + deep learning + regularization 这条线；
- 如果投 `SISC`，完全没必要为了“讨好编委”而把它放成核心相关工作。

### 结论

Bin Dong 的文献对你是“辅助增强相关性”，不是“主战场”。

---

## 3. 备投 SIIMS 时，最值得优先引用的“编委方向”文章

如果转投 `SIIMS`，就要把理论引用稍微“成像化”，也就是说：

- 保留 stochastic/nonconvex/primal-dual 理论主线；
- 但要把这条线落在 imaging algorithm 的传统脉络上。

## 3.1 Emilie Chouzenoux + Jean-Christophe Pesquet：这是 SIIMS 线里最该引用的一组

### 推荐文章 H

`Convergence Results for Primal-Dual Algorithms in the Presence of Adjoint Mismatch`  
Andres Contreras, Émilie Chouzenoux, Jean-Christophe Pesquet, Marion Savanier  
来源：

- [Optimization Online 页面](https://optimization-online.org/2022/04/8877/)
- [PDF 直链](https://optimization-online.org/wp-content/uploads/2022/04/8877.pdf)

### 推荐文章 I

`A Stochastic Majorize-Minimize Subspace Algorithm for Online Penalized Least Squares Estimation`  
Émilie Chouzenoux, Jean-Christophe Pesquet  
来源：

- [arXiv 页面](https://arxiv.org/abs/1512.08722)
- [Optimization Online PDF](https://optimization-online.org/wp-content/uploads/2016/01/5275.pdf)

### 为什么最适合你

这组文献和你的重合度极高：

- stochastic optimization；
- convergence analysis；
- primal-dual / proximal；
- imaging problems；
- penalized objectives。

相比单纯引用一般成像论文，这两篇更容易让 `SIIMS` 的编辑/审稿人觉得你“是按他们社区的话语方式在写文章”。

### 最适合放的位置

- `Related Work`：成像中的 primal-dual / stochastic optimization 小节；
- `Introduction` 后半段：说明你的 work 也属于“把现代优化理论带入 imaging-related learning tasks”的脉络。

---

## 3.2 Matthias Ehrhardt：最适合支撑“stochastic PDHG in imaging”

### 推荐文章 J

`Stochastic Primal-Dual Hybrid Gradient Algorithm with Arbitrary Sampling and Imaging Applications`  
Antonin Chambolle, Matthias Ehrhardt, Peter Richtárik, Carola-Bibiane Schönlieb  
来源：

- [arXiv 页面](https://arxiv.org/abs/1706.04957)
- [Matthias Ehrhardt 主页（含正式发表信息）](https://mehrhardt.github.io/research.html)

### 为什么适合引

这篇几乎是你“投 SIIMS 时最自然的祖谱文献”之一：

- stochastic primal-dual；
- arbitrary sampling；
- imaging applications；
- clear convergence theory。

### 最适合放的位置

- `Related Work` 的 primal-dual 段落；
- 甚至 `Methodology` 第一段，你现在已经写了 PDHG inspiration，这里可以顺着扩展到 stochastic PDHG 的 imaging 版本。

### 你可以借的说法

> 与随机 PDHG 在成像反问题中的成功类似，本文也利用原始-对偶结构来处理深度哈希中的复合量化正则；不同之处在于我们面对的是非凸网络映射与 W-type 正则带来的更强非线性结构。

---

## 3.3 Xiaoqun Zhang：适合作为“成像 primal-dual 传统”的经典引用

### 推荐文章 K

`A General Framework for a Class of First Order Primal-Dual Algorithms for Convex Optimization in Imaging Science`  
Ernie Esser, Xiaoqun Zhang, Tony F. Chan  
来源：

- [HKUST research portal 页面](https://researchportal.hkust.edu.hk/en/publications/a-general-framework-for-a-class-of-first-order-primal-dual-algori)

### 为什么适合引

这篇不是新文献，但如果投 `SIIMS`，它能让你的 primal-dual 叙事“接地到 imaging community 的经典谱系”。

### 最适合放的位置

- `Methodology` 第一段：你已经引用 `PDHG`，如果走 `SIIMS` 路线，可以加一句说明 primal-dual frameworks 在 imaging science 中有长期发展历史。

---

## 3.4 Yifei Lou：适合作为“非凸正则 + 可计算 proximal” 的近邻文献

### 推荐文章 L

`Fast L1-L2 Minimization via a Proximal Operator`  
Yifei Lou, Ming Yan  
来源：

- [arXiv 页面](https://arxiv.org/abs/1609.09530)
- [J. Sci. Comput. PDF 镜像](https://archive.ymsc.tsinghua.edu.cn/pacm_download/332/9032-10.10072Fs10915-017-0463-2.pdf)

### 为什么适合引

这篇和你的方法论上的共鸣非常强：

- 都是在处理非凸正则；
- 都非常强调 proximal operator 的显式/可计算性；
- 都属于“正则项有效，但直接优化困难，因此转向结构化算法设计”的思路。

### 最适合放的位置

- `Introduction` 中关于 proximal-based methods 的段落；
- `Methodology` 中关于为什么 `h^*` 的 proximal 更可解的解释。

### 借用方式

这篇特别适合帮助你把贡献说得更数学化：

> 我们的工作并不是再发明一个哈希损失，而是像若干非凸正则优化工作那样，试图找出一种使 proximal / dual step 重新变得 tractable 的结构化 reformulation。

---

## 4. 哪些名字不建议为了“投其所好”而硬引

你文档里有些名字是“方向上有关系”，但对正文帮助有限。

## 4.1 Delin Chu

- 你文件里明确写的是“人情推荐”。
- 这种情况适合体现在 `Suggested Editor`，不适合硬塞进正文引用。

结论：**不建议为了 Delin Chu 特意补 cite**。

## 4.2 Michael Ng / Martin Stoll / Tao Zhou / Markos Katsoulakis

这些人当然都很强，但从你现在这篇稿子的核心来看：

- 他们不是最贴近 `stochastic primal-dual + nonconvex nonsmooth + deep learning optimization` 的文献源头；
- 强行引用容易显得散。

结论：**除非你正文新增了数值线性代数 / UQ / scientific ML generalities 的段落，否则不必硬引**。

## 4.3 Bin Dong

前面已经说过：

- 可以引用；
- 但不应承担“核心理论 related work”的角色。

---

## 5. 如果只能加 6 篇“最划算”的编委定向引用，我建议这 6 篇

### 面向 `SISC`

1. Ruthotto & Haber, `Deep Neural Networks Motivated by PDEs`
2. Milzarek, Schaipp, Ulbrich, `A Semismooth Newton Stochastic Proximal Point Algorithm with Variance Reduction`
3. Pham et al., `ProxSARAH`
4. Tran-Dinh & Zhu, `Non-Stationary First-Order Primal-Dual Algorithms`
5. Wang, Yin, Zeng, `Global Convergence of ADMM in Nonconvex Nonsmooth Optimization`
6. Zeng, Yin, Zhou, `Moreau Envelope Augmented Lagrangian Method`

### 面向 `SIIMS`

1. Contreras, Chouzenoux, Pesquet, Savanier, `Convergence Results for Primal-Dual Algorithms in the Presence of Adjoint Mismatch`
2. Chouzenoux & Pesquet, `A Stochastic Majorize-Minimize Subspace Algorithm`
3. Chambolle, Ehrhardt, Richtárik, Schönlieb, `Stochastic Primal-Dual Hybrid Gradient Algorithm with Arbitrary Sampling`
4. Esser, Zhang, Chan, `A General Framework for a Class of First Order Primal-Dual Algorithms in Imaging Science`
5. Lou & Yan, `Fast L1-L2 Minimization via a Proximal Operator`
6. Ruthotto & Haber, `Deep Neural Networks Motivated by PDEs`

---

## 6. 最后给你的实操建议

### 如果主投 `SISC`

你的引用策略应该是：

- `deep hashing 最新文献` 只保留必要代表；
- `优化理论文献` 大幅增强；
- 让读者一眼看到你不是“CV 稿改投数值期刊”，而是“有应用背景的随机非凸优化论文”。

### 如果备投 `SIIMS`

你的引用策略应该是：

- 保留 `SISC` 那条理论线；
- 再叠加 `stochastic/primal-dual imaging` 的祖谱；
- 让读者觉得这篇文章属于“imaging-related learning + optimization theory”的自然延伸。

### 一句最重要的话

不要把“按编委口味引用”理解成“多引编委名字”。

真正有效的做法是：

> 让你的相关工作、方法动机和理论定位，使用目标期刊熟悉的那套学术语言来组织。

对 `DualHash` 而言，这套语言就是：

- stochastic composite optimization
- primal-dual / ALM / ADMM
- nonsmooth / nonconvex regularization
- convergence and complexity
- learning or imaging as application

只要这个骨架搭对了，引用数量不是关键。
