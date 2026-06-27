# Heterogeneity-Aware, Privacy-Preserving Federated Learning for Clinical Risk Prediction Across Distributed Healthcare Networks

**Applicant:** [Applicant Name] &nbsp;|&nbsp; **Proposed Supervisor:** Prof. Li &nbsp;|&nbsp; **Institution:** Imperial College London &nbsp;|&nbsp; **Programme:** PhD in Computing / Machine Learning

---

## 1. Introduction and Motivation

Modern healthcare generates vast quantities of electronic health record (EHR) data, medical imaging, and wearable signals—on the order of hundreds of petabytes annually worldwide—that could fuel accurate clinical prediction models for early sepsis detection, in-hospital mortality, and readmission risk. Models trained on sufficiently large and diverse cohorts could transform care, especially for smaller hospitals that lack in-house data-science capacity. Yet despite this abundance, the institutions that hold these data—hospitals, NHS trusts, and national registries—are largely unable to pool them. Legal frameworks such as the GDPR and HIPAA, institutional governance, ethical review, and commercial sensitivities make raw-data centralisation slow, costly, and frequently impossible. As a result, most clinical models are trained on single-institution cohorts, limiting generalisability and leaving specialist or minority-serving hospitals with underperforming tools.

Federated learning (FL) offers a principled alternative: a global model is trained collaboratively by exchanging only model updates, never raw patient data. The paradigm has matured rapidly since the introduction of FedAvg, but its translation to healthcare remains difficult for four intertwined reasons. First, clinical data are severely non-IID across sites—different case mixes, demographic profiles, coding practices, and recording densities—so naive FL often converges slowly or to a model that under-serves minority populations. Second, model updates are not inherently private: gradient-inversion and membership-inference attacks can expose sensitive patient information. Third, a single global model inevitably compromises local performance, raising fairness concerns across institutions and demographic subgroups. Fourth, repeated communication rounds impose costs that may be prohibitive for resource-limited trusts.

This proposal targets these obstacles jointly rather than in isolation. I aim to develop a federated framework that is provably private, robust to the heterogeneity typical of EHR data, and locally personalised so that collaboration never comes at the cost of vulnerable subpopulations. The work is designed to advance both the methodology of privacy-preserving federated learning and its credible deployment in clinical settings.

## 2. Background and Related Work

**Federated learning foundations.** FedAvg introduced the iterate-local-then-average paradigm that underpins most FL systems, in which clients perform several local epochs before the server averages their models. Subsequent work addressed statistical heterogeneity through proximal regularisation (FedProx), variance reduction (SCAFFOLD), and adaptive optimisation. Communication efficiency has been improved via gradient sparsification and quantisation. These methods, however, were developed largely on image and text benchmarks whose heterogeneity is milder than that of multi-site EHR data.

**FL in healthcare.** Early demonstrations showed FL feasible for brain-tumour segmentation, EHR-based prediction, and COVID-19 screening across hospital consortia. These studies typically report that FL matches or exceeds local-only training, but they also reveal sharp performance drops under skewed data and provide limited or informal privacy guarantees. Few evaluate fairness across demographic subgroups, and fewer still report a formal privacy budget.

**Privacy attacks and defences.** Membership-inference attacks can identify whether a specific patient contributed to training; gradient-inversion attacks such as Deep Leakage from Gradients can reconstruct input data from shared gradients, with the risk amplified under fewer clients or untrained models. Defences include differential privacy (DP-SGD), secure aggregation, and hybrid constructions. In healthcare, these defences are seldom deployed with rigorous accounting, and their interaction with highly non-IID data is poorly understood—precisely the regime where they are most needed.

**Personalisation and fairness.** Clustered FL, multi-task learning, and meta-learning approaches (e.g., FedMAML) produce per-client or per-cluster models, while fairness-aware FL seeks equitable performance across protected groups. However, personalisation, privacy, and heterogeneity are usually studied separately, and combining them can introduce tensions: stronger personalisation may leak more local information, while stronger privacy noise can erode the very gains personalisation provides.

**The gap.** Existing work rarely integrates (i) distribution-aware aggregation for highly skewed EHR, (ii) formal differential privacy with secure aggregation, (iii) personalisation that explicitly protects underrepresented groups, and (iv) empirical validation on real multi-site clinical data with explicit fairness and privacy accounting. Closing this gap is the goal of the proposed research.

## 3. Research Problem and Questions

The central problem is: *how can we train federated models on heterogeneous, distributed EHR data that are simultaneously clinically accurate, formally private, and locally fair?* I decompose this into four research questions.

**RQ1 — Heterogeneity.** How should aggregation be designed to remain stable and accurate when client data distributions differ sharply in label prevalence, feature support, and temporal structure, as is typical in EHR cohorts? In particular, can compact, privacy-safe distribution summaries guide clustering and weighting better than uniform averaging?

**RQ2 — Privacy.** What differential-privacy guarantees (ε, δ) are achievable for realistic federated clinical training, and how can secure aggregation and gradient compression reduce the noise—and hence the utility loss—required to defend against gradient-inversion and membership-inference attacks under a stated fraction of colluding clients?

**RQ3 — Personalisation and fairness.** How can the global model be adapted to local populations, particularly underrepresented demographic subgroups, without re-introducing privacy risks, and how should the resulting disparity be measured and bounded?

**RQ4 — Empirical validation.** On real multi-site clinical tasks, how does the proposed framework compare to centralised, local-only, and standard FL baselines along the joint axes of predictive performance, privacy budget, subgroup fairness, and communication cost?

## 4. Proposed Approach

I propose **FedHPF** (Heterogeneity-aware, Private, Federated learning with personalisation), a framework that co-designs three components rather than bolting them together. The guiding hypothesis is that heterogeneity, privacy, and personalisation interact strongly, and that jointly optimising them yields a better privacy–utility–fairness frontier than treating each in isolation.

**Component A — Distribution-aware aggregation.** Each client securely shares compact, privacy-safe statistics—label histograms, feature-presence masks, and coarse moment estimates—rather than raw data. The server uses these to (i) cluster clients with similar distributions and (ii) compute adaptive aggregation weights that down-weight dominant but redundant clients and up-weight underrepresented yet clinically important groups. A FedProx-style proximal term limits client drift during local training, and the divergence threshold that triggers clustered personalisation is learned online from the distribution sketches rather than fixed manually, so the framework can adapt as cohorts evolve. The aggregation is also designed to be robust to client dropout and partial participation, both common in real hospital networks where a site may miss a round due to maintenance or governance review. Where distributions diverge beyond the learned threshold, the framework switches to clustered personalisation rather than forcing a single global model. This directly targets RQ1 and is designed to be robust to the long-tailed label distributions common in critical-care data, where positive events such as sepsis or in-hospital death may be rare and unevenly spread across sites.

**Component B — Privacy layer.** Training is protected by client-level differential privacy: per-update gradient clipping, calibrated Gaussian noise, and secure aggregation so the server observes only the noised sum of updates and never an individual model. I will develop an adaptive noise-allocation scheme that distributes the privacy budget according to per-update sensitivity, and I will investigate gradient compression as a means to reduce the update magnitude—and therefore the noise—required for a fixed guarantee. A formal accountant (Rényi DP / Gaussian DP) will track the cumulative budget across rounds, and I will derive bounds on residual leakage under a stated fraction of colluding clients. This addresses RQ2 and provides the rigorous accounting that healthcare deployments demand.

**Component C — Personalisation and fairness.** To address RQ3, the framework learns a meta-initialisation via a FedMAML-style inner-loop/outer-loop structure, enabling each site to reach a strong local model with a few fine-tuning steps and limited additional privacy cost. A fairness-aware regulariser penalises disparity in performance across protected demographic subgroups, measured by equalised odds and calibration error. Crucially, the personalisation step is constrained to remain within the global DP budget, and I will analyse the privacy cost of personalisation explicitly to avoid the common pitfall of "free" local adaptation that silently leaks information.

**Validation.** I will treat MIMIC-III/IV, eICU, and PhysioNet Challenge data as distinct "sites", partitioning by hospital or ICU unit to emulate realistic non-IID conditions. Tasks will include in-hospital mortality, early sepsis prediction, and 30-day readmission. Baselines: centralised training (upper bound), local-only training (lower bound), FedAvg, FedProx, and DP-only FL. Metrics: AUROC, AUPRC, calibration error, worst-subgroup performance, privacy budget consumed, and communication cost. To move beyond point estimates, I will report confidence intervals via bootstrapping and use paired significance tests across sites, and all experiments, splits, and privacy-accounting parameters will be released to support reproducible comparison. All datasets used are publicly available under credentialed access agreements, so no new ethical approval is required for the core experiments; any subsequent NHS case study will follow the appropriate governance and information-governance review. This design speaks directly to RQ4 and supports reproducible comparison.

## 5. Expected Contributions

1. **A unified framework** that co-designs heterogeneity-aware aggregation, formal differential privacy, and personalisation for clinical FL—moving beyond single-challenge treatments that dominate the current literature.
2. **Provable privacy guarantees** tailored to federated clinical training, with an explicit analysis of the privacy–utility–communication trade-off under colluding clients and an accounting of the additional cost introduced by personalisation.
3. **A fairness analysis** that quantifies how federated training affects minority populations across both sites and demographics, together with mechanisms that reduce disparity without sacrificing privacy.
4. **A reproducible benchmark** and open-source toolkit on real multi-site clinical data, supplying baselines and standardised privacy and fairness reporting for the community.
5. **Deployment-oriented guidelines** that translate the framework's trade-offs into practical recommendations for healthcare consortia considering federated adoption.

## 6. Research Plan and Timeline

**Year 1 — Foundations.** Systematic literature review; construct the multi-site EHR data pipeline; reproduce FedAvg, FedProx, and DP-SGD baselines; empirically characterise cross-site heterogeneity. *Output: workshop paper and a validated benchmark.*

**Year 2 — Heterogeneity and privacy (RQ1–RQ2).** Develop distribution-aware aggregation and the DP/secure-aggregation layer; derive formal privacy analysis; run ablations on utility versus privacy budget. *Output: a top-tier conference submission.*

**Year 3 — Personalisation and fairness (RQ3).** Implement meta-learning personalisation and the fairness regulariser; conduct large-scale multi-task experiments; perform subgroup disparity analysis. *Output: a journal paper and, if possible, an NHS case study.*

**Year 4 — Translation and thesis.** Real-world case study with a clinical partner; refine the toolkit and guidelines; complete the thesis and final publications. *Output: thesis and open-source release.*

*Risk mitigation.* If formal client-level DP proves too costly for the most sensitive tasks, I will fall back to weaker-but-practical guarantees—for example, local differential privacy applied only to the shared distribution statistics in Component A—and document the resulting trade-off explicitly. If a clinical partner is delayed, the multi-site public-data benchmark (MIMIC/eICU/PhysioNet) ensures the core methodological claims remain testable.

## 7. Fit with Imperial and Prof. Li's Group

The proposed work aligns directly with Prof. Li's research in federated learning and privacy-preserving machine learning, and it is designed to extend that programme by integrating heterogeneity, privacy, and personalisation within a clinically grounded setting. Imperial offers an exceptional environment for this agenda: a strong UKRI-funded healthcare-AI ecosystem, established partnerships with NHS trusts, and deep strengths in biomedical engineering and computational privacy. Combining Prof. Li's methodological expertise with Imperial's clinical collaborators and computational infrastructure would allow me to progress from algorithms to genuine healthcare impact—precisely the trajectory this proposal describes. I am eager to contribute to the group's ongoing work, and I believe my undergraduate training in computer science, together with coursework in machine learning and distributed systems, provides a strong foundation for the research above. I would welcome the opportunity to discuss how this plan could be refined and aligned with the group's current directions.

## References (selected)

1. McMahan, B. et al. *Communication-Efficient Learning of Deep Networks from Decentralized Data* (FedAvg). AISTATS 2017.
2. Li, T. et al. *Federated Optimization in Heterogeneous Networks* (FedProx). MLSys 2020.
3. Karimireddy, S. et al. *SCAFFOLD: Stochastic Controlled Averaging for Federated Learning*. ICML 2020.
4. Shokri, R. & Shmatikov, V. *Privacy-Preserving Deep Learning*. CCS 2015.
5. Zhu, L., Liu, Z. & Han, S. *Deep Leakage from Gradients*. NeurIPS 2019.
6. Abadi, M. et al. *Deep Learning with Differential Privacy* (DP-SGD). CCS 2016.
7. Bonawitz, K. et al. *Practical Secure Aggregation for Privacy-Preserving Machine Learning*. CCS 2017.
8. Sheller, M. et al. *Multi-institutional Deep Learning Modeling Without Sharing Patient Data: A Feasibility Study on Brain Tumor Segmentation*. Sci. Data 2018.
9. Rieke, N. et al. *The Future of Digital Health with Federated Learning*. NPJ Digital Medicine 2020.
10. Fallah, A., Mokhtari, A. & Ozdaglar, A. *Personalized Federated Learning with Theoretic Guarantees* (FedMAML). NeurIPS 2020.
