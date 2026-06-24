---
title: "Supervisor-Skills handbook deep fusion notes for ADTOP"
created: "2026-06-24"
source_repo: "https://github.com/HKUSTDial/Supervisor-Skills"
local_clone: "C:/tmp/Supervisor-Skills"
purpose: "把 Supervisor-Skills handbook 中更系统的写作经验二次融合到 anomaly detection 论文写作流程"
---

# 1. 这次补读了什么

这次不是只看 README，也不是只看本机已安装的 skill 文件，而是 clone 了 `HKUSTDial/Supervisor-Skills` 仓库，并阅读了 handbook 中与写作直接相关的章节：

- `01_Preliminary/1.1_how-to-evaluate-paper-quality.md`
- `03_Paper_Writing/3.1_the-essentials-of-a-research-paper.md`
- `03_Paper_Writing/3.2_introduction-writing-flowchart.md`
- `03_Paper_Writing/3.3_technical-paper-template.md`
- `03_Paper_Writing/3.5_writing-details-and-checklist.md`
- `04_Scientific_Plotting/4.1_motivated-example-figure.md`
- `04_Scientific_Plotting/4.2_solution-overview-figure.md`
- `04_Scientific_Plotting/4.3_experimental-results-figure.md`
- `04_Scientific_Plotting/4.4_plotting-checklist-and-tools.md`
- `06_Case_Studies/6.1_icml2025-alpha-sql-analysis.md`
- `06_Case_Studies/6.2_iclr2025-aflow-analysis.md`
- `06_Case_Studies/6.3_vldb2026-lead-analysis.md`

结论：之前的融合主要吸收了 skill 层的“操作格式”；这次补进了 handbook 层的“写作品味与导师判断”。

# 2. Handbook 给 ADTOP 写作带来的新增规则

## 2.1 论文质量四要素

Supervisor-Skills 从审稿人视角把论文质量归纳为：

1. Novel Problem
2. Novel Method
3. Nice Story
4. Nice Presentation

对我们的 anomaly detection 论文，不能只追求 Novel Method。一个 normality-flow 方法如果只有模型模块，而没有清楚故事和高质量图表，很容易被看成 delta work。

转化到 ADTOP：

| 质量维度 | ADTOP 写作要求 |
|---|---|
| Novel Problem | 除非定义新 setting，否则不要硬装新问题；我们更可能是 Technique Paper |
| Novel Method | normality-flow 的新意必须区别于 CFLOW/FastFlow/diffusion/memory |
| Nice Story | 用真实工业 running example 把 failure mode 讲清 |
| Nice Presentation | Figure 1、Figure 2 和实验图必须先设计，不能最后补 |

## 2.2 写作主线不是“章节顺序”，而是 Why -> What -> How -> So What

Handbook 强调论文写作不是语言表达，而是研究想法的逻辑构造。

转成 AD 写作：

```text
Why:
  工业异常检测为什么值得做，真实约束是什么。

What:
  现有 normality representation 为什么不够，技术挑战是什么。

How:
  normality-flow 如何一一回应挑战。

So What:
  主实验、消融、case study、效率和 harder setting 证明了什么。
```

这比“Abstract/Intro/Method/Experiment”更重要。章节只是容器，Why/What/How/So What 才是写作骨架。

## 2.3 先画主图，再写正文

Handbook 明确建议：第一阶段先画好主图，再写文字。因为图画不清楚，说明作者自己还没想清楚。

对我们来说：

1. 先画 Figure 1 motivated example，再写 Introduction。
2. 先画 Figure 2 method overview，再写 Method。
3. 先设计实验图模板，再写 Results。

这条要写进工作流，因为它能防止我们一开始陷入“写很多空泛段落”。

## 2.4 Figure 1 不是插图，是审稿人 30 秒理解论文的入口

Handbook 对 Motivated Example Figure 的要求比 skill 文件更强：Figure 1 是整篇论文最重要的图，花 1-2 天是正常投入。

对 anomaly detection：

Figure 1 必须同时回答两个问题：

1. 我们到底解决什么问题？
2. 为什么现有方法在这个具体场景失败？

推荐采用：

```text
真实工业样本 -> 现有方法失败 -> normality-flow 视角解释
```

不要只放一个 pipeline，也不要只放一张 heatmap。

## 2.5 实验结果段落不能只报数字，必须给原因和 insight

Handbook 强调：实验不仅展示 performance，还要解释 why it is good。每个实验结果后面应该有 1-2 句分析。

对 ADTOP：

弱写法：

```text
Our method achieves the best AUROC on MVTec AD and VisA.
```

合格写法：

```text
The gain is larger on categories with subtle local defects, suggesting that local normality-deviation scoring captures abnormal regions that are smoothed by reconstruction residuals or diluted by nearest-neighbor patch retrieval.
```

每张实验表要对应一个 finding，而不是堆数字。

## 2.6 AI 顶会论文可以不显式列 Challenges，但不能没有 non-triviality

Alpha-SQL case study 指出：AI 会议论文有时不会单独写 technical challenges，而会直接进入 methodology。但这不等于 challenge 不重要。它只是把 non-triviality 压缩进方法概述。

对我们：

- 如果 Introduction 篇幅紧，可以不写 `This problem has three challenges`。
- 但每个 normality-flow 模块仍必须隐含回答：为什么 naive extension 不够？
- Method 段落必须显示设计不是 3-5 分钟就能想到的简单拼接。

## 2.7 方法部分按 novelty 强弱排序

Handbook 建议：Method 先写最有 novelty 的点，再写次要点；没有 novelty 的已知组件少写。

对我们：

1. normality-flow formulation / objective 最先写。
2. class-conditioned multi-scale modeling 其次。
3. score calibration / anomaly map construction 再写。
4. backbone、optimizer、common preprocessing 放 implementation details，不要抢正文篇幅。

这能避免“backbone 像贡献”的风险。

## 2.8 Related Work 不能抄句，不能过度批评

Handbook 把 plagiarism 作为红线：Related Work 不允许照搬别人的句子。

ADTOP 要求：

- 只用自己的话概括 PatchCore、RD、DRAEM、CFLOW、DiAD 等。
- 直接相关工作可以明确指出 limitation。
- 间接相关工作少批评、多定位。
- 不要用“prior methods fail”这种过度攻击句式，改成“they primarily focus on ... leaving ... less explored”。

# 3. 三个 case study 对 AD 写作的启发

## 3.1 Alpha-SQL: 两类方法对比后沿一条路线创新

Alpha-SQL 的写作模式是：

```text
已有方法 A 有成本问题。
已有方法 B 更实际，但有迁移/generalization 问题。
我们沿 B 路线做机制创新。
```

迁移到 AD：

```text
Memory/retrieval methods are effective but approximate normality discretely.
Reconstruction/diffusion methods are expressive but may depend on reconstruction fidelity or iterative inference.
Flow/density methods are efficient, but their likelihood scoring does not fully resolve local, conditional normality deviation.
We therefore develop normality-flow modeling in feature space.
```

## 3.2 AFlow: 一个技术点可以“kill two birds with one stone”

AFlow 的 case 分析强调：一个核心技术点可以同时解决搜索空间和搜索效率两个问题。

对我们写法的启发：

如果 normality-flow 能同时做到：

- reconstruction-free anomaly scoring；
- localizable heatmap；
- lower inference cost than diffusion；

那就不要把它写成三个散模块，而要写成一个 key idea 的多个技术后果。

写法：

```text
This formulation serves two purposes: it avoids reconstructing test images, and it makes local anomaly scores directly available from feature-space normality deviations.
```

## 3.3 LEAD: 新 setting 必须由自然研究问题推出

LEAD 的 case 是 New Problem/Setting Paper：从两类已有方法的缺陷自然提出 research question。

对我们：

除非我们能提出类似自然问题：

```text
Can we retain the benefits of flow-based likelihood estimation while making anomaly scores locally comparable across categories and feature scales?
```

否则不要把论文定位为 New Setting。更稳妥是 Technique Paper。

但这个问句可以作为 Introduction 的过渡句，而不是 paper type 声明。

# 4. 对现有融合手册的修订要求

`异常检测论文写作操作手册_融合版.md` 应增加以下内容：

1. 明确写出 handbook deep reading 的二次融合来源。
2. 加入 Why/What/How/So What 工作流。
3. 加入“先画 Figure 1/Figure 2，再写 Introduction/Method”的规则。
4. 加入 Figure 1 投入 1-2 天、30 秒理解测试。
5. 加入实验结果段落的 `result -> reason -> finding` 写法。
6. 加入 AI venue 可不显式写 challenges，但必须在 method 中证明 non-triviality。
7. 加入 Method 按 novelty 强弱排序。
8. 加入 Related Work 防 plagiarism 和不过度批评规则。

# 5. 最终使用方式

以后写作时，不应该只说：

```text
按 ADTOP 学习结果写 Introduction。
```

而应该说：

```text
按 ADTOP + Supervisor-Skills handbook 融合流程写 Introduction。
先输出 Why/What/How/So What，
再输出 Figure 1 running example，
再输出六段 reverse outline，
最后做 limitation -> key idea -> challenge -> module -> contribution 一致性检查。
```

这样才真正吸收了 Supervisor-Skills 的 handbook 经验。
