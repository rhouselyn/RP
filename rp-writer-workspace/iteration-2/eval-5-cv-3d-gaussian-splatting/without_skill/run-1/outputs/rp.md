# Robust Few-Shot 3D Scene Reconstruction via Geometry-Aware and Generative-Regularized 3D Gaussian Splatting

## 1. Introduction

Recent advances in neural rendering have fundamentally reshaped how we capture, represent, and synthesize 3D scenes. Neural Radiance Fields (NeRF) demonstrated that photorealistic novel-view synthesis is achievable from a sparse set of posed images, and 3D Gaussian Splatting (3DGS) has further pushed quality and real-time performance by representing scenes as collections of anisotropic 3D Gaussians rasterized through differentiable tile-based rendering. Despite these successes, both families of methods rely on a dense set of well-distributed input views—typically tens to hundreds of images—and degrade sharply when only a handful of views are available. This "few-shot" or "sparse-view" regime is precisely the one that matters most in practice: robotics, AR/VR, cultural-heritage digitization, and large-scale outdoor mapping all face constraints on capture time, bandwidth, accessibility, and cost that preclude dense capture.

I propose a research program that develops principled, geometry-aware, and generatively-regularized 3D Gaussian Splatting methods for few-shot 3D scene reconstruction. The central thesis is that the failure of current 3DGS approaches under sparse inputs stems from an ill-posed inverse problem that demands three complementary forms of inductive bias: (i) geometric priors that constrain the 3D structure beyond photometric loss, (ii) generative priors that hallucinate plausible content in unobserved regions while remaining consistent with observed views, and (iii) uncertainty-aware optimization that distinguishes evidence from speculation. The proposed work will produce methods, benchmarks, and theoretical analysis that advance the frontier of sparse-view 3DGS and, more broadly, contribute to a unified understanding of priors in differentiable rendering.

## 2. Background and Motivation

3DGS represents a scene as a set of 3D Gaussians, each parameterized by a position, covariance, opacity, and spherical-harmonic appearance coefficients. Differentiable rasterization enables end-to-end optimization of these parameters from image-space supervision. Because Gaussians are explicit primitives, 3DGS supports fast rendering and intuitive editing, but it also inherits a fundamental ambiguity: many configurations of Gaussians can explain the same set of pixels, especially when pixels are few. Without dense multi-view constraints, the optimizer settles into minima that overfit training views and produce floaters, blurred depth transitions, and severe artifacts in novel views.

Existing sparse-view NeRF and 3DGS efforts tackle this ill-posedness with various priors. Geometric priors include monocular depth and normal estimators, point-cloud initialization from COLMAP with multi-view stereo densification, and epipolar consistency. Generative priors leverage 2D diffusion models—most prominently Score-Distillation Sampling (SDS) and its variants—to supervise unobserved regions with learned image distributions. Yet each prior comes with trade-offs: depth priors are noisy and scale-ambiguous; diffusion priors are slow, mode-seeking, and produce global style inconsistent with the observed scene; and structural priors alone cannot fill in genuinely missing content. A coherent framework that fuses these priors with calibrated uncertainty, and that respects the explicit Gaussian representation, is currently missing. This research aims to close that gap.

## 3. Problem Statement

Given K posed RGB images (K ∈ {3, …, 12}) of an unknown scene, the goal is to recover a 3D Gaussian representation that (a) accurately reconstructs observed views, (b) synthesizes photorealistic and geometrically consistent novel views, and (c) honestly quantifies where the reconstruction is confident versus speculative. Existing methods fail to satisfy all three simultaneously: they either overfit, hallucinate incoherent content, or provide no uncertainty. The central technical challenge is to combine heterogeneous priors in a principled way, without letting any single prior dominate the optimization to the detriment of fidelity.

## 4. Research Objectives

The proposed research is organized around four objectives.

**O1. Geometry-Aware Sparse-View 3DGS.** Develop a 3DGS optimization scheme that tightly integrates monocular depth, surface normals, and silhouettes as geometric regularizers, with learned per-pixel confidence weights that prevent the priors from overriding genuine multi-view evidence.

**O2. Generative-Regularized Completion.** Design a diffusion-based completion framework that is conditioned on the current Gaussian renderings and observed views, providing targeted supervision only in regions the geometric priors flag as under-constrained. A key innovation will be a *consistency-preserving* SDS variant that prevents the generative prior from drifting away from the scene's true appearance.

**O3. Uncertainty Quantification for 3DGS.** Equip the Gaussian representation with calibrated uncertainty—per-Gaussian opacity and covariance posterior variances—and use this uncertainty to drive active view selection, i.e., to suggest where the next few captures would be most informative.

**O4. Benchmark and Analysis.** Construct a unified benchmark for few-shot 3DGS spanning indoor, outdoor, object-centric, and large-scale scenes, with standardized protocols for view sampling, metrics, and prior ablations.

## 5. Proposed Approach

### 5.1 Geometry-Aware Sparse-View 3DGS

I will introduce a unified energy that augments the standard photometric reconstruction loss with three terms. First, a *depth-consistency term* aligns the depth rendered from Gaussians with a monocular depth estimate for each training view, using a scale-and-shift-invariant formulation that compensates for the metric ambiguity of monocular predictors. Second, a *normal-consistency term* encourages the local surface normals implied by the Gaussian covariances to match a predicted normal map. Third, a *silhouette term* uses foreground masks (predicted or provided) to suppress floaters. Crucially, each term is weighted by a learned confidence map that down-weights regions where the prior is unreliable—thin structures, occlusion boundaries, and textureless areas. The confidence maps will be produced by a lightweight network trained jointly with the Gaussians, supervised by an auxiliary loss that rewards agreement with the (few) multi-view constraints.

A particularly important sub-problem is Gaussian densification under sparse views. The default 3DGS densification heuristic—splitting and cloning high-gradient Gaussians—tends to explode the primitive count when few views are available, because almost every Gaussian sees high photometric gradient. I will design a densification criterion that is gated by both the gradient magnitude and the local uncertainty (Section 5.3), so that densification occurs only where evidence genuinely supports finer geometry.

### 5.2 Generative-Regularized Completion

For regions where geometric priors are insufficient—typically large unobserved regions behind or beside the captured object—I will employ a 2D diffusion prior with two innovations. First, the diffusion model will be *conditioned on a partial render* of the current 3DGS scene plus a binary visibility mask, producing plausible completions that respect the known content. This "inpainting-style" conditioning avoids the global-style drift of vanilla SDS. Second, I will introduce a *consistency-preserving* distillation loss: at each step, the diffusion-generated completions are evaluated against the existing Gaussians, and only those pixels whose rendered appearance falls outside a calibrated acceptance region are updated. This prevents the generative prior from overwriting confidently-learned structure.

To reduce the cost of SDS-style optimization, I will explore diffusion models in a lower-resolution latent space and amortize guidance across multiple Gaussians simultaneously, leveraging the explicit structure of 3DGS to batch gradient computation. I will also investigate a *multi-view consistent* diffusion guidance: rather than supervising each novel view independently, I will synthesize a small set of pseudo-views jointly constrained by epipolar geometry, so that the generative prior injects content that is consistent across views rather than averaged across them.

### 5.3 Uncertainty Quantification and Active Capture

I will model each Gaussian's opacity and anisotropic scale as distributions rather than point estimates, using a variational formulation with a normalizing-flow prior over Gaussian parameters. The posterior variance of these distributions yields a per-Gaussian uncertainty that can be projected into image space as a pixel-wise uncertainty map. This uncertainty will serve three purposes: (i) modulating the strength of generative supervision (more uncertain regions receive stronger prior), (ii) yielding calibrated novel-view metrics that report confidence alongside PSNR/SSIM/LPIPS, and (iii) driving an *active view suggestion* policy that recommends the next few camera poses to maximize information gain. The active-capture formulation will treat view selection as a Bayesian optimization problem over SE(3), using the projected uncertainty to define the acquisition function.

### 5.4 Benchmark and Analysis

The benchmark will integrate datasets including LLFF, Mip-NeRF360, DTU, ScanNet, and Tanks-and-Temples, with a standardized sparse-view protocol that samples K views using a farthest-point heuristic in camera-pose space. Methods will be evaluated on standard photometric metrics, geometry metrics against captured LiDAR where available, and a new *3D consistency metric* that measures the temporal stability of rendered depth under small pose perturbations. I will release a leaderboard, reproducible baselines, and ablation protocols that isolate the contribution of each prior type. A particular focus will be *failure-mode analysis*: characterizing systematically when each prior helps or hurts, in order to give practitioners principled guidance for new capture scenarios.

## 6. Expected Contributions

1. A geometry-aware, confidence-weighted sparse-view 3DGS optimizer that meaningfully outperforms strong baselines on standard benchmarks, especially under K ≤ 6 views, together with an uncertainty-gated densification rule that prevents primitive explosion.
2. A consistency-preserving, inpainting-conditioned diffusion regularizer that fills unobserved regions without sacrificing fidelity to observed ones, including a multi-view-consistent guidance scheme.
3. The first calibrated per-Gaussian uncertainty framework for 3DGS, enabling honest reporting of reconstruction confidence and principled active view selection.
4. A unified benchmark and analysis that disentangles the contributions of geometric, generative, and structural priors in few-shot 3DGS, accompanied by reproducible baselines and a public leaderboard.

## 7. Research Plan

**Year 1 (Months 1–12).** Foundations. Implement the geometry-aware sparse-view 3DGS optimizer (O1); reproduce baselines (DNGaussian, FSGS, Splatter Image, SparseNeRF); publish a workshop paper establishing the confidence-weighted geometric prior and its ablations. Begin collaborations with the host lab on diffusion-prior integration and survey the existing priors literature to position the work.

**Year 2 (Months 13–24).** Generative completion. Develop the inpainting-conditioned diffusion regularizer (O2); address the consistency-preservation problem; investigate multi-view-consistent guidance. Target a publication at a top venue (CVPR/ICCV/NeurIPS). Begin the variational uncertainty modeling (O3) in parallel.

**Year 3 (Months 25–36).** Uncertainty and benchmark. Complete the variational uncertainty framework and active-capture policy (O3); release the unified benchmark (O4); publish a flagship paper combining the three priors. Pursue an internship in industry (e.g., NVIDIA, Adobe, Apple, or a robotics company) to validate on large-scale and unconstrained data.

**Year 4 (Months 37–48).** Extensions and dissertation. Extend to dynamic scenes with temporal Gaussian deformation, jointly estimate lighting and material for inverse rendering under sparse views, and complete the dissertation. Target one further top-venue publication on the dynamic extension.

## 8. Broader Impact and Fit

Sparse-view 3D reconstruction sits at the intersection of computer vision, graphics, and machine learning, with direct applications in robotics (where every camera motion is costly), AR/VR (where capture must be casual), and scientific imaging (where samples are delicate or inaccessible). Reducing the capture burden also democratizes 3D content creation, lowering barriers for educators, artists, and small studios. At the same time, generative completion introduces risks of hallucinated content being mistaken for real geometry; the proposed uncertainty quantification is therefore not only a technical contribution but also a safeguard, enabling downstream systems to flag speculative regions rather than silently trusting them.

The proposed research aligns naturally with the host lab's expertise in neural rendering and 3D Gaussian Splatting: it builds directly on the Gaussian representation, raises open questions about priors and uncertainty that the lab is uniquely positioned to address, and offers a concrete path from novel methods to deployed systems. By combining explicit geometric structure with learned generative priors, this work aspires to make photorealistic 3D capture as effortless as taking a few photographs.

## 9. Conclusion

Few-shot 3D scene reconstruction with 3D Gaussian Splatting is a high-impact, technically rich, and timely problem. The proposed program addresses the core ill-posedness through three complementary priors—geometric, generative, and uncertainty-aware—and delivers methods, theory, and benchmarks that will advance the field. I am excited to pursue this work under the supervision of a lab whose contributions have defined the frontier of neural rendering, and I look forward to contributing to its continued leadership in making 3D vision systems both more capable and more trustworthy.
