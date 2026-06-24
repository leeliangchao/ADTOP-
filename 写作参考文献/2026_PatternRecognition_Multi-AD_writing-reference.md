---
title: "Writing reference - Multi-AD"
paper: "Multi-AD: cross-domain unsupervised anomaly detection for medical and industrial applications"
authors: "Wahyu Rahmaniar; Kenji Suzuki"
venue: "Pattern Recognition 172 (2026) 112486"
doi: "10.1016/j.patcog.2025.112486"
local_pdf: "2026_PatternRecognition_Multi-AD_cross-domain_unsupervised_anomaly_detection.pdf"
created: "2026-06-24"
tags: ["paper", "anomaly-detection", "writing-reference", "industrial-visual-anomaly-detection"]
---

# 1. 这篇论文的核心写作策略

这篇 Pattern Recognition 论文的核心策略不是提出一个极其单一的新理论，而是把一个工程上容易理解的目标包装成跨域问题：医学图像和工业图像都缺少异常标注，异常形态又细小、多样、尺度不一，因此需要一个能在正常样本上学习、并在不同域中稳定定位异常的模型。

它的写作逻辑可以概括为：

```text
异常标注稀缺
  -> 单域方法泛化不足
  -> 教师-学生蒸馏能学习正常表征，但定位仍不够稳
  -> 加入通道注意力、判别器、多尺度融合
  -> 用医学数据集 + MVTec AD 证明跨域和定位能力
```

对我们写工业视觉 anomaly detection 论文的启发是：不要先讲模型组件，而要先把异常检测的核心难点拆成几个可验证的失败模式，再让每个模块对应解决一个失败模式。

# 2. 摘要写法

这篇摘要的顺序很清楚：

1. 先提出共同约束：异常检测缺少标注，医学和工业都面临这个问题。
2. 再给出方法身份：Multi-AD 是无监督 CNN 异常检测模型。
3. 随后按模块说明机制：SE block 增强特征提取，KD 迁移教师信息，判别器提高正常/异常区分，多尺度特征处理不同大小异常。
4. 最后用实验范围和关键结果收束：医学 MRI/CT/OCT 和工业 MVTec AD，报告 image-level 与 pixel-level AUROC。

可借鉴点：

- 摘要先写实际约束，再写方法，不急于堆术语。
- 每个模块都绑定一个功能动词：enhance, transfer, distinguish, integrate。
- 结果不只说 outperform，而是给出数据集范围和 image/pixel 两级证据。

不适合照搬点：

- 它把医学和工业同时作为应用域，跨度很大；我们的论文若主攻工业视觉异常检测，应避免把叙事扩得过宽。
- 它主要使用 AUROC，工业 anomaly detection 论文最好补充 AP、AUPRO、FPR@TPR、阈值稳定性、效率和失败案例。

适合我们 normality-flow 方向的摘要骨架：

```text
Industrial anomaly detection requires learning normal visual variation from scarce normal samples and exposing local, global, and structural deviations without anomalous supervision. Existing reconstruction, memory, and flow-based methods improve benchmark accuracy, but they often model normality as a single feature distance or likelihood score, which is fragile under multi-class appearance variation, subtle local defects, and logical anomalies. We formulate industrial anomaly detection as [normality-flow formulation], where [core mechanism] learns [what normality means] and derives anomaly evidence from [inference score]. The method includes [module 1], [module 2], and [module 3], corresponding to [failure mode 1], [failure mode 2], and [failure mode 3]. Experiments on [datasets] using [metrics] evaluate image-level recognition, pixel-level localization, and robustness under [specific settings].
```

# 3. 引言写法

这篇引言的漏斗结构是：

1. 应用重要性：医学早诊和工业质检都需要异常检测。
2. 数据困难：异常样本稀少，标注昂贵，异常形态多样。
3. 现有路径：SSL 可以利用正常样本，KD 可以提升鲁棒性和效率。
4. 未解决问题：现有 KD 多偏单域，跨域特征泛化不足。
5. 方法提出：Multi-AD 用 domain-independent feature representation 与 domain-specific adaptation 处理医学和工业图像。
6. 贡献列表：跨域框架、多尺度融合、判别器/注意力、精确 anomaly map。

可借鉴的思考方式：

- 先定义困难，不先定义模型。
- 每个前人方向只保留一个限制，避免 related work 式堆论文。
- 贡献项不是纯模块名，而是“模块 + 解决的能力”。

对我们更合适的引言漏斗：

```text
工业质检需要正常样本驱动的 anomaly detection
  -> 传统 reconstruction / memory / distillation / flow 方法分别有局限
  -> MVTec AD 和 VisA 已经接近饱和，真实难点转向多类别、细小缺陷、边界缺陷、光照变化和逻辑异常
  -> 当前方法常把 normality 压成单一分数，缺少类别条件、局部尺度和部件关系层面的约束
  -> 我们提出 normality-flow，把正常性建模为跨特征分布、类别条件、局部尺度和结构关系的约束系统
  -> 贡献与实验逐一对应
```

# 4. Related Work 写法

这篇文章的 related work 按应用域分成 medical 和 industrial 两节，并在每节里用方法家族推进：GAN / autoencoder / memory / transformer / feature matching 等。它的优点是读者能看到“为什么这些方法还不够”；缺点是工业异常检测部分相对短，对近年 diffusion、VLM、Real-IAD、LOCO、MVTec AD 2 等现实挑战覆盖不足。

我们写工业 anomaly detection 更建议按功能分组，而不是只按时间或应用分组：

- Reconstruction / generative methods: 说明重建误差容易复制异常或产生模糊定位。
- Feature memory / statistical modeling: 说明检索和局部 patch 距离有效，但内存、速度、位置关系和逻辑异常有风险。
- Teacher-student / distillation: 说明特征差异适合定位，但教师偏置、尺度信息和域变化仍会影响结果。
- Normalizing flow / Flow Matching: 说明密度或速度场提供正常性建模，但需要解释 anomaly score、局部定位和多类别条件化。
- Diffusion-based methods: 单独成类，不要并入普通生成式方法。
- VLM / zero-shot methods: 说明开放词汇能力强，但工业细粒度定位、阈值稳定和类别提示仍是问题。
- Benchmarks and metrics: 用 MVTec AD、VisA、Real-IAD、MVTec LOCO AD、MVTec AD 2 组织评价边界。

# 5. 方法写法

Multi-AD 的方法部分值得学习的是“每个模块都有前置动机”：

- Backbone + SE: 异常可能细小，通道注意力帮助突出相关特征。
- KD: 学生模型模仿教师的多层特征，正常样本驱动地学习正常分布。
- Discriminator: 用对抗判别加强教师/学生特征差异的反馈。
- Multi-scale feature fusion: 不同尺度的异常需要不同层级特征共同定位。

它的模块论证模板是：

```text
现有做法的问题
  -> 本模块接收什么输入
  -> 本模块如何变换特征
  -> 对 anomaly detection 的具体好处
  -> 用公式或结构图固定实现
```

我们写 normality-flow 方法时也应保持这个模板，但要把 novelty 放在异常检测表述上，而不是泛泛说“用了 Flow Matching”：

```text
Feature-space normality flow:
  输入: pretrained / task-specific features
  建模: 正常样本在深层特征空间的连续概率路径或速度场
  输出: deviation score / local anomaly evidence
  解决: reconstruction-free normality modeling

Class-conditional normality flow:
  输入: category label / learned condition token
  建模: 类别条件下的正常外观变化
  输出: category-aware anomaly score
  解决: multi-class collapse 和类别间正常模式混淆

Multi-scale local normality flow:
  输入: 多尺度 patch/feature maps
  建模: 局部正常变化与细粒度偏差
  输出: pixel/patch-level anomaly map
  解决: tiny defect, boundary defect, low-contrast defect

Part-relation normality flow:
  输入: part tokens / relation graph / structural feature
  建模: 部件数量、位置、关系和语义一致性
  输出: logical anomaly evidence
  解决: LOCO 类结构异常和逻辑异常
```

# 6. 实验写法

这篇文章的实验证据链是：

1. 先给硬件、训练设置、输入尺寸、优化器和训练轮数。
2. 说明训练只用正常样本，测试用正常和异常样本。
3. 用 image-level AUROC 和 pixel-level AUROC 评价分类与定位。
4. 分医学和工业两个大表对比 PaDiM、PatchCore、SimpleNet、STFPM、RD4AD、MKD。
5. 用可视化 heatmap 强化定位质量。
6. 用消融表证明 SE、D、MFF 三个模块分别有效。
7. 讨论复杂度和实时性限制。

我们可以借鉴它的证据排列，但需要增强工业论文的 reviewer 防御能力：

- 不只 MVTec AD，最好加入 VisA、Real-IAD、MVTec LOCO AD 或 MVTec AD 2。
- 不只 AUROC，加入 AP、AUPRO、FPR@TPR、threshold stability、latency、memory。
- 表格要标清 one-class / multi-class / zero-shot / few-shot 设置。
- 消融必须对应 claim：如果 claim 是类别条件化，就要有无 class condition 的消融；如果 claim 是局部流场，就要有无 multi-scale/local branch 的消融；如果 claim 是逻辑异常，就必须有 LOCO 或结构异常证据。
- 可视化不能只展示成功案例，要有 failure cases，尤其是 tiny defects、boundary defects、illumination shifts、logical anomalies。

# 7. 这篇论文真正值得学的“思考方式”

第一，问题不是“异常检测效果不够好”，而是拆成跨域泛化、细粒度定位、尺度变化、正常样本学习四个可验证的子问题。

第二，方法组件不是并列堆叠，而是对应一个失败模式：SE 对特征选择，KD 对正常特征迁移，D 对判别反馈，MFF 对多尺度定位。

第三，实验不是只给主表，而是主表、可视化、消融、讨论形成闭环。主表回答“比别人强不强”，可视化回答“定位是否可信”，消融回答“为什么强”，讨论回答“代价是什么”。

第四，top 期刊论文的贡献写法通常不是“我们使用了 X 技术”，而是“我们解决了 Y 场景中的 Z 难点，并用 A/B/C 证据支持”。这点对我们最重要。

# 8. 不能照搬的地方

- 这篇论文的工业部分主要依赖 MVTec AD。对当前工业 anomaly detection 来说，MVTec AD 已经高度饱和，仅靠它不足以支撑强泛化或部署论断。
- 主要指标是 AUROC，缺少 AP、AUPRO、FPR@TPR、阈值稳定性和效率评估。
- 方法由多个成熟组件组成，真正新意更偏工程组合；我们若走 normality-flow，必须把科学问题和建模定义讲清楚，不能只堆模块。
- “cross-domain medical and industrial”虽然显得范围大，但也容易稀释问题边界。我们的论文应优先把工业场景讲深，而不是把应用面铺宽。
- 它的讨论部分承认了复杂度与实时性问题，但还不够量化。我们的论文若提 industrial deployment，必须给 latency、memory 或 calibration cost。

# 9. 给我们论文的直接写作清单

写 abstract 前先回答：

- 我们的核心问题是 feature-space normality、class-conditional normality、local normality，还是 relation normality？
- 哪个现有方法失败模式最直接支撑这个问题？
- 我们的 anomaly score 从哪里来，为什么比 reconstruction error / memory distance / likelihood 更合适？
- 哪些数据集和指标能证明这个 claim？

写 introduction 前先固定 6 段：

1. 工业异常检测的现实约束：异常样本稀缺，缺陷形态多样，定位和误报都重要。
2. 现有方法进展：reconstruction、memory、distillation、flow、diffusion、VLM。
3. 剩余缺口：多类别、细小/边界缺陷、光照/材质变化、逻辑异常、真实部署。
4. 我们的核心观察：normality 不是单一分数，而是跨特征分布、类别条件、局部尺度和部件关系的约束系统。
5. 我们的方法：normality-flow 的具体建模和 anomaly evidence。
6. 贡献：每条贡献都绑定实验或消融。

写 method 前先画出模块-失败模式对应表：

| Failure mode | Existing limitation | Our component | Evidence |
|---|---|---|---|
| Multi-class normal variation | One-class assumptions or category collapse | Class-conditional normality flow | Multi-class setting + ablation |
| Tiny/local defects | Coarse feature scores miss small regions | Multi-scale local flow | Pixel AP/AUPRO + tiny-defect cases |
| Logical anomalies | Patch-level distance ignores part relations | Part-relation normality flow | LOCO / structural benchmark |
| Real-world shifts | Saturated benchmarks overestimate robustness | Robustness evaluation | Real-IAD / MVTec AD 2 / shift tests |

写 experiments 前先确认：

- 主表是否覆盖至少一个饱和基准和一个真实/复杂基准。
- 是否同时有 image-level 和 pixel-level 指标。
- 是否有每个模块的消融。
- 是否有和 claim 对应的定性图。
- 是否报告失败案例和计算成本。

# 10. 一句话结论

这篇论文最值得学习的是“用清晰的问题-模块-证据链组织论文”，而不是具体的 SE、KD、D 或 MFF 组件本身。对我们的 anomaly detection 论文，真正可迁移的是：把 normality-flow 写成一个解决工业异常检测失败模式的建模框架，并让每一条贡献都能在数据集、指标、消融和可视化中被验证。
