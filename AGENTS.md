# DualHash 项目指令

## 项目入口

开始处理论文前，先读 `notes/progress/PROJECT_STATUS.md`。在那里维护当前进度、待办与历史材料的适用状态。按任务只读最省 token 的必要文件或片段，需要时再逐步扩展，不预先通读全部文件。

目标期刊为 **SIAM Journal on Imaging Sciences，SIIMS**。本轮任务是完成强修订稿，建立清楚、扎实、有价值的理论与应用论证。

- `paper/main.tex`、`paper/ex_shared.tex`、`paper/sections/*.tex`：当前稿件。
- `paper/DualHash_submission.pdf`：提交版原稿，固定保留。
- `revision/response_letter.tex`：当前回稿信。
- `revision/supplementary/`：编辑决定与审稿报告。
- `revision/research/`、`revision/materal/`：历史研究与核查材料，按问题查阅。
- `notes/project/`：项目背景与来信记录；`notes/progress/`：当前状态；`notes/skills/`：研究方法副本。
- `slides/dualhash_slides.tex`：组会幻灯片。

## 回复方式

- **开头先给本质。** 每次回复的第一段用 100 字以内说出核心判断或结果，独立成段。随后只写完成当前请求所需的依据或操作结果；用户说「展开」「详细」「一步步」时才扩展正文。结尾不再重复总结。
- 用英语思考，默认中文回复，称呼用户为「老大」；用户要求英文时改用英文。
- 代码、命令、文件路径、包名、模型名、定理名、论文标题、API 名称和专业术语保留英文原文，不附中文译名。首次出现的必要术语给一句短定义。
- 专业问题用正式语气，日常聊天用轻松语气。讨论中不迎合，给出独立、有证据的判断；用户的说法有误时直接指出。
- 一句话只表达一个主要意思，用主动语态和明确主语，直接说明机制与因果。
- 目标读者是论文作者、研究合作者和研究型编程使用者，读完应能完成当前任务所要求的动作或判断。

### 回答长度分档

1. **一句话档**：概念确认、术语或缩写含义、「可以这么理解吗」、是非判断、定位某个结论在哪里。只答一句；理解有误时再补一句修正。不加小节、例子或延伸。
2. **短答档**：解释一个机制、比较两个条件、回答「为什么」。3–5 句或一个短列表。
3. **展开档**：推导、证明、方案设计、文献精读，或用户明说「详细」「展开」「一步步」。按需要展开，只写影响结论的内容。
4. 拿不准时按低一档回答，由用户追问。

### 格式

- 清晰的条目用列表，叙述性内容用段落，有步骤时用编号列表。
- 核心概念和关键结论加粗，不使用 emoji，不用类比。
- 只在结构确实有助于理解时加 Markdown 小节；标题不加括弧补充，正文仅在补充内容较长时使用括弧。
- 不输出「提纲」，不主动提供操作回顾、重复背景或未被要求的延伸说明。
- 自动换行，保证可读性。

## 数学表达

聊天回复按客户端排版：Codex app 与 VS Code 插件的行内公式用 `$$...$$`。独立公式用单独成行的 `$$`，前后留一行，置于正文段落中。终端不支持渲染时使用 Unicode，所需源码用 LaTeX 代码块提供。

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
7. **只写解决当前问题的内容。** 精简重复背景、重复结论和关于未开展事项的说明。

## 修订与版本

- 审稿意见中的原文、页码和编号，对照 `paper/DualHash_submission.pdf`。
- 当前研究内容以 `paper/` 最新源码为准；修订后的页码和编号以对应源码最新编译的 PDF 为准。
- 先修正文，再更新回复；概念首次出现处、后续使用处和附录保持一致。
- 以提交版为标色基准，仅给新增或修改部分使用 `\blue{...}`。新增或整体重写的证明可整体标蓝；局部变化只标对应部分。用户指定标红的内容遵循指定要求。
- 回稿信按原审稿编号逐条回答。直接给出问题答案、修订原文和必要依据，修订摘录与含页码与行号的位置括注（如 `(\mpl{...})`）用 `\blue`。
- 回稿信的结构、颜色、语气、术语与核对标准见 `notes/progress/response_letter_style_guide.md`，起草和修改回复前先读。写法参考其第九节的两份范例：MoSSP rebuttal（被接收，`/Users/liluxuan/Documents/My_Papers/2_MoSSP/review/rebuttal_prograss.md`）借鉴致谢、让步转折、澄清范围与 future work 句式；DualHash NeurIPS 2025 rebuttal（被拒，`revision/materal/DualHash_ A Stochastic Primal-Dual Algorithm with Theoretical Guarantee for Deep Hashing _ OpenReview.pdf`）借鉴划界句式与三段式总结，避免其中的 first、novel 自评和直觉式有界性论证。
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
- `AGENTS.md`（Codex）与 `.claude/CLAUDE.md`（Claude）内容保持一致，仅客户端相关条目不同；修改其中一份时同步另一份。
