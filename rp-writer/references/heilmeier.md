# 自检工具:Heilmeier Catechism 与 Reviewer 视角

写完初稿后用这两个工具自检。它们能在交付前抓出 RP 最深的硬伤。

## 目录
- [Heilmeier Catechism(DARPA 经典 9 问)](#heilmeier-catechism)
- [Reviewer 视角清单(模拟审稿人挑刺)](#reviewer-视角清单)
- [CS/LLM 可复现性硬关](#csllm-可复现性硬关)

---

## Heilmeier Catechism

DARPA 前署长 George Heilmeier 提出的 9 问,被全球研究机构广泛用作 proposal 自检。**逐条作答,任何一条答不清就是 RP 的漏洞**。

1. **你想做什么?**(What are you trying to do?)——用一句话说清目标,不要套话。
2. **现状如何?已有什么做法?局限在哪?**(What is the state of the art? What are the limits?)——明确 gap,而不是泛说"很少有人研究"。
3. **你的方法和现有方法有什么不同?新在哪?**(What is new in your approach?)——concept delta,不能只是"我用了更新的模型"。
4. **如果成功,谁会关心?带来什么改变?**(Who cares? If you succeed, what difference will it make?)——回答 So what,关联到具体利益方。
5. **风险在哪?**(What are the risks?)——技术风险 + 现实风险(算力/数据/被抢发)。
6. **要花多少?**(How much will it cost?)——时间、算力、人力。
7. **如何衡量进展与成功?**(How will you measure progress/success?)——可量化的中间里程碑与最终指标。
8. **多久能看到结果?**(How long will it take?)——分阶段 timeline,含 early signal。
9. **有什么中间结果能证明可行?**(What are the mid-term and final "exams"?)

**用法**:把 RP 每段对照 9 问,看是否都能在文中找到答案。第 3、4、5 问最常翻车。

---

## Reviewer 视角清单

模拟一位严格但公正的审稿人/导师读你的 RP,逐条挑刺。任何一条答"否"都要改。

### A. 贡献与定位
- [ ] 读完 problem setting,我能不能猜出你全部技术内容?(能猜出 = 太浅,你只在做工程)→ 改:留出开放的研究问题,别把方案写死。
- [ ] 你的贡献相对 prior work 的 **concept delta** 清不清楚?(不能只是 block citation,要说明思想差异)
- [ ] 你是否在描述一个**研究方向**而非端到端**解决方案**?(PhD 是找方案的过程)
- [ ] 2% 提升 / 刷榜式表述有没有?有没有回答 "so what / 学到什么 / 揭示什么 root cause"?
- [ ] 是否在看 tomorrow's problem 而非 today's?

### B. 评估与方法
- [ ] evaluation 部分是否具体可信?(eval section 是论文/RP 最常见拒因)
- [ ] baseline 是不是近 1-2 年 SOTA?有没有只跟老方法比?
- [ ] 消融实验是否证明各组件贡献?
- [ ] 是否过度野心?(承诺的算力/数据远超实验室现实)
- [ ] 第一年有没有具体的 goals 与 deliverables?

### C. 可复现性(CS/LLM 必查,见下节)
- [ ] 用的是 open weights 还是闭源 API?
- [ ] 有没有精确 model ID + commit hash?
- [ ] seed / temperature / 量化是否说明?
- [ ] 代码与 checkpoint 开源计划?

### D. 写作与定位
- [ ] 是否写成 personal statement 了?(RP 不是 PS,要 stand-alone research proposal)
- [ ] 是否 paraphrase 导师主页?(导师一看就知道,显没想象力)
- [ ] 开篇几句话能不能抓住 reviewer?(first pass 只 2-3 分钟)
- [ ] 是否与目标导师近 3 年工作对齐?
- [ ] 字数是否合规?(常见 ≤3000 词)

### E. 伦理与风险
- [ ] 涉人/涉数据是否有 IRB 说明?
- [ ] LLM 涉人研究是否预算了 2-3 月 IRB amendment?
- [ ] 风险预案是否具体(不是"will find alternatives")?
- [ ] 是否含"方向被抢发""算力不足"两条 CS 高频风险?

---

## CS/LLM 可复现性硬关

审稿人(reviewer)对 LLM/AI 研究的可复现要求近年急剧提高,RP 阶段就要体现意识,否则导师会怀疑你能否发表。

### 硬性要求
1. **open weights 优先**:核心方法用 Llama / Mistral / Qwen 等开源模型,而非闭源 API(GPT-4/Claude)作主推理引擎。闭源 API 无法被他人复现,reviewer 会直接质疑。若必须用闭源,要说明并给 open 等价物的复现路径。
2. **精确 model ID**:不能只写"Llama 7B"——要给完整 HuggingFace ID(如 `meta-llama/Llama-2-7b-hf`)+ commit hash。Llama 2 与 Llama 3 同为 7B,因 RLHF 不同输出差异显著。
3. **seed 与超参**:记录每个 random seed、inference temperature、top-p。验证你的推理框架是否真尊重 seed——某些量化库(GPTQ/AWQ)即便设 seed 也有非确定性推理路径,reviewer 会试图复现来抓。
4. **provenance 日志**:用 Weights & Biases 或 MLflow 自动记录 model version / seed / 超参,reviewer 越来越期望机器可读的 provenance,而非仅 README。
5. **GitHub + Zenodo DOI**:代码与配置版本化,prompt 也要版本化(prompt 改动会破坏复现)。目标是 bit-identical reproducibility。
6. **IRB 时间预算**:LLM 涉人类研究(人类评估、用户日志)需 IRB amendment,近年审批 2-3 月起。RP 的 timeline 要把这个时间算进去,别写"立即开始人类评估"。
7. **数据许可与泄漏**:公开数据集注明许可;自采数据注明 IRB + 脱敏。划分按 subject/site 而非随机,明确 no data leakage。
8. **量化库非确定性**:若用 GPTQ/AWQ 量化,明说复现可能有数值波动,报告多 seed 均值±std。

### RP 中的体现
在 Methodology 的 "Compute & Reproducibility" 小节写明以上要点;在 Timeline 留出 IRB 窗口;在 Risk 表里加"闭源模型行为漂移/API 端点更新导致 baseline 偏移"。

reviewer 视角一句话:**"Can we reproduce this with your exact code and data?"** 答"否"就丢 credibility。
