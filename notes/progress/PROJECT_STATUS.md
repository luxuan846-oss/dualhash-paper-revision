# DualHash 当前项目状态

更新日期：2026-10-17。本文档是当前进度入口；数学内容以 `paper/` 最新源码为准。

## 目标与阶段

- 目标期刊：SIAM Journal on Imaging Sciences（SIIMS）。编辑给予强修订再提交机会，来信见 `../project/editor-correspondence.md`。
- 当前处于论文与回稿信的实质修订阶段。近期工作集中于模型与驻点关系、复杂度分析、初始化、势函数下界及附录组织。
- 本轮准备通过 Booming、Sparkling 对当前成果作独立研究评议。评价 Taste、Insight、Importance、Novelty、Solid、Vision，回稿信完成程度不进入研究评分。

## 当前成果位置

| 内容 | 当前来源与状态 |
|---|---|
| 论文主入口 | `paper/main.tex` 与 `paper/ex_shared.tex`；标题已调整为 W-Type Regularized Deep Hashing 的随机原始—对偶算法 |
| 模型与最优性关系 | §2 `paper/sections/problem_reformulation.tex` 合并模型与预备知识，包含 W 类定义与结构性质、原目标 P0、量化误差 Q、罚模型 P 和基本假设；通用定义保留在 `preliminary.tex`；Q 使用到二值集合的逐元素 ℓ₁ 距离，列出 ℓ₁ W 正则的精确等式及一般 W 类的零值等价；最优性证明见 `supplementaryB.tex` 中 `app:optimality-properties` |
| 算法及采样 | §3 `paper/sections/methodology.tex`；原预备知识中的 StoRM 估计器介绍移至 Recursive Momentum 小节，采样与 §4 复杂度章节联合说明 |
| 理论条件与复杂度 | `problem_reformulation.tex` 模型后列 Basic regularity，W 类讨论保留共轭有效域条件；`convergence.tex` 列 Stochastic first-order oracle，方差条件限定于已有迭代有界区域，StoRM 的 Mean-square smoothness 独立保留；相关引用已统一 |
| 理论证明 | `paper/sections/supplementaryB.tex`；含共同估计、两种算法的势函数下界与复杂度证明 |
| 实验 | `paper/sections/numerical_experiments.tex`、`supplementaryA.tex` 与图片目录；本次同步未开展新实验 |
| 回稿信 | `revision/response_letter.tex` 已有多项实质回复，仍含 summary 占位、`xxx` 和 `Page XX`；这些是交付待办，不参与研究质量评分 |

2026-10-17 完成假设分组、§2 合并及量化误差定义；原有公式与命题标签保留，新增 `eq:quantization-error`。最新 `latexmk -pdf` 编译生成 PDF，无语法错误，仍有 `prop:original-certificate` 未定义引用；模型章、算法章和复杂度章编号分别为 §2、§3、§4。本次未进行完整数学复核。

## 下一步

1. 独立评议当前论文，确定最有价值的贡献与一项核心深化任务。完整任务指令见 `notes/prompts/gptpro-review-prompt.md`，方法入口为 `notes/skills/booming/SKILL.md` 和 `notes/skills/sparkling/SKILL.md`。
2. 对评议中发现的实质缺口，定位到具体命题、假设或证明步骤后处理；重点关联原问题、KKT 目标、有限精度保证与实验主张。
3. 2026-10-16 Booming 研讨确定初步科学问题（出口 A0）：`DualHash_A0_科学问题.md`。主线为「对偶化换来光滑随机速率、付出量化误差」；「对偶相对拆分加 $\operatorname{prox}_h$ 多换来什么」待验证。
4. 正文内容稳定后，更新对应审稿回复、主要变化摘要与修订摘录，编译并核对最终编号、页码和标色。

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
