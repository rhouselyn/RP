# Research Proposal 修改报告

> 本文件分两部分:第一部分为对照 `references/checklist.md` 的诊断报告;第二部分为改后完整 RP。两部分以分隔线隔开。
> 申请人方向:美国 BME 博士,可穿戴生理信号监测用于心血管疾病早期预警;目标导师方向为生物传感器与信号处理。

---

# 第一部分:诊断报告

按技能"阶段 C:有初稿要打磨"流程,先对照 `references/checklist.md` 逐项核对初稿,按"致命问题 > 结构问题 > 表达问题"排序,列出 top 问题。每条引用初稿原文具体句子。

## 致命问题(Top 3,结构级,必须重写)

### 问题 1:Title 过宽,无方法 / 无问题 / 无场景(checklist A1;tips #1)

- **原文**:`Title: Wearable Devices for Health`
- **问题**:"Health" 是过宽词,导师无法从标题判断:(a) 做哪类心血管问题;(b) 用什么传感器 / 信号 / 方法;(c) 在什么场景下监测。违背"题目含方法+问题+场景"的要求。
- **后果**:招生官 10 秒内无法判断方向是否匹配,直接被刷。
- **改法**:收敛到一个可被回答的 RQ,Title 写成 `[方法] for [问题] in [场景]`。

### 问题 2:无 Research Questions,只有主题陈述(checklist B-RQ、C1;tips #2)

- **原文**:`In this research, I will use machine learning to analyze wearable signals.`
- **问题**:全文没有任何疑问句式 RQ,只有"我会用 ML 分析信号"这类主题陈述。没有 RQ 就没有评估标准,无法回答"做出来算成功吗"。
- **后果**:RP 失去灵魂,变成项目说明而非研究计划。
- **改法**:写出 2-3 个互相有逻辑递进、可被回答的疑问句式 RQ,每个对应一个 objective 与假设。

### 问题 3:Methodology 是 to-do list,无方法细节(checklist B-Methodology;tips #5;stem-methods.md)

- **原文**:`First I will collect data. Then I will train a model. Then I will evaluate it.` 以及 `The methodology is: collect data, train model, evaluate.`
- **问题**:纯任务清单,没有数据集 / 传感器 / 信号处理方法 / 评估指标 / 统计 / 伦理,也没解释"为什么这是回答 RQ 的最佳方式",更没有替代方案比较与风险评估。
- **后果**:导师在此判断"这学生能不能做出成果"——to-do list 等于没说,被判为可行性不足。
- **改法**:按数据驱动/ML 模板展开到可复现程度,含数据集、传感器、信号处理、模型、评估指标、统计、伦理、风险预案。

## 结构问题(必须补齐)

### 问题 4:无 Literature Review,背景无证据支撑(checklist B-Intro/LR、C;tips #4)

- **原文**:`Cardiovascular disease is the leading cause of death worldwide. Many people die from it every year. Wearable devices are very important.`
- **问题**:每个"重要"都是空洞断言,无数据 / 文献支撑;完全没有文献综述与 gap 识别,无法证明"这个问题值得做"。
- **改法**:Introduction 漏斗式收敛到 RQ,每个判断给数据或文献;Literature Review 按主题组织、5C 齐全、收敛到 gap。

### 问题 5:无可行性 / 风险论证、无 Timeline(checklist B-Methodology/Timeline、C4;tips #6、#12)

- **原文**:整篇无 feasibility、risk、timeline 内容。
- **问题**:导师最怕招来做不下去的学生,缺这部分直接判负。
- **改法**:补 feasibility 四要素 + 风险表(含具体 Plan B)+ 分阶段 Timeline(留缓冲)。

## 表达问题(逐句修)

### 问题 6:过度承诺(checklist D;tips #7)

- **原文**:`This will definitely revolutionize cardiovascular care.`
- **问题**:RP 是计划不是结论,"definitely revolutionize"过度承诺,显不严谨。
- **改法**:改为 `aims to` / `is expected to` / `may inform`。

### 问题 7:主观立场过重(checklist D;tips #8)

- **原文**:`I think we can use them to detect heart problems early.` / `I believe this approach is the best.`
- **问题**:学术文书应客观,主观词显不专业。
- **改法**:用证据和文献推导;热情靠精准识别 gap 展现。

### 问题 8:编造引用(checklist C;tips #9)

- **原文**:`[1] Some paper on wearables.` / `[2] Another paper on ML.`
- **问题**:非真实可核实文献,一查即穿,直接出局。
- **改法**:引用必须真实;拿不准的标 `[需核实]`,绝不编造。本改稿所有引用均以 `[需核实]` 占位并附检索建议,请申请人核实后补全。

## 与导师方向的匹配(初稿完全缺失)

- 初稿未体现与"生物传感器 + 信号处理"导师方向的任何呼应点。改稿将以 PPG/ECG 传感器融合与运动伪影信号处理为主线,直接对接导师两条研究脉络。

---

# 第二部分:改后完整 Research Proposal

> 中文说明以引用块 `> 中文说明:` 形式穿插于各节之后,便于理解。所有引用均标注 `[需核实]`,申请人需自行核实并补全完整书目信息(作者、年份、期刊、DOI),本稿不编造任何引用。

---

## Title

**Motion-Robust PPG–ECG Sensor Fusion for Early Detection of Paroxysmal Atrial Fibrillation in Wearable Cardiovascular Monitoring**

> 中文说明:Title 含三要素——方法(motion-robust PPG–ECG sensor fusion,运动鲁棒的 PPG–ECG 传感器融合)、问题(early detection of paroxysmal atrial fibrillation,阵发性房颤早期检测)、场景(wearable cardiovascular monitoring,可穿戴心血管监测)。已去掉"Health"等过宽词,且不含"Novel"等自评词。该题目同时呼应导师"生物传感器"(PPG/ECG 硬件传感)与"信号处理"(运动伪影鲁棒处理)两条脉络。

---

## Abstract

Cardiovascular disease remains the leading cause of mortality globally, and paroxysmal atrial fibrillation (pAF)—often asymptomatic and self-terminating—is a major precursor of stroke that frequently goes undetected by clinic-based electrocardiography. Wrist-worn photoplethysmography (PPG) enables continuous, unobtrusive monitoring, yet its reliability for arrhythmia detection degrades sharply under daily-life motion artifacts. This research investigates how motion-robust signal processing combined with PPG–single-lead-ECG sensor fusion can improve the early detection of pAF in free-living wearable settings. Specifically, it aims to (i) develop an adaptive motion-artifact removal pipeline that preserves arrhythmia-relevant morphology, (ii) quantify whether multi-modal PPG–ECG fusion improves pAF sensitivity relative to PPG-only baselines, and (iii) characterize the latency–false-alarm trade-off of lightweight models deployable on edge wearable hardware. The expected outcomes include a validated signal-processing pipeline, an open benchmark protocol, and design guidelines that may inform next-generation wearable cardiovascular sensors—directly extending the host group's work on biosensor development and physiological signal processing.

> 中文说明:Abstract 四句式——背景(pAF 是卒中前兆且临床 ECG 难捕捉)、gap(PPG 在日常运动下可靠性差)、做法(运动鲁棒信号处理 + PPG-ECG 融合,三个具体目标)、意义(验证流程 + 开放基准 + 设计指南,呼应导师方向)。约 180 词,符合 200-300 词区间偏紧凑,可按学校要求扩。

---

## 1. Introduction & Background

Cardiovascular disease (CVD) is the leading cause of death worldwide, accounting for an estimated 17.9 million deaths annually, with atrial fibrillation (AF) being the most common sustained cardiac arrhythmia and a major contributor to AF-related stroke [WHO global estimates, 需核实]. Paroxysmal AF (pAF) episodes are short-lived, frequently asymptomatic, and self-terminating, so they are often missed by sporadic clinic 12-lead electrocardiography (ECG); continuous ambulatory monitoring is therefore critical for early detection and stroke-prevention therapy [需核实: 近年 AF 检测综述].

Wrist-worn photoplethysmography (PPG) sensors—already embedded in consumer wearables—offer a low-cost, unobtrusive, and continuous route to vascular-pulse monitoring, and have been shown to detect AF from pulse irregularity with reasonable performance under resting conditions [需核实: PPG-AF 检测代表性研究]. However, PPG is exquisitely sensitive to motion artifacts: during activities of daily living, accelerometer-correlated noise can corrupt the waveform to the point that arrhythmia-relevant morphology is lost, causing both false negatives (missed pAF) and false alarms [需核实: 运动伪影对 PPG 影响的研究]. Single-lead ECG patches provide higher-fidelity cardiac signals but are less convenient for long-term wear and still face motion-induced baseline wander.

This creates a clear gap: no current wearable approach robustly detects pAF under free-living motion while remaining comfortable for multi-day monitoring. The convergence of (i) low-power biosensor hardware, (ii) mature adaptive signal-processing methods, and (iii) publicly available annotated arrhythmia datasets makes this an opportune moment to address it. This research focuses on wearable pAF early detection; it explicitly does not address stroke-risk stratification, therapy selection, or in-hospital monitoring, to keep scope tractable.

> 中文说明:Introduction 漏斗式——宽(CVD/AF 全球负担,给数据)→ 中(PPG 可持续监测但受运动伪影影响,ECG 保真但不便)→ 尖(gap:无方法在自由活动下鲁棒检测 pAF 且适合多日佩戴)→ 收敛到 RQ 并设定边界。每个"重要"都有占位证据(标 [需核实])。边界声明避免 scope 漂移。

---

## 2. Literature Review

The relevant literature clusters into three themes.

**Theme 1 — PPG-based AF detection.** Prior work has consistently demonstrated that pulse-to-pulse interval irregularity derived from wrist PPG can identify AF during rest, with several studies reporting sensitivities above 90% on controlled datasets [需核实: PPG-AF 代表性论文 2-3 篇,优先近 3-5 年]. While these results are encouraging, they rely on clean, motion-free segments, and performance collapses when the same pipelines are applied to free-living data—leaving the motion-robustness question unresolved.

**Theme 2 — Motion-artifact handling in PPG.** A second line of work tackles artifact removal via accelerometer-based adaptive filtering, wavelet denoising, and signal-quality indices that discard corrupted segments [需核实: 运动伪影去除方法对比研究]. These approaches improve signal quality but tend to either over-suppress arrhythmia-relevant morphology or discard large fractions of usable data, and there is active debate over which quality metric best preserves diagnostic content—a gap this RP directly addresses.

**Theme 3 — Multi-modal sensor fusion for arrhythmia detection.** More recent studies fuse PPG with accelerometry or with short-duration ECG to improve robustness [需核实: PPG-ECG/PPG-IMU 融合研究]. Comparing these, fusion generally improves specificity, but the added value of single-lead ECG specifically for pAF (rather than persistent AF) under motion remains under-explored, and the latency–false-alarm trade-off on edge hardware has not been systematically characterized.

**Synthesis & gap.** Across the three themes, the unresolved problem is not whether PPG can detect AF in principle, but how to make detection motion-robust and deployable for pAF in daily life. This RP addresses this gap by combining adaptive artifact suppression (Theme 2) with PPG–ECG fusion (Theme 3), evaluated under realistic motion and edge-deployment constraints.

> 中文说明:Lit Review 按主题(三 theme)组织,非流水账。每个 theme 内 cite + compare + contrast + critique,末尾点 gap 并 connect 到本 RP。引用全部 [需核实],需申请人补全近 3-5 年(占比 ≥50%)对口文献。

---

## 3. Research Questions & Objectives

Building on the gap above, this research is guided by three logically progressive questions:

- **RQ1 (signal-processing foundation):** How can motion artifacts in wrist-worn PPG signals be adaptively suppressed while preserving the waveform morphology required to detect paroxysmal atrial fibrillation?
- **RQ2 (sensor-fusion contribution):** To what extent does fusing PPG with single-lead ECG improve pAF detection sensitivity and specificity compared with PPG-only baselines under free-living motion conditions?
- **RQ3 (deployable trade-off):** What is the trade-off between detection latency and false-alarm rate when the fused model is constrained to run on edge wearable hardware with limited compute and power?

**Objectives.**
- O1: To develop and validate an adaptive motion-artifact-removal pipeline that maximizes retained diagnostic waveform content.
- O2: To design and evaluate a PPG–ECG fusion model for pAF detection and quantify its marginal benefit over PPG-only baselines.
- O3: To characterize the latency–false-alarm trade-off of lightweight, edge-deployable implementations.

**Corresponding hypotheses (falsifiable).**
- H1: Under matched motion conditions, the proposed adaptive pipeline retains ≥ X% more arrhythmia-relevant morphology than fixed-threshold quality-index discarding, without increasing residual artifact energy (`<待填: 阈值 X 由预实验确定>`).
- H2: PPG–ECG fusion improves pAF sensitivity by a statistically significant margin over the best PPG-only baseline at a fixed specificity.
- H3: On edge hardware, a sub-`<待填: 参数>` quantized model can achieve a detection latency below `<待填: 秒>` with a false-alarm rate within an actionable clinical range.

> 中文说明:3 个 RQ 疑问句式,逻辑递进(信号处理基础 → 多模态融合 → 边缘部署权衡),每个对应一个 objective 与可证伪假设。RQ1 直接对接导师"信号处理"方向,RQ2 对接"生物传感器"多模态采集。阈值用 `<待填>` 占位,不编造。

---

## 4. Methodology & Feasibility

### 4.1 Overall Design

This is a **data-driven / machine-learning study with a signal-processing core**, combining public annotated datasets with a small prospective self-collection for external validation. The workflow proceeds: dataset preparation → adaptive artifact suppression (RQ1) → PPG–ECG fusion modeling (RQ2) → edge deployment & trade-off characterization (RQ3) → statistical evaluation. Each step is chosen because it directly answers one RQ, as justified below.

### 4.2 Datasets & Sensors

- **Primary public datasets (annotated):** `<待填: 具体数据集名,如包含同步 PPG/ECG 与 AF 标注的公开库>` [需核实], used for training and internal validation. These provide synchronized PPG, accelerometry, and reference ECG with beat-level AF labels.
- **Self-collected validation set:** A prospective cohort of `<待填: 目标例数,基于 power analysis>` consenting adults wearing a research-grade wrist PPG + single-lead ECG patch during a standardized activity protocol (rest, walking, typing, stair-climbing), to assess generalization to free-living motion. Sample size will be determined by a power analysis targeting a clinically meaningful sensitivity difference (e.g., α = 0.05, power = 0.8) [需核实].
- **Sensors:** research-grade wrist PPG (green/infrared dual-wavelength) and a single-lead ECG patch; on-device 3-axis accelerometer for motion reference. `<待填: 具体传感器型号/课题组现有设备>`.
- **Preprocessing:** 50/60 Hz notch filtering, band-pass for PPG (0.5–8 Hz) and ECG (0.5–40 Hz), segment alignment, normalization, and explicit train/validation/test splitting by subject (no data leakage across subjects).

### 4.3 Signal-Processing Methods (RQ1)

- **Adaptive artifact suppression:** a motion-reference adaptive filter (e.g., LMS/RLS using accelerometer channels as reference) combined with a wavelet-based residual denoiser, plus a learned signal-quality index that *flags* (rather than discards) low-quality segments.
- **Why this over alternatives:** fixed-threshold discarding is simplest but loses arrhythmia morphology during motion exactly when pAF is hardest to detect; pure wavelet denoising cannot distinguish artifact from physiological irregularity. The hybrid adaptive+learned-quality approach is chosen to *preserve* diagnostic content—the core of RQ1.
- **Artifact metrics:** signal-to-noise ratio improvement, percentage of retained usable waveform, and preservation of arrhythmia-relevant morphological features measured against reference ECG.

### 4.4 Fusion Modeling (RQ2)

- **Baselines (≥3):** PPG-only pulse-irregularity detector; PPG + accelerometry quality-gated detector; a published PPG-AF deep-learning baseline [需核实].
- **Proposed fusion model:** a late/intermediate fusion architecture combining PPG and single-lead ECG feature streams (e.g., 1-D CNN or CNN-Transformer backbone) with accelerometer-derived quality weights; loss function tuned for class imbalance (pAF is rare).
- **Hyperparameter search:** Bayesian optimization over a defined search space; all runs reported as mean ± std over multiple seeds and subject-level k-fold cross-validation.
- **Why fusion over PPG-only:** ECG provides morphological ground truth that PPG lacks; fusion is hypothesized to recover pAF episodes that PPG alone misses during motion—but this is exactly what RQ2 tests, rather than assumes.

### 4.5 Edge Deployment & Trade-off (RQ3)

- **Implementation:** model quantization (INT8) and pruning; inference benchmarked on a representative low-power wearable SoC / microcontroller `<待填: 具体平台>`.
- **Trade-off characterization:** detection latency vs. false-alarm rate curves via threshold sweeping; memory/energy budgets reported.

### 4.6 Evaluation Metrics & Statistics

- **Detection metrics:** sensitivity, specificity, F1, and AUROC for pAF episode detection; separately reported for rest vs. motion segments. Class imbalance justifies F1/AUROC over accuracy.
- **Signal metrics (RQ1):** SNR improvement, retained-segment ratio, morphological-preservation score.
- **Edge metrics (RQ3):** latency (ms), peak memory (kB), energy per inference (mJ).
- **Statistical testing:** paired tests across subjects for H2 (e.g., DeLong test for AUROC comparison; McNemar for sensitivity/specificity), significance level α = 0.05 with multiple-comparison correction; effect sizes reported. Software: Python (NumPy/SciPy/scikit-learn/PyTorch) and MATLAB for signal-processing prototyping.

### 4.7 Ethics & Compliance

This study uses (i) de-identified public datasets under their original consent terms and (ii) a prospective self-collected cohort involving human participants. The prospective component will require Institutional Review Board (IRB) approval, written informed consent, and data de-identification in compliance with HIPAA (and GDPR-equivalent standards where applicable). No animal subjects are involved. Participants' right to withdraw and data-retention limits will be specified in the consent form `<待填: 目标院校 IRB 流程>`.

### 4.8 Feasibility (Four Elements)

1. **Resource feasibility:** PPG/ECG hardware and edge-compute platforms are available through the host group's biosensor lab `<待填: 课题组设备>`; public annotated datasets reduce primary data-collection burden.
2. **Technical feasibility:** adaptive filtering, CNN/Transformer arrhythmia models, and quantized edge inference are individually mature in the literature [需核实]; the novelty lies in their integration for motion-robust pAF, lowering technical risk.
3. **Human-resource feasibility:** the 3-year timeline (Section 5) with built-in buffers accommodates the planned scope.
4. **Applicant feasibility:** relevant coursework/projects in signal processing and biomedical instrumentation are detailed in the CV `<待填: 申请人具体经历>`.

### 4.9 Risk & Contingency

| Risk | Likelihood | Impact | Concrete contingency |
|---|---|---|---|
| Public dataset lacks synchronized PPG–ECG under motion | Medium | High | Use PPG+accelerometry-only datasets for RQ1; partner with `<待填: 合作实验室>` for ECG-paired data; expand self-collection scope. |
| Adaptive filter over-suppresses arrhythmia morphology | Medium | Medium | Fall back to quality-gated discarding for RQ1 sub-question; report retained-morphology trade-off curves rather than a single point. |
| Fusion gain over PPG-only is not statistically significant | Medium | Medium | Reframe RQ2 contribution as a negative-result benchmark + analysis of when fusion helps (subgroup by motion type); still publishable. |
| Edge hardware cannot meet latency target | Low | High | Move heavy inference to a paired smartphone (edge-cloud hybrid); report the device–phone partition trade-off. |
| Prospective cohort recruitment slower than planned | Medium | Medium | Reduce cohort to a pilot validation set and rely primarily on public data; extend recruitment into Y2 buffer. |
| Research direction pre-empted by concurrent work | Low | Medium | Quarterly arXiv/trial tracking; pivot angle toward the under-studied pAF (not persistent AF) specificity. |

> 中文说明:Methodology 已非 to-do list——每个方法都给了"为什么选它而非替代方案"(4.3、4.4),具体到数据集/传感器/信号处理/模型/评估指标/统计/伦理/可行性四要素/风险表。风险表每条预案"具体可执行",非"will find alternatives"。明确声明研究类型(数据驱动+信号处理核心)。变量与假设见第 3 节。直接呼应导师"生物传感器"(PPG/ECG 硬件、多模态采集)与"信号处理"(自适应去噪、运动鲁棒)两条脉络。

---

## 5. Research Plan / Timeline (3-Year PhD)

| Phase | Time | Tasks | Expected Output |
|---|---|---|---|
| Y1 | M1–9 | Deep literature reading; reproduce PPG-only baseline; dataset preparation; IRB submission | Lit-review chapter; baseline reproduction report; IRB approval |
| Y1–Y2 | M10–18 | RQ1: adaptive artifact-suppression pipeline development & validation | Conference paper (RQ1) |
| Y2 | M19–27 | RQ2: PPG–ECG fusion modeling; prospective self-collection | Journal submission (RQ2) |
| Y2–Y3 | M28–33 | RQ3: edge deployment & trade-off characterization | Journal submission (RQ3) |
| Y3 | M34–36 | Integration, thesis writing, defense prep | Thesis draft; defense |

Built-in buffer: M9 (IRB), M18, M27 each reserve ~2 months for unexpected delays; phases are dependent—RQ2 builds on RQ1's clean-signal output, RQ3 builds on RQ2's frozen model.

> 中文说明:Timeline 分阶段表格,每阶段含时间/任务/预期产出;留缓冲;体现 RQ 间依赖(基于 RQ1 结果做 RQ2)。

---

## 6. Expected Outcomes & Significance

- **Theoretical contribution:** a characterization of how motion-artifact suppression and multi-modal fusion interact for pAF detection, including a quantified latency–false-alarm trade-off on edge hardware.
- **Practical contribution:** a validated, reproducible signal-processing pipeline; an open evaluation protocol benchmarking motion-robustness; and design guidelines that may inform next-generation wearable cardiovascular sensors.
- **Significance:** earlier pAF detection can expand the pool of patients eligible for stroke-prevention therapy. By centering on sensor fusion and signal processing, the work is designed to extend the host group's biosensor and physiological-signal-processing program rather than duplicate it.

> 中文说明:理论 + 实践贡献兼顾;用"may inform""is designed to extend"克制表述,无"revolutionize"式过度承诺;结尾点明与导师课题组的互补匹配。

---

## 7. References

> 以下均为占位条目,申请人须自行核实并补全完整书目(作者、年份、期刊/会议、卷期页、DOI)。本稿不编造任何引用;凡 [需核实] 处需替换为真实可核实文献。

1. [需核实] Global cardiovascular disease mortality and atrial fibrillation prevalence statistics — authoritative source (e.g., WHO Global Health Estimates / Global Burden of Disease report). *检索建议:WHO 官网 + GBD 报告,近 3 年数据。*
2. [需核实] 近 3-5 年 AF/pAF 检测与临床意义综述 1-2 篇。*检索建议:PubMed "paroxysmal atrial fibrillation detection review 2022-2025"。*
3. [需核实] PPG-based AF detection 代表性 original 论文 2-3 篇(含 rest-condition 高性能基线)。*检索建议:IEEE TBME / npj Digital Medicine,近 5 年。*
4. [需核实] 运动伪影对 PPG 影响及伪影去除方法(自适应滤波 / 小波 / 信号质量指数)对比研究 2-3 篇。*检索建议:IEEE EMBC / TBME,关键词 "PPG motion artifact removal"。*
5. [需核实] PPG–ECG / PPG–IMU 多模态融合用于心律失常检测研究 2-3 篇。*检索建议:IEEE JBiH / TBME,近 3-5 年。*
6. [需核实] 含同步 PPG/ECG/加速度与 AF 标注的公开数据集(用于 4.2 训练)。*检索建议:PhysioNet / MIMIC 系列及相关 wearable AF 数据库。*
7. [需核实] 模型量化/剪枝与边缘微控制器推理 benchmark 方法论文献(用于 RQ3)。*检索建议:TinyML / edge ML for biomedical signals,近 3 年。*
8. [需核实] 目标导师课题组近 3 年生物传感器与信号处理相关论文 2-3 篇(用于匹配论证)。*检索建议:导师主页 Publications,务必核实其当前方向后再写入正文。*

> 中文说明:参考文献全部 [需核实] 占位 + 检索建议,绝不编造。第 8 条专为指导匹配预留——申请人务必去导师主页核实其近 3 年实际工作后,在正文 Introduction/Lit Review 中具体引用并呼应。引用格式建议统一用 IEEE numeric(理工科 BME 常用),补全后核对格式一致。

---

> **交付前 3 连问自测**
> 1. 招生官 10 分钟能否一句话说出"这学生要做什么"?——能:用运动鲁棒的 PPG-ECG 融合做可穿戴阵发性房颤早期检测。
> 2. 导师读完能否判断"课题接得上我的 lab"?——能:主线是传感器融合(生物传感器)+ 自适应伪影处理(信号处理),与导师两条脉络直接对应。
> 3. 任何一句"重要/合适/创新"是否有证据或比较支撑?——是:每个判断均配 [需核实] 占位证据或方法比较。
