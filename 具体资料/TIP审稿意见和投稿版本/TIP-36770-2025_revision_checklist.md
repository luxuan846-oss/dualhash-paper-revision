# TIP-36770-2025 Revision Checklist | 修改清单

> Organized by issue priority. Each item lists the source (AE/R1/R2/R3) and required action.
> 按问题优先级排列，标注来源及所需修改动作。

---

## 【必须修改 / Must Fix】

### 1. 基线方法过时 | Outdated Baselines
**来源 / Source:** AE + R1 + R2 + R3

| 中文 | English |
|------|---------|
| 仅涵盖截至2023年的方法（MDSHC），缺少2024–2025年最新SOTA。 | Only covers methods up to 2023 (MDSHC); missing 2024–2025 SOTA. |
| 完全缺少基于Transformer架构的深度哈希方法对比。 | No Transformer-based deep hashing comparison included. |
| 缺少GreedyHash (NeurIPS 2018)、CSQ (CVPR 2020)等已确立方法。 | Missing established baselines: GreedyHash (NeurIPS 2018), CSQ (CVPR 2020). |
| 建议补充：DAHNet (TKDE 2024)、FTH (ACM MM 2025)及至少一种Transformer方法。 | Suggested additions: DAHNet (TKDE 2024), FTH (ACM MM 2025), ≥1 Transformer method. |

**修改动作 / Action:** 补充2024–2025年至少2–3篇SOTA方法的实验对比，含至少一种Transformer架构。

---

### 2. StoRM vs. StoM 理论与实验矛盾 | Theory–Empirical Contradiction
**来源 / Source:** AE + R1 + R3

| 中文 | English |
|------|---------|
| StoRM理论收敛率更优（$O(\varepsilon^{-3})$），但实验mAP分数持续低于StoM。 | StoRM has superior theoretical rate $O(\varepsilon^{-3})$ but consistently lower mAP than StoM. |
| 现有解释（"faster convergence ≠ better retrieval"）过于表面，缺乏机理分析。 | Current explanation is superficial; lacks mechanistic analysis. |
| 候选解释：方差缩减机制可能抑制了有益的随机性，阻碍更优局部极小值的探索。 | Candidate explanation: variance reduction may suppress beneficial stochasticity that aids local minima exploration. |

**修改动作 / Action:** 在正文中增加专项分析段落，从优化景观（loss landscape）角度解释该矛盾，或通过额外实验（如不同学习率、batch size下的行为对比）提供实证支撑。

---

### 3. 数据集规模不足 | Insufficient Dataset Scale
**来源 / Source:** AE + R1 + R2

| 中文 | English |
|------|---------|
| 仅在CIFAR-10、NUS-WIDE、ImageNet-100上验证，缺少大规模数据集。 | Only validates on CIFAR-10, NUS-WIDE, ImageNet-100; no large-scale benchmarks. |
| 建议补充：MS COCO 或 ImageNet-1K；MIRFlickr-25K（多标签场景标准数据集）。 | Suggested additions: MS COCO or ImageNet-1K; MIRFlickr-25K (standard multi-label benchmark). |

**修改动作 / Action:** 至少在一个大规模数据集（MS COCO或ImageNet-1K）上补充实验。

---

### 4. 16-bit 性能劣势缺乏分析 | 16-bit Underperformance Unanalyzed
**来源 / Source:** R1 + R3

| 中文 | English |
|------|---------|
| 在NUS-WIDE和ImageNet-100的低比特（16-bit）设置下，方法表现劣于基线。 | Method underperforms baselines at 16-bit on NUS-WIDE and ImageNet-100. |
| 仅陈述现象，未分析根因：是否存在表示坍塌（representation collapse）或类间距缩小？ | Phenomenon stated without root-cause analysis: representation collapse or reduced inter-class separation? |
| 建议通过t-SNE可视化或特征分布分析佐证。 | Suggest t-SNE visualization or feature distribution analysis as evidence. |

**修改动作 / Action:** 补充16-bit设置的定性分析（t-SNE或类间/类内距离统计），解释过度量化优化损害语义区分性的机理。

---

## 【建议修改 / Should Fix】

### 5. 内存开销分析缺失 | Memory Overhead $O(Nd)$ Not Discussed
**来源 / Source:** AE + R1

| 中文 | English |
|------|---------|
| 算法维护辅助变量和对偶变量，内存开销为$O(Nd)$，大规模场景下存在实际隐患。 | Algorithm maintains auxiliary and dual variables with $O(Nd)$ memory cost; practical concern at scale. |
| 正文缺少内存效率的讨论或实证验证。 | No discussion or empirical verification of memory efficiency in the paper. |

**修改动作 / Action:** 在实验部分或补充材料中增加内存消耗分析（如不同$N$下的实测内存占用）。

---

### 6. ELU平滑假设的实用性 | ELU Smoothness Assumption Practicality
**来源 / Source:** AE + R1

| 中文 | English |
|------|---------|
| 收敛分析依赖ELU激活函数的光滑性（Assumption ii），与主流ReLU预训练骨干网络不兼容。 | Convergence analysis relies on ELU smoothness (Assumption ii), incompatible with standard ReLU pre-trained backbones. |
| 需讨论在ReLU架构下光滑条件不严格成立时的鲁棒性。 | Need to discuss robustness when smoothness condition does not strictly hold under ReLU. |

**修改动作 / Action:** 补充使用标准ReLU骨干的消融实验，或在理论部分说明该假设的可放宽范围。

---

### 7. 训练策略一致性 | Training Strategy Consistency
**来源 / Source:** R2

| 中文 | English |
|------|---------|
| 未说明所有基线方法是否采用与所提方法完全一致的训练配置。 | Not clarified whether all baselines use exactly the same training configuration as the proposed method. |

**修改动作 / Action:** 在实验设置节明确说明所有方法的超参数配置统一原则，或对差异进行说明。

---

### 8. 计算时间分析 | Wall-clock Training Time
**来源 / Source:** R3

| 中文 | English |
|------|---------|
| 对偶变量维护和近端更新引入额外计算开销，但训练时间分析仅在补充材料中一带而过。 | Dual variable maintenance adds computational cost; wall-clock analysis is only briefly touched in supplementary. |
| 未分析随数据集或模型规模增大时的时间缩放行为。 | No analysis of time scaling as dataset/model size increases. |

**修改动作 / Action:** 在主文中增加与基线的训练时间对比表，并报告不同数据集规模下的实测时间。

---

## 【小修改 / Minor Fixes】

### 9. 冗余公式定义 | Redundant Formula Definitions
**来源 / Source:** R2

| 中文 | English |
|------|---------|
| 公式(I.1)与公式(III.1)重复定义，建议删除冗余项。 | Formula (I.1) repeats Formula (III.1); remove the redundant definition. |

### 10. 相关工作更新 & 参考文献格式 | Related Work & Reference Formatting
**来源 / Source:** R2

| 中文 | English |
|------|---------|
| 相关工作节缺少近五年文献综述。 | Related Work section lacks coverage of literature from the past five years. |
| 参考文献[32]和[37]存在格式错误，需更正。 | References [32] and [37] contain formatting errors; correct accordingly. |

### 11. 独立运行方差报告 | Variance Across Independent Runs
**来源 / Source:** R3

| 中文 | English |
|------|---------|
| 主表中未报告10次独立实验的方差/标准差。 | Main table does not report variance/standard deviation from 10 independent runs. |

**修改动作 / Action:** 在主结果表中以下标或括号形式补充标准差。

---

## 优先级总结 | Priority Summary

| 优先级 | 问题 | 工作量 |
|--------|------|--------|
| 🔴 关键 | 补充2024–2025 SOTA + Transformer基线 | 高（需重跑实验） |
| 🔴 关键 | StoRM vs. StoM矛盾深度分析 | 中（理论+实验） |
| 🔴 关键 | 大规模数据集实验（MS COCO / ImageNet-1K） | 高（需重跑实验） |
| 🟡 重要 | 16-bit劣势根因分析（t-SNE等） | 中 |
| 🟡 重要 | 内存开销$O(Nd)$量化分析 | 低–中 |
| 🟡 重要 | ELU→ReLU鲁棒性实验 | 中 |
| 🟢 次要 | 训练时间对比、方差报告、冗余公式、文献格式 | 低 |
