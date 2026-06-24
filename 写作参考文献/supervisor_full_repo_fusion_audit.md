---
title: "Full Supervisor-Skills repository fusion audit for ADTOP"
created: "2026-06-24"
repo: "https://github.com/HKUSTDial/Supervisor-Skills"
local_clone: "C:/tmp/Supervisor-Skills"
repo_commit: "6b46ff1 docs: enrich WeChat community section with benefits and adjust QR code size"
purpose: "完整盘点 Supervisor-Skills 仓库中还有哪些经验需要融合到 anomaly detection 写作体系"
---

# 1. 审计范围

本次不是只看 README，也不是只看已安装 skills，而是完整盘点了 clone 下来的仓库结构：

- root docs: `README.en.md`, `README.md`, `llms.txt`, `CHANGELOG.md`, `CONTRIBUTING.md`, `LICENSE`
- handbook English mirror: `docs/en/handbook/01-06`
- handbook Chinese originals: `handbook/01-06`
- executable skills: `plugins/phd-research/skills/*`
- skill references: `idea-evaluator`, `benchmark-paper-template`, `vibe-research-workflow`, plus已融合过的 `tech-paper-template`, `intro-drafter`, `figure-designer`, `pre-submission-reviewer`
- assets/images and scripts

阅读策略：

- 英文 mirror 与中文 handbook 结构对应，优先读英文 mirror 以避免编码问题。
- 中文原文作为对应源存在，未逐字二次阅读；其方法论内容在英文 mirror 中已覆盖。
- PDF 讲义未解析。它是入门辅导配套讲义，和 handbook 文本高度重叠；除非后续要系统学习课程版，否则不需要融入 ADTOP 写作流程。
- images 资产尝试视觉查看时受 Windows sandbox 限制无法直接打开；但文件名和对应章节已经说明其用途，属于 Figure 范式示例，不包含额外可融合的文字规则。

# 2. 已经融合的内容

以下内容已经进入 `supervisor_skills_fused_ad_writing_playbook.md` 或 `supervisor_handbook_deep_fusion_notes.md`：

1. 论文质量四要素：Novel Problem、Novel Method、Nice Story、Nice Presentation。
2. Technical paper / New Problem / Benchmark 的 paper type positioning。
3. Technical full paper thinking template。
4. Introduction 六段式 flowchart。
5. Running example 与 Figure 1 motivated example。
6. Solution overview Figure 2 和实验图原则。
7. Contribution-section mapping。
8. Pre-submission review：logic、paragraph、grammar、LaTeX、figure、AI-tone。
9. Case studies：Alpha-SQL、AFlow、LEAD 对写作路线的启发。
10. Handbook 深度规则：Why/What/How/So What、先画主图再写正文、result -> reason -> finding、Method 按 novelty 排序。

这些内容已经覆盖“如何把 AD 顶会论文学习结果转成写作流程”。

# 3. 还需要融合的内容

## 3.1 Idea Evaluator: 写作前的 idea gate

需要融合。

原因：我们目前的 ADTOP 体系已经很强地关注“怎么写”，但还缺一个写作前的判断：这个 normality-flow idea 是否值得写成 top-level paper。如果 idea 自身有 fatal flaw，再好的写作也救不了。

应融合内容：

### Five dimensions

把 Higher / Faster / Stronger / Cheaper / Broader 变成 AD 论文的贡献强度雷达：

| Dimension | ADTOP 转译 |
|---|---|
| Higher | 是否显著提升 detection/localization，而不是微小 AUROC 抖动 |
| Faster | 是否比 diffusion/memory bank/large retrieval 更快或更省显存 |
| Stronger | 是否在 Real-IAD、MVTec LOCO AD、MVTec AD2、跨类别、噪声或小缺陷上更稳 |
| Cheaper | 是否降低异常标注、合成异常、训练或部署成本 |
| Broader | 是否把 normality-flow 统一到 reconstruction-free、multi-class、multi-scale、part-relation 等多个 AD 子问题 |

### Fatal flaws

以下 fatal flaws 要写进 ADTOP 写作前检查：

- F1 no novelty versus closest prior work：如果说不清和 CFLOW/FastFlow/PyramidFlow/diffusion 的区别，不能写。
- F3 baseline is not the real baseline：不能只比老方法或弱方法。
- F4 no compelling motivation：没有真实工业 failure case，Introduction 会空。
- F6 unverifiable claim：写 robustness/real-world/deployment 但没有对应实验。
- F8 overly ambitious scope：一篇论文同时想做方法、benchmark、theory、dataset，容易散。
- F9 solution hunting for a problem：从“我想用 Flow Matching”出发找问题，是危险信号。
- F10 no failure case considered：不写失败边界，容易过度 claim。

融合方式：在 playbook 增加 `Step -1: Idea Gate`。

## 3.2 Benchmark Paper Template: benchmark/evaluation 分支

需要融合，但不是并入当前 normality-flow 方法论文主线，而是作为 fork。

原因：我们当前第一篇更像 Technique Paper。但 anomaly detection 领域也可能做 Real-IAD/MVTec AD2/工业真实场景 benchmark 或评测论文。如果未来论文变成 benchmark/evaluation 类型，叙事骨架完全不同。

应融合内容：

1. Five pillars:
   - Research Gap
   - Construction Pipeline
   - Evaluation Framework
   - Empirical Findings
   - Companion Method
2. Benchmark Introduction:
   - Background + Running Example
   - Existing benchmark limitations
   - Research Questions
   - Design Considerations
   - Our Benchmark
   - Contributions
3. Benchmark 实验写法：
   - RQ-driven experiments
   - Finding X summaries
   - Error taxonomy
   - capability boundary analysis
   - research opportunities

ADTOP 转译：

- 如果我们只是做 normality-flow 方法，不启用 benchmark fork。
- 如果我们定义一个真实工业 AD 评测协议、数据集或能力边界分析，必须切换到 benchmark-paper-template，不要用 technique paper 模板硬写。

融合方式：在 playbook 增加 `Benchmark/Evaluation fork`。

## 3.3 Vibe Research / Vibe Writing: 写作过程治理

需要融合。

原因：我们后续一定会用 AI 辅助写作、绘图和代码。如果没有过程红线，容易出现 AI 编引用、AI 腔、claim drift、直接复制生成段落等问题。

应融合内容：

1. AI 只做辅助，不替代研究判断。
2. 用户先写 core claim 和 paragraph skeleton。
3. AI 只做 polish / structure suggestion。
4. 每句 polish 后必须逐句核验。
5. 禁止 AI 编 references。
6. 不能复制 AI-generated paragraphs 作为最终投稿文本。
7. Vibe Figure 可用 AI 生成 draft，但最终 paper figure 必须手工转成 vector。
8. 小步迭代 + Git 提交，降低失控风险。

融合方式：在 playbook 增加 `Vibe Writing integrity gate` 和 `Vibe Figure production loop`。

# 4. 可选择融合，但当前优先级较低

## 4.1 Disruptive Innovation / Paradigm Shift

部分融合。

这对长期博士课题和后续大论文方向有价值，例如维护一个 “important AD problem list”：

- real-world logical anomaly
- multi-view industrial anomaly
- open-world / zero-shot AD
- AD under production threshold constraints
- part-relation normality modeling

但对当前第一篇 normality-flow 方法论文，不能强行追求 paradigm shift，否则容易过度包装。建议作为长期 idea bank，而不是当前写作主线。

## 4.2 Vibe Coding

部分融合。

对实验代码、绘图代码、Git 管理有用，但它不是写作经验本身。可作为工程流程规则，而不是写进论文写作 playbook 的核心。

## 4.3 Assets images

不直接融合。

这些图是 intro flowchart、AFlow overview、canvas good/bad 等范式示例。它们支持我们理解 figure style，但不应复制到 ADTOP。后续如果要画 Figure 1 / Figure 2，可以用这些资产作为视觉参考。

## 4.4 Chinese originals and companion PDF

暂不额外融合。

中文 originals 与 English mirror 对应；PDF 是课程讲义版。当前工作目标是 ADTOP 写作流程，不需要额外把 PDF 内容转录进仓库。若后续要做中文论文写作培训或导师课件版，可以再读 PDF。

## 4.5 scripts / lint

不融合。

`scripts/lint_skills.py` 是插件质量维护工具，不影响 AD 论文写作。

# 5. 需要更新的 ADTOP 文件

应更新：

1. `supervisor_skills_fused_ad_writing_playbook.md`
   - 增加 Step -1 Idea Gate。
   - 增加 Benchmark/Evaluation fork。
   - 增加 Vibe Writing integrity gate。
   - 增加 Vibe Figure production loop。

2. `supervisor_handbook_deep_fusion_notes.md`
   - 可以保留，不必再改；它已经记录 handbook 深度融合。

3. `qualified_top_anomaly_detection_paper_writing_study.md`
   - 不必直接改；它是 AD top paper 学习主体，不应被过多 workflow 内容稀释。

# 6. 总结判断

完整看下来，还必须融合三类内容：

```text
1. Idea gate：判断 normality-flow idea 值不值得写。
2. Benchmark fork：如果论文转向 benchmark/evaluation，必须切换模板。
3. Vibe integrity：AI 辅助写作、绘图、代码时的红线与流程。
```

其余内容要么已经融合，要么是特定场景备用，不应全部塞进主写作手册。
