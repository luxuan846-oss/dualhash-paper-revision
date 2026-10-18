# DualHash 项目指令

## 项目入口

开始处理论文前，先读 `notes/progress/PROJECT_STATUS.md`。在那里维护当前进度、待办与历史材料的适用状态。

目标期刊为 **SIAM Journal on Imaging Sciences，SIIMS**。本轮任务是完成强修订稿，建立清楚、扎实、有价值的理论与应用论证。

- `paper/main.tex`、`paper/ex_shared.tex`、`paper/sections/*.tex`：当前稿件。
- `paper/DualHash(2).pdf`：提交版原稿，固定保留。
- `revision/response_letter.tex`：当前回稿信。
- `revision/supplementary/`：编辑决定与审稿报告。
- `revision/research/`、`revision/materal/`：历史研究与核查材料，按问题查阅。
- `notes/project/`：项目背景与来信记录；`notes/progress/`：当前状态；`notes/skills/`：研究方法副本。
- `slides/dualhash_slides.tex`：组会幻灯片。

## 回复与协作

- 默认中文，称呼用户为「老大」。先给核心判断或结果，再说明必要依据。
- 小任务短答；研究讨论保留影响判断的条件和关键推导。一般控制在 250–500 字以内，确有必要时再展开。每句话表达一个主要意思。
- 使用准确、熟悉的词；首次出现的必要术语附简短解释。核心结论加粗，结构按内容安排。
- 代码、路径、论文标题与标准英文术语保留原文。
- 用户明确要求英语交流时，依次给出简短的加粗英文纠正、英文回答、中文翻译；数学表达就近附英文读法，同一表达标注一次。
- 在授权范围内直接完成任务。低风险歧义按合理假设推进；影响正确性的缺失信息用一个简洁问题澄清。

## 数学表达

聊天回复按客户端排版：Codex app 与 VS Code 插件的行内公式用 `$$...$$`；Claude 桌面端与 Cowork 用 `$...$`。独立公式用单独成行的 `$$`，前后留一行，置于正文段落中。终端不支持渲染时使用 Unicode，所需源码用 LaTeX 代码块提供。

写入 Markdown 文件时，行内用 `$...$`，独立公式用 `$$` 块。TeX 源文件遵循原有格式。

聊天公式展开论文自定义宏，如 `\EE` 写为 `\mathbb{E}`、`\bx` 写为 `\mathbf{x}`。命令参数显式加花括号，使用标准数学符号。长推导分成简单公式与文字，表格中的竖线使用 `\lvert`、`\lVert`、`\mid`。用户要求渲染公式时直接展示公式。

## 论文写作

以用户的 MoSSP 论文为主要写作参照，源码位于 `/Users/liluxuan/Documents/My_Papers/2_MoSSP/main_body/icml_2026/sections/`：

- `appendices/complexity_analysis.tex`：`Lemma: smoothness-penalty-function`、`Lemma: approximate-sufficient-descent` 及其证明。
- `algorithm.tex`：复杂度结论与 `Remark: role of momentum`。
- `appendices/preliminaries.tex`：`Prop: approximate correspondence` 的证明。

1. **先结论，后推导。** 命题先写假设、结论与常数。优先使用 `Under Assumption ..., F is ...-smooth, where ... .` 等直接句式。
2. **正文给结论，附录给依据。** 正文说明性质、常数出处和证明位置，附录展开推导。
3. **证明沿数学依赖推进。** 从定义、更新式或最优性条件出发，公式承担计算，文字说明使用的假设和关键不等式。
4. **Remark 聚焦作用。** 解释条件、复杂度或结果之间的一项明确关系。
5. **常数与条件就近说明。** 保留影响结论的前提；参数依赖由公式支持。尚未建立的结论说明具体缺口。
6. **围绕贡献组织叙事。** 摘要与引言交代问题、缺口、方法和结果；实验承担明确的论证任务。术语和固定表达参照相关文献与本文既有定义。
7. **只写解决当前问题的内容。** 精简重复背景、重复结论和关于未开展事项的说明。修改文章时，参考项目 GitHub 中 `claude.md` 的写作要求。

## 修订与版本

- 审稿意见中的原文、页码和编号，对照 `paper/DualHash(2).pdf`。
- 当前研究内容以 `paper/` 最新源码为准；修订后的页码和编号以对应源码最新编译的 PDF 为准。
- 先修正文，再更新回复；概念首次出现处、后续使用处和附录保持一致。
- 以提交版为标色基准，仅给新增或修改部分使用 `\blue{...}`。新增或整体重写的证明可整体标蓝；局部变化只标对应部分。用户指定标红的内容遵循指定要求。
- 回稿信按原审稿编号逐条回答。直接给出问题答案、修订原文和必要依据，修订摘录用 `\blue`、位置用 `\green`。
- 首页概括四至五项主要变化；页码与行号在最终编译后统一填写。回应审稿意见时明确分歧来源；原文确有表述问题时承认并修正。
- 编辑允许强修订再提交，要求逐条回复及修订标色，并指定 E. Chouzenoux 为期望 AE。来信原文见 `notes/project/editor-correspondence.md`。

## 研究判断与核查

- 从数学结构、原文与实验依据形成独立判断，区分已证结论、合理推断与待验证设想。
- 对等价、蕴含、覆盖等判断，明确对象、方向和条件。优先核对原始来源；存在冲突或影响关键结论时，再用独立证据复核。
- 历史审阅和对话用于定位问题；当前结论以最新源码与证据为准。
- 研究评议可使用 `notes/skills/booming/` 与 `notes/skills/sparkling/`，关注 Taste、Insight、Importance、Novelty、Solid 与 Vision，并以 SIIMS 相近优秀工作校准。
- 研究质量评分以论文的贡献、正确性和证据为依据；回稿信用于理解背景。

## 工作范围与同步

- 围绕当前任务解决有证据支持的问题，采用满足需求的最小修改。检查应服务于明确的正确性或交付要求；验证充分后完成交付。
- 修改数学或实验内容后，更新项目状态中的成果位置、未解决问题与验证记录。
- GitHub 主仓库为 `luxuan846-oss/dualhash-paper-revision`，工作分支为 `master`。同步时保留用户修改，核对远端差异后提交与推送。
- `paper-overleaf/` 与 `.claude/worktrees/` 是独立工作副本；主仓库提交以当前根目录文件为准。
