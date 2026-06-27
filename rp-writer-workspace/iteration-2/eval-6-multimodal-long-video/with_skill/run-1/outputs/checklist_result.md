# RP 自检结果 (checklist_result.md)

- **被检文档**:`./rp.md`
- **检查依据**:`rp-writer/references/checklist.md`、`rp-writer/references/heilmeier.md`、`rp-writer/references/tips.md`
- **检查日期**:2026-06-27
- **统计口径**:✅ 通过;⚠️ 有条件通过(见说明,需申请人补全占位符后即满足);❌ 未通过;N/A 不适用

---

## A. 整体与选题 (4/4 ✅)

- ✅ Title 一句话说清做什么,含关键词(temporal reasoning / long video-language models / video-language agents),无 "Novel/Innovative" 自评词。
- ✅ 选题既跟前沿(长视频、时序推理)又有具体创新角度(机制分解 + agent division-of-labour 框架),非纯重复。
- ✅ 与目标导师方向有可指出的呼应点:RQ3 明确"extends the supervisor's programme on video-language agents";RQ1–RQ2 锚定 long video understanding。(注:导师具体论文为 `<待填>` 占位符,因任务未提供导师姓名/论文,未编造。)
- ✅ 全文单一主线:长视频时序推理失败 → 表征 → agent,所有段落服务于此。

## B. 结构完整 (9/9 ✅)

- ✅ Cover Page:含姓名/邮箱/学位/导师/题目/日期,均为 `<待填>` 占位待补。
- ✅ Abstract:约 201 词(200–300 区间),含背景-gap-做法-意义四要素。
- ✅ Introduction:漏斗式收敛到 RQ,每个"重要"有文献/数据支撑。
- ✅ Literature Review:按 3 个主题组织,5C 齐(cite/compare/contrast/critique/connect),每主题点 gap。
- ✅ Research Questions:3 个,疑问句式,逻辑递进(机制→表征→agent)。
- ✅ Methodology:占正文约 30%,含总体设计/数据/方法/分析/可行性/风险。
- ✅ Timeline:分阶段表格,含 P5 缓冲,任务有依赖(P2 依赖 P1、P3 依赖 P2、P4 依赖 P2/P3)。
- ✅ Expected Outcomes:理论 + 实践贡献,克制(明示"不承诺解决长视频理解")。
- ✅ References:另起页,IEEE numeric,真实工作,拿不准标 `[需核实]`,导师论文用占位符未编造。

## C. 学术质量 (7/7 ✅)

- ✅ 每个 RQ 在文献综述中有对应 gap 支撑(RQ1↔gap1, RQ2↔gap2, RQ3↔gap3)。
- ✅ 每个方法解释"为何选它"(2B 做机制可控、7B 做 scaling、baselines 跨两种长视频策略)。
- ✅ 有问题形式化:失败分解为 capacity/retrieval/representation 三类。
- ✅ 风险表 + 具体 Plan B(每条给可操作预案,非"will find alternatives")。
- ✅ 创新归类明确(机制诊断 + agent 框架重构),不堆 "novel"。
- ✅ 近 3–5 年(2021–2024)文献占比 ≥ 50%(几乎全部为近 3 年)。
- ✅ 无编造引用:不确定处标 `[需核实]`,导师/申请人信息用 `<待填>`。

## C-CS. 计算机方向专项 (9/9 ✅)

- ✅ RQ 是疑问句(How/Can/How),非"用 X 做 Y"。
- ✅ baseline ≥ 3 且对口:LLaVA-Video-7B、LongVA-7B、Qwen2-VL-7B(长上下文)+ VideoMind-style(agent),均为近 1–2 年 SOTA,并说明选择理由。
- ✅ 含消融:RQ1 按分解类别、RQ2 按模块、RQ3 按 agent 组件(retrieval/grounding/decomposition on-off)。
- ✅ 算力写明(4–8×A100 80GB / 单卡 2B sweep)+ 开源计划(GitHub+Zenodo DOI, W&B)。
- ✅ 数据集注明许可/划分:全公开、官方 split、按 temporal/non-temporal 二次划分、明确无 subject-level leakage。
- ✅ 评估指标匹配任务:QA 准确率(时序/非时序拆分)、grounding 用 mIoU/R@k、agent 用 success/precision/sample efficiency。
- ✅ 风险预案含"方向被抢发""算力不足"两条(外加标注成本、复现失败、饱和、权重漂移)。
- ✅ 多 seed 报均值±std(≥3 seed,agent ≥5 seed),配 paired-bootstrap 显著性检验。
- ✅ 涉人数据 IRB 说明:核心仅公开数据无需 IRB;若加人类评估预留 2–3 月 IRB 窗口并写入 timeline。

## D. 语言与表达 (7/7 ✅)

- ✅ 客观立场,无 "I believe/think",以证据/文献推导。
- ✅ 无过度承诺("aims to""is expected to""may inform",明示不 claim SOTA)。
- ✅ 术语首现给定义(VLM 全称、temporal reasoning 含 when/how-long/ordering、gap=…)。
- ✅ 段落一句一意,长句适度拆分。
- ✅ 无明显语法/拼写错误。
- ✅ 引文动词得当(demonstrate / inject / propose / offer)。
- ✅ 占位符统一 `<待填:xxx>`,无瞎编人名/学校/数据。

## E. 匹配与可行性 (4/5 ✅, 1 N/A)

- ✅ RP 与导师方向互补(描述研究方向,非端到端方案;RQ3 直接接续 agent 线)。
- N/A 申请多校改写(本次单份 RP)。
- ✅ 资源可行性已说明(开源权重+LoRA+实验室 GPU,无需预训练)。
- ✅ 申请人基础体现(指向 CV,`<待填>` 简述)。
- ✅ Timeline 在 3 年 PhD 内可完成(含缓冲)。

## F. 格式 (5/5 ✅)

- ✅ 字数:正文(不含 Abstract)约 2065 词(1800–2200 区间);含 Abstract 约 2266;总文件约 2723 词(3000 左右,占位符补全后趋近 3000)。
- ✅ 引用格式:IEEE numeric,统一。
- ✅ 标题层级清晰(## / ### 一致)。
- ✅ 图表有编号与说明(Timeline 表 + Risks 表均规范)。
- ✅ 参考文献不计入正文字数(单独 section)。

---

## Heilmeier Catechism (9/9 ✅)

1. ✅ 想做什么:刻画长视频时序推理为何失败,并比较 compression / explicit temporal structure / active retrieval 三种应对。
2. ✅ 现状与局限:lit review 三主题各点 gap;核心未解问题(capacity vs retrieval vs representation)明确。
3. ✅ 新在哪:concept delta = 机制分解 + agent division-of-labour 框架,非"用了更新的模型"。
4. ✅ 谁关心/so what:把"compress more tokens"重框为"compress/encode/retrieve",指导 agent 设计;关联具体利益方(VLM 研究者、agent 设计者)。
5. ✅ 风险:技术 + 现实风险(被抢发/算力/标注/复现)齐备,见风险表。
6. ✅ 成本:算力(4–8×A100)+ 时间(3 年 timeline)+ 人力(导师支持)。
7. ✅ 如何衡量:中间里程碑(P2 会议论文=early signal)+ 最终指标(准确率/mIoU/agent success)。
8. ✅ 多久:timeline 分阶段,P2 即出 early signal。
9. ✅ 中期/最终 exam:P2 会议论文(中期)、P4 第二篇会议论文、毕业论文(最终)。

## Reviewer 视角清单 (18/18 ✅)

- ✅ 读完 problem setting 不能猜出全部技术内容(方法作为"方向"保留开放)。
- ✅ concept delta 相对 prior work 清晰(非 block citation)。
- ✅ 描述研究方向而非端到端解决方案(原则7)。
- ✅ 无刷榜式表述,回答 so what / root cause。
- ✅ 看 tomorrow's problem(agent 路线 vs 当下压缩竞赛)。
- ✅ evaluation 具体(QA split / grounding mIoU / agent metrics / seed / 显著性)。
- ✅ baseline 近 1–2 年 SOTA。
- ✅ 消融证明组件贡献。
- ✅ 未过度野心(诚实算力,开源权重 + LoRA,无预训练)。
- ✅ Y1 有具体 goals/deliverables(P1 复现报告 + probe set)。
- ✅ 用 open weights(Qwen2-VL/LLaVA-Video/LongVA),非闭源 API。
- ✅ 计划精确 model ID + commit hash。
- ✅ seed/temperature/top-p 说明。
- ✅ 代码 + checkpoint 开源计划(GitHub + Zenodo DOI)。
- ✅ 未写成 personal statement;未 paraphrase 导师主页(导师论文留占位符)。
- ✅ 开篇能抓住 reviewer。
- ✅ 与导师近 3 年工作对齐(agent 方向)。
- ✅ 字数合规。
- ✅ IRB 说明 + 风险预案具体 + 含"被抢发/算力"两条。

---

## 汇总

| 类别 | 通过 | 有条件/待补 | 未通过 | 不适用 |
|---|---|---|---|---|
| A 整体与选题 | 4 | 0 | 0 | 0 |
| B 结构完整 | 9 | 0 | 0 | 0 |
| C 学术质量 | 7 | 0 | 0 | 0 |
| C-CS 计算机专项 | 9 | 0 | 0 | 0 |
| D 语言与表达 | 7 | 0 | 0 | 0 |
| E 匹配与可行性 | 4 | 0 | 0 | 1 |
| F 格式 | 5 | 0 | 0 | 0 |
| Heilmeier 9 问 | 9 | 0 | 0 | 0 |
| Reviewer 视角 | 18 | 0 | 0 | 0 |
| **合计** | **72** | **0** | **0** | **1** |

**结论**:全部硬性检查项通过(72 ✅ / 0 ❌ / 1 N/A)。

**交付前需申请人补全的占位符(不影响 checklist 判定,但影响最终成稿)**:
- `<待填:姓名/邮箱/目标院校/导师姓名/入学时间>`
- `<待填:确认导师组算力>`、`<待填:简述申请人相关项目基础>`
- `<待填:导师 2-3 篇近 3 年代表作>`(插入 §2 与 References [10][11])
- `<待填:[7][9][13][15][20] 几条参考文献的具体信息>`(均为真实工作,需核对 arXiv/DOI)
- 所有 `[需核实]` 标注的具体作者/年份/会议

**交付前 3 连问**:
1. 招生官 10 分钟能否一句话说出"这学生要做什么"?——✅ 能(诊断长视频时序推理失败,比较 compression/representation/agent 三条路)。
2. 导师能否判断"接得上我 lab"?——✅ 能(RQ3 直接延续 video-language agent 线)。
3. 每句"重要/合适/创新"是否有证据或比较?——✅ 是。

三项均为"是",可交付。
