# Research Proposal

**Towards Scalable and Reasoning-Capable Long Video-Language Models: Memory-Efficient Long-Context Modelling and Compositional Temporal Reasoning for Video-Language Agents**

Applicant: [Candidate Name]
Intended Supervisor: [Supervisor Name]
Programme: Doctor of Philosophy in Computer Science / Artificial Intelligence
Proposed Duration: 36 months (Full-time)

---

## 1. Abstract

Recent video-language models (VidLMs) have made remarkable progress on short clips, yet they degrade sharply when confronted with long-form video (minutes to hours). The bottleneck is twofold: the quadratic cost of self-attention over thousands of frames, and the inability of current models to perform fine-grained *temporal reasoning*—ordering, duration estimation, causal attribution, and state-change tracking—over extended temporal horizons. This PhD proposes an integrated research programme to advance long video understanding along two complementary axes: (i) memory-efficient long-context modelling that preserves temporally localised detail while remaining computationally tractable, and (ii) compositional temporal reasoning that equips video-language agents with structured, tool-augmented capacities for retrieval, planning, and grounded question answering. The work will produce open models, benchmarks, and an agent framework evaluated on long-form understanding tasks, with the goal of moving VidLMs from passive "video captioners" toward deliberative agents that understand video over time.

## 2. Introduction and Motivation

Video is the dominant modality of human experience and an ever-growing fraction of the web. Understanding long video—lectures, films, surveillance, egocentric life logs, and instructional content—requires not only recognising *what* is in a scene, but reasoning about *when*, *how long*, *why*, and *what happens next*. These are temporal reasoning problems, and they are precisely where contemporary video-language models fail.

The current paradigm—sampling a handful of frames, encoding them with a vision encoder, and projecting the tokens into a large language model (LLM)—scales poorly. A 32-frame sampling strategy collapses a one-hour video into less than a second of effective coverage, destroying the very temporal structure needed for reasoning. Naively increasing the frame count is infeasible: self-attention scales quadratically with sequence length, and even with linearised or sparse attention, the GPU memory cost of caching thousands of visual tokens during autoregressive decoding becomes prohibitive. Recent efforts such as memory-bank models, hierarchical token merging, and streaming architectures mitigate but do not resolve this tension, and they rarely evaluate the *reasoning* capabilities that long-context is supposed to enable.

A second, under-explored gap concerns *agency*. The supervisor's group has pioneered work on long video understanding and video-language agents, framing video comprehension as an interactive, multi-step process rather than a single forward pass. This perspective reframes the problem: rather than stuffing an entire video into a fixed context window, an agent should *decide what to look at*, retrieving moments on demand, accumulating evidence, and composing answers through deliberate reasoning steps. This is analogous to how a human rewinds, skims, and focuses when answering a question about a film.

This proposal aligns directly with that vision. I will investigate how to build video-language models that (a) maintain a faithful, queryable, and memory-efficient representation of long video, and (b) reason over that representation compositionally and temporally, with the option to act as an agent that revisits the video as needed. The research is timely: long-video benchmarks (LongVideoBench, Video-MME, HourVideo, EgoSchema) have matured to the point where reasoning failures are measurable, and open foundation models now permit the controlled experimentation required for a PhD.

## 3. Background and Related Work

**Long-context video-language models.** Early VidLMs such as Video-LLaMA, VideoChat, and Video-LLaVA demonstrated that image-pretrained LLMs can be adapted to short clips. Extending to long video has produced several families of approaches. *Sparse-sampling* methods (e.g., VideoChatGPT, LLaVA-NeXT-Video) keep the token budget small but lose temporal resolution. *Memory-bank* models (MovieChat, VideoChat-Flash, LongVU) maintain a growing memory of past segments, often with consolidation strategies that merge or forget tokens. *Streaming* architectures process video frame-by-frame or chunk-by-chunk, updating a hidden state. *Hierarchical* approaches pool tokens across spatial and temporal scales to compress long sequences. Each family trades off fidelity against tractability, and none has established a principled method for preserving the fine-grained temporal cues required for reasoning.

**Temporal reasoning in video.** Benchmarks such as Next-QA, CLEVRER, and the temporal splits of TVQA and EgoSchema probe specific reasoning skills—causality, temporal order, counterfactuals, and state transitions. Findings consistently show that even strong models regress on questions requiring multi-step temporal inference, and that performance correlates weakly with overall captioning quality. This suggests temporal reasoning is a distinct capability that is not an emergent by-product of scale.

**Efficient long-context transformers.** The broader NLP community has developed linear attention, sparse attention, retrieval-augmented generation (RAG), and token pruning to handle long contexts. Transferring these to *visual* sequences is non-trivial because visual tokens are dense, redundant, and spatially structured. Recent work on visual token merging, query-based compression, and learned memory suggests that vision-specific designs outperform off-the-shelf text mechanisms.

**Video-language agents.** Agentic LLM systems that call tools, retrieve documents, and decompose tasks have transformed NLP. The video analogue—agents that can *seek* within a video, call a temporal localisation tool, or maintain a working memory of watched moments—is nascent but fits naturally with the supervisor's research agenda. Existing prototypes treat the video as a static knowledge base; a more powerful framing treats the model itself as a controller that decides what to attend to next.

A clear opportunity therefore exists to unify memory-efficient long-context modelling with structured temporal reasoning in an agentic architecture, which is the gap this proposal addresses.

## 4. Research Questions and Objectives

The overarching research question is: *How can video-language models represent and reason over long-form video efficiently and faithfully enough to support temporally-grounded, agentic comprehension?*

This decomposes into three sub-questions:

- **RQ1 (Memory):** How can we construct a video memory representation that is simultaneously compact, queryable, and sufficient for preserving temporally localised detail over hour-scale video?
- **RQ2 (Reasoning):** How can we induce compositional temporal reasoning—ordering, duration, causality, and state change—over such a memory, rather than relying on it to emerge from scale?
- **RQ3 (Agency):** How should a video-language agent decide when and where to "look again" within a long video, and how does agentic retrieval compare to monolithic long-context attention in terms of accuracy, latency, and cost?

The objectives are to (O1) design and train a memory-efficient long-context video encoder; (O2) develop a temporal reasoning module and corresponding training methodology; (O3) integrate these into an agent framework with tool use and active retrieval; and (O4) evaluate on standard long-video benchmarks and a newly constructed reasoning-focused evaluation, releasing all code and models.

## 5. Proposed Approach

The programme is organised into three work packages (WPs), each addressing one research question, with a shared evaluation infrastructure.

### 5.1 WP1: Hierarchical, Event-Centred Video Memory (addresses RQ1)

The first contribution is a *temporal hierarchical memory* that represents long video at multiple granularities. Rather than uniformly compressing frames, the model will maintain: (i) a *frame-level* stream of fine-grained tokens for short-term, high-resolution access; (ii) an *event-level* layer of learned tokens that abstract contiguous segments into semantic events; and (iii) a *video-level* summary token set for global context. Critically, the event layer will be induced through a contrastive objective that aligns event tokens with temporally localised text descriptions and with the underlying frame tokens, enforcing that abstractions remain *grounded* and *reconstructable*.

To control cost, the memory will support (a) token merging guided by temporal continuity—adjacent and visually consistent tokens are merged preferentially, preserving change-points—and (b) a learned forgetting policy that retains tokens likely to be queried, trained with a meta-objective derived from downstream QA performance. The hypothesis is that a hierarchy whose abstraction is explicitly aligned to events outperforms uniform compression at equal token budgets, particularly on questions targeting specific moments. I will evaluate this against strong baselines using uniform frame sampling, existing memory banks, and off-the-shelf token merging, all measured at matched FLOPs and memory.

### 5.2 WP2: Compositional Temporal Reasoning (addresses RQ2)

WP2 tackles the reasoning deficit directly. The central idea is that temporal reasoning is *compositional*—a long-horizon question decomposes into operations over the memory: *locate* a moment, *order* two events, *compare* states before and after, *measure* duration, *infer* cause. I will introduce a lightweight *temporal reasoning module* that, conditioned on the question, emits a structured program (or latent plan) of such operations, executes them against the WP1 memory, and returns an answer.

Two training regimes will be explored. First, *program-supervised* training on synthetic data where the decomposition is known, generated from procedural video-and-question templates (order, duration, causality, state change). Second, *self-supervised* bootstrapping on real long video, where the model proposes decompositions, executes them, and is rewarded by answer correctness and temporal consistency losses—e.g., predicted event orderings must agree with ground-truth timestamps where available. The compositional prior is expected to improve sample efficiency and interpretability, and to generalise better to out-of-distribution reasoning patterns than monolithic end-to-end baselines.

A secondary deliverable is a diagnostic benchmark isolating each reasoning primitive, enabling fine-grained attribution of failures—something current holistic benchmarks do not support.

### 5.3 WP3: Agentic Long-Video Comprehension (addresses RQ3)

WP3 unifies WP1 and WP2 into a *video-language agent*. The agent operates in a loop: given a question, it (i) queries the video-level memory for a coarse plan, (ii) issues tool calls—`seek(t)`, `summarise(t1,t2)`, `compare(t1,t2)`, `count(event)`—against the hierarchical memory, (iii) accumulates evidence in a working memory, and (iv) composes a final answer. The tools wrap the WP1 memory and WP2 reasoning primitives, making the agent's deliberation transparent and auditable.

Training will combine imitation from a weaker teacher's tool traces with reinforcement from answer correctness and a cost penalty that discourages redundant retrieval. The agent will be compared against (a) monolithic long-context models with the same backbone and (b) retrieval-augmented baselines that treat the video as a static corpus. The hypothesis is that agentic retrieval matches or exceeds monolithic attention at a fraction of the compute for hour-scale video, and that the gap widens with video length and question complexity.

### 5.4 Shared Evaluation and Infrastructure

All WPs share a common evaluation harness spanning LongVideoBench, Video-MME, EgoSchema, HourVideo, and the new diagnostic benchmark from WP2, reported at matched token budgets and wall-clock latency. Models will be built on open foundation backbones to ensure reproducibility.

## 6. Evaluation Plan

Evaluation proceeds at three levels. *Capability*: accuracy on standard long-video QA benchmarks, reported separately for short, medium, long, and hour-scale subsets. *Reasoning*: accuracy on the WP2 diagnostic benchmark, broken down by primitive (order, duration, causality, state change), enabling targeted failure analysis. *Efficiency*: peak GPU memory, FLOPs, and end-to-end latency as functions of video length, compared against baselines at matched accuracy. Ablation studies will isolate the contribution of the hierarchical memory, the compositional reasoning module, and the agentic loop. Where temporal ground truth is available (e.g., from dense captioning datasets), I will additionally measure *temporal localisation error* to verify that improvements stem from genuinely better temporal grounding rather than dataset artefacts.

## 7. Project Timeline

- **Months 1–6:** Literature consolidation; reproduce baseline VidLMs and long-video benchmarks; finalise the WP1 architecture design.
- **Months 7–15 (WP1):** Implement and train the hierarchical event-centred memory; ablate against compression baselines; first conference submission.
- **Months 16–24 (WP2):** Develop the compositional reasoning module and synthetic/real training data; construct the diagnostic benchmark; second submission.
- **Months 25–33 (WP3):** Integrate the agent loop and tools; run imitation and reinforcement training; large-scale evaluation.
- **Months 34–36:** Consolidate findings; complete the thesis; release code, models, and benchmarks.

## 8. Expected Contributions and Impact

The thesis will deliver four artefacts: (1) a memory-efficient, event-centred long-context video representation with principled forgetting and grounding; (2) a compositional temporal reasoning methodology with an accompanying diagnostic benchmark that decomposes long-video reasoning into measurable primitives; (3) an open video-language agent framework that retrieves and reasons on demand, validating an agentic alternative to monolithic long-context attention; and (4) empirical evidence, at matched compute, characterising when hierarchical memory, structured reasoning, and agency each pay off.

Beyond the academic contributions, the work has direct relevance to video search, content moderation, accessibility (audio description and navigation for long media), embodied AI, and educational technology. By engaging closely with the supervisor's ongoing programme on long video understanding and video-language agents, the project is positioned to both draw from and extend a leading research agenda, with outputs intended for top-tier venues (e.g., CVPR, ICCV, NeurIPS, ICLR) and released openly to maximise community impact.

## 9. Why This Programme and Supervisor

The proposed work sits squarely within the supervisor's established research line on long video understanding and video-language agents. The combination of memory-efficient long-context modelling (WP1), structured temporal reasoning (WP2), and an agentic architecture (WP3) is a natural and ambitious extension of that agenda, and one that I am uniquely motivated to pursue given my background in multimodal learning. The programme's structure—three nested contributions that build on one another—provides both clear milestones and the flexibility to adapt as the fast-moving field evolves, making it well-suited to a three-year UK PhD.

---

*References are available on request; key works include MovieChat, VideoChat-Flash, LongVU, LongVideoBench, Video-MME, HourVideo, EgoSchema, Next-QA, CLEVRER, and recent literature on retrieval-augmented and agent-based multimodal systems.*
