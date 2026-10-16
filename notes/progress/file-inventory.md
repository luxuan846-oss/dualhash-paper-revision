# 文件用途与精简建议

检查日期：2026-10-16。依据：Git 跟踪状态、工作副本状态、当前 TeX 文件引用和文件内容直接比较。目录大小为本地磁盘占用的近似值。

## 已确认的重复或可再生成内容

| 内容 | 判断与建议 |
|---|---|
| `paper/papers_img/sensitivity_2datasets.pdf` | 与 `paper/results_images/sensitivity_2datasets.pdf` 字节完全相同；当前论文使用后者。前者是可移除的重复副本 |
| `tmp/pdfs/d1-d2-color-review/` | 约 1 MB，四张提交版页面截图，用于历史标色检查，可从提交版重新生成 |
| 论文、回复和幻灯片的编译辅助文件 | 本次统计 22 个常见辅助文件，约 1.14 MiB，包含日志、索引、参考文献输出和 SyncTeX；可再生成，按需清理。SyncTeX 用于编辑器正反向跳转，清理后须重新编译 |
| `paper/tmp1.tmp`、`paper/tmp2.tmp` | 两个零字节临时文件 |

## 归档候选

- `paper/results_images/ex1_4figs.pdf`、`paper/papers_img/V_W_regularization.pdf`：在当前论文、回稿与幻灯片的 TeX 中均未找到文件名引用；可归档为旧图，不能据此认定历史用途已经结束。
- `slides/DualHash_SIIMS_WebSlides.html`：约 33 MB，旧网页幻灯片；独立展示材料，当前论文不使用。若不再展示，可归档。
- `output/pdf/DualHash_可重投期刊与会议.pdf`：约 73 KB，早期投稿去向材料；当前 SIIMS 目标已经明确，适合历史归档。
- `paper/response_letter_templete`：旧模板与返修规则，主要规则已归入 `AGENTS.md`，模板仍可作为写作参考。
- `paper/clean.bat`：Windows 编译文件清理脚本，当前 macOS 工作流未使用。

## 独立工作副本

- `paper-overleaf/`：约 62 MB，独立 Git 仓库，远端为 Overleaf。本次检查工作区无未提交修改；它用于 Overleaf 同步，主仓库忽略该目录。
- `.claude/worktrees/vibrant-dubinsky-1a6e5e/`：约 40 MB，已注册 Git worktree。本次检查无未提交修改，相对检查时的 `master` 无独有提交。确认不再使用后可通过 `git worktree remove` 回收。
- Git 还登记了若干标为 `prunable` 的旧工作副本，目录已不存在；这些只是可清理的登记信息。

## 保留的材料

- `paper/DualHash(2).pdf`、编辑决定与审稿报告是版本核对依据。
- `revision/materal/` 中两份带“副本”的审稿 PDF 与 `revision/supplementary/` 对应文件的字节内容不同；保留可能存在的批注或导出差异。
- `revision/supplementary/0_DualHash.pdf` 与提交版字节内容不同，保留版本区分。
- `revision/research/`、历史对话、有界性草稿与申诉信用于追溯论证；阅读时以最新源码确认当前状态。
- `slides/dualhash_slides.tex` 及其所用图片承担组会展示用途。
- `paper/siamonline220329.cls`、BibTeX 与 `.bst` 文件属于编译依赖。

## 已完成的指令精简

统一 Codex 与 Claude 的项目规则，合并重复格式条款，将固定三次核查和长篇禁止性说明替换为按证据缺口开展核查的工作规则。保留 MoSSP 写作参照、版本区分、修订标色和关键数学前提。编辑来信原文移至 `notes/project/editor-correspondence.md`。
