# Efficient LLM Alignment via Sample-Economic Direct Preference Optimization

**Applicant:** Master's candidate, Natural Language Processing
**Target Program:** Ph.D. in Computer Science (The Chinese University of Hong Kong / The Hong Kong University of Science and Technology)
**Research Area:** Efficient Alignment of Large Language Models

---

## Abstract

Direct Preference Optimization (DPO) has emerged as a compelling alternative to Reinforcement Learning from Human Feedback (RLHF): it removes the explicit reward model and the instability of policy-gradient training while retaining the preference-driven objective. Yet DPO inherits RLHF's most expensive bottleneck—the reliance on large volumes of high-quality human preference data, which is costly to collect and difficult to reproduce, especially for laboratories with modest compute. This proposal investigates data-centric methods to make DPO substantially more sample-efficient, so that strong alignment can be achieved with a small fraction of the usual annotation budget. We propose three integrated and mutually reinforcing components: (1) **active preference acquisition**, which selects the most informative prompt–response pairs for human annotation; (2) **curriculum-guided DPO**, which orders preference pairs by difficulty to stabilize optimization and improve convergence; and (3) **controlled synthetic preference augmentation**, which expands scarce human preferences using the model itself while bounding distribution shift through a consistency filter. All experiments are designed to run on models in the 7B–13B range on a small A100 cluster. Expected outcomes include reproducible, budget-aware alignment recipes, open-source tooling, and empirical evidence clarifying when each efficiency lever helps most.

---

## 1. Introduction

The rapid deployment of large language models (LLMs) has made *alignment*—steering models toward helpful, honest, and harmless behavior—a central research problem. Reinforcement Learning from Human Feedback (RLHF) has been the dominant paradigm, but it is notoriously complex: it requires training a separate reward model, performing unstable policy optimization with PPO, and managing large amounts of preference data. Direct Preference Optimization (DPO) reframes the alignment objective as a closed-form classification loss over preference pairs, eliminating the reward model and the PPO loop. Despite this elegance, DPO does not relax the underlying *data* requirement: state-of-the-art results typically rely on tens to hundreds of thousands of curated preference pairs, and performance degrades sharply when data is scarce.

This data dependency creates an accessibility gap. Well-resourced industry labs can annotate at scale; academic groups, smaller teams, and researchers in lower-resourced settings cannot. My home laboratory operates a small cluster of A100 GPUs, which is sufficient for fine-tuning 7B–13B models but precludes the data-hungry, compute-hungry alignment pipelines used by frontier labs. This constraint is not merely an inconvenience—it is representative of a large and growing population of alignment researchers who need methods that work under realistic budgets.

The overarching goal of this Ph.D. proposal is therefore to make LLM alignment **efficient along the data axis**: to achieve comparable alignment quality using far fewer annotated preferences, and to do so with methods that are principled, reproducible, and compatible with the DPO family of objectives. Rather than pursuing scaling, this work pursues *sample economy*—getting the most alignment signal out of each annotated pair.

---

## 2. Background and Related Work

**RLHF and the reward-model paradigm.** RLHF fits a reward model to human pairwise preferences and then optimizes a language model policy against it using PPO. It delivers strong results but suffers from reward hacking, training instability, and high engineering cost.

**DPO and its successors.** DPO derives a loss that directly optimizes the policy to satisfy observed preferences under a fixed reference model, side-stepping the reward model entirely. Subsequent variants extend it in several directions: *IPO* and *KTO* modify the loss to reduce overfitting and to support non-pairwise feedback; *SimPO* removes the reference model entirely for further efficiency; *ORPO* folds alignment into supervised fine-tuning. These works improve the *objective* but largely take the preference dataset as given.

**Data-efficient and self-improving alignment.** A recent line of work attempts to reduce annotation cost. *Self-rewarding language models* and *constitutional AI* use the model (or a stronger model) to generate its own preferences or critiques. Active learning has been applied to reward-model training, and curriculum strategies have been explored for supervised fine-tuning. However, **a unified, data-centric treatment of DPO**—combining active acquisition, curriculum, and controlled synthetic augmentation within a single efficiency framework—remains underexplored. This proposal targets precisely that gap, with a focus on methods that are tractable on modest academic compute.

---

## 3. Problem Statement and Research Questions

**Problem.** Given a base or instruction-tuned LLM and a small annotation budget *B* (e.g., 1k–8k preference pairs), how can we align the model as effectively as possible—approaching the quality achievable with an order of magnitude more data—using only DPO-style objectives and a small A100 cluster?

This decomposes into three research questions:

- **RQ1 (Selection).** Which unannotated prompt–response pairs, if labeled, would most reduce alignment error? Can an *active preference acquisition* strategy reduce the number of pairs needed to reach a target quality by a measurable factor?
- **RQ2 (Ordering).** Does the *order* in which preference pairs are presented to DPO matter? Can a *curriculum* over preferences improve convergence, final quality, or robustness when data is scarce?
- **RQ3 (Augmentation).** To what extent can *synthetic preferences*, generated by the model itself under a consistency filter, substitute for human labels without introducing distribution shift or reward hacking?

A cross-cutting question—**RQ4 (Interaction)**—asks how these three levers interact: do their gains stack, or do they saturate and overlap?

---

## 4. Proposed Approach

We propose **SE-DPO** (Sample-Economic DPO), a unified pipeline that integrates active acquisition, curriculum ordering, and controlled synthetic augmentation around a DPO core. Each component is designed to be independently ablatable and collectively complementary.

### 4.1 Active Preference Acquisition

Standard DPO datasets are sampled uniformly from a prompt distribution, which is wasteful: many pairs are either trivially separable (carrying little signal) or near-ties (noisy and hard). We frame pair selection as an *acquisition* problem under a fixed annotation budget.

Given a pool of candidate prompts each with two candidate responses, we score candidates for labeling using a composite acquisition function combining:
- **Disagreement / uncertainty.** We measure the disagreement between the current policy and a small ensemble of reference checkpoints (or dropout-based stochastic forward passes). Pairs on which the ensemble disagrees most are expected to be most informative.
- **Margin.** Pairs whose estimated reward margin is moderate—neither trivially easy nor impossibly hard—are prioritized, following the intuition that mid-difficulty examples carry the strongest gradient signal for DPO's logistic loss.
- **Diversity.** A clustering step over prompt embeddings (using the reference model's encoder) ensures coverage of the prompt distribution and avoids redundant annotations within a single cluster.

We will compare against random selection, uncertainty-only, and margin-only baselines, measuring alignment quality per annotated pair. The hypothesis is that active acquisition yields a constant-factor improvement in sample efficiency (e.g., matching random-pool quality at 30–50% of the budget).

### 4.2 Curriculum-Guided DPO

DPO optimizes a sum of per-pair losses, and standard practice shuffles pairs uniformly. We hypothesize that *ordering* matters when data is scarce: starting with high-signal, unambiguous pairs may stabilize the policy's early gradient trajectory, while introducing hard or noisy pairs too early can destabilize optimization and amplify reference-model miscalibration.

We define a difficulty score for each preference pair based on (a) the reward margin estimated by the reference model, (b) the agreement among the ensemble used in §4.1, and (c) response-length and lexical-overlap confounds that correlate with annotator noise. We then train with several curricula—easy-to-hard, hard-to-easy, and a self-paced variant in which the model progressively up-weights pairs it currently misclassifies—and compare against shuffled training. We additionally study whether curriculum mitigates known DPO failure modes such as *likelihood drift* on the chosen response and degradation on out-of-distribution prompts.

### 4.3 Controlled Synthetic Preference Augmentation

Human annotation is the binding cost. A natural remedy is synthetic preferences: use the model (or a stronger open model) to label additional pairs. The risk is distribution shift—synthetic labels may reflect the model's biases rather than genuine human preference—and self-reinforcing error. We address this with a **consistency filter**: a synthetic pair is admitted to training only if (i) the labeling model is confident, (ii) the labeling is stable across paraphrased prompts and across decoding seeds, and (iii) the pair does not contradict a small held-out set of human labels. We further mix synthetic and human pairs at a controlled ratio and monitor for reward-hacking signatures (e.g., length exploitation, sycophancy) using targeted probes.

Ablations will isolate the contribution of each filter component and quantify how far synthetic data can stretch a fixed human budget before quality plateaus or degrades.

### 4.4 Integration

The three components compose naturally: active acquisition decides *which* pairs humans label; curriculum decides *how* the (human + admitted synthetic) pairs are consumed; the augmentation module decides *which* synthetic pairs are admitted. We will evaluate the full SE-DPO pipeline and ablate each component to test RQ4.

---

## 5. Experimental Design

**Models.** Llama-3-8B and Mistral-7B as primary base models, with Qwen2.5-7B as a third family for transferability; selective 13B runs to test scale sensitivity. All fit comfortably in mixed-precision DPO on a few A100s.

**Datasets.** UltraFeedback and HH-RLHF as human-preference pools; we will subsample to simulate scarce-budget regimes (1k, 2k, 4k, 8k pairs). A held-out human-evaluated set will anchor quality measurement.

**Baselines.** (i) DPO with random subsampling; (ii) DPO with uniform synthetic augmentation (no filter); (iii) SimPO and ORPO under the same budgets; (iv) full-data DPO as an upper bound. Where useful, a small RLHF/PPO baseline will be included for calibration.

**Metrics.** AlpacaEval-2 and MT-Bench for instruction-following quality (length-controlled win rate); reward-model accuracy and DPO loss on held-out pairs for in-distribution fit; targeted probes for honesty, refusal, and sycophancy; KL divergence from the reference model to monitor over-optimization; annotation efficiency curves (quality vs. number of labeled pairs).

**Human evaluation.** A modest blind pairwise human evaluation (a few hundred comparisons) will validate automatic metrics for the headline result, acknowledging the known noise of automatic judges.

**Compute plan.** Each DPO run on 7B models completes within a few hours on a single A100; the full ablation grid (≈3 models × 4 budgets × 5 conditions ≈ 60 runs) is feasible across a small A100 cluster over the project's first phase, with 13B and 70B (via LoRA) runs reserved for the most informative conditions.

---

## 6. Expected Contributions

1. **A unified, data-centric efficiency framework for DPO** (SE-DPO) integrating active acquisition, curriculum, and filtered synthetic augmentation, with each component independently ablated.
2. **Empirical efficiency curves** quantifying how much human annotation each lever saves, and where the levers stack vs. saturate—directly useful to budget-constrained alignment researchers.
3. **Open-source tooling** (acquisition functions, curriculum schedulers, consistency filters) released as a drop-in enhancement to popular DPO training stacks.
4. **Analysis of DPO failure modes under scarcity**, including likelihood drift and reward hacking, and evidence on whether curriculum and filtering mitigate them.

---

## 7. Timeline (Indicative, Years 1–4)

- **Year 1.** Literature consolidation; reproduce DPO/SimPO/ORPO baselines on 7B models; implement and validate active acquisition (RQ1); first workshop submission.
- **Year 2.** Curriculum-guided DPO and failure-mode analysis (RQ2); consistency-filtered synthetic augmentation (RQ3); integrate SE-DPO; first top-tier conference submission.
- **Year 3.** Scale and transferability studies (13B, additional model families, multilingual extension); human evaluation; second conference submission.
- **Year 4.** Dissertation writing; open-source release and maintenance; exploration of follow-up directions (e.g., extending the framework to online/iterative DPO and to safety-critical alignment).

---

## 8. Feasibility and Resource Considerations

The proposal is explicitly scoped to modest compute. All core experiments run on 7B–13B models on a small A100 cluster; 70B-scale runs, where included, use parameter-efficient (LoRA/QLoRA) training. Annotation costs are minimized by design: active acquisition reduces the human-label count, and synthetic augmentation substitutes for additional human labels. The methods build on open datasets and open training stacks, ensuring reproducibility. Risks include (i) synthetic-label distribution shift, mitigated by the consistency filter and targeted probes; (ii) automatic-judge bias, mitigated by a held-out human-evaluated anchor set; and (iii) curriculum instability, mitigated by self-paced scheduling with gradient-norm monitoring. Each risk has a concrete fallback and an ablation that quantifies its impact.

---

## 9. Alignment with the Supervisor's Research Program

The proposed work sits squarely within the supervisor's recent program on RLHF and DPO. Rather than competing on scale, it contributes a complementary, data-centric axis—sample economy—that is scientifically novel and practically important, and that produces artifacts (efficiency curves, filters, curricula) likely to be reused by the broader alignment community. It also opens several natural collaboration surfaces: extending the framework to online DPO variants the supervisor may be developing, studying interaction with reward-model-based methods, and joint analysis of over-optimization and reward hacking. I am particularly drawn to this group because of its balance between mathematical rigor and empirical care, which matches the methodological posture this proposal requires.

---

## Selected References (Indicative)

- Rafailov et al. *Direct Preference Optimization: Your Language Model is Secretly a Reward Model.* NeurIPS 2023.
- Ouyang et al. *Training language models to follow instructions with human feedback (InstructGPT).* NeurIPS 2022.
- Meng et al. *SimPO: Simple Preference Optimization with a Reference-Free Reward.* 2024.
- Hong et al. *ORPO: Monolithic Preference Optimization without Reference Model.* 2024.
- Ethayarajh et al. *IPO: Identity Preference Optimization.* 2024.
- Yuan et al. *Self-Rewarding Language Models.* 2024.
- Bai et al. *Constitutional AI: Harmlessness from AI Feedback.* 2022.
- Cui et al. *UltraFeedback: Boosting Language Models with Scaled AI Feedback.* 2024.
- Settles. *Active Learning.* Synthesis Lectures on AI and ML, 2009.

---

*Word count (main text, excluding title block and references): approximately 2,100 words.*
