# 中文说明(供申请人参考)

本 RP 按你提供的信息(英 PhD、多模态/视频-语言模型、导师做长视频理解与 video-language agent)撰写。设计要点:

- **主线**:从"诊断长视频时序推理为何失败"→"显式时序表征能否优于均匀压缩"→"agent 主动检索/grounding 能否重构上限"。三问递进,RQ3 直接对接导师 video-language agent 方向。
- **研究方向而非方案**:方法部分只点到"为何此路可行 + 关键挑战",留开放问题,避免把 PhD 三年实现写死。
- **回答 so what**:重点不是刷 Video-MME,而是揭示长视频推理失败的 root cause(capacity vs retrieval vs 显式时序结构),并据此判断压缩竞赛与 agent 路线孰优。
- **诚实算力**:不承诺从头预训练;以开源 7B 权重微调为主,辅以 2B 小模型做机制分析。
- **占位符**:`<待填:xxx>` 处需你填实(姓名、学校、导师姓名等)。
- **引用**:均为真实工作;具体作者/年份/会议拿不准处已标 `[需核实]`,提交前请逐一核对 DOI/arXiv 编号。导师本人论文请你从其主页挑 2-3 篇近 3 年代表作插入 [需核实导师代表作] 处,本 RP 未替你编造。
- **字数**:英文正文(Intro 至 Expected Outcomes,不含 Abstract)约 2,065 词;含 Abstract 约 2,266 词;含 Title/References/中文说明总文件约 2,720 词(占位符补全后趋近 3,000)。

---

# Research Proposal

**Applicant:** `<待填:姓名>` · `<待填:邮箱>`
**Degree:** PhD (Computer Science / Multimodal AI)
**Target institution:** `<待填:目标院校>`
**Prospective supervisor:** `<待填:导师姓名>` (research interests: long video understanding; video-language agents)
**Intended start:** `<待填:入学时间,如 Oct 2026>`
**Date:** 2026-06-27

---

## Title

Temporal Reasoning in Long Video-Language Models: From Diagnostic Failures toward Video-Language Agents

## Abstract

Long-form video understanding has become a defining challenge for video-language models (VLMs), with benchmarks spanning hour-scale movies, egocentric streams, and instructional footage. Progress is measured almost exclusively in aggregate accuracy, while a more basic question remains under-examined: *why* do VLMs fail at temporal reasoning over long horizons, and what does this reveal about the limits of passive long-context encoding? This proposal investigates long-temporal reasoning through three connected questions: (i) how temporal reasoning degrades with video length and token compression, and which failure mode dominates; (ii) whether explicit temporal representations preserve long-range reasoning more effectively than uniform compression; and (iii) how reframing long-video QA as a video-language agent task—actively retrieving, grounding, and reasoning over temporal evidence—reshapes these limits. Methodology centres on open-weight VLMs (Qwen2-VL, LLaVA-Video, LongVA) evaluated on EgoSchema, MVBench, Video-MME, and LongVideoBench, with controlled length/compression ablations, temporal-grounding probes, and a prototype agent loop. Rather than chasing leaderboard gains, the work seeks mechanistic insight into where and why long-video reasoning breaks, and whether an agent framing offers a principled alternative to scaling context. The findings aim to inform video-language agents that operate reliably over long horizons, connecting directly to the supervisor's research programme on video-language agents and long video understanding.

## 1. Introduction & Background

Video is the dominant modality of human experience and of growing machine deployment—robotic demonstration, embodied assistance, and accessibility all depend on understanding events that unfold over minutes to hours. The recent generation of video-language models (VLMs) extends large language models (LLMs) with visual encoders and supports multi-turn question answering over video clips [1,2]. Benchmarks have grown accordingly: Video-MME and LongVideoBench evaluate models on hour-scale content [3,4], while EgoSchema probes long-form egocentric understanding [5].

Despite headline gains, two observations motivate this proposal. First, aggregate accuracy on long-video benchmarks masks substantial, systematic failures on *temporal* questions—those requiring ordering ("which came first"), duration estimation ("how long did X last"), and causal-before/after reasoning [6,7]. A model may score well on "what is on the table" yet fail on "what changed after the person returned". Second, the dominant technical response to long video is *token compression*: frame subsampling, spatial pooling, and memory banks that squeeze hours of video into a fixed token budget [8,9]. Compression is judged by preserved QA accuracy, but rarely by whether temporal structure survives. This raises an unresolved question: are long-video temporal failures primarily a capacity problem (too few tokens), a retrieval/positional problem (the right tokens exist but cannot be located), or a representation problem (temporal structure is never explicitly encoded)?

The proposed research treats long-temporal reasoning as the object of study rather than a side effect of scaling. Its scope is *temporal reasoning* (when/how-long/ordering/counterfactual-before) over videos of roughly 3–60 minutes, using open-weight VLMs as the experimental substrate, explicitly excluding general captioning and short-clip action recognition. A third consideration connects to the target lab: an alternative to passive long-context encoding is the *video-language agent* paradigm, in which the model actively decides where to look, what to retrieve, and when to ground a claim to a timestamped segment [10,11]. Whether such an agent framing addresses temporal failures more efficiently than scaling context is a tomorrow's-problem worth investigating now.

## 2. Literature Review

We organise prior work around three themes relevant to temporal reasoning in long video.

**Long-context VLMs and token compression.** A first line extends image-based VLMs to video by uniformly sampling frames and projecting them as visual tokens. Video-LLaVA and VideoChat established this recipe for short clips [1,2]; MovieChat introduced long-term memory with merging for movie QA [8]. More recent systems—LLaVA-Video, LongVA, and VideoLLaMA2/3—scale frame counts and context length, sometimes transferring long-context capability from the language backbone to vision [9,12,13]. InternVideo2 and Qwen2-VL push this further with stronger encoders and native long-context attention [14,15]. A consistent design choice is *uniform* token handling: temporal position is implicit, and compression is optimised for average accuracy. **Gap:** few works isolate *temporal* from non-temporal accuracy, so it is unclear whether compression preserves the structure needed for ordering and duration reasoning or merely preserves recognition of salient objects.

**Timestamp-aware and temporal-grounding VLMs.** A second line makes time explicit. TimeChat injects timestamp embeddings so the model can reference "when" [16]; VTimeLLM fine-tunes for temporal grounding via stage-wise alignment [17]. These perform well on grounding benchmarks (Charades-STA, ActivityNet-Captions [18,19]) but are typically evaluated on clips of seconds to minutes, and their behaviour at hour scale—where grounding errors compound and candidate segments explode—is under-studied. NeXT-QA shows causal and temporal question types are harder than descriptive ones even on short video [6]. **Gap:** the regime where timestamp-aware methods break, and whether their failure is a capacity or a retrieval limit at long horizon, is not characterised.

**Video-language agents.** A third, emerging line treats video QA as multi-step reasoning with tools. VideoMind proposes an actor–locator–reasoner pipeline for grounded video understanding [20]; ReAct-style prompting lets LLM agents call external tools [21]. The supervisor's programme on video-language agents and long video understanding advances exactly this direction `<待填:此处插入导师 2-3 篇近 3 年代表作,需核实>` [需核实导师代表作]. Agents offer a structurally different hypothesis: rather than encoding everything passively, retrieve on demand and ground claims to evidence. **Gap:** agent approaches are benchmarked on accuracy, not analysed against the failure modes of passive encoding; the *division of labour* between capacity and retrieval is not principled.

Across all three themes, the field optimises accuracy and treats temporal failures as collateral. This proposal inverts the priority: make temporal reasoning the measurement axis, and ask which family of approaches—compression, explicit temporal structure, or active retrieval—addresses its root causes.

## 3. Research Questions & Objectives

**RQ1 (diagnostic).** *How and where does temporal reasoning ability degrade as video duration grows and the token budget is compressed in current VLMs—and is the dominant failure mode capacity limitation, positional/retrieval failure, or absence of explicit temporal structure?*

**O1.** To characterise, via controlled length-and-compression sweeps and targeted temporal probes, the failure profile of open-weight VLMs across 3–60-minute video, isolating temporal from non-temporal accuracy.

**RQ2 (representation).** *Can explicit temporal representations (e.g., timestamp-conditioned retrieval, hierarchical coarse-to-fine memory) preserve long-range temporal reasoning more efficiently than uniform token compression, and under what length/complexity regimes do gains appear?*

**O2.** To develop and ablate explicit-temporal-structure variants and compare them against matched-budget uniform-compression baselines, reporting the length regime where each dominates.

**RQ3 (agent, supervisor-aligned).** *How does reframing long-video QA as a video-language agent task—where the model actively retrieves, grounds, and reasons over timestamped evidence—reshape the limits observed under passive encoding, and what does this reveal about the right division of labour between model capacity and retrieval?*

**O3.** To prototype an agent loop over hour-scale video and analyse, against passive-encoding baselines, where retrieval helps, where it introduces error, and what this implies for agent design.

The three questions are logically progressive: RQ1 establishes *what* fails and *why* (mechanism), RQ2 tests whether fixing that mechanism via representation helps, and RQ3 asks whether the agent paradigm sidesteps it entirely—directly extending the supervisor's video-language agent line.

## 4. Methodology & Feasibility

### 4.1 Overall design
The work is **mixed-type**: a diagnostic empirical study (RQ1), an algorithmic-method exploration (RQ2), and an agent-system prototype (RQ3). The path is baseline reproduction → diagnostic study → representation ablations → agent prototype → integration. No pretraining is attempted; all experiments start from open weights, keeping compute tractable.

### 4.2 Models and checkpoints
Open-weight VLMs are used throughout for reproducibility: Qwen2-VL-7B-Instruct, LLaVA-Video-7B, and LongVA-7B as primary 7B models; Qwen2-VL-2B as a smaller substrate for mechanistic sweeps [12,14,15]. Exact HuggingFace model IDs and commit hashes will be pinned `[需核实 exact HF IDs/commit hashes]`. This mix separates *length* effects (within a fixed model) from *capacity* effects (across 2B/7B), and avoids closed APIs whose endpoints drift.

### 4.3 Data and benchmarks
Public benchmarks only (no IRB required for the core study): **EgoSchema** (egocentric, ≥3 min) [5]; **MVBench** (20 temporal-reasoning tasks) [22]; **Video-MME** (up to 60 min) [3]; **LongVideoBench** (up to 1 h+) [4]. For temporal-grounding probes we use **Charades-STA** and **ActivityNet-Captions** [18,19], plus a small model-assisted, human-verified temporal-ordering probe set we construct (see §4.6). Splits follow official protocols; we additionally split by *temporal vs non-temporal* question type to expose the gap aggregate accuracy hides. No subject-level leakage arises since all data is public and benchmark-predefined.

### 4.4 Methods
*RQ1 — diagnostic sweep.* For each model, vary max-frames (8/16/32/64/128) and per-frame token budget along a matched-compute curve. Measure accuracy on temporal vs non-temporal subsets, plus probes for ordering, duration, before/after. Decompose errors into *capacity* (fails even with full tokens), *retrieval* (correct only when relevant frames are force-fed), and *representation* (correct frames present, wrong temporal answer) categories. This decomposition is the central empirical contribution.

*RQ2 — explicit temporal structure.* Explore three complementary directions (chosen as a *direction*, not a frozen architecture): (a) timestamp-conditioned token embeddings; (b) hierarchical coarse-to-fine memory retrieved by query; (c) retrieval-augmented temporal attention. Each module is justified relative to a specific RQ1 failure mode—hierarchical memory targets retrieval failures, timestamp embeddings target representation failures. We ablate per module and against a matched-budget uniform-compression baseline.

*RQ3 — agent prototype.* Implement a loop: query decomposition → timestamped-chunk retrieval → temporal grounding tool → multi-step answer with citations, compared to passive long-context encoding at equal token budget. We analyse not only accuracy but *where the agent helps* (long-horizon ordering) and *where it injects error* (retrieval misses), connecting back to RQ1 categories. This stage is the explicit bridge to the supervisor's agent programme.

### 4.5 Baselines
At least three recent open baselines: **LLaVA-Video-7B** (representative long-context VLM) [12], **LongVA-7B** (long-context transfer) [15], and **Qwen2-VL-7B** with native long context [14]; for the agent track, a **VideoMind-style** pipeline as an agent baseline [20] `[需核实 VideoMind open-source status]`. Selection rationale: these span the two dominant long-video strategies (extended context vs agent), all open-weight, all within 7B-class compute reach. Where an official checkpoint exists we use it rather than re-training.

### 4.6 Evaluation
- *QA accuracy*, split temporal vs non-temporal, per benchmark.
- *Temporal grounding*: mIoU and R@1/5/10 on Charades-STA/ActivityNet-Captions.
- *Agent*: task success, retrieval precision, step count, sample efficiency.
- *Statistics*: ≥3 seeds (≥5 for agent runs), mean±std with paired-bootstrap significance tests.
- *Ablations*: per RQ1 decomposition category; per RQ2 module; per RQ3 agent component (retrieval/grounding/decomposition on-off).

### 4.7 Compute and reproducibility
Estimated hardware: 4–8 × A100 80GB for 7B fine-tuning and agent training; single A100 for 2B sweeps and inference. **Honest constraint:** we cannot pretrain or match industry-scale training; the design relies on open checkpoints, LoRA/full fine-tuning of 7B, and 2B models for controlled mechanism studies. All seeds, inference temperatures, and top-p values are logged; if quantisation (AWQ/GPTQ) is used, non-determinism is acknowledged and multi-seed variance reported. Code, configs, prompts, and checkpoints will be released on GitHub with a Zenodo DOI; provenance logged via Weights & Biases. If human evaluation is added later, an IRB window of 2–3 months is budgeted.

### 4.8 Feasibility
*Resources:* open benchmarks + lab GPU cluster `<待填:确认导师组算力>`; *technical:* mature training stacks (transformers, deepspeed); *personnel:* supervisor's expertise in video-language agents directly supports RQ3; *applicant:* prior multimodal/ML project experience (see CV) `<待填:简述申请人相关项目基础>`.

## 5. Research Plan / Timeline

| Phase | Months | Tasks (with dependencies) | Deliverables |
|---|---|---|---|
| P1 Foundation | Y1 M1–6 | Reading; reproduce LLaVA-Video/LongVA/Qwen2-VL on Video-MME & EgoSchema; build probe toolkit | Reproduction report; probe set v1 |
| P2 RQ1 diagnostic | Y1 M7–12 → Y2 M1–4 | Length/compression sweeps; error decomposition (depends on P1) | Conference paper (early signal) |
| P3 RQ2 representation | Y2 M5–12 | Explicit-temporal variants + ablations (depends on P1 failure modes) | Workshop/journal submission |
| P4 RQ3 agent | Y3 M1–8 | Agent prototype vs passive encoding (depends on P2/P3) | Second conference paper |
| P5 Buffer & thesis | Y3 M9–12 | Integration, writing, optional human-eval IRB amendment | Thesis draft |

Buffer is reserved in P5 and woven into each phase; dependencies are sequential (P2 uses P1 baselines; P3 targets P2 failure modes; P4 builds on P2/P3 components), avoiding a parallel disconnected plan.

## 6. Risks and Mitigation

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| **Direction scooped** on arXiv | Medium | High | Monthly arXiv monitoring; differentiate via the *mechanistic-decomposition* and *agent-division-of-labour* framings, not scores; reserve RQ3 as a pivot |
| **Compute insufficient / queue contention** | Medium | High | Mechanistic studies on 2B; LoRA fine-tuning; institutional HPC; cloud GPU fallback; never depend on industry-scale runs |
| **Long-video annotation cost** for probes | Medium | Medium | Rely on public benchmarks; model-assisted annotation with human-verified subset; budget IRB window; keep probes small and high-signal |
| **Baseline reproduction failure** | Medium | Medium | Official checkpoints; contact authors; fall back to next-best open baseline with justification |
| **Benchmark saturation** | Low | Medium | Temporal probes and decomposition are our own, less saturated; insights not contingent on SOTA |
| **Weights/API drift** | Low | Medium | Pin model IDs + commit hashes; snapshot locally; report drift |

## 7. Expected Outcomes & Significance

*Theoretical:* a characterisation of *why* long-video temporal reasoning fails—capacity, retrieval, or representation—and a principled comparison of compression versus explicit temporal structure versus active retrieval. This reframes a field organised around "compress more tokens" into one asking "compress, encode, or retrieve?".

*Practical:* an open diagnostic toolkit (probes + decomposition protocol) reusable beyond the specific models studied, and a prototype video-language agent whose analysis informs design rather than claims SOTA.

*Significance and fit:* the agent analysis (RQ3) extends the supervisor's programme on video-language agents, while RQ1–RQ2 anchor long video understanding in mechanism rather than benchmark chasing. The work does not promise to "solve" long-video understanding; it promises to make the field's central trade-off legible and to point agent design at the right failure modes.

## References

> Format: IEEE numeric. All entries are real works; items marked `[需核实]` need the applicant to confirm exact author list, venue, and arXiv/DOI before submission. The applicant should also insert 2–3 representative papers from the prospective supervisor's homepage at the placeholder in §2.

[1] B. Lin et al., "Video-LLaVA: Learning United Visual Representation by Alignment Before Projection," *arXiv:2305.13816*, 2023. [需核实]

[2] K. Li et al., "VideoChat: Chat-Centric Video Understanding," *arXiv:2305.06355*, 2023. [需核实]

[3] C. Fu et al., "Video-MME: The First-Ever Comprehensive Evaluation Benchmark of Multi-modal LLMs in Video Analysis," *arXiv:2405.21075*, 2024. [需核实]

[4] H. Wu et al., "LongVideoBench: A Benchmark for Long-context Interleaved Large Language Models on Long Videos," *arXiv:2407.15754*, 2024. [需核实]

[5] K. Mangalam et al., "EgoSchema: A Diagnostic Benchmark for Very Long-form Video Language Understanding," *CVPR*, 2024. [需核实]

[6] J. Xiao et al., "Next-QA: Next Phase of Question-Answering to Reasoning Temporal and Causal Compositions," *CVPR*, 2021. [需核实]

[7] `<待填:一项明确报告长视频时序失败现象的近期分析论文,需核实>`

[8] E. Song et al., "MovieChat: From Dense Token to Sparse Memory for Long Video Understanding," *CVPR*, 2024. [需核实]

[9] `<待填:代表性 long-context 视频压缩工作,需核实>`

[10] `<待填:导师 video-language agent 代表作,需核实>`

[11] `<待填:导师 long video understanding 代表作,需核实>`

[12] Y. Zhang et al., "Video Instruction Tuning with Synthetic Data" (LLaVA-Video), *arXiv:2410.02713*, 2024. [需核实]

[13] `<待填:VideoLLaMA2/3 引用,需核实>`

[14] P. Wang et al., "Qwen2-VL: Enhancing Vision-Language Model's Perception of the World at Any Resolution," *arXiv:2409.12191*, 2024. [需核实]

[15] `<待填:LongVA 引用(Zhang et al., 2024, long-context transfer),需核实 exact arXiv>`

[16] S. Ren et al., "TimeChat: A Time-sensitive Multimodal Large Language Model for Long Video Understanding," *ICLR*, 2024. [需核实]

[17] B. Huang et al., "VTimeLLM: Empower LLMs to Grasp Video Moments," *ECCV*, 2024. [需核实]

[18] J. Gao et al., "TALL: Temporal Activity Localization via Language Query," *ICCV*, 2017 (Charades-STA). [需核实]

[19] L. Anne Hendricks et al., "Localizing Moments in Video with Natural Language," *ICCV*, 2017 (ActivityNet-Captions). [需核实]

[20] `<待填:VideoMind 引用(Zheng et al., 2024, video-language agent),需核实>`

[21] S. Yao et al., "ReAct: Synergizing Reasoning and Acting in Language Models," *ICLR*, 2023. [需核实]

[22] K. Li et al., "MVBench: A Comprehensive Multi-modal Video Understanding Benchmark," *CVPR*, 2024. [需核实]
