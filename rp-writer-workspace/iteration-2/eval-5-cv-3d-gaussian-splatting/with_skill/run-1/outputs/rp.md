# Research Proposal

**Title:** Geometric Ambiguity and Unseen-Region Generation in Sparse-View 3D Gaussian Splatting: Toward Robust Few-Shot Scene Reconstruction

**Applicant:** <待填:your name>
**Email:** <待填:your email>
**Target degree:** PhD in Computer Science (3D Vision)
**Target institution:** <待填:target university>
**Prospective advisor:** <待填:advisor name> (neural rendering / 3D Gaussian Splatting)
**Intended start:** <待填:start semester, e.g., Fall 2027>
**Date:** 2026-06-27

---

## Abstract

3D Gaussian Splatting (3DGS) has advanced real-time, high-fidelity novel-view synthesis from dense multi-view images, yet its behavior in the sparse-view regime—the setting that matters for drone surveying, in-the-wild capture, and assistive or medical imaging—remains poorly understood. When only a handful of input views are available, the underconstrained per-pixel optimization of Gaussian primitives typically produces geometric ambiguity, floating artifacts ("floaters"), and implausible holes in unobserved regions. Existing few-shot 3DGS methods mitigate these symptoms with depth priors or diffusion guidance, but treat them as engineering patches rather than diagnosable failure modes; consequently, it is unclear which components of the Gaussian representation and its optimization are responsible, how much regularization is enough before valid geometry is suppressed, and how to hallucinate unseen content without breaking multi-view consistency. This proposal investigates three interrelated questions on (i) the origins of geometric ambiguity and floaters in sparse-view 3DGS, (ii) the regime in which geometric priors help versus over-constrain, and (iii) the integration of generative priors for unseen-region completion with geometric consistency. Using LLFF, DTU, BlendedMVS and Replica under controlled sparse-view protocols, and benchmarking against 3DGS, Mip-NeRF 360, FSGS, DNGaussian and feed-forward reconstructors, the work aims to deliver diagnostic understanding and reusable design guidelines—not merely a PSNR improvement—with implications for deploying 3DGS on resource-constrained devices and beyond controlled captures. All code, checkpoints, and sparse-view splits will be released for reproducibility.

---

## 中文说明(供申请人理解,不计入正文字数)

- **选题定位**:本 RP 严格遵循"问题是中心"原则,把"few-shot/sparse-view 下 3DGS"收敛成三个有逻辑递进的疑问句(诊断→正则化→生成),而不是"我要用 X 方法做 Y 任务"的方法陈述。
- **研究方向 vs 解决方案**:Methodology 只点到"为何这条路可行 + 关键挑战",具体架构(如用哪种 diffusion、哪种 loss)留作待回答的开放问题,符合剑桥 CS 教授"Finding a good solution is what the PhD is for"的原则。
- **So what**:核心贡献是 *mechanistic understanding*(哪些 Gaussian 参数导致 floater、prior 力度的 frontier、生成先验与 3D 一致性的 trade-off),而非刷 PSNR。负结果(如证明 2D 生成先验无法保 3D 一致性)同样作为贡献报告。
- **诚实算力**:per-scene 3DGS 单卡 24GB GPU 可跑;feed-forward baseline 用官方 checkpoint 微调,只在 RQ3 的生成实验用集群,总预算 ≤2000 GPU-hours,并留 <待填:实验室集群规模> 让你按实填写。
- **匹配导师**:全篇围绕 neural rendering / 3DGS,把导师方向延伸到"决定 3DGS 能否走出实验室"的低数据场景。
- **未给信息**:申请人姓名/邮箱/学校/导师姓名/入学时间/集群规模/申请者背景均用 `<待填:xxx>` 占位,绝不编造。
- **引用**:确信的标真实出处;拿不准的 venue/year/标题一律标 `[需核实]`,请你在 Google Scholar / DBLP 核对后替换。
- **占位符**:`<待填:xxx>` 需你填实;`[需核实]` 需你核对文献元数据。

---

## 1. Introduction & Background

Neural radiance fields and their explicit successor, 3D Gaussian Splatting (3DGS) [4], have reshaped 3D scene reconstruction and novel-view synthesis. 3DGS represents a scene as a set of anisotropic 3D Gaussians—each carrying position, covariance, opacity, and spherical-harmonic appearance—and renders them through differentiable tile-based rasterization. Compared to NeRF [1], 3DGS offers orders-of-magnitude faster rendering, an explicit and editable geometry representation, and competitive reconstruction quality under dense views. These properties have made it a default backbone for downstream 3D vision tasks including SLAM, digital twins, and AR/VR content.

The dominant regime for 3DGS, however, assumes dozens to hundreds of well-posed, densely overlapping input images. This assumption breaks in many realistic settings: a drone may capture a structure from a few fly-overs; a smartphone user may snap only a handful of shots around an object; clinical or industrial scans may be limited by dose, time, or access. In these sparse-view (few-shot) regimes, the per-pixel photometric loss that drives 3DGS optimization becomes severely underconstrained: many configurations of Gaussians render nearly identically on observed views while differing drastically elsewhere. The visible symptoms are well documented—floating Gaussian blobs ("floaters") that do not correspond to any real surface, geometric ambiguity between near and far surfaces along viewing rays, and large implausible holes in unobserved regions.

A growing line of work [9, 10, 8] addresses sparse-view 3DGS and NeRF by injecting priors—depth maps, epipolar attention, or diffusion-based hallucination. Yet these works largely report aggregate metrics (PSNR/SSIM/LPIPS) and treat artifacts as a black box to be suppressed. What is missing is a mechanistic understanding: which components of the Gaussian parameterization cause which artifacts, where the regularization–fidelity trade-off lies, and how generative and geometric priors can be composed without destroying the multi-view consistency that is the entire point of a 3D representation. This proposal targets exactly that gap, framing sparse-view 3DGS as a diagnosable *research direction* rather than a benchmark to be topped.

This work aligns directly with the host lab's focus on neural rendering and 3D Gaussian Splatting, extending that agenda into the under-explored low-data regime that ultimately determines whether 3DGS can leave the laboratory.

## 2. Literature Review

I organize prior work into four themes.

**Dense-view neural rendering.** NeRF [1] introduced volumetric radiance fields trained from posed images; Mip-NeRF 360 [3] extended it to unbounded scenes with anti-aliasing and a contraction function, establishing a strong NeRF baseline. 3DGS [4] replaced the implicit MLP with explicit Gaussians and tile-based rasterization, achieving real-time rendering at comparable or higher fidelity. These methods are the dense-view reference points against which sparse-view degradation is measured; the gap between dense and sparse performance quantifies the problem this proposal targets.

**Sparse-view NeRF.** PixelNeRF [5] introduced a feed-forward architecture conditioned on image features to amortize across scenes. DietNeRF [6] used a pretrained CLIP prior for semantic consistency. InfoNeRF [7] added an information-theoretic ray sampling objective. SparseNeRF [8] injected depth-ranking and local geometry priors from a depth foundation model. These methods demonstrate that priors help, but they are NeRF-specific, and NeRF's implicit representation makes it difficult to isolate geometric failure modes (e.g., floaters manifest as diffuse density blobs rather than discrete primitives).

**Sparse-view 3DGS.** FSGS [9] introduces geometry-aware initialization and a flow-based consistency loss for few-shot 3DGS. DNGaussian [10] uses frequency-domain regularization and a small trainable set of anchor Gaussians. Feed-forward reconstructors such as GS-LRM [12] and pixelSplat [11] regress 3DGS parameters directly from a few posed images via large transformer backbones, trading per-scene optimization for cross-scene generalization. The first two optimize per scene with priors; the latter amortize over large multi-view datasets. They report strong PSNR gains, but rarely analyze *why* floaters still appear or *where* the prior helps most—precisely the gap this proposal addresses.

**Priors for geometry and appearance.** Depth Anything [13] provides monocular depth estimates usable as geometric supervision; diffusion-based models [需核实: specific 3D/video diffusion works] provide strong appearance priors for unseen content. The open question is composability: monocular depth is scale/shift-ambiguous and noisy at occlusion boundaries; diffusion priors are 2D-biased and can violate multi-view geometry. No existing work cleanly disentangles how much each prior contributes, or how they interact.

**Synthesis and gap.** Three observations frame this proposal. First, sparse-view artifacts (floaters, ambiguity, holes) are reported but not mechanistically explained; we do not know which Gaussian parameters—opacity, scale, rotation, or position—drive each artifact. Second, prior strength is treated as a hyperparameter to be tuned rather than a regime to be characterized; the boundary between under-regularized (floaters persist) and over-regularized (valid thin structures suppressed) is unknown. Third, generative completion of unseen regions is attempted, but the consistency–plausibility trade-off is unquantified. These three gaps map onto the three research questions below.

## 3. Research Questions & Objectives

**RQ1 (origins).** *Which components of the 3D Gaussian parameterization, and which stages of its optimization, are responsible for geometric ambiguity and floater artifacts under sparse-view supervision?*
— Objective O1: design diagnostic experiments that attribute floaters to opacity, scale, rotation, or position updates, and characterize the optimization trajectory (when floaters emerge, whether they are stable or transient).

**RQ2 (priors).** *Under what sparse-view conditions do geometric priors (depth, epipolar, multi-view consistency) reduce ambiguity without over-regularizing and suppressing valid geometry?*
— Objective O2: map a regularizer-strength-vs-fidelity frontier across view counts, scene types, and prior sources, and identify the failure mode of over-regularization.

**RQ3 (unseen regions).** *How can generative priors be integrated into 3DGS to hallucinate plausible unseen regions while preserving geometric consistency with observed views?*
— Objective O3: compare integration strategies (initialization, loss, prior-as-decoder) on consistency–plausibility trade-offs and quantify residual inconsistency.

The RQs progress from diagnosis (RQ1) to controlled intervention (RQ2) to the open frontier of generation (RQ3), each feeding the next—RQ2's frontier requires RQ1's instrumentation, and RQ3's consistency metric is built from RQ1's floater diagnostics.

## 4. Methodology & Feasibility

**4.1 Overall design.** This is an algorithmic-plus-empirical study. It deliberately describes a *research direction*—diagnose, regularize, generate—rather than a fixed architecture; concrete modules are research questions to be answered, not pre-committed solutions. The pipeline is: (i) establish diagnostic instrumentation on top of existing 3DGS; (ii) characterize the prior-fidelity frontier; (iii) study generative-prior integration; (iv) cross-validate with ablations and geometric metrics.

**4.2 Datasets.** Public datasets only: LLFF [2] (forward-facing; license [需核实]), DTU [14] (object-centric multi-view; license [需核实]), BlendedMVS [15] (hybrid scenes; license [需核实]), and Replica [16] (indoor room-scale; license [需核实]). Sparse-view protocols use 3, 6, and 9 input views with fixed splits released for reproducibility; held-out test views follow official splits where available. Splits are per-scene and each scene is optimized independently, so cross-scene leakage is impossible by construction. No human subjects are involved; **no IRB is required** ("This study uses only public datasets and simulation; no human subjects"). Local backups of all four datasets will be maintained from project start in case a dataset is re-licensed or withdrawn (see Risks).

**4.3 Baselines (≥3, near SOTA).** (1) 3DGS [4]—the dense-view reference and sparse-view degradation baseline. (2) Mip-NeRF 360 [3]—strong NeRF baseline for unbounded scenes. (3) FSGS [9]—representative few-shot 3DGS with priors. (4) DNGaussian [10]—frequency-regularized few-shot 3DGS. (5) pixelSplat [11] / GS-LRM [12]—feed-forward reconstructors for the amortized regime. These span dense NeRF, dense 3DGS, prior-based few-shot 3DGS, and feed-forward 3DGS, so any finding is attributable to a regime rather than a single method's idiosyncrasy. Each baseline is chosen because it is (a) released with code/checkpoints and (b) cited as the current comparison point in 2024–2025 few-shot 3DGS papers.

**4.4 Proposed investigations (direction, not solution).** For RQ1, instrumentation will log per-Gaussian parameter trajectories, opacity histograms, and a floater-detection heuristic (Gaussians whose projected footprint is large but whose contribution to any training view is negligible). Controlled interventions—freeze opacity, clamp scale, mask rotation gradients—will attribute artifacts to specific parameters. For RQ2, depth priors from Depth Anything [13], epipolar/multi-view consistency losses, and their combination will be swept over a regularizer-weight grid; the frontier will be characterized on geometric metrics, not just photometric. For RQ3, candidate generative-prior integration sites—(a) initialization of unseen Gaussians, (b) a generative appearance loss, (c) a prior-as-decoder that proposes Gaussians—will be compared; the key measurable is residual multi-view inconsistency (reprojection error, depth disagreement) versus plausibility (FID/KID on held-out views, human spot-check). Specific architectures remain open because the research question is *which integration site works and why*, not a fixed model.

**4.5 Evaluation.** Photometric: PSNR/SSIM/LPIPS [17] on held-out views. Geometric: depth error against rendered/estimated depth, Chamfer distance on DTU where ground-truth point clouds exist, and a floater-count metric from RQ1 instrumentation. Generative: FID/KID plus a small human study (n ≥ 3 raters, inter-rater agreement reported [需核实: exact protocol]) for unseen-region plausibility. Multiple seeds (≥ 3, ≥ 5 for high-variance generative runs), mean ± std reported; paired t-tests where differences are claimed.

**4.6 Ablations.** Per-prior (depth vs epipolar vs both), per-Gaussian-parameter freeze (RQ1), per-integration-site (RQ3), and view-count sweeps (3/6/9). Each ablation is tied to an RQ so that every component's contribution is testable, following the principle that an un-ablated method is an un-proven method.

**4.7 Compute & reproducibility.** Honest assessment: per-scene 3DGS optimization fits on a single 24 GB GPU (RTX 4090 / A5000) in tens of minutes; feed-forward baselines require 4–8 A100/H100 for full retraining, but **we will use official checkpoints** and only fine-tune, keeping total compute within a modest cluster budget (estimated ≤ 2,000 GPU-hours over the project, contingent on <待填:lab cluster size>). Code, configs, sparse-view splits, seeds, and commit hashes will be released under a permissive license with a Zenodo DOI; pretrained checkpoints we generate will be released; third-party checkpoints will be cited by model ID + commit hash. Provenance will be logged via Weights & Biases. The study uses only public data and simulation; no human-subjects data, so no IRB is required—stated explicitly here so the reviewer's compliance question is pre-answered.

**4.8 Feasibility.** *Resources:* public datasets and released baselines keep the entry bar low; only RQ3's generative runs are compute-heavy, mitigated by using frozen pretrained priors. *Technical:* the applicant has <待填:relevant background—e.g., PyTorch, prior NeRF/3DGS project, link to CV>. *Human:* the host lab's neural-rendering expertise directly supports all three RQs. *Applicant:* <待填:link to CV / prior project>.

**4.9 Risks and contingencies.**

| Risk | Prob | Impact | Mitigation |
|---|---|---|---|
| Direction scooped (arXiv same problem) | Med | High | Monthly arXiv watch; pre-register the RQ1 *diagnostic protocol* as the differentiator (analysis, not a method, is hard to scoop); maintain a backup RQ on cross-scene generalization of feed-forward models. |
| Dataset licensing change / withdrawal | Low | High | Local backups of all four datasets from M1; maintain a synthetic BlenderMVS-style fallback for continuous evaluation. |
| Baseline unreproducible | Med | Med | Use official checkpoints; contact authors; if a baseline fails, document and substitute the next-best with explicit justification. |
| Generative prior too 2D-biased | Med | Med | RQ3 explicitly quantifies inconsistency; if it dominates, pivot RQ3 to a diagnostic study of *why* 2D priors break 3D consistency—an equally valid contribution. |
| Compute shortage / queue | Med | High | Front-load cheap per-scene experiments (RQ1/RQ2) on a single GPU; reserve cluster budget for RQ3; use smaller pretrained priors as a proxy before scaling. |
| Over-regularization undetectable | Low | Med | The RQ2 frontier methodology is itself the detector; the failure mode is a documented result, not a project failure. |

## 5. Research Plan / Timeline

The PhD is assumed to total 5 years (US norm); core research occupies Years 1–4 with Year 5 reserved for thesis writing and an internship. Months are project months from start. Buffers (~20%) are embedded; each phase depends on prior diagnostics so RQ2/RQ3 cannot start before RQ1's instrumentation exists.

| Phase | Time | Tasks (with dependencies) | Deliverable |
|---|---|---|---|
| P1 Foundations | M1–9 | Deep literature reading; reproduce 3DGS / Mip-NeRF 360 / FSGS / DNGaussian on LLFF/DTU; build sparse-view splits; instrument Gaussians | Reproduction report; released splits; diagnostic logger |
| P2 RQ1 | M7–18 (overlaps P1 instrumentation) | Diagnostic attribution of floaters/ambiguity; controlled freeze interventions | Conference paper #1 (analysis track) |
| P3 RQ2 | M16–28 (depends on P2 diagnostics) | Prior-fidelity frontier sweep; over-regularization characterization | Journal/conference paper #2 |
| P4 RQ3 | M25–40 (depends on P2+P3) | Generative-prior integration study; consistency–plausibility trade-off | Conference paper #3; checkpoint release |
| P5 Buffer & thesis | M37–48 | Buffer for scooped-direction pivot; integration writing | Thesis draft; full code release |

## 6. Expected Outcomes & Significance

The expected outcomes are (i) a public diagnostic toolkit and the first mechanistic attribution of sparse-view 3DGS artifacts to specific Gaussian parameters; (ii) a characterized prior-fidelity frontier that lets practitioners choose regularization strength from view count and scene type rather than trial-and-error; (iii) a quantified consistency–plausibility trade-off for generative completion. The significance is *not* a PSNR increment: it is reusable understanding of when and why 3DGS fails under low data, which directly governs whether 3DGS can be deployed on drones, phones, and clinical/industrial scanners. For the host lab, this extends the neural-rendering and 3DGS agenda into the regime that determines real-world adoption. Negative results—e.g., a clean demonstration that 2D generative priors cannot preserve 3D consistency above a quantified threshold—are equally valuable and will be reported as such, since they redirect the field toward genuinely 3D-aware generative priors.

---

## References

[1] B. Mildenhall, P. P. Srinivasan, M. Tancik, J. T. Barron, R. Ramamoorthi, R. Ng, "NeRF: Representing scenes as neural radiance fields for view synthesis," in *Proc. ECCV*, 2020.

[2] B. Mildenhall, P. P. Srinivasan, R. Ortiz-Cayon, N. K. Kalantari, R. Ramamoorthi, R. Ng, A. Kar, "Local Light Field Fusion: Practical view synthesis with prescriptive sampling guidelines," *ACM Trans. Graph.*, 2019.

[3] J. T. Barron, B. Mildenhall, M. Tancik, P. Hedman, R. Martin-Brualla, P. P. Srinivasan, "Mip-NeRF 360: Unbounded anti-aliased neural radiance fields," in *Proc. CVPR*, 2022.

[4] K. Kerbl, G. Kopanas, T. Leimkühler, G. Drettakis, "3D Gaussian Splatting for real-time radiance field rendering," *ACM Trans. Graph.*, 2023.

[5] A. Yu, V. Ye, M. Tancik, A. Kanazawa, "PixelNeRF: Neural radiance fields from one or few images," in *Proc. CVPR*, 2021.

[6] A. Jain, M. Tancik, P. Abbeel, "Putting NeRF on a Diet: Semantically consistent few-shot view synthesis (DietNeRF)," *Proc. ICLR*, 2022. [需核实 venue]

[7] M. Kim, S. Seo, K. Jung, "InfoNeRF: Ray entropy minimization for few-shot neural radiance fields," *Proc. CVPR*, 2023. [需核实 venue/year]

[8] G. Wang, Z. Chen, C. Loy, Z. Liu, "SparseNeRF: Distilling depth ranking for few-shot novel view synthesis," in *Proc. ICCV*, 2023. [需核实]

[9] Z. Zhu, Z. Liang, F. Li, Q. Zhao, "FSGS: Few-shot Gaussian Splatting," *Proc. NeurIPS*, 2024. [需核实 venue/year/authors]

[10] J. Lee, D. Pham, B. Choi, K. Hwang, "DNGaussian: Improving few-shot 3D Gaussian Splatting with error feedback and sharpness regularization," *Proc. CVPR*, 2024. [需核实 exact title/venue]

[11] D. Charatan, S. Li, R. Tagliasacchi, V. Sitzmann, "pixelSplat: 512x512 feed-forward 3D Gaussian Splatting," *Proc. 3DV/CVPR*, 2024. [需核实 venue]

[12] K. Zhang, S. Bi, H. Tan, Y. Xiangli, N. Zhao, K. Sunkavalli, Z. Xu, "GS-LRM: Large reconstruction model for 3D Gaussian Splatting," 2024. [需核实 venue]

[13] L. Yang, B. Kang, Z. Huang, X. Xu, J. Feng, H. Zhao, "Depth Anything: Unleashing the power of large-scale unlabeled data," *Proc. CVPR*, 2024. [需核实]

[14] R. Jensen, A. Dahl, G. Vogiatzis, E. Tola, H. Aanæs, "Large scale multi-view stereopsis evaluation," in *Proc. CVPR*, 2014.

[15] Y. Yao, Z. Luo, S. Li, T. Fang, L. Quan, "BlendedMVS: A large-scale dataset for generalized multi-view stereo networks," in *Proc. ECCV*, 2020. [需核实]

[16] J. Straub, T. Whelan, L. Ma, Y. Chen, E. Wijmans, S. Green, et al., "The Replica dataset: A digital replica of indoor spaces," *arXiv:1906.05797*, 2019. [需核实]

[17] R. Zhang, P. Isola, A. A. Efros, E. Shechtman, O. Wang, "The unreasonable effectiveness of deep features as a perceptual metric (LPIPS)," in *Proc. CVPR*, 2018.
