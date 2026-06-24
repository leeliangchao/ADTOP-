---
title: "Qualified study of top anomaly-detection paper writing patterns"
created: "2026-06-24"
workspace: "D:/1博士/SCI/第一篇"
purpose: "学习 top-level anomaly detection 论文的写作风格、思考方式和审稿防线，为第一篇 anomaly detection / normality-flow 论文写作提供直接模板"
sources:
  - "Local Zotero metadata, tags, abstracts, and semantic-search context"
  - "Official CVF/AAAI/arXiv pages listed in the source section"
  - "Local writing guides: industrial-cv-paper and research-paper-writing"
---

# 0. 合格标准

这份学习结果只在满足以下标准时才算合格：

1. 不是把论文按年份罗列，而是拆出每篇 top 论文的「问题矛盾 -> 方法动作 -> 证据动作 -> 审稿防线」。
2. 不是总结别人做了什么，而是提炼我们写第一篇 anomaly detection 论文时可以复用的叙事模板。
3. 不把 normalizing flow / Flow Matching 本身包装成未经验证的绝对创新，而是把 novelty 落在「normality-flow 如何建模正常性、如何定位异常、如何跨类别/跨尺度工作、如何比 reconstruction/memory/diffusion 更合适」。
4. 每个建议都能落到论文具体位置：Abstract、Introduction、Related Work、Method、Experiments、Contribution、Limitations。
5. 对审稿人最可能攻击的问题给出预设防线：贡献是否足够、是否只是换骨干、是否缺强 baseline、是否只在饱和数据集上有效、是否没有真实工业意义。

本文件的定位是「写作与思考方式训练集」，不是最终投稿综述。最终投稿前仍需要逐条核验 citation、venue、baseline 数字和实验协议。

# 1. 样本构造

我采用双轨样本，而不是只按 Zotero 星级筛选。

- A 轨：Zotero 中带有 `⭐⭐⭐`、`top`、`一区/top`、或明确高等级备注的 anomaly detection 论文。这一轨反映我们库里已经标注为重点的研究方向，尤其是 diffusion、few-shot generation、masked diffusion、adaptive diffusion 等新近方向。
- B 轨：即使 Zotero 没有星级，也必须纳入的领域骨干顶会论文。这一轨包括 DRAEM、PatchCore、Reverse Distillation、DeSTSeg、SimpleNet、Real-IAD、DiAD 等，因为它们构成近年 industrial visual anomaly detection 写法的主干。

这样筛选的原因是：写 top 论文不能只学「我们标星的论文」，还要学领域评审已经默认熟悉的代表作。否则 Introduction 和 Related Work 会漏掉审稿人的隐性参照系。

# 2. Top 论文写作动作矩阵

| 论文 | 它在领域里的写作角色 | 问题矛盾怎么写 | 方法动作怎么写 | 证据动作怎么写 | 对我们 normality-flow 论文的启发 |
|---|---|---|---|---|---|
| DRAEM, ICCV 2021 | 从 reconstruction 转向 discriminative localization 的代表 | 生成式重建能发现异常，但常依赖后处理，异常定位不够直接 | 把异常检测写成「学习联合的重建-判别表示与决策边界」 | 用 MVTec AD 上的 detection/localization 支撑端到端判别优势 | 如果我们不用重建，就要明确说：normality-flow 不是修补重建误差，而是直接建模正常特征分布与异常偏离 |
| PatchCore, CVPR 2022 | normal-only / cold-start 工业设定的标杆写法 | 工业场景几乎只有正常样本，系统既要高召回又要可部署 | 用预训练特征和代表性 memory bank 捕获 nominal patch context | 主结果、localization、few-shot、推理效率一起证明 total recall | Introduction 可学习它的开法：先讲「无异常样本 + 局部缺陷 + 召回要求」，再引出正常性建模 |
| Reverse Distillation, CVPR 2022 | teacher-student 路线的机制化写法 | 同构 teacher-student 容易复制异常，normal/abnormal discrepancy 不够大 | 用 teacher encoder 与 student decoder 的反向蒸馏制造结构差异，并加 one-class bottleneck | 通过 anomaly map 和 ablation 证明 discrepancy 真的来自结构设计 | 我们的方法模块不能只说有效，要说明为什么它会放大 normal/abnormal 的可分性 |
| CFLOW-AD, WACV 2022 | flow/density 方向的直接近邻 | 工业检测需要实时 localization，复杂模型部署成本高 | 条件 normalizing flow 在多尺度预训练特征上估计似然 | 用速度、参数量、AUROC/AUPRO 证明实时性和精度 | 我们必须正面区分 CFLOW/FastFlow/CS-Flow：不是泛泛说 flow，而是说明 normality-flow 的条件、尺度、局部关系或训练目标有什么新贡献 |
| DeSTSeg, CVPR 2023 | student-teacher + synthetic corruption 的强写法 | 只靠正常约束的 student-teacher 缺乏显式异常监督，定位边界不稳定 | 用 synthetic corrupted normal images 训练 denoising student 和 segmentation network | 用 segmentation mask supervision、ablation、qualitative map 证明定位提升 | 如果我们使用 synthetic anomaly 或伪异常，要写清它服务于哪个模块，而不是作为通用增强 |
| SimpleNet, CVPR 2023 | 简单方法也能 top 的样板 | 复杂方法未必适合实际部署，核心是域偏置、异常模拟和判别边界 | 在 feature space 中生成 anomaly-like noise，再训练简单 discriminator | 同时展示高 AUROC 和高 FPS，把 simple 写成 technical advantage | 如果我们方法简洁，要像 SimpleNet 一样把「简单」写成机制：减少重建、减少迭代、避免 memory 膨胀 |
| Real-IAD, CVPR 2024 | benchmark/dataset 论文的审稿逻辑 | MVTec AD 等数据集已接近饱和，公开基准与真实工业落差明显 | 构建大规模、多视角、真实工业异常数据集和更贴近样本级的指标 | 用 scale、object diversity、baseline re-evaluation 证明现有方法未解决真实场景 | 我们不能只在 MVTec AD 上讲 top 贡献；必须加入更难数据集或明确说明 scope |
| DiAD, AAAI 2024 | multi-class diffusion reconstruction 的写法 | diffusion 重建强，但多类别下类别保持和像素结构完整性会冲突 | 用 semantic guidance 和 spatial feature fusion 约束重建 | 用 multi-class setting、localization、ablation 证明解决的是多类别失真 | 如果我们做 multi-class normality-flow，必须把类别条件写成核心挑战，而不是实验细节 |
| DefectFill, CVPR 2025 metadata / arXiv | 真实缺陷生成方向 | 缺陷样本少，传统合成异常不真实，无法支撑 robust detector | 用 inpainting diffusion 和 defect/object/attention losses 生成更真实缺陷 | 用生成质量、下游检测提升、低质样本筛选证明真实感有用 | 如果我们需要 synthetic abnormal evidence，要防止被问「合成缺陷是否逼真」 |
| AnomalyDiffusion, AAAI metadata / arXiv | few-shot anomaly generation 方向 | 少量异常样本不足，生成图像常有真实性差或 mask 不对齐问题 | 用 spatial anomaly embedding 和 attention re-weighting 对齐异常区域 | 同时评估生成质量与检测收益 | 我们若不走生成路线，可把它作为对照：normality-flow 避免依赖真实异常或生成异常 |
| OmiAD, ICML metadata | one-step masked diffusion / deployment 压力 | diffusion 迭代去噪太慢，难以实时部署 | 用 one-step adaptive masked diffusion 压缩推理过程 | 用速度提升、多指标、多数据集证明实用性 | 任何新模型都要回答速度和部署成本；normality-flow 不能只比精度 |
| GLAD, arXiv / Zotero top tag | adaptive diffusion reconstruction | 不同异常样本和局部异常区域难度不同，统一 denoising 策略不足 | 全局 sample-specific denoising + 局部特征融合 | 用局部/全局 ablation 证明自适应必要性 | 对我们有启发：异常不是统一难度，normality-flow 可写成 spatially/locality-aware normality estimation |
| Anomaly-OV, CVPR 2025 | VLM / zero-shot 新趋势 | 通用 MLLM 有推理潜力，但细粒度工业异常细节识别弱 | 构建 instruction 数据和 look-twice feature matching 的 specialist assistant | 用 zero-shot detection + reasoning 数据集证明开放词汇能力 | 我们不做 VLM 时也要说明边界：本文关注 closed-set/normal-only 工业检测，不抢 zero-shot reasoning 的叙事 |
| RareCLIP, ICCV metadata | CLIP zero-shot online detection 趋势 | 只看单张图的 CLIP 方法忽略异常 patch 的稀有性，batch 方法又有延迟 | 在线序列中估计 rarity，利用 CLIP patch 特征做实时 zero-shot AD | 用 online、latency、no target-domain prior 证明设定价值 | 若我们的数据设定不是 zero-shot，要在 Related Work 中分清 supervised / unsupervised / zero-shot / online |

核心结论：顶会论文的「创新」不是先声明一个新模块，而是先建立一个审稿人承认的失败机制，再让每个模块看起来像这个失败机制的自然解法。

# 3. 从 top 论文抽取出的共同思考方式

## 3.1 先写真实约束，再写算法缺口

优秀 anomaly detection 论文通常不是这样开头：

```text
Anomaly detection is important. Many methods exist. We propose a new method.
```

它们更接近：

```text
Industrial anomaly detection is constrained by scarce abnormal samples, subtle local defects, strict localization needs, and deployment cost. Existing methods address part of this setting, but their core normality representation fails under [specific condition]. We therefore need [new formulation].
```

这意味着我们的开头必须同时包含两个层面：

- 真实约束：normal-only / few-shot abnormal / multi-class / high-resolution / subtle defect / speed。
- 技术缺口：memory matching、reconstruction residual、teacher-student discrepancy、diffusion denoising、density estimation 各自的失败原因。

只有真实约束，没有技术缺口，会像工程报告；只有技术缺口，没有真实约束，会像方法小修。

## 3.2 先承认强 baseline，再指出它们强在哪里仍不够

顶会写法不会把前人写弱。它们通常先承认：

- PatchCore 类方法强在 nominal patch retrieval。
- Reverse Distillation 类方法强在 teacher-student discrepancy。
- DeSTSeg / DRAEM 强在 synthetic anomaly supervision。
- DiAD / diffusion 类方法强在重建和生成能力。
- CFLOW / FastFlow 类方法强在特征密度建模和高效 localization。

然后指出一个更深的 technical reason：

- memory bank 捕获的是离散近邻，不一定是连续的 normality evolution；
- reconstruction residual 容易受重建保真度和异常修复能力影响；
- teacher-student discrepancy 需要结构差异，否则可能把异常也学进去；
- diffusion reconstruction 有推理成本和类别/结构保持问题；
- flow likelihood 需要说明多尺度、类别条件、局部结构和异常分数校准，否则容易被看成旧 density estimation。

我们论文的 Related Work 必须采用这种写法：先尊重，再分解，再定位。

## 3.3 方法模块必须一一对应 failure mode

顶会 Method 写法的底层公式是：

```text
Failure mode -> Module design -> Mechanism explanation -> Verifiable evidence
```

对 normality-flow，可以写成：

| Failure mode | 对应模块 | 机制解释 | 需要的证据 |
|---|---|---|---|
| 正常特征分布是连续且多模态的，memory bank 只能离散检索 | feature-space normality-flow | 学习从 normal features 到 canonical normality distribution 的映射或演化 | 与 PatchCore/PaDiM/FastFlow 的主表比较 |
| 多类别物体的正常模式差异大，共享模型容易混淆类别条件 | class-conditional / object-conditional normality modeling | 条件变量控制正常性流形，减少类别间错误匹配 | multi-class vs per-class / without conditioning ablation |
| 小缺陷是局部的，全局图像分数无法稳定定位 | multi-scale local normality scoring | 在不同层级聚合局部 normality deviation | pixel-AUROC、AUPRO、PRO、qualitative heatmap |
| 真实工业异常可能不符合合成异常分布 | reconstruction-free / anomaly-free training | 不依赖异常生成质量，把训练重点放在 normal distribution | 与 DRAEM/DeSTSeg/DefectFill 类思路的边界比较 |
| 新模型可能更慢、更复杂 | lightweight inference / no iterative denoising | 避免 diffusion 多步采样和大 memory bank | FPS、latency、parameters、memory footprint |

如果某个模块找不到对应 failure mode，就不该在论文中被写成核心贡献。

## 3.4 贡献句要写成「贡献 + 技术优势」

弱贡献句：

```text
We propose a normality-flow module for anomaly detection.
```

合格贡献句：

```text
We propose a reconstruction-free normality-flow framework that models the evolution of normal feature distributions, enabling localized anomaly scoring without relying on image reconstruction residuals or large nearest-neighbor memory banks.
```

更像 top 论文的贡献句：

```text
We introduce a class-conditional normality-flow formulation for industrial anomaly detection. Instead of storing nominal patches or reconstructing abnormal inputs, the proposed framework learns a continuous normality transformation in feature space, which allows category-aware density estimation and multi-scale anomaly localization under normal-only training.
```

它同时回答：我们做了什么、不是做什么、为什么有优势、在哪个设定下成立。

# 4. 可直接迁移到我们论文的结构

## 4.1 Abstract 模板

建议采用「Challenge -> Insight -> Contribution」模板，而不是直接堆模块。

```text
Industrial visual anomaly detection aims to identify and localize defective regions from predominantly normal training images. Recent memory-based, reconstruction-based, distillation-based, and diffusion-based methods have achieved strong performance, yet they often represent normality through discrete patch retrieval, reconstruction residuals, or iterative denoising, which can be sensitive to category variation, subtle local defects, and deployment cost.

In this paper, we argue that anomaly detection should explicitly model how normal feature distributions are organized across categories and spatial scales. Based on this insight, we propose [MethodName], a reconstruction-free normality-flow framework for [setting]. [MethodName] learns [core mechanism] and converts local deviations from the learned normality flow into anomaly scores.

Specifically, [module 1] addresses [failure 1], while [module 2] improves [failure 2]. This design enables [advantage 1], [advantage 2], and [advantage 3] without requiring abnormal training samples.

Extensive experiments on [datasets] show that [MethodName] achieves competitive/superior image-level anomaly detection, pixel-level localization, and practical inference efficiency compared with recent strong baselines including [baselines].
```

注意事项：

- 如果实验还没有大幅超过 SOTA，不要写 `substantially outperforms`。
- 如果只在 MVTec AD 有结果，不要写 `real-world industrial robustness`。
- 如果没有和 diffusion / flow 强 baseline 比，不要声称解决 generative method 的普遍问题。
- 如果没有速度数据，不要写 `deployment-friendly`，只能写 `conceptually avoids iterative denoising`。

## 4.2 Introduction 六段式

第一段：任务与工业约束。

```text
Industrial visual anomaly detection is essential for automated quality inspection, where models are expected to detect subtle local defects under limited or no abnormal training samples. This setting differs from generic defect classification because anomalies are rare, heterogeneous, and often localized at small regions.
```

第二段：现有方法谱系，先承认有效。

```text
Recent methods have made substantial progress by modeling nominal patch distributions, reconstructing normal counterparts, enforcing teacher-student discrepancy, or using generative/diffusion priors. These methods provide strong baselines and have pushed several benchmarks close to saturation.
```

第三段：提出核心 unresolved contradiction。

```text
However, the way normality is represented remains a bottleneck. Memory-based methods approximate normality by discrete nearest neighbors, reconstruction-based methods rely on residuals that can be confounded by reconstruction quality, and diffusion-based methods often introduce iterative inference and category-preservation challenges. These limitations become more visible when defects are subtle, categories are diverse, or deployment cost is constrained.
```

第四段：我们的 insight。

```text
We observe that normal industrial patterns are not merely a set of stored patches or reconstructed pixels; they form structured feature distributions whose deviations can be measured locally and conditionally. This motivates us to model normality as a learnable feature-space flow rather than as a post-hoc distance or reconstruction error.
```

第五段：方法概述与模块映射。

```text
Motivated by this observation, we propose [MethodName], a [class-conditional / multi-scale / reconstruction-free] normality-flow framework. The framework contains [module 1], [module 2], and [module 3]. [Module 1] addresses [failure], [module 2] handles [failure], and [module 3] converts normality deviation into localized anomaly maps.
```

第六段：实验与贡献。

```text
We evaluate [MethodName] on [datasets] with image-level and pixel-level metrics, compare it with recent memory-based, distillation-based, reconstruction-based, flow-based, and diffusion-based baselines, and conduct ablations for each module. The results show [bounded empirical claim].
```

贡献列表不要超过三条：

1. Formulation contribution: reconstruction-free / class-conditional / feature-space normality-flow。
2. Mechanism contribution: multi-scale local normality deviation scoring。
3. Evidence contribution: broad evaluation with strong recent baselines, ablation, speed, and failure cases。

## 4.3 Related Work 分组

推荐四组，不按年份罗列。

### Memory and statistical feature modeling

写作目的：说明 PatchCore/PaDiM 等很强，但它们主要是离散匹配或统计近似。

结尾句模板：

```text
Different from these methods, our framework does not rely on storing or searching a large set of nominal patches; instead, it learns a continuous normality transformation in feature space and derives anomaly scores from local deviations to the learned normal distribution.
```

### Reconstruction, generation, and diffusion

写作目的：承认 DRAEM、DiAD、AnomalyDiffusion、DefectFill、GLAD 等的重建/生成能力，同时指出推理成本、类别保持、合成真实性或 residual ambiguity。

结尾句模板：

```text
Rather than reconstructing a normal counterpart or generating synthetic defects, our method focuses on normal-only feature distribution modeling, which avoids making anomaly localization depend on reconstruction fidelity or generated-defect realism.
```

### Teacher-student and discriminative localization

写作目的：承认 RD、DeSTSeg、SimpleNet 等通过 discrepancy 或 synthetic anomaly supervision 建立判别信号。

结尾句模板：

```text
Our method shares the goal of enhancing normal-abnormal separability, but it obtains the signal from learned normality deviation instead of explicit abnormal simulation or teacher-student reconstruction discrepancy.
```

### Flow and density-based anomaly detection

写作目的：这是最危险也最关键的一组。必须正面讲 CFLOW-AD、FastFlow、CS-Flow 等，不能回避。

结尾句模板：

```text
Compared with existing flow-based anomaly detectors that estimate feature likelihoods with conditional normalizing flows, our work focuses on [exact distinction: e.g., normality-flow objective, class-conditional alignment, spatial relation modeling, multi-scale deviation calibration]. This distinction allows the proposed framework to address [specific failure] while retaining efficient likelihood-style anomaly scoring.
```

这里的 `[exact distinction]` 必须由真实方法决定。没有实验和机制支撑时，不要写成泛泛的 `more effective flow model`。

## 4.4 Method 写法

Method 的每个 subsection 都按三件事写：motivation、design、advantage。

### Overview

```text
Given a normal training set [notation], our goal is to learn a feature-space normality model that assigns low deviation scores to normal local patterns and high deviation scores to abnormal regions at test time. Figure 2 illustrates the proposed [MethodName]. It consists of a pretrained feature extractor, a [conditional normality-flow module], a [multi-scale local deviation scorer], and an [aggregation/calibration module].
```

### Module 1: Feature-space normality-flow

```text
A key challenge is that industrial normal patterns are multi-modal across object categories and spatial locations. To model this structure, we represent each image by multi-scale feature maps extracted from [backbone]. Given a local feature [notation] and condition [notation], the normality-flow module learns [mapping/process]. The output is used to estimate [density/deviation/transport cost].
```

必须补上的机制句：

```text
This module is expected to work because normal samples provide dense coverage of acceptable local appearances, while anomalous regions deviate from the learned conditional normality transformation.
```

### Module 2: Conditional or multi-scale modeling

```text
A remaining problem is that normal appearances differ across categories and feature scales. We therefore introduce [conditioning/multi-scale fusion] to prevent normal patterns from different categories or resolutions from being compared in an incompatible space.
```

### Module 3: Anomaly score and localization

```text
At inference time, we convert local normality deviations into anomaly maps. Specifically, [step 1], [step 2], and [step 3]. The image-level anomaly score is obtained by [aggregation]. This design preserves local defect sensitivity while producing a stable image-level decision.
```

Method 写作警戒线：

- 不要先堆公式，再解释动机。每个公式前要有「为什么需要」。
- 不要把 backbone 写成贡献。除非你训练了新 backbone，否则它只是实现细节。
- 不要用多个近义词指同一个概念，例如 normality flow / normal flow / feature flow / anomaly flow 混用。
- 如果用了 flow matching，要说明它解决的是 normality modeling 的哪一环，而不是把 Flow Matching 当 novelty 本身。

## 4.5 Experiments 证据链

顶会 anomaly detection 论文的实验不是只比一张 AUROC 表。建议至少构成五层证据：

| 证据层 | 目的 | 必须回答的问题 |
|---|---|---|
| Main comparison | 证明有效性 | 是否超过或接近 recent strong baselines，而不是只超过弱 baseline |
| Localization evaluation | 证明定位价值 | pixel-AUROC、AUPRO、AP/F1 是否支撑 local defect claim |
| Ablation | 证明因果性 | 每个模块拿掉后是否下降，下降是否对应 claim |
| Generalization / harder setting | 证明不是只吃 MVTec AD | VisA、BTAD、Real-IAD、MVTec AD2 或跨类别设置是否成立 |
| Efficiency and cost | 证明实用性 | FPS、latency、参数量、显存、memory bank size、diffusion steps 是否有优势 |

推荐 baseline 分组：

- Memory/statistical: PatchCore、PaDiM、SPADE。
- Distillation: Reverse Distillation、STPM、EfficientAD。
- Discriminative/synthetic: DRAEM、DeSTSeg、SimpleNet。
- Flow/density: CFLOW-AD、FastFlow、CS-Flow 或最接近的公开实现。
- Reconstruction/diffusion: DiAD、GLAD、RD++/diffusion-based methods，视可复现性决定。
- Zero-shot/VLM: WinCLIP、Anomaly-OV、RareCLIP 只在我们声称 zero-shot/open-vocabulary 时必须纳入。

必须做的 ablation：

1. Without normality-flow objective / replace with nearest-neighbor distance。
2. Without class/category condition。
3. Single-scale vs multi-scale。
4. Without local deviation calibration。
5. Different backbones，证明不是只靠 backbone。
6. Speed/memory comparison，特别是与 PatchCore memory bank 和 diffusion inference 相比。

实验结论写法：

```text
The improvement is most visible on [dataset/category/metric], where subtle local defects require accurate normality modeling rather than global image reconstruction. This supports our hypothesis that [module] improves [specific ability].
```

不要写：

```text
The results demonstrate the superiority of our method.
```

要写：

```text
These results indicate that the proposed normality-flow modeling improves [specific metric/setting], especially when [specific condition], while maintaining [efficiency/parameter] comparable to [baseline].
```

# 5. 写作风格：top 论文的句子级特征

## 5.1 句子先给信息，再给态度

弱句：

```text
Our method is very effective and robust.
```

强句：

```text
Our method improves pixel-level localization by explicitly scoring local deviations from a learned class-conditional normality distribution.
```

## 5.2 段落第一句承担 message

每段第一句应该能单独组成 reverse outline。例如：

1. Industrial anomaly detection is constrained by scarce abnormal samples and localized defects.
2. Existing methods represent normality through memory retrieval, reconstruction, discrepancy, or generative priors.
3. These representations remain limited when normality is multi-modal and local deviations are subtle.
4. We therefore model normality as a conditional feature-space flow.
5. The proposed design enables localized scoring without reconstruction or abnormal samples.

如果把每段第一句抽出来后不是一条完整逻辑链，Introduction 就不合格。

## 5.3 少用营销词，多用可验证词

| 避免 | 替代 |
|---|---|
| superior performance | higher image-AUROC and pixel-AUPRO under the same protocol |
| robust | consistent gains across datasets/categories/noise levels |
| effective | improves [metric] by [delta] because [module] |
| novel | differs from [closest method] by [mechanism] |
| practical | reduces inference cost / memory footprint / number of denoising steps |

# 6. 针对我们第一篇论文的审稿防线

| 可能攻击 | 为什么危险 | 写作/实验防线 |
|---|---|---|
| “Flow-based AD 已经有 CFLOW/FastFlow，你的新意在哪里？” | 这是最直接拒稿点 | Related Work 单独列 flow/density；明确 exact distinction；ablation 证明该 distinction 有效 |
| “只是换了特征或 backbone” | 会被判为 incremental | 做不同 backbone 实验；把贡献写在 normality formulation 和 local scoring，而不是 backbone |
| “MVTec AD 已饱和” | 结果很难说服 top venue | 加 VisA、BTAD、Real-IAD、MVTec AD2 或明确 harder setting；引用 Real-IAD 的 benchmark-saturation 逻辑 |
| “没有异常样本却说 localization 强，证据不足” | 定位 claim 容易被质疑 | pixel-AUROC + AUPRO + qualitative + small-defect category analysis |
| “与 diffusion 方法比不公平” | diffusion 是近年强趋势 | 至少讨论 DiAD/GLAD/AnomalyDiffusion/DefectFill 的设定差异；能复现则加表，不能复现则解释 scope |
| “方法复杂但收益小” | top 审稿关注 net value | 报 FPS、参数、显存、训练/推理成本；说明相比 memory/diffusion 的实际收益 |
| “normality-flow 概念不清” | 新名词若不清楚会降低可信度 | 在 Method 开头给严格定义：输入、输出、学习目标、异常分数 |
| “claim 过大” | Abstract/Intro 容易被抓 | 把 all-purpose claim 改成 dataset/setting-bounded claim |

# 7. 可直接放进写作任务的 mini-outline

## Paper story in one sentence

```text
We model industrial anomaly detection as class-conditional local normality estimation in feature space, using a normality-flow formulation to localize deviations without reconstructing images, storing large nominal memories, or relying on abnormal training samples.
```

## Main claim

```text
Normality-flow provides a continuous and condition-aware representation of normal feature distributions, which is better aligned with localized industrial defects than discrete memory retrieval or image-level reconstruction residuals.
```

## Contributions

```text
1. We formulate industrial anomaly detection as reconstruction-free feature-space normality-flow modeling, allowing anomaly scores to be derived from deviations to learned normal feature distributions.
2. We introduce [module] to handle multi-class and multi-scale normality variation, improving local anomaly localization under normal-only training.
3. We provide a comprehensive evaluation against recent memory-based, distillation-based, synthetic-anomaly, flow-based, and generative baselines on [datasets], with ablations and efficiency analysis.
```

## Figure 1 应该画什么

Figure 1 不应该只是 pipeline。最好是 motivated example：

- 左侧：同一个工业类别中正常纹理的局部变化和一个细微异常区域。
- 中间：memory/reconstruction/diffusion/flow baseline 的典型失败机制，用小示意图表示。
- 右侧：normality-flow 如何把正常局部特征映射到可校准的 normal distribution，并在异常区域产生高 deviation。

Figure 2 再画 pipeline：

- Backbone feature extractor。
- Class/object condition。
- Multi-scale normality-flow。
- Local deviation map。
- Image-level score aggregation。

# 8. 阶段学习成果记录

## 阶段 1：合格标准

合格学习结果必须覆盖真实 top 论文，而不是只写泛泛套路；必须拆出 problem framing、method move、evidence move、contribution move 和 reviewer defense。

## 阶段 2：样本筛选

只靠 Zotero 星级不够，因为部分领域骨干论文没有星标；只靠顶会名单也不够，因为 Zotero 中的 `⭐⭐⭐` 备注反映了我们当前课题更关注的新近方向。因此采用双轨：Zotero top 标注 + 领域公认顶会骨干。

## 阶段 3：逐篇拆解

最重要发现：top anomaly detection 论文通常不是从「我有一个新模块」开始，而是从「现有 normality representation 在某个真实工业约束下失败」开始。PatchCore 写 cold-start normal-only，SimpleNet 写 domain bias 与 feature-space synthetic anomaly，Real-IAD 写 benchmark saturation 与真实工业落差，DiAD 写 multi-class diffusion 的类别保持和结构完整性。

## 阶段 4：迁移到我们论文

normality-flow 论文的核心不是声称 flow 新，而是把 flow 写成解决 normality representation 问题的工具。我们的写作主线应是：工业 AD 的异常样本稀缺和局部缺陷要求更好的正常性建模；现有 memory/reconstruction/distillation/diffusion/flow 各有强点但 normality 表示仍有限；normality-flow 用连续、条件化、多尺度的 feature-space normality modeling 来产生可定位的 anomaly deviation。

# 9. 自我反思：这版是否合格

我认为这版已经达到「可辅助写第一篇 top-level anomaly detection 论文」的合格线，原因是：

1. 它不只是综述论文，而是把每篇代表作转成可复用的写作动作。
2. 它没有把 top 写作误解成华丽措辞，而是提炼出审稿逻辑：真实约束、技术失败、机制解法、证据闭环。
3. 它把最危险的 flow/density 近邻方法单独列为防线，避免我们写出“别人已经做过”的 Introduction。
4. 它给出了 Abstract、Introduction、Related Work、Method、Experiments 的具体句式和段落骨架，可以直接进入论文草稿。
5. 它明确限制了 claim 的边界：没有实验就不写 deployment-friendly，没有多数据集就不写 real-world robustness，没有强 baseline 就不写 superiority。

但它还不是最终投稿级材料，原因是：

1. Zotero 中部分 `⭐⭐⭐` 新论文的 venue/status 需要在投稿前逐条核验官方页面。
2. normality-flow 的 exact distinction 取决于我们最终方法细节；如果方法还没有定型，Related Work 里的区别句只能作为占位。
3. 实验蓝图必须在真实结果出来后反向修订 Abstract 和 Contribution，不能先写大 claim 再找数字支撑。

下一步最应该做的是：用这份学习结果反向设计我们论文的 `Figure 1 motivated example`、`Introduction reverse outline` 和 `claim-evidence map`。如果这三件事能闭环，论文叙事才真正接近 top 论文的写法。

# 10. Source links checked

- DRAEM, ICCV 2021: https://openaccess.thecvf.com/content/ICCV2021/html/Zavrtanik_DRAEM_-_A_Discriminatively_Trained_Reconstruction_Embedding_for_Surface_Anomaly_ICCV_2021_paper.html
- PatchCore, CVPR 2022: https://openaccess.thecvf.com/content/CVPR2022/html/Roth_Towards_Total_Recall_in_Industrial_Anomaly_Detection_CVPR_2022_paper.html
- Reverse Distillation, CVPR 2022: https://openaccess.thecvf.com/content/CVPR2022/html/Deng_Anomaly_Detection_via_Reverse_Distillation_From_One-Class_Embedding_CVPR_2022_paper.html
- CFLOW-AD, WACV 2022: https://openaccess.thecvf.com/content/WACV2022/html/Gudovskiy_CFLOW-AD_Real-Time_Unsupervised_Anomaly_Detection_With_Localization_via_Conditional_Normalizing_WACV_2022_paper.html
- DeSTSeg, CVPR 2023: https://openaccess.thecvf.com/content/CVPR2023/html/Zhang_DeSTSeg_Segmentation_Guided_Denoising_Student-Teacher_for_Anomaly_Detection_CVPR_2023_paper.html
- SimpleNet, CVPR 2023: https://openaccess.thecvf.com/content/CVPR2023/html/Liu_SimpleNet_A_Simple_Network_for_Image_Anomaly_Detection_and_Localization_CVPR_2023_paper.html
- Real-IAD, CVPR 2024: https://cvpr.thecvf.com/virtual/2024/poster/30294
- DiAD, AAAI 2024: https://ojs.aaai.org/index.php/AAAI/article/view/28690
- Anomaly-OV, CVPR 2025: https://openaccess.thecvf.com/content/CVPR2025/html/Xu_Towards_Zero-Shot_Anomaly_Detection_and_Reasoning_with_Multimodal_Large_Language_CVPR_2025_paper.html
- DefectFill, arXiv: https://arxiv.org/abs/2503.13985
- AnomalyDiffusion, arXiv: https://arxiv.org/abs/2312.05767
- GLAD, arXiv: https://arxiv.org/abs/2406.07487
