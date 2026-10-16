# DualHash 当前项目状态

更新日期：2026-10-16。本文档是当前进度入口；数学内容以 `paper/` 最新源码为准。

## 目标与阶段

- 目标期刊：SIAM Journal on Imaging Sciences（SIIMS）。编辑给予强修订再提交机会，来信见 `../project/editor-correspondence.md`。
- 当前处于论文与回稿信的实质修订阶段。近期工作集中于模型与驻点关系、复杂度分析、初始化、势函数下界及附录组织。
- 本轮准备通过 Booming、Sparkling 对当前成果作独立研究评议。评价 Taste、Insight、Importance、Novelty、Solid、Vision，回稿信完成程度不进入研究评分。

## 当前成果位置

| 内容 | 当前来源与状态 |
|---|---|
| 论文主入口 | `paper/main.tex` 与 `paper/ex_shared.tex`；标题已调整为 W-Type Regularized Deep Hashing 的随机原始—对偶算法 |
| 模型与最优性关系 | `paper/sections/problem_reformulation.tex`、`preliminary.tex` 及 `supplementaryB.tex` 中 `app:optimality-properties`；包含共轭、对偶间隙与驻点对应论证 |
| 算法及采样 | `paper/sections/methodology.tex`；与复杂度章节联合说明 |
| 理论条件与复杂度 | `paper/sections/convergence.tex`；目前明确共轭有效域与有界性条件、初始化、随机信息结构、StoM 与 StoRM 的复杂度结果 |
| 理论证明 | `paper/sections/supplementaryB.tex`；含共同估计、两种算法的势函数下界与复杂度证明 |
| 实验 | `paper/sections/numerical_experiments.tex`、`supplementaryA.tex` 与图片目录；本次同步未开展新实验 |
| 回稿信 | `revision/response_letter.tex` 已有多项实质回复，仍含 summary 占位、`xxx` 和 `Page XX`；这些是交付待办，不参与研究质量评分 |

上述记录表示内容已存在于源码，本次整理未进行完整数学复核或重新编译。

## 下一步

1. 独立评议当前论文，确定最有价值的贡献与一项核心深化任务。完整任务指令见 `notes/prompts/gptpro-review-prompt.md`，方法入口为 `notes/skills/booming/SKILL.md` 和 `notes/skills/sparkling/SKILL.md`。
2. 对评议中发现的实质缺口，定位到具体命题、假设或证明步骤后处理；重点关联原问题、KKT 目标、有限精度保证与实验主张。
3. 正文内容稳定后，更新对应审稿回复、主要变化摘要与修订摘录，编译并核对最终编号、页码和标色。

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
