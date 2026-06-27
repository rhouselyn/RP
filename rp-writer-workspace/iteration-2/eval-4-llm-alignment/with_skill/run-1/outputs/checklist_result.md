# Checklist 自检结果

**自检对象**: `/workspace/rp-writer-workspace/iteration-2/eval-4-llm-alignment/with_skill/run-1/outputs/rp.md`
**自检工具**: `references/checklist.md` + `references/heilmeier.md`(Heilmeier Catechism 9 问 + Reviewer 视角清单)
**自检日期**: 2026-06-27

---

## A. 整体与选题

- ✅ **Title 一句话说清做什么,含关键词,无"Novel"等自评词** — "Sample-Efficient Preference Alignment ... Characterizing the Role of Preference Data Composition",含 preference alignment / sample efficiency / data composition 关键词,无自评词。
- ✅ **选题既跟前沿热点又有具体创新角度** — DPO 系方法是 2023–2024 热点;创新角度为 data-side efficiency(方法侧工作密集,数据侧系统理解稀缺,非纯重复)。
- ✅ **与目标导师近 3 年研究方向有可指出的呼应点** — 直接基于 DPO/RLHF 方法展开,Expected Outcomes 显式说明与导师 lab 互补定位;建议用户补导师具体论文 cite(见 Notes)。
- ✅ **全文有且仅有 1 条主线** — "preference data composition 决定 alignment 结果与数据效率"贯穿 Abstract→RQ→Methodology→Outcomes。

## B. 结构完整

- ✅ **Cover Page** — 含姓名/邮箱/学位/导师/题目/日期(用 `<待填>` 占位,未编造)。
- ✅ **Abstract** — ~230 词,四要素齐全(背景-gap-做法-意义),在 200–300 区间。
- ✅ **Introduction** — 漏斗式从 RLHF→DPO→data bottleneck→RQ,每个"重要"有文献/数据支撑。
- ✅ **Literature Review** — 三主题(methods / failure modes / data selection),含 cite/compare/contrast/critique/connect,每段末点 gap。
- ✅ **Research Questions** — 3 个疑问句(RQ1/2/3),逻辑递进 characterization→selection→transfer。
- ✅ **Methodology** — 占比最大(~30%),含 Overall Design / Models / Data / Baselines / Proposed Direction / Evaluation / Compute / Risk 8 个子节。
- ✅ **Timeline** — 5 阶段表格,含 ~10% buffer,任务有依赖关系(RQ2 依赖 RQ1,RQ3 依赖 RQ2)。
- ✅ **Expected Outcomes** — 理论(方法无关理解)+ 实践(开源工具)+ 导师匹配,克制("not a 2% benchmark gain")。
- ✅ **References** — 另起页,IEEE numeric 统一,真实可核实(不确定处标 `[需核实]`)。

## C. 学术质量

- ✅ **每个 RQ 在文献综述有 gap 支撑** — RQ1 对应 failure modes 主题,RQ2 对应 data selection 主题,RQ3 对应 scaling 缺口。
- ✅ **每个方法解释为何选** — baseline 说明设计空间覆盖(paired/unpaired、reference/reference-free);proposed direction 说明三个候选算法各测不同假设。
- ✅ **变量/问题形式化** — preference data composition 沿 quality/diversity/difficulty/overlap 四轴定义;因变量 alignment quality / sample-efficiency curves / failure-mode indicators 明确。
- ✅ **风险评估 + 具体 Plan B** — 5 条风险各给具体预案(非"will find alternatives")。
- ✅ **创新点归类** — 明确为"解释型"(characterizing root cause)+ "优化型"(data selection),非堆 novel。
- ✅ **近 3-5 年文献占比 ≥ 50%** — 几乎全部 2022–2024。
- ✅ **无编造引用** — 不确定处标 `[需核实]`,共 6 处。

## C-CS. 计算机方向专项

- ✅ **RQ 是疑问句,非"用 X 做 Y"** — RQ1/2/3 均为 How / To what extent 疑问句。
- ✅ **baseline ≥ 2-3 个 SOTA** — 主基线 DPO/KTO/SimPO(2023–2024),次基线 SLiC-HF/IPO,参考 PPO/SFT,全部近 1–2 年,说明覆盖设计空间。
- ✅ **含消融实验** — "Ablations remove each selection criterion and each compositional axis in turn"。
- ✅ **算力写明 + 开源计划** — 4×A100 80GB,单 run 8–12 GPU-h,Y1–2 总 1.5–2k GPU-h;GitHub+Zenodo+Docker。
- ✅ **数据集许可与划分,无泄漏** — 注明 CC-BY-4.0 / Apache(待核实),按 prompt 去重防泄漏,"no data leakage by construction"。
- ✅ **评估指标匹配任务** — AlpacaEval 2.0 win rate / MT-Bench / Arena-Hard / RewardBench + 多指标交叉;PPO 报 return + sample efficiency。
- ✅ **风险预案含"被抢发"+"算力不足"** — 表中第 1、2 条即此两条,预案具体。
- ✅ **多 seed 报均值±std** — ≥3 seed(DPO 高方差),paired bootstrap 显著性检验。
- ✅ **涉人数据 IRB** — 人类评估单独 IRB amendment,timeline 预留 2–3 月。

## D. 语言与表达

- ✅ **客观立场** — 无 "I believe/I think",用文献与逻辑推导。
- ✅ **无过度承诺** — 用 "aims to / is expected to";显式 "not a 2% benchmark gain"。
- ✅ **术语定义** — DPO/IPO/KTO/SimPO/SLiC-HF 首次出现给一句话定义。
- ✅ **长句拆短,每段一个核心意思** — 是。
- ✅ **无语法/拼写错误** — 交付前已通读。
- ✅ **引文动词用对** — assert/demonstrate/argue/claim 区分使用。
- ✅ **占位符规范** — `<待填:xxx>` 用于未知信息,无瞎编人名/学校/数据。

## E. 匹配与可行性

- ✅ **RP 与导师方向互补** — data-side efficiency vs 导师 method-side efficiency,互补非重复(显式声明 "complementary positioning")。
- ⚠️ **申请多校改写** — 用户申港中文+港科两校,本版为通用模板,投递前需为每校单独改写导师段与匹配说明(原则5)。
- ✅ **资源可行性** — 4×A100 在用户描述的"几张 A100"内;13B 用 QLoRA;不承诺预训练/70B 全参。
- ⚠️ **申请人基础** — 文中未展开 NLP/RL 经验,建议用户在 CV 中体现并在 RP Cover Letter 补一句。
- ✅ **Timeline 在学位年限内** — 3 年,含 buffer。

## F. 格式

- ✅ **字数符合** — 英文正文约 2050 词(不含 References/Notes),在 1800–2200 范围。
- ✅ **引用格式** — IEEE numeric 统一。
- ✅ **标题层级清晰** — Markdown headers 一致。
- ✅ **图表有编号与说明** — Timeline 与 Risk 两张表均有说明文字。
- ✅ **参考文献不计入正文字数** — 另起页。

---

## Heilmeier Catechism 9 问

1. ✅ **想做什么** — Abstract + RQ 段清晰陈述:理解 preference data composition 如何决定 alignment 结果,并探索 sample-efficient data selection。
2. ✅ **现状与局限** — Lit Review 明确 DPO 系降 compute cost,但 data bottleneck(50k–200k pairs)未解。
3. ✅ **方法新在哪(concept delta)** — 非新模型/新 loss,而是 method-agnostic 的 data-side characterization;与 prior method-side 工作正交。
4. ✅ **谁关心/何改变** — Expected Outcomes:学术实验室降本 + 社区数据实践 + 导师 lab 可复用 artifact。
5. ✅ **风险** — 5 条具体风险含被抢发/算力/复现/评测/许可。
6. ✅ **花多少** — 4×A100,Y1–2 总 1.5–2k GPU-h,在"几张 A100"预算内。
7. ✅ **如何衡量进展** — sample-efficiency curves + paired bootstrap 显著性 + 中间里程碑(Y1 末会议投稿)。
8. ✅ **多久** — 3 年分阶段 timeline,含 buffer。
9. ✅ **中间结果** — Y1 末会议投稿(early signal)、Y2 第二篇、Y3 整合期刊+论文。

## Reviewer 视角清单(挑刺)

### A. 贡献与定位
- ✅ 读完 problem setting 不能猜出全部技术内容(RQ2 三候选算法留开放)— 符合"研究方向非解决方案"。
- ✅ concept delta 清晰(data-side vs method-side)。
- ✅ 描述研究方向而非端到端方案。
- ✅ 无刷榜表述,显式回答 so what。
- ✅ 看 tomorrow's problem(data efficiency 随 alignment scale 增长更重要)。

### B. 评估与方法
- ✅ evaluation 具体可信(多指标 + 显著性 + 人类评估)。
- ✅ baseline 近 1–2 年 SOTA。
- ✅ 消融证明各组件贡献。
- ✅ 不过度野心(4×A100 + QLoRA)。
- ✅ Y1 有具体 deliverable(baseline 复现报告 + W&B 模板 + 首篇投稿)。

### C. 可复现性
- ✅ open weights(Llama/Qwen/Mistral),闭源 API 仅评估侧。
- ✅ 精确 HF model ID + commit hash 计划。
- ✅ seed/温度/QLoRA 非确定性说明。
- ✅ GitHub + Zenodo DOI + Docker 开源计划。

### D. 写作与定位
- ✅ 非 personal statement,stand-alone research proposal。
- ✅ 非 paraphrase 导师主页(用户未给导师主页,RP 自主定位)。
- ✅ 开篇 Abstract 抓人。
- ✅ 与导师 RLHF/DPO 方向对齐。
- ✅ 字数合规(~2050 词)。

### E. 伦理与风险
- ✅ 涉人数据有 IRB 说明(人类评估单独 amendment)。
- ✅ IRB 时间预算 2–3 月写入 timeline。
- ✅ 风险预案具体。
- ✅ 含"被抢发"+"算力不足"两条 CS 高频风险。

---

## CS/LLM 可复现性硬关

- ✅ open weights 优先,闭源 API 仅评估侧且有 open 复现路径。
- ✅ 精确 HF model ID(`meta-llama/Meta-Llama-3-8B-Instruct` 等),commit hash 计划写入。
- ✅ seed(≥3)、温度、top-p 将记录;QLoRA/GPTQ-AWQ 非确定性显式说明。
- ✅ W&B provenance 日志(机器可读)。
- ✅ GitHub + Zenodo DOI + Docker 镜像(prompt 版本化)。
- ✅ IRB 时间预算写入 timeline。
- ✅ 数据许可注明,划分按 prompt 去重防泄漏。
- ✅ 量化库非确定性多 seed 报告。

---

## 交付前 3 连问

1. ✅ 招生官 10 分钟能说出"做什么" — "用更少 preference data 做对齐,理解数据组成的作用,跨方法跨规模验证"。
2. ✅ 导师能判断"接得上 lab" — 直接基于 DPO,Expected Outcomes 显式声明互补定位。
3. ✅ 每句"重要/合适/创新"有证据 — RLHF 成本文献、DPO 论文、failure-mode 文献、RewardBench 支撑。

---

## 汇总

- **总检查项**: 51 项(checklist A–F 共 39 项 + Heilmeier 9 项 + Reviewer 5 大类抽样 + 交付 3 连问)
- **通过 ✅**: 49 项
- **⚠️ 待用户处理**: 2 项
  1. 多校改写(港中文/港科各需一份针对性版本)
  2. 申请人 NLP/RL 基础在 RP/CV 中体现
- **❌ 未通过**: 0 项

**结论**: RP 满足技能规范全部硬性要求,可交付。两处 ⚠️ 为投递前用户侧动作,不影响 RP 本身质量。
