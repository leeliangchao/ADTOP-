---
title: "Conference anomaly detection writing framework from Zotero"
created: "2026-06-24"
source: "Local Zotero library; conferencePaper items; semantic index rebuilt with local default embeddings"
zotero_status: "Document count 265; embedding model default; local Chroma path D:\\ReadPaper\\.zotero-mcp\\chroma_db_local_default; last_update 2026-06-24T10:42:56.973970; has_nvidia=False; has_default=True"
topic: "industrial visual anomaly detection; first paper writing framework"
tags: ["writing-reference", "zotero", "conference-paper", "anomaly-detection", "industrial-visual-anomaly-detection"]
---

# 1. Source Scope

This note is extracted from conference-type anomaly detection papers found in the local Zotero library. The Zotero MCP semantic index has been migrated away from the blocked NVIDIA embedding provider and rebuilt with the local default embedding function at `D:\ReadPaper\.zotero-mcp\chroma_db_local_default`. The final CLI status reports 265 indexed documents, embedding model `default`, and the Chroma schema check reports `has_nvidia=False`, `has_default=True`. This framework uses Zotero metadata, abstracts, local semantic search context, and local PDF first-page/intro extraction.

Representative Zotero items:

| Function | Paper | Venue/year | Zotero key |
|---|---|---:|---|
| Synthetic anomaly / discriminative reconstruction | DRAEM | ICCV 2021 | `PIFF6TUJ` |
| Self-supervised anomaly simulation | CutPaste | CVPR 2021 | `SVGKSTPM` |
| Patch distribution modeling | PaDiM | ICPR 2021 | `I2NKPTQF` |
| Feature memory bank | PatchCore | CVPR 2022 | `QJFLPVH2`, duplicate with PDF `6T72EKQN` |
| Reverse / teacher-student distillation | Reverse Distillation | CVPR 2022 | `LHGWMGHZ` |
| Conditional normalizing flow | CFLOW-AD | WACV 2022 | `QJA5WLCL` |
| Dataset / pretraining benchmark | VisA / SPD | ECCV 2022 | `NYSRWXL4` |
| Segmentation-guided student-teacher | DeSTSeg | CVPR 2023 | `C79YIUF2` |
| Simple feature-space classifier | SimpleNet | CVPR 2023 | `7E49DKT2` |
| Diffusion localization | Removing Anomalies as Noises | ICCV 2023 | `B75WMWAJ` |
| Multi-class diffusion | DiAD | AAAI 2024 | `IZVWYTFB` |
| Real-world benchmark | Real-IAD | CVPR 2024 | `GCIBF9BM` |
| One-step diffusion efficiency | OmiAD | ICML 2025 | `K8HHUIVC` |
| Multi-class simple baseline pressure | Dinomaly | CVPR 2025 | `N8TKVTXF` |
| Zero-shot / reasoning | Anomaly-OV | CVPR 2025 | `VGMXLRLF` |
| Zero-shot industrial CLIP | RareCLIP | ICCV 2025 | `LTMFN45P` |

# 2. The Common Conference-Paper Logic

Top conference anomaly detection papers are usually not written as broad surveys. They are written as a tight failure-mode argument:

```text
industrial inspection has scarce anomalies
  -> normal-only / one-class setting is necessary
  -> existing family solves part of the problem
  -> one specific failure mode remains
  -> proposed method changes the training signal / representation / anomaly score
  -> evidence: benchmark table + localization map + ablation + efficiency or harder setting
```

The strongest papers do not open with "anomaly detection is important" for long. They quickly state what makes the setting technically hard: small anomalous pixels, cold-start training, multi-class normal variation, reconstruction over-generalization, memory/inference cost, diffusion speed, benchmark saturation, or zero-shot lack of reasoning.

# 3. Abstract Routine

Conference abstracts in this field follow a six-part routine:

1. Task and constraint: visual anomaly detection under normal-only or data-restricted training.
2. Existing dominant route: reconstruction, memory, distillation, flow, diffusion, VLM.
3. Concrete failure mode: over-generalized reconstruction, costly memory search, weak multi-level fusion, slow diffusion sampling, multi-class category drift, or lack of reasoning.
4. Method identity: one sentence naming the method and its core mechanism.
5. Technical components: 2-3 modules, each tied to a reason.
6. Evidence: datasets, metrics, and one boundary claim.

Reusable abstract skeleton for our first paper:

```text
Industrial visual anomaly detection requires identifying and localizing unseen defects from normal training samples only. Existing [family] methods have improved [benchmark/metric], but they often [specific failure mode], especially under [multi-class/local/logical/real-world condition]. We formulate this problem as [normality-flow statement], where [core mechanism] models normal visual variation in [feature/class/local/relation] space and derives anomaly evidence from [score]. The proposed framework consists of [module 1], [module 2], and [module 3], which respectively address [failure 1], [failure 2], and [failure 3]. Experiments on [MVTec AD/VisA/Real-IAD/LOCO/AD2] with [AUROC/AP/AUPRO/FPR/efficiency] show [bounded result claim]. The method is designed for [setting], while [limitation] remains open.
```

Avoid in the abstract:

- "first Flow Matching method" unless thoroughly verified against current Flow Matching anomaly papers.
- "solves industrial anomaly detection."
- SOTA language based only on MVTec AD.
- a component list without a failure-mode explanation.

# 4. Introduction Framework

The introduction should be six paragraphs, not a long literature dump.

## Paragraph 1: Industrial Constraint

Purpose: establish why normal-only anomaly detection is necessary.

Good moves from PatchCore, CFLOW-AD, DRAEM, DeSTSeg:

- defects are rare and expensive to label;
- anomaly types are long-tailed;
- pixel-level localization matters, not just image-level classification;
- one model should work without handcrafted per-class rules.

Template:

```text
Industrial visual inspection often requires detecting defects whose appearances are rare, diverse, and unavailable during training. This naturally leads to a normal-only anomaly detection setting, where a model must learn normal visual variation from non-defective samples and expose deviations at image and pixel levels during inference.
```

## Paragraph 2: Existing Progress

Purpose: show the field is not empty.

Group by function:

- reconstruction and synthetic anomaly methods: DRAEM, CutPaste;
- pretrained feature distribution and memory: PaDiM, PatchCore;
- student-teacher/distillation: Reverse Distillation, DeSTSeg;
- flow/density: CFLOW-AD, FastFlow-like methods;
- diffusion: Removing Anomalies as Noises, DiAD, OmiAD;
- zero-shot/VLM: Anomaly-OV, RareCLIP.

Do not cite chronologically paper by paper. Use one sentence per family.

## Paragraph 3: The Specific Gap

Purpose: define the paper's battlefield.

Each strong paper chooses one central gap:

- DRAEM: reconstruction alone is not optimized for discriminative localization.
- PatchCore: feature matching is strong, but nominal patch context and memory efficiency are not maximized.
- CFLOW-AD: strong methods are too complex for real-time processing; exact likelihood in feature space can be efficient.
- DeSTSeg: student-teacher methods lack anomaly-side constraints and use empirical multi-level fusion.
- DiAD: diffusion reconstruction struggles with category preservation and pixel-wise structure in multi-class settings.
- OmiAD: diffusion is accurate but too slow and suffers from shortcut reconstruction.
- Real-IAD: MVTec-like benchmarks are saturated and too far from practical multi-view settings.

For our first normality-flow paper, the cleanest gap is:

```text
Existing methods often compress normality into a single reconstruction error, feature distance, likelihood, or prompt similarity. This makes the anomaly score fragile when normal appearance varies across classes, when defects are local and low contrast, or when anomalies involve structure rather than texture.
```

## Paragraph 4: Core Insight

Purpose: introduce the conceptual move before modules.

Use this as the paper's backbone:

```text
We argue that normality should be modeled as a constraint system rather than a single score: normal feature distribution, category-conditioned variation, local multi-scale consistency, and optionally part-relation structure. This motivates a normality-flow formulation that learns how normal samples occupy and move within feature space, and treats deviations from the learned normal path as anomaly evidence.
```

## Paragraph 5: Method Overview

Purpose: explain the method in one figure's worth of text.

Use the Figure 1 style of DRAEM, PatchCore, CFLOW-AD, DeSTSeg, and DiAD:

```text
Given a normal image, the encoder extracts multi-scale features. A class-conditional normality-flow module models the normal feature trajectory/distribution. A local scoring head converts deviations into patch-level anomaly evidence, and an aggregation rule produces image-level scores and pixel-level maps.
```

## Paragraph 6: Contributions

Contribution bullets should be claims, not component names.

Weak:

- We propose a flow module.
- We add a multi-scale branch.
- We conduct experiments.

Stronger:

- We formulate industrial anomaly detection as feature-space normality-flow modeling, reducing reliance on pixel-space reconstruction.
- We introduce class-conditioned and multi-scale local normality constraints to separate normal category variation from true defect evidence.
- We evaluate the method under one-class and multi-class settings on MVTec AD and VisA, and include ablations that isolate the effect of class conditioning and local scoring.

# 5. Related Work Routine

Conference related work should be short, functional, and adversarial in a polite way. Each subsection should end with the limitation that motivates our method.

Recommended subsections:

1. Reconstruction, synthetic anomalies, and discriminative training.
   - Use DRAEM and CutPaste as anchors.
   - Limitation: synthetic anomalies may not cover real defect diversity; reconstruction may over-generalize.

2. Feature distribution and memory-bank methods.
   - Use PaDiM and PatchCore.
   - Limitation: patch-level features are strong but can be memory-heavy and weak on relational/logical anomalies.

3. Student-teacher and distillation.
   - Use Reverse Distillation and DeSTSeg.
   - Limitation: teacher-student discrepancy can be empirical; fusion and anomaly-side constraints matter.

4. Flow and density-based anomaly detection.
   - Use CFLOW-AD and related flow papers.
   - Limitation: density/likelihood alone must be tied to localization, class condition, and local anomaly evidence.

5. Diffusion and generative reconstruction.
   - Use Removing Anomalies as Noises, DiAD, OmiAD.
   - Limitation: reconstruction quality, shortcut behavior, semantic preservation, and inference speed are core issues.

6. Benchmarks, multi-class, real-world and zero-shot settings.
   - Use VisA, Real-IAD, Dinomaly, Anomaly-OV, RareCLIP.
   - Limitation: old one-class benchmark gains do not automatically prove practical robustness.

# 6. Method Section Routine

A strong method section answers five questions for every module:

1. What input does it receive?
2. What normality assumption does it model?
3. How is anomaly evidence extracted?
4. Which existing failure mode does it fix?
5. What supervision is used?

For our normality-flow direction:

| Module | What it should claim | Required evidence |
|---|---|---|
| Feature-space normality flow | Normality is modeled in semantic feature space, not reconstructed pixels | compare to reconstruction/diffusion or feature-distance baseline |
| Class-conditioned flow | One model separates class variation from abnormal deviation | one-for-all vs class-conditioned ablation |
| Multi-scale local scoring | Tiny, boundary, and texture defects remain visible | pixel AP/AUPRO and qualitative maps |
| Relation/global branch, if used | Logical anomalies are not just local texture defects | MVTec LOCO AD or comparable structural benchmark |
| Efficient inference, if claimed | Practical inspection needs bounded latency/memory | FPS, memory, resolution, hardware |

Equation rule:

- Each equation must correspond to training objective, normality path/velocity, anomaly score, map aggregation, or threshold calibration.
- Do not include equations that are only decorative.

Figure 1 rule:

- It should show the whole inference path from image to anomaly map.
- The core novelty should be visually central.
- Include one failure case that existing methods cannot resolve if space allows.

# 7. Experiments Routine

Conference anomaly detection papers usually win or lose in experiments. The minimal credible structure is:

1. Datasets and settings.
   - State one-class / multi-class / zero-shot / few-shot clearly.
   - MVTec AD is necessary but no longer sufficient for broad claims.
   - Add VisA for multi-object comparison.
   - Add Real-IAD for real-world/multi-view claims.
   - Add MVTec LOCO AD for logical/structural claims.

2. Metrics.
   - Image-level: AUROC, AP, F1 if thresholded decisions matter.
   - Pixel-level: AUROC, AP, AUPRO.
   - Robustness/deployment: FPR at high TPR, latency, memory, resolution, calibration cost.

3. Baselines.
   - Reconstruction/synthetic: DRAEM, CutPaste.
   - Feature/memory: PaDiM, PatchCore, SimpleNet.
   - Distillation: Reverse Distillation, DeSTSeg, EfficientAD if available.
   - Flow: CFLOW-AD, FastFlow/PyramidFlow-like competitors.
   - Diffusion: DiAD, Removing Anomalies as Noises, OmiAD-type if claiming diffusion/flow competition.
   - VLM/zero-shot: AnomalyCLIP/WinCLIP/RareCLIP/Anomaly-OV if making zero-shot or generalist claims.

4. Main comparison.
   - Separate image-level and pixel-level tables.
   - Mark same backbone, resolution, setting, and postprocessing.
   - Do not hide one-class vs multi-class differences.

5. Ablation.
   - Remove flow module.
   - Replace normality-flow with ordinary feature distance or normalizing flow.
   - Remove class conditioning.
   - Remove multi-scale local branch.
   - Remove relation/global branch if used.
   - Test resolution / timestep / layer / threshold sensitivity.

6. Qualitative analysis.
   - Show small defect, boundary defect, texture defect, structural/logical defect, lighting shift if claimed.
   - Include failure cases.

7. Efficiency.
   - If the paper mentions industry or real-time, report latency and memory.
   - CFLOW-AD and OmiAD show that efficiency can be a primary contribution, not an appendix.

# 8. Three Viable Storylines For Our First Paper

## Storyline A: Reconstruction-Free Feature-Space Normality Flow

Best if current evidence is mainly MVTec AD / VisA with good pixel maps.

Core claim:

```text
We model normal industrial appearance in feature space and derive anomaly evidence from deviations from learned normality flow, avoiding brittle pixel reconstruction.
```

Need:

- compare to reconstruction and feature-memory baselines;
- show anomaly maps;
- ablate feature layers and flow scoring.

Risk:

- reviewers may say it is another flow variant unless the anomaly score and localization mechanism are clear.

## Storyline B: Multi-Class Conditional Normality Flow

Best if you want one unified model across product categories.

Core claim:

```text
We separate category-conditioned normal variation from abnormal deviations, enabling a unified multi-class model without collapsing class-specific normal patterns.
```

Need:

- one-for-all setting;
- class-conditioned ablation;
- MVTec AD + VisA;
- per-class analysis.

Risk:

- reviewers will compare against UniAD, DiAD, Dinomaly, multi-class diffusion and VLM methods.

## Storyline C: Multi-Scale Local Normality Flow For Subtle Defects

Best if your method has strong local maps and small-defect behavior.

Core claim:

```text
We learn multi-scale local normality constraints so that tiny, boundary, and low-contrast defects remain detectable without relying on global image reconstruction.
```

Need:

- pixel AP/AUPRO;
- tiny/boundary qualitative cases;
- ablate local scales;
- avoid broad logical-anomaly claims unless LOCO evidence exists.

Risk:

- if only MVTec AD is used, reviewers may see it as benchmark tuning.

# 9. First-Paper Writing Blueprint

Recommended paper title shape:

```text
[Core Method Name]: [Specific Normality-Modeling Claim] for Industrial Anomaly Detection
```

Recommended section structure:

```text
1. Introduction
2. Related Work
   2.1 Reconstruction and Synthetic Anomaly Learning
   2.2 Feature Distribution, Memory, and Distillation
   2.3 Flow, Diffusion, and Multi-Class Anomaly Detection
3. Method
   3.1 Problem Formulation
   3.2 Feature-Space Normality Flow
   3.3 Class-Conditional / Multi-Scale Local Normality Module
   3.4 Anomaly Scoring and Map Aggregation
   3.5 Training and Inference
4. Experiments
   4.1 Datasets and Settings
   4.2 Metrics and Baselines
   4.3 Main Results
   4.4 Ablation Study
   4.5 Qualitative Analysis
   4.6 Efficiency and Failure Cases
5. Discussion / Limitation
6. Conclusion
```

The paper should not read like "we apply Flow Matching to anomaly detection." It should read like:

```text
industrial anomaly detection fails when normality is treated as a single score;
we decompose normality into feature distribution, category condition, and local scale;
normality-flow is the mechanism that makes this decomposition trainable and scorable.
```

# 10. Reviewer-Risk Checklist

Before writing strong claims, check:

- If claiming SOTA: are baselines current and same-setting?
- If claiming industrial practicality: are latency, memory, resolution, and thresholding reported?
- If claiming multi-class: is one unified model used, or are per-class models hidden?
- If claiming logical anomaly detection: is LOCO or a structural benchmark included?
- If claiming robustness: is there evidence beyond MVTec AD/VisA?
- If claiming flow novelty: have CFLOW-AD, FastFlow, PyramidFlow, and recent Flow Matching anomaly papers been positioned?
- If using diffusion comparisons: are DiAD, Removing Anomalies as Noises, and OmiAD-type speed/shortcut arguments handled?
- If using VLM comparisons: is the paper clear whether it is unsupervised, zero-shot, few-shot, or prompt-based?

# 11. Most Useful Takeaway

The strongest conference papers in this Zotero set succeed because they compress the whole paper into one precise technical tension:

- DRAEM: reconstruction is not enough; learn discriminative localization.
- PatchCore: use nominal patch memory maximally but keep it efficient.
- CFLOW-AD: exact likelihood can be efficient if moved into conditional feature space.
- DeSTSeg: student-teacher needs anomaly-side constraints and learned fusion.
- DiAD/OmiAD: diffusion is powerful but must solve category preservation, shortcut, and speed.
- Real-IAD: benchmarks must expose real-world failure modes.
- Anomaly-OV/RareCLIP: zero-shot anomaly detection needs object-aware abnormality reasoning, not generic visual-language prompting.

For our first paper, the analogous tension should be:

```text
Industrial anomaly detection should not reduce normality to one distance, reconstruction error, or likelihood. Normality is a structured constraint over feature distribution, category condition, local scale, and possibly part relation. Our method makes this structured normality trainable and turns violations into reliable anomaly maps.
```
