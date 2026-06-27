# Research Proposal

## Cover Page

- **Applicant:** <待填:姓名>
- **Email:** <待填:邮箱>
- **Degree applied for:** PhD in Computer Science
- **Target institution:** <待填:港中文 CUHK / 港科 HKUST — 投递时确定主投校>
- **Prospective supervisor:** <待填:导师姓名 — 用户未提供,绝不编造>
- **Intended start date:** <待填:入学时间,如 2026 Fall>
- **Title:** Sample-Efficient Preference Alignment for Large Language Models: Characterizing the Role of Preference Data Composition
- **Date:** 2026-06-27

---

## Title

**Sample-Efficient Preference Alignment for Large Language Models: Characterizing the Role of Preference Data Composition**

---

## Abstract

Preference-based alignment methods such as Direct Preference Optimization (DPO) and its variants have made large language model (LLM) alignment substantially cheaper than Reinforcement Learning from Human Feedback (RLHF) with PPO, yet they still depend on tens to hundreds of thousands of human preference pairs whose collection is costly, slow, and noisy. This proposal investigates how the *composition* of preference data—its quality, diversity, difficulty, and chosen–rejected distributional overlap—shapes alignment outcomes, and whether alignment quality can be preserved with substantially fewer, more carefully selected preference pairs. Rather than proposing a single end-to-end method, this work aims to (i) empirically characterize which preference pairs are "alignment-relevant" across DPO, KTO, and SimPO; (ii) examine whether data-selection criteria derived from this characterization yield sample-efficient alignment without reward hacking or distributional collapse; and (iii) study how such efficiency gains transfer across model scales (1.5B–13B) and base families (Llama, Qwen, Mistral). The findings are expected to produce actionable understanding of when preference learning succeeds or fails under data scarcity, reduce the annotation and compute burden of alignment for academic labs, and yield open-source benchmarks and selection tools that complement recent efficiency-oriented alignment work. The direction directly extends the supervisor's line of work on RLHF and DPO by targeting the data-side bottleneck that currently limits the scalability of preference-based alignment.

---

## 1. Introduction & Background

The deployment of large language models in open-ended settings has made alignment with human preferences a central research problem [1], [2]. Reinforcement Learning from Human Feedback (RLHF) with PPO was the first widely adopted recipe [2], [3], but its two-stage reward-model-plus-RL pipeline is computationally expensive, unstable, and prone to reward hacking [4]. Direct Preference Optimization (DPO) reframed preference learning as a closed-form classification loss on preference pairs, eliminating the reward model and the RL loop [5]. A family of variants—IPO [6], KTO [7], SLiC-HF [8], and SimPO [9]—has since extended this paradigm, reducing alignment *compute* cost by roughly an order of magnitude.

Despite these advances, all such methods remain data-hungry. State-of-the-art open alignment recipes typically consume 50k–200k preference pairs [10], [9], collected via expensive human annotation or distillation from larger proprietary models. For academic labs with limited annotation budgets and modest compute (e.g., a handful of A100 GPUs), this *data* bottleneck is often more binding than the compute bottleneck: a single 7B DPO run is feasible on 4×A100, but obtaining and curating high-quality preference data at scale is not.

This raises a question that has received comparatively little systematic attention: do all preference pairs contribute equally to alignment, and if not, what makes a preference pair "alignment-relevant"? Existing studies report that DPO is sensitive to data quality and exhibits failure modes such as length bias [11, 需核实], chosen–rejected distribution shift [需核实], and degradation on out-of-distribution evaluation; RewardBench [12] further reveals substantial variance in reward model quality across datasets and scales. Yet there is no principled characterization of which preference pairs drive these outcomes, nor a consensus on whether the field's reliance on large preference datasets reflects genuine sample complexity or simply the absence of data-selection methodology.

This proposal targets that gap. It frames *sample-efficient preference alignment* as a research direction—understanding the data-side determinants of alignment quality and exploring whether principled data selection can match full-data alignment at a fraction of the cost. The direction is timely: open preference datasets are maturing, alignment methods have stabilized enough to serve as controlled baselines, and the supervisor's recent work on RLHF/DPO provides the methodological context in which this data-centric inquiry can be grounded. The scope is deliberately bounded to preference-learning methods applied to open-weight base models; pretraining, constitutional-AI, and closed-API alignment are out of scope.

---

## 2. Literature Review

Prior work is organized into three themes.

**Preference-based alignment methods.** RLHF with PPO established the modern alignment paradigm but suffers from reward-model miscalibration and RL instability [2], [4]. DPO [5] derived an equivalent objective that bypasses the reward model, training directly on preference pairs with a sigmoid loss on the implicit reward gap. IPO [6] argued that DPO's loss can overfit on deterministic preference data and proposed a KL-regularized alternative. KTO [7] recast alignment through prospect theory, requiring only binary thumbs-up/down signals rather than paired comparisons, thereby halving annotation cost. SLiC-HF [8] used sequence-likelihood calibration with rank-based losses, while SimPO [9] removed the reference model entirely, simplifying memory and improving throughput. These methods collectively reduce the *compute* cost of alignment; this proposal targets the orthogonal and equally important *data* cost. The gap in this theme: efficiency gains have come from changing the *loss*, not from understanding the *data* the loss consumes.

**Failure modes of preference learning.** A growing body of work documents when preference learning breaks down. DPO has been observed to inflate response length [11, 需核实] and to degrade when chosen and rejected distributions diverge [需核实]. Reward hacking in RLHF remains an open problem [4]. RewardBench [12] benchmarked reward-model quality and revealed substantial variance across datasets and model sizes. These findings motivate the central premise of this proposal: failure modes are *not uniformly distributed* across preference data but concentrate in identifiable subsets, which—if characterized—could be either pruned (for efficiency) or oversampled (for robustness). The gap: no existing work provides a controlled, method-agnostic mapping from preference-pair attributes to failure modes.

**Data selection and active learning for alignment.** Data selection is well established in supervised learning, but its application to preference alignment is nascent and fragmented. Preliminary efforts have explored preference-data filtering, active preference learning, and synthetic preference-data quality control [需核实 — several preliminary arXiv works; to be pinned during Year 1]. Each proposes a heuristic; none provides a controlled characterization of how preference-data composition interacts with *method choice* (DPO vs. KTO vs. SimPO) and *model scale*. The gap this proposal addresses is the absence of a systematic, method-agnostic understanding of preference-data efficiency—one that explains *why* certain subsets matter and yields transferable selection criteria rather than method-specific tricks.

In summary, prior work has made alignment cheaper to *compute* but has not systematically addressed whether it can be cheaper to *annotate*. This proposal positions sample-efficient preference alignment as a research direction that complements existing method-oriented work and connects directly to the supervisor's DPO/RLHF line.

---

## 3. Research Questions & Objectives

This proposal is organized around three logically progressive research questions:

- **RQ1 (Characterization).** How does the composition of preference data—along axes of quality, diversity, difficulty, and chosen–rejected distributional overlap—shape the alignment outcomes (helpfulness, harmlessness, robustness) of contemporary preference-learning methods (DPO, KTO, SimPO)?
- **RQ2 (Selection).** To what extent can preference data be pruned or actively selected to preserve alignment quality with an order of magnitude fewer pairs, and which data-selection criteria (e.g., implicit reward margin, response-difficulty estimates, embedding-space diversity) most reliably predict alignment-relevant samples *across* methods?
- **RQ3 (Transfer).** How do the data-efficiency gains and selection criteria identified in RQ1–RQ2 transfer across model scales (1.5B→7B→13B) and base families (Llama, Qwen, Mistral), and what does this reveal about the underlying sample complexity of preference alignment?

Corresponding objectives:

- **O1:** To produce a controlled empirical characterization of preference-data-composition effects on alignment outcomes.
- **O2:** To develop and rigorously evaluate data-selection criteria for sample-efficient alignment, with ablations isolating each criterion.
- **O3:** To derive scaling-aware insights about preference-data requirements that generalize beyond a single method or model.

Each RQ is formulated as a falsifiable question whose answer is *not* predetermined by the problem setting: the most predictive selection criterion, and the shape of the sample-efficiency curve, are open empirical questions.

---

## 4. Methodology & Feasibility

### 4.1 Overall Design

This is primarily an empirical/algorithmic study with a controlled-experiment structure: baseline reproduction → compositional analysis (RQ1) → data-selection method development (RQ2) → scaling transfer (RQ3) → ablation and robustness checks. The unit of analysis is the (preference dataset × method × model scale) triple; dependent variables are alignment quality, sample-efficiency curves, and failure-mode indicators.

### 4.2 Models & Checkpoints

Base models are restricted to open-weight families with permissive licenses: `meta-llama/Meta-Llama-3-8B-Instruct`, `Qwen/Qwen2.5-7B-Instruct`, and `mistralai/Mistral-7B-Instruct-v0.3`; for scaling analysis, `Qwen/Qwen2.5-1.5B-Instruct` and `Qwen/Qwen2.5-14B-Instruct` (the latter via QLoRA given compute limits). Exact HuggingFace commit hashes, the `transformers`/`trl` versions, and inference engine (vLLM version) will be pinned and logged in a Weights & Biases provenance log. Closed APIs (e.g., GPT-4-class models) are used *only* as an evaluation-side LLM judge, never as the core alignment engine, so that all main results are reproducible from open weights.

### 4.3 Data

Preference datasets: Anthropic HH-RLHF (CC-BY-4.0) [13], Stanford SHP-2 (CC-BY-4.0) [14, 需核实 license], and UltraFeedback [10] (license [需核实]). Train/val/test splits follow dataset conventions; to prevent leakage, prompts are deduplicated across splits and held-out test prompts are never used in any selection or training stage (no data leakage by construction). Evaluation: MT-Bench, AlpacaEval 2.0, Chatbot Arena Hard (arena-hard-auto), and RewardBench [12] for reward-model quality. No human subjects are involved in primary experiments; any human evaluation requires a separate IRB amendment budgeted in the timeline (~2–3 months).

### 4.4 Baselines (≥3, all recent SOTA)

DPO [5], KTO [7], and SimPO [9] serve as primary preference-learning baselines; SLiC-HF [8] and IPO [6] as secondary; SFT-only and PPO-RLHF (via the TRL implementation) as reference lower/upper bounds. These are chosen to span the design space—paired vs. unpaired, reference vs. reference-free—so that any data-efficiency finding is *not* method-specific.

### 4.5 Proposed Direction (intentionally not a fixed solution)

RQ1 will construct compositional subsets by stratifying preference pairs along measurable axes: implicit reward margin under a reference model, response-length difference, lexical and embedding-space diversity, and prompt-difficulty proxies. RQ2 will evaluate whether selection criteria derived from these axes—applied *before* training—can recover full-data alignment quality with ~10× fewer pairs. The proposal deliberately does *not* commit to a single selection algorithm; candidate directions include (a) influence-based selection (tests the hypothesis that *counterfactual importance* matters), (b) diversity-aware sampling (tests the *coverage* hypothesis), and (c) uncertainty-based active selection (tests the *learnability* hypothesis). Choosing among these on the basis of RQ1 evidence is itself a research outcome, not a predetermined design.

### 4.6 Evaluation & Statistics

Automatic metrics: AlpacaEval 2.0 win rate, MT-Bench score, Arena-Hard win rate, RewardBench accuracy; sample-efficiency curves (quality vs. # preference pairs). All runs use ≥3 random seeds (DPO-style training is moderately high-variance), reporting mean ± std; paired bootstrap significance tests on win rates. Ablations remove each selection criterion and each compositional axis in turn to isolate contributions. RL-style baselines (PPO) additionally report return and sample efficiency, given their high variance.

### 4.7 Compute & Reproducibility

Hardware: 4×A100 80GB (consistent with the host lab's resources), with optional scaling to 8×A100 via the university HPC cluster when available. Estimated cost: a single 7B DPO run on 100k pairs ≈ 8–12 GPU-hours; the full RQ1 characterization across methods × subsets × seeds ≈ 1.5–2k GPU-hours over Years 1–2, well within budget; 13B runs use QLoRA. All code, configs, prompts (version-controlled), seeds, and selected-data indices will be released on GitHub with a Zenodo DOI; a Docker image will pin dependencies for bit-identical reproduction. QLoRA/GPTQ-AWQ runs will report multi-seed variance, given known non-determinism in quantized inference paths.

### 4.8 Feasibility & Risk Mitigation

| Risk | Prob. | Impact | Mitigation |
|---|---|---|---|
| Direction scooped (arXiv same-topic) | Med | High | Monthly arXiv monitoring; explicit differentiation via the method-agnostic characterization angle; pre-registered fallback RQ on cross-method transfer. |
| Insufficient compute / queueing | Med | High | QLoRA for 13B; sub-sampling experiments on 1.5B/7B first; apply for university HPC allocation in Year 1; rent cloud A100s as backstop. |
| Baseline reproduction failure | Med | Med | Contact authors; use official checkpoints; fall back to TRL reference implementations with documented deviation. |
| Automatic evaluation unreliability | Med | Med | Multi-metric cross-checks; Arena-Hard (correlates with human judgment); small-scale human eval under IRB. |
| Dataset license / takedown change | Low | High | Local backups; secondary datasets (HH-RLHF + SHP-2 as redundant pairs). |

---

## 5. Research Plan / Timeline

| Phase | Time | Tasks | Deliverables |
|---|---|---|---|
| Y1 Q1 | M1–3 | Literature deep-dive; reproduce DPO/KTO/SimPO on HH-RLHF; pin reproducibility pipeline | Baseline reproduction report; W&B provenance template |
| Y1 Q2–Q3 | M4–9 | RQ1 compositional study; build stratified subsets; first 1.5B/7B scaling run | First conference submission (data-composition characterization) |
| Y1 Q4–Y2 Q2 | M10–18 | RQ2 data-selection development + ablations; IRB amendment submitted | Second conference submission (sample-efficient alignment) |
| Y2 Q3–Q4 | M19–24 | RQ3 scaling transfer (7B→13B); small human evaluation | Journal-track extended study |
| Y3 | M25–36 | Integration, cross-method generalization, thesis writing | Thesis draft; final open-source release |

A ~10% buffer is reserved each year for reproduction surprises and arXiv monitoring. Tasks are explicitly dependent: RQ2 selection criteria are derived from RQ1 evidence; RQ3 scaling builds on RQ2's chosen criteria.

---

## 6. Expected Outcomes & Significance

**Theoretical contribution.** A method-agnostic understanding of how preference-data composition drives alignment outcomes—explaining *why* certain preference pairs matter, not merely *that* they do. This answers the "so what" beyond leaderboard movement: the expected impact is not a 2% benchmark gain but a transferable characterization that informs how the community *collects and curates* preference data.

**Practical contribution.** Open-source data-selection tools, curated subsets, and sample-efficiency benchmarks that reduce the annotation and compute burden of alignment to a level reachable by academic labs with a handful of A100s, broadening participation in alignment research.

**Fit with the host lab.** This work directly extends the supervisor's DPO/RLHF line by surfacing the data-side bottleneck that limits its scalability, and it produces reusable artifacts (curated subsets, selection criteria, efficiency curves) that can seed follow-up work on preference-data quality, synthetic data, and active alignment. The complementary positioning—data-side efficiency alongside the lab's method-side efficiency—maximizes synergy rather than overlap.

---

## References

[1] L. Ouyang *et al.*, "Training language models to follow instructions with human feedback," in *NeurIPS*, 2022.
[2] N. Stiennon *et al.*, "Learning to summarize from human feedback," in *NeurIPS*, 2020.
[3] Y. Bai *et al.*, "Training a helpful and harmless assistant with RLHF," arXiv:2204.05862, 2022. (Anthropic HH-RLHF dataset.)
[4] K. Casper *et al.*, "Open problems and fundamental limitations of RLHF," *NeurIPS Alignment Workshop*, 2023. [需核实 venue]
[5] R. Rafailov *et al.*, "Direct preference optimization: Your language model is secretly a reward model," in *NeurIPS*, 2023.
[6] M. G. Azar *et al.*, "A general theoretical paradigm to understand learning from human preferences," in *AISTATS*, 2024. (IPO.)
[7] K. Ethayarajh *et al.*, "KTO: Model alignment as prospect theoretic optimization," in *ICML*, 2024.
[8] Y. Zhao *et al.*, "SLiC-HF: Sequence likelihood calibration with human feedback," arXiv:2305.10425, 2023.
[9] Y. Meng *et al.*, "SimPO: Simple preference optimization with a reference-free reward," in *NeurIPS*, 2024. [需核实 venue]
[10] G. Cui *et al.*, "UltraFeedback: Boosting language models with high-quality feedback," in *ICML*, 2024. [需核实 venue/license]
[11] R. Park *et al.*, "Disentangling length from quality in DPO," 2024. [需核实 — existence and venue]
[12] N. Lambert *et al.*, "RewardBench: Evaluating reward models for language modeling," in *ICML*, 2024. [需核实 venue]
[13] Anthropic, "HH-RLHF: Helpful and harmless human preference data," 2022. (CC-BY-4.0.)
[14] Stanford CRFM, "SHP-2: A large-scale dataset of human preferences," 2023. [需核实 citation/license]

---

## Notes (中文说明)

1. **选题角度**: 用户给定"让 LLM 对齐更高效"较模糊,本 RP 收敛到 **sample-efficient preference alignment**,即从 *data-side* 切入(DPO 系方法已大幅降低 compute cost,但 data cost 仍是学术实验室主要瓶颈)。该角度与导师 RLHF/DPO 方向直接衔接,同时避免与"再提一个 DPO 变种"的红海竞争。
2. **研究方向而非解决方案**(原则7): RQ2 明确不预设单一选择算法,而是把"哪种 selection criterion 有效"作为研究产出。读者读完 problem setting 后,仍无法猜出最终采用 influence-based / diversity-aware / uncertainty-based 中的哪一种——这正是研究而非工程。
3. **回答 "so what"**(原则8): Expected Outcomes 显式声明"impact is not a 2% benchmark gain but a transferable characterization",落点在理解与社区数据实践。
4. **算力诚实**(原则9): 实验室"几张 A100"按 **4×A100 80GB** 规划;13B 用 QLoRA;**不承诺**从头预训练、全参微调 70B 或自建大规模众包标注。
5. **Open weights + 可复现**: 全部核心实验用 Llama-3-8B / Qwen2.5 / Mistral;闭源 API 仅作评估侧 LLM judge,主结果可复现。给出精确 HF model ID、commit hash 计划、seed(≥3)、QLoRA 非确定性说明、GitHub+Zenodo+Docker 开源计划。
6. **占位项**: `<待填:姓名/邮箱/导师姓名/院校/入学时间>` —— 用户未给的信息绝不编造,投递前补全。
7. **待核实引用**: 标 `[需核实]` 的条目(SimPO/RewardBench/UltraFeedback venue、Park et al. length-bias 论文是否存在、SHP-2 citation/license、Casper workshop venue)建议用户用 Google Scholar / DBLP 核实后再投递。
8. **导师匹配**: 用户套磁的导师"做 LLM 对齐,最近发 RLHF 和 DPO"。本 RP 直接基于 DPO 系方法展开,Literature Review 与 Expected Outcomes 显式对接。**强烈建议**用户拿到导师近 3 年论文列表后,在 Lit Review 增加一段 *"Closely related work from the host lab"* 显式 cite 2–3 篇,并在 Methodology 中说明可复用导师组的开源代码(如 TRL/DPO 实现链路)。
9. **多校改写提醒**: 用户申港中文+港科两校,本版为通用模板。投递前需为每校单独改写导师段与匹配说明(原则5)。
10. **未做的事**: 未编造导师姓名、论文标题、arXiv ID;未使用 "will prove/revolutionize" 等过度承诺表述。
