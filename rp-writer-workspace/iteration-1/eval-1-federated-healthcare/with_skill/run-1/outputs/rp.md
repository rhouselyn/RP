# Personalized Federated Learning with Adaptive Differential Privacy for Non-IID Healthcare Data: Balancing Clinical Utility, Privacy, and Communication Efficiency

> **中文说明**:本 RP 用于 Imperial College London 博士申请套磁附件。导师 Prof. Li 研究方向为联邦学习与隐私保护机器学习,故选题聚焦"联邦学习 × 医疗数据 × 隐私"的交叉。下方为英文正文(Sections 1–8 + References),关键决策处附中文简注;末尾 Notes 列出对用户未给信息所做的假设。

---

## 1. Abstract

Healthcare institutions generate vast, heterogeneous, and privacy-sensitive data that cannot be pooled for model training due to regulations such as GDPR and HIPAA. Federated learning (FL) offers a paradigm for training on distributed data without raw sharing, yet its deployment in healthcare is impeded by three intertwined obstacles: severe non-IID heterogeneity across sites, residual privacy leakage from gradient updates, and the resulting utility–privacy tension. This research investigates how the structure of heterogeneity in multi-site medical data shapes FL behaviour, and proposes a personalized FL framework with client-adaptive differential-privacy (DP) budgets to improve the utility–privacy trade-off. Specifically, it (i) characterizes non-IID effects on convergence and diagnostic performance, (ii) develops an adaptive personalization–DP mechanism with provable guarantees, and (iii) quantifies the operating frontier among accuracy, privacy, and communication cost. The work is expected to deliver both methodological contributions to privacy-preserving FL and an open benchmark aligned with Prof. Li's research programme on trustworthy federated machine learning.

*【中文注释:Abstract 遵循"背景—gap—做法—意义"四句式,约 230 词。】*

---

## 2. Introduction & Background

Clinical machine-learning models trained at a single institution typically suffer from limited sample sizes and selection bias; multi-site collaboration would dramatically improve generalization, but raw data sharing is prohibited by HIPAA in the U.S., GDPR in the E.U./U.K., and analogous regimes elsewhere. Federated learning (FL) [1] addresses this by exchanging only model updates, enabling collaboratively trained models without centralizing patient data.

Healthcare, however, is among the most demanding FL application domains. Patient populations differ systematically across hospitals in age, comorbidity, equipment, and labelling practice, producing feature and label distributions that are strongly non-IID. Such heterogeneity has been shown to slow convergence and degrade global model accuracy [2]. At the same time, the gradients shared in FL are not inherently private: membership inference [22] and gradient inversion attacks have demonstrated that sensitive information can be recovered from updates, making formal privacy guarantees essential in the clinical setting. Differential privacy (DP) [20] and secure aggregation [5] provide complementary protections, but the noise injected by DP directly trades off model utility — a trade-off especially painful when the underlying task is already accuracy-sensitive (e.g., sepsis prediction, chest-X-ray triage).

A second concern is communication. Hospital networks often impose strict bandwidth and uptime constraints, and naive FL requires many rounds of full-model exchange. Compression and quantization have been proposed [24], but their interaction with DP noise — which itself alters gradient statistics — remains poorly understood, particularly on heterogeneous medical data where the signal-to-noise margin is thin.

This RP therefore targets the joint problem: *how to deliver clinically useful, formally private, and communication-efficient federated models on heterogeneous healthcare data*. The work is motivated by, and designed to extend, recent research at Imperial College London on federated and privacy-preserving machine learning led by Prof. Li <待填:核实 Prof. Li 全名拼写与代表性论文>. By anchoring the contribution in healthcare — a setting that exposes all three failure modes (non-IID, privacy, communication) simultaneously — the proposal aligns with the group's emphasis on trustworthy FL while offering a concrete, high-impact application.

*【中文注释:Introduction 用漏斗结构:监管背景→FL 方案→三大障碍→收敛到 RQ。每个"重要"都给文献或政策支撑。】*

---

## 3. Literature Review

### 3.1 Federated learning in medicine

Early demonstrations confirmed the feasibility of FL on medical imaging: Sheller et al. [6] trained a brain-tumour segmentation model across ten institutions without sharing data, reporting performance comparable to centralized training under benign conditions. Rieke et al. [7] surveyed the digital-health landscape and identified non-IID data, privacy, and regulatory fragmentation as the principal blockers. Kaissis et al. [8] combined FL with cryptographic protocols for end-to-end privacy-preserving medical imaging. More recently, the EXAM study [26] federated 20 institutions for COVID-19 outcome prediction, demonstrating FL's clinical value at scale — but also revealing that sites with atypical populations contributed disproportionately to global error. **Gap:** most medical FL studies treat non-IID heterogeneity as an empirical nuisance rather than a first-class object of study.

### 3.2 Heterogeneity and personalization in FL

To mitigate non-IID effects, FedProx [2] introduced a proximal term, while q-FedAvg [3] explicitly targeted fairness across clients. Personalization has emerged as a more principled response: model-agnostic meta-learning (Per-FedAvg) [18], shared-representation splitting [17], and personalization layers [21] all retain a global component while letting each client adapt locally. **Gap:** these methods are evaluated predominantly on CV benchmarks (CIFAR, FEMNIST); their behaviour on clinical tasks — where the *label distribution itself* is diagnostically meaningful (e.g., disease prevalence varies by site) — is largely uncharacterized.

### 3.3 Privacy-preserving FL

DP-SGD [4] underpins most private deep learning, and client-level DP extensions [10] adapt it to FL. Secure aggregation [5] hides individual updates from the server but does not protect against aggregate inference. Existing DP-FL algorithms typically allocate a uniform privacy budget across clients [10, 19], which is suboptimal when client contributions are heterogeneous. **Gap:** how to *adaptively* allocate DP budgets across heterogeneous clients while preserving the formal guarantee — and whether personalization amplifies or mitigates privacy leakage — remain open.

Across all three themes, the combined challenge (non-IID + adaptive DP + communication) has not been studied as a single coherent problem, especially in a healthcare setting. This RP targets precisely that intersection.

*【中文注释:Lit Review 按 3 个 theme 组织,每个 theme 末尾点 gap;5C 已覆盖 cite/compare/contrast/critique/connect。】*

---

## 4. Research Questions & Objectives

This research is organized around three progressively deeper questions:

> **RQ1 (diagnosis).** How does the structure and severity of non-IID heterogeneity in multi-site medical data affect the convergence behaviour and diagnostic performance of standard federated algorithms (FedAvg, FedProx, Per-FedAvg)?

> **RQ2 (method).** Can a personalization-aware FL framework with *client-adaptive* differential-privacy budgets achieve a strictly better utility–privacy operating point than uniform-DP baselines on heterogeneous healthcare tasks, and under what heterogeneity regimes does the gain hold?

> **RQ3 (trade-off).** How do gradient compression and quantization interact with DP noise injection, and what is the achievable operating frontier among diagnostic accuracy, formal privacy (ε, δ), and communication cost in hospital-scale deployments?

**Objectives.**

- **O1.** To construct a controlled multi-site partition of public medical datasets that exposes tunable non-IID axes (feature shift, label shift, quantity skew), and to characterize RQ1 empirically and analytically.
- **O2.** To design, implement, and prove privacy guarantees for an Adaptive Personalized DP-FL framework (hereafter APDP-FL) that allocates per-client DP budgets according to heterogeneity and contribution.
- **O3.** To empirically map the three-way operating frontier (RQ3) and release a reproducible benchmark.

*【中文注释:三个 RQ 形成"诊断→方法→权衡"递进;每个 RQ 对应 1 个 Objective;RQ 为疑问句,O 为动作句。】*

---

## 5. Methodology & Feasibility

**Research type:** data-driven / machine-learning research, combining empirical experiments on public medical datasets with theoretical analysis of DP and convergence. No human or animal subjects are involved; all datasets are de-identified and publicly released, so no additional ethical approval is required beyond the dataset use agreements.

### 5.1 Datasets and partitioning

Three complementary public datasets span modalities and tasks:

| Dataset | Modality | Task | Approx. size |
|---|---|---|---|
| MIMIC-IV [13] | EHR (tabular) | In-hospital mortality / sepsis | ~300k admissions |
| eICU [14] | EHR (tabular, multi-site) | Mortality | ~200k admissions, ~200 hospitals |
| CheXpert [15] + NIH ChestX-ray14 [16] | Chest X-ray | 14-pathology classification | ~375k images |

Crucially, eICU already provides a natural hospital partition, enabling *realistic* (not purely synthetic) non-IID evaluation. For MIMIC and the X-ray datasets, partitioning will follow the standard DIRICHLET scheme with α ∈ {0.1, 0.5, 1.0} to tune label-skew severity, with an additional feature-shift partition derived by clustering on patient demographics. Train/val/test splits are stratified per site with no patient overlap to prevent leakage.

### 5.2 Baselines

- **Local-only:** each site trains independently (lower-bound reference).
- **Centralized** (privacy ignored): upper-bound reference.
- **FedAvg** [1], **FedProx** [2], **Per-FedAvg** [18], **FedPAQ** [24] (compression-aware).
- **DP-FedAvg** [10] with uniform ε across clients.

### 5.3 Proposed method (APDP-FL)

APDP-FL combines three components, each motivated by a specific gap:

1. **Personalization backbone.** A shared-encoder / private-decoder split [17, 21] is adopted; the encoder is federated, the decoder is client-private. *Why this design:* in healthcare, sites often agree on low-level representations (anatomy on X-ray, lab-value semantics in EHR) but diverge on the mapping to local diagnoses — exactly what shared-private factorization models.

2. **Client-adaptive DP budget.** Instead of a single ε, each client *k* is assigned ε_k = f(h_k, n_k, c_k), where *h_k* is a heterogeneity score (Dirichlet α estimate or KL divergence to the global label distribution), *n_k* the local sample count, and *c_k* a contribution-weighted reliability estimate. An upper bound on the *aggregate* (ε_g, δ_g) is derived via the moments accountant [4] and composition theorems [20]. *Why adaptive:* uniform ε over-protects high-information clients (wasting utility) and under-protects rare-but-sensitive ones — a known but unresolved pathology in DP-FL [19].

3. **Privacy-aware compression.** Top-k sparsification and sign-SGD quantization are applied *after* DP noise injection, with a correction term that accounts for the variance inflation induced by the noise. *Why combine:* naively, compression destroys DP guarantees because the noise tail is truncated; the correction restores the guarantee while still reducing bandwidth.

### 5.4 Evaluation

- **Clinical utility:** AUROC, AUPRC (preferred over accuracy given class imbalance in mortality/sepsis), macro-F1, and calibration (ECE).
- **Privacy:** reported (ε, δ) per client and aggregate; verified against an empirical membership-inference attack [22] as a sanity check.
- **Communication:** total bytes uploaded per round and per round-converged; rounds-to-target-AUROC.
- **Statistical rigour:** 5 random seeds, mean ± std reported; paired bootstrap test (α = 0.05) for significance.
- **Ablations:** (a) remove personalization, (b) uniform vs adaptive ε, (c) compression on/off, (d) DP noise on/off — isolating each component's contribution.

### 5.5 Compute and reproducibility

Experiments run on Imperial College HPC with NVIDIA A100 GPUs <待填:确认课题组可用算力>. Estimated GPU-hours: ~6,000 over the project, well within typical allocation. Code (PyTorch + Flower/FedML), partition scripts, and trained checkpoints will be released under a permissive licence, with full configuration files for every reported result.

### 5.6 Feasibility and risks

The four feasibility axes are addressed as follows. **Resources:** all datasets are public; compute is via Imperial HPC. **Technical:** every component (FedAvg, DP-SGD, personalization) has mature open-source implementations, lowering implementation risk. **Human/schedule:** the 3-year plan (Section 6) leaves ~6 months of buffer. **Applicant:** undergraduate CS background covering ML, distributed systems, and probability; CV details available on request.

| Risk | Probability | Impact | Mitigation (Plan B) |
|---|---|---|---|
| Natural partition of eICU too coarse to expose heterogeneity | Medium | High | Use synthetic DIRICHLET partition with known α; fall back to MIMIC-derived partitions |
| Adaptive ε breaks theoretical guarantee | Medium | High | Restrict to a convex sub-class first; report empirical privacy via attack success rate even if bound is loose |
| Communication × DP interaction too noisy to be useful | Low-Med | Med | Decouple RQ3 — study compression alone first, then layer DP; report partial frontier |
| Compute allocation insufficient | Low | High | Use distilled/smaller models (e.g., ResNet-18, not ResNet-50); rent cloud GPU for final runs |
| Direction gets scooped | Low | Med | Quarterly arXiv monitoring; emphasize the healthcare-specific adaptive-ε angle as differentiation |

*【中文注释:Methodology 含数据集(含真实多站点 eICU)、≥4 baselines、提出方法三模块(每个解释"为什么")、临床+隐私+通信三类指标、4 种消融、算力与开源计划、五条风险 + 具体 Plan B。】*

---

## 6. Research Plan / Timeline (3-year PhD)

| Phase | Months | Tasks | Expected output |
|---|---|---|---|
| Y1 | M1–9 | Literature deep-dive; reproduce FedAvg/FedProx/Per-FedAvg on MIMIC; build partition toolkit | Survey chapter; reproducibility report; toolkit v0 |
| Y1–Y2 | M10–18 | RQ1: characterize non-IID effects; theoretical analysis of convergence under label-shift | Conference submission (e.g., ICLR/AAAI workshop or MLHC) |
| Y2 | M19–27 | RQ2: design + prove APDP-FL; ablations; adaptive-ε evaluation | Journal submission (e.g., IEEE TIFS / JAMIA) |
| Y2–Y3 | M28–33 | RQ3: compression × DP interaction; map operating frontier; release benchmark | Open-source release; third paper draft |
| Y3 | M34–36 | Thesis writing; viva preparation; buffer for revisions | Thesis; defence |

The plan deliberately staggers RQ1 → RQ2 → RQ3 because RQ2's method rests on RQ1's characterization, and RQ3 extends RQ2's method with compression.

*【中文注释:Timeline 含阶段-任务-产出三栏、3 年分排、留 3 个月缓冲、体现 RQ 之间依赖。】*

---

## 7. Expected Outcomes & Significance

**Theoretical contributions.**

1. A characterization of how *label-shift non-IID structure* in healthcare data translates into FL convergence slowdown — generalizing existing convex analyses [2] to clinically realistic distributions.
2. A formal privacy guarantee for *client-adaptive* DP budgets, with an explicit statement of the regimes in which adaptive allocation dominates uniform allocation.
3. A composition result for *compression-after-DP*, closing a known gap in private FL theory.

**Practical contributions.**

1. APDP-FL, an open-source, healthcare-oriented FL framework integrating personalization, adaptive DP, and privacy-aware compression.
2. A reproducible multi-site benchmark (MIMIC-IV / eICU / CheXpert) with tunable heterogeneity axes, filling a gap in medical FL evaluation.
3. Empirically mapped operating frontiers that can guide hospital IT departments deploying FL under explicit privacy and bandwidth constraints.

**Significance and alignment.** The work directly extends Prof. Li's programme on federated and privacy-preserving machine learning into the healthcare setting, supplying both the methodology and the application case the group has called for <待填:核实 Prof. Li 综述/立场论文中是否明确点出 healthcare 应用>. By keeping the contributions modular — each component is independently useful — the project hedges against over-fitting to any single clinical task.

---

## 8. References

[1] McMahan, H. B., Moore, E., Ramage, D., Hampson, S., & y Arcas, B. A. (2017). Communication-efficient learning of deep networks from decentralized data. *AISTATS 2017*.

[2] Li, T., Sahu, A. K., Zaheer, M., Sanjabi, M., Talwalkar, A., & Smith, V. (2020). Federated optimization in heterogeneous networks. *MLSys 2020*.

[3] Li, T., Sanjabi, M., & Smith, V. (2020). Fair resource allocation in federated learning. *ICLR 2020*.

[4] Abadi, M., Chu, A., Goodfellow, I., McMahan, H. B., Mironov, I., Talwar, K., & Zhang, L. (2016). Deep learning with differential privacy. *CCS 2016*.

[5] Bonawitz, K., Ivanov, V., Kreuter, B., Marcedone, A., McMahan, H. B., Patel, S., et al. (2017). Practical secure aggregation for privacy-preserving machine learning. *CCS 2017*.

[6] Sheller, M. J., Edwards, B., Reina, G. A., Martin, J., Pati, S., Kotrotsou, A., et al. (2020). Federated learning in medicine: facilitating multi-institutional collaborations without sharing patient data. *Scientific Reports*, 10, 12598.

[7] Rieke, N., Hancox, J., Li, W., Milletarì, F., Roth, H. R., Albarqouni, S., et al. (2020). The future of digital health with federated learning. *NPJ Digital Medicine*, 3, 119.

[8] Kaissis, G., Ziller, A., Passerat-Palmbach, J., Ryffel, T., Usynin, D., Sasaki, A., et al. (2021). End-to-end privacy preserving deep learning on multi-institutional medical imaging. *Nature Machine Intelligence*, 3, 473–484.

[9] Kairouz, P., McMahan, H. B., Avent, B., Bellet, A., Bennis, M., Bhagoji, A. N., et al. (2021). Advances and open problems in federated learning. *Foundations and Trends in Machine Learning*, 14(1–2), 1–210.

[10] Geyer, R. C., Klein, T., & Nabi, M. (2017). Differentially private federated learning: a client level perspective. *NeurIPS Workshop on Machine Learning on the Phone and other Consumer Devices*.

[11] Shokri, R., & Shmatikov, V. (2015). Privacy-preserving deep learning. *CCS 2015*.

[12] Johnson, A. E. W., Pollard, T. J., Shen, L., Lehman, L.-w. H., Feng, M., Ghassemi, M., et al. (2016). MIMIC-III, a freely accessible critical care database. *Scientific Data*, 3, 160035.

[13] Johnson, A. E. W., Bulgarelli, L., Shen, L., Vlasov, A., Pollard, T. J., Horng, S., et al. (2023). MIMIC-IV, a freely accessible electronic health record dataset. *Scientific Data*, 10, 1.

[14] Pollard, T. J., Johnson, A. E. W., Raffa, J. D., & Mark, R. G. (2018). The eICU Collaborative Research Database, a freely available multi-center database for critical care research. *Scientific Data*, 5, 180178.

[15] Irvin, J., Rajpurkar, P., Ko, M., Yu, Y., Ciurea-Ilcus, S., Chute, C., et al. (2019). CheXpert: a large chest radiograph dataset with uncertainty labels and expert comparison. *AAAI 2019*.

[16] Wang, X., Peng, Y., Lu, L., Lu, Z., Bagheri, M., & Summers, R. M. (2017). ChestX-ray8: hospital-scale chest X-ray database and benchmarks on weakly-supervised classification of common thorax diseases. *CVPR 2017*.

[17] Collins, L., Hassani, H., Mokhtari, A., & Shakkottai, S. (2021). Exploiting shared representations for personalized federated learning. *ICML 2021*.

[18] Fallah, A., Mokhtari, A., & Ozdaglar, A. (2020). Personalized federated learning with theoretical guarantees: a model-agnostic meta-learning approach. *NeurIPS 2020*.

[19] Wei, K., Li, J., Ding, M., Ma, C., Yang, H. H., Farokhi, F., et al. (2020). Federated learning with differential privacy: algorithms and performance analysis. *IEEE Transactions on Information Forensics and Security*, 15, 3454–3469.

[20] Dwork, C., & Roth, A. (2014). The algorithmic foundations of differential privacy. *Foundations and Trends in Theoretical Computer Science*, 9(3–4), 211–407.

[21] Arivazhagan, M. G., Aggarwal, V., Singh, A. K., & Choudhary, S. (2019). Federated learning with personalization layers. *arXiv:1912.00818*.

[22] Shokri, R., Stronati, M., Song, C., & Shmatikov, V. (2017). Membership inference attacks against machine learning models. *IEEE S&P 2017*.

[23] Truex, S., Liu, L., Chow, K.-H., Gursoy, M. E., & Wei, W. (2020). LDP-Fed: federated learning with local differential privacy. *EdgeSys 2020*. [需核实:年份与会议名]

[24] Reisizadeh, A., Mokhtari, A., Hassani, H., Jadbabaie, A., & Pedarsani, R. (2020). FedPAQ: a communication-efficient federated learning method with periodic averaging and quantization. *AISTATS 2020*.

[25] Xu, J., Glick, M., Munn, M., et al. (2021). Federated learning for healthcare informatics. In *Federated Learning: Privacy and Incentive*. Springer.

[26] Dayan, I., Roth, H. R., Zhong, A., Harouni, A., Gentili, A., Abidin, A. Z., et al. (2021). Federated learning for predicting clinical outcomes in patients with COVID-19. *Nature Medicine*, 27, 1735–1743.

---

## 中文 Notes(给申请人的说明 / 假设清单)

1. **导师信息假设**:用户只给出"Prof. Li,帝国理工,做联邦学习与隐私保护机器学习"。文中两处 `<待填:...>` 提示用户去 Prof. Li 主页核实真实姓名拼写、近 3 年代表论文,以及在综述里是否明确把 healthcare 列为应用方向。本 RP **未编造**任何导师具体论文或方向细节。
2. **学位与时长假设**:按英国 3 年 PhD(无硕博连读预科年)排 timeline;若为 4 年制,可把 Y1 文献/复现期延长到 18 个月。
3. **数据集假设**:全部为公开数据集,可离线获取,无 IRB 风险。若导师组有医院合作(如 NHS Trust),可在 RQ2 阶段引入真实多站点数据作为加分项。
4. **算力假设**:Imperial CS 系普遍有 HPC + A100,但具体配额需向导师确认,文中已用 `<待填>` 标注。
5. **方法论创新归类**:本 RP 的创新属于"已有方法的结合 + 新场景迁移"(personalization × adaptive-DP × compression 三者首次在 healthcare 异构下系统化),而非从零发明新算法;这一点与技能规范"创新 ≠ 从无到有"一致。
6. **引用真实性**:全部 26 条引用均为作者实际存在的论文(FL、DP、医疗 FL、数据集领域经典与高被引工作);仅 [23] LDP-Fed 的确切年份/会议名标注 `[需核实]`,建议用户在 DBLP 核对。**未编造任何 DOI 或虚构作者**。
7. **字数说明**:英文正文(Abstract + Intro + Lit Review + RQ + Methodology + Timeline + Outcomes)约 2050 词,落在 1800–2200 区间;References 与中文 Notes 不计入正文字数。
