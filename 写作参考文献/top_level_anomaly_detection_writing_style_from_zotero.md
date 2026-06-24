---
title: "Top-level anomaly detection writing style from Zotero"
created: "2026-06-24"
source: "Local Zotero SQLite snapshot; filtered by user top-level tags/remarks"
zotero_filter: "Primary: ⭐⭐⭐ tagID=34; Secondary: venue/extra markers such as CVPR, AAAI, ICML, JCR Q1, 中科院1区; topic filter: anomaly/defect/industrial"
output_goal: "Writing style and thinking references for our first industrial anomaly detection paper"
---

# 1. 筛选规则

本笔记不再按“我认为哪些 venue 顶”来筛，而是优先采用 Zotero 内部已有等级标注。

本次在 Zotero 中确认到三个星级标签：`⭐`、`⭐⭐`、`⭐⭐⭐`。其中 `⭐⭐⭐` 对应 tagID=34，共 28 个条目。筛选时只把以下条目作为主样本：

1. 带 `⭐⭐⭐`；
2. 主题与 anomaly detection / defect generation / industrial inspection 直接相关；
3. 属于 conferencePaper，或 venue/metadata 中明确出现 CVPR、AAAI、ICML 等顶会信息；
4. 去除 annotation、note、通用 diffusion 基础论文、普通 survey、普通期刊论文。

保留但降级使用的条目：Zotero 标为 `⭐⭐⭐` 但 venue 未写明或仍是 preprint 的直接竞争工作，例如 InvAD、MSFlow、VQ-flow、Multi-flow。它们用于 related work 风险边界，不作为“顶会写作风格”的主样本。

# 2. 主样本清单

| Role | Zotero key | Title | Zotero level / venue evidence | 可学习的写作切入点 |
|---|---|---|---|---|
| Defect generation / CVPR-style problem framing | `BZEEMHNP` | DefectFill: Realistic defect generation with inpainting diffusion model for visual inspection | `⭐⭐⭐`; venue field contains `2025 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)` | 从“缺陷样本稀缺”切入，但马上收窄到“真实缺陷生成困难”；强调 realism、few reference defects、mask/object/attention losses |
| Few-shot anomaly generation | `Q3I3ELAS` | AnomalyDiffusion: Few-shot anomaly image generation with diffusion model | `⭐⭐⭐`; venue field: `Proceedings of The AAAI Conference On Artificial Intelligence` | 从“异常数据不足”切入，再具体化为“生成真实性差、异常与 mask 对齐差”；方法模块直接对齐 authenticity 和 alignment |
| Efficiency / real-time deployment | `DI6V4V6P` | OmiAD: One-step adaptive masked diffusion model for multi-class anomaly detection via adversarial distillation | `⭐⭐⭐`; venue: `International Conference on Machine Learning`; tag: `one-step` | 典型顶会叙事：先承认 diffusion 强，再指出 iterative denoising 太慢；贡献不是“又一个 diffusion”，而是 one-step + adaptive mask + distillation |
| Global/local reconstruction difficulty | `XYQIJ5II` | GLAD: Towards better reconstruction with global and local adaptive diffusion models for unsupervised anomaly detection | `⭐⭐⭐`; conferencePaper; tag includes `Adaptive Denoising Process` | 把问题拆成 global difficulty 和 local anomaly treatment，不泛泛说 reconstruction bad；方法名和模块名直接服务这个拆分 |

# 3. 直接竞争但不作为主样本的高等级条目

这些条目在 Zotero 中也带 `⭐⭐⭐`，与我们的 normality-flow 方向高度相关，但本地 metadata 没有明确顶会 venue，建议作为 related work 风险边界：

| Zotero key | Title | 用法 |
|---|---|---|
| `XBAKTWR7` / `NRCXKUPT` | InvAD: Inversion-based reconstruction-free anomaly detection with diffusion models | 防止我们把“reconstruction-free”说成独占创新；需要强调我们的 normality-flow 是 feature/class/local/relation decomposition，而不是单纯避免 reconstruction |
| `MQTN7VYD` | MSFlow: multi-scale flow-based framework for unsupervised anomaly detection | flow 方向的 multi-scale baseline；写 related work 时必须承认 flow 已用于 multi-scale localization |
| `MXBUHMEP` | VQ-flow: taming normalizing flows for multi-class anomaly detection via hierarchical vector quantization | multi-class flow 风险；我们的 class-conditional normality-flow 需要区别于 VQ 概念分离 |
| `TQ2TFBGK` | Multi-flow: multi-view-enriched normalizing flows for industrial anomaly detection | multi-view flow 风险；若我们碰 Real-IAD/multi-view，必须写清楚是否建模 view relation |
| `HDDYD9GJ` | DiAD: a diffusion-based framework for multi-class anomaly detection | multi-class diffusion anchor；适合支撑“multi-class setting needs category/structure preservation”的背景 |

# 4. Top 论文的共同思考方式

## 4.1 不是从“大背景”开始，而是从一个矛盾开始

顶会论文通常不会长篇写“工业质检很重要”。它们更快进入一个可验证矛盾：

- 缺陷样本稀缺，但模型需要学到真实异常外观；
- diffusion 生成/重建能力强，但多步采样不适合实时检测；
- reconstruction 能恢复图像，但不同异常的恢复难度不一样；
- multi-class 场景更真实，但类别结构和局部异常容易互相干扰；
- flow 能建模正常分布，但多尺度、类条件、视角关系还没有被充分拆解。

这对我们的启发：Introduction 第一段后半就要出现矛盾，不要只写应用重要性。

## 4.2 先承认已有方法强，再指出它强在哪里仍不够

高水平写法不是“existing methods fail”。更像：

```text
Diffusion/flow/memory/distillation methods have achieved strong performance on industrial anomaly detection benchmarks. However, their success relies on [specific assumption], which becomes problematic under [specific setting].
```

对我们的 normality-flow 论文，建议这样写：

```text
Feature memory, reconstruction, distillation, normalizing flow, and diffusion methods have improved one-class industrial anomaly detection. Yet most of them treat normality as either nearest-neighbor consistency, pixel reconstruction, likelihood, or denoising behavior. This makes it difficult to jointly expose class-specific, local, and relational deviations in realistic multi-class inspection.
```

## 4.3 方法模块必须一一对应 failure mode

DefectFill、AnomalyDiffusion、OmiAD 的共同点是：每个模块都不是装饰，而是回应一个明确痛点。

可迁移到我们论文的模块写法：

| Failure mode | 我们的方法模块应如何命名/解释 |
|---|---|
| feature-space normality 不紧致 | normality-flow encoder / compact normal transport objective |
| multi-class normal pattern 混淆 | class-conditional normality token / conditional vector field |
| tiny defect localization 弱 | multi-scale local normality residual / patch-wise transport discrepancy |
| logical anomaly 不只是像素异常 | relation-aware normality graph / part-relation consistency flow |
| inference score 不稳定 | calibrated normality deviation score / category-wise threshold stability |

写作上不要只列模块名。每个模块都要用一句话说明：输入是什么、建模哪种 normality、怎样产生 anomaly evidence、解决哪个失败模式。

# 5. Abstract 写作套路

从这些 `⭐⭐⭐` top 条目中抽出的 abstract 逻辑是六步：

1. 任务约束：normal-only / few-shot / scarce defect data / multi-class industrial AD。
2. 现有强方法：diffusion / reconstruction / generation / flow 已经有效。
3. 精确失败：slow iterative denoising、poor realism、mask misalignment、uniform treatment of anomalies、category/structure preservation。
4. 方法身份：不是泛泛的 framework，而是一个带动作的机制名，如 one-step adaptive masked diffusion、inpainting diffusion defect generation、global/local adaptive denoising。
5. 组件映射：每个组件对应一个失败点。
6. 证据边界：数据集、metric、runtime/realism/localization/multi-class，不做无限外推。

我们第一篇 anomaly detection 论文 abstract 可按这个骨架：

```text
Industrial visual anomaly detection requires learning normal visual variation from anomaly-free training data and localizing unseen defects under realistic multi-class inspection. Existing memory, reconstruction, diffusion, and flow-based methods have achieved strong benchmark performance, but they often model normality through a single evidence source, making it difficult to simultaneously preserve class-specific patterns, local details, and structural relations. We formulate anomaly detection as normality-flow modelling in feature space, where deviations are exposed by [core score]. The proposed framework contains [module 1], [module 2], and [module 3], which respectively address [class conditionality], [local defect precision], and [relation-aware normality]. Experiments on [datasets] evaluate image-level detection, pixel-level localization, robustness, and inference cost. Results suggest that normality-flow modelling provides a reconstruction-free route for [bounded setting], while [limitation] remains open.
```

# 6. Introduction 框架

建议写 6 段：

1. 工业检测现实约束：异常少、类别多、缺陷形态未知、需要定位。
2. 方法谱系快速综述：memory / reconstruction / distillation / flow / diffusion / VLM，不展开成 survey。
3. benchmark 变化：MVTec AD 已饱和，VisA、Real-IAD、LOCO、AD2 更强调 multi-class、multi-view、logical、lighting/border/tiny defects。
4. 核心缺口：已有方法没有把 normality 拆成 class-specific、local、relation-aware 三层证据。
5. 我们的 normality-flow：一句话给出 formulation，再列 2-3 个机制。
6. 贡献：每条贡献必须能对应实验表、消融、可视化或 runtime。

# 7. 对我们论文最重要的写作警戒线

- 不要写“first Flow Matching for anomaly detection”。Zotero 里已有 flow matching/flow/diffusion 相关高等级或高风险条目。
- 不要只拿 MVTec AD 做 SOTA 叙事。至少在文字上把 VisA、Real-IAD、LOCO、MVTec AD 2 作为评价边界。
- 不要把“reconstruction-free”当唯一卖点。InvAD 这类条目已经在做 reconstruction-free diffusion/anomaly scoring。
- 不要把方法模块写成堆料。顶会论文的模块通常是 failure-mode driven。
- 不要过度承诺 deployment。若提工业部署，必须给 latency、memory、threshold stability 或 calibration evidence。

# 8. 可直接迁移的段落模板

## Related Work 结尾句

```text
These methods demonstrate that modelling normal samples can be highly effective, but they usually instantiate normality as nearest-neighbor consistency, reconstruction fidelity, likelihood, or denoising behavior. In contrast, our goal is to decompose normality into class-conditional, local, and relational evidence, so that deviations can be exposed without relying on anomalous training samples or pixel-space reconstruction alone.
```

## Method 开头句

```text
We view industrial anomaly detection as a normality-flow modelling problem. Given anomaly-free training images from one or multiple product categories, the model learns how normal visual features should move toward a compact normal manifold. At inference time, regions that require inconsistent or high-cost normality transport are treated as anomaly evidence.
```

## Contribution 写法

```text
Our contributions are threefold. First, we formulate reconstruction-free industrial anomaly detection as feature-space normality-flow modelling. Second, we introduce [class/local/relation module] to separate category-specific normal patterns from local defect evidence. Third, we evaluate the method under [datasets/settings], including [localization/multi-class/robustness/runtime], to test whether the formulation remains reliable beyond saturated single-class benchmarks.
```
