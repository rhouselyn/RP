# RP 各部分详细写法

写每一节前读本文件对应小节。每节给出:目的、内容要点、句式模板、常见错误。

## 目录
- [Title 标题](#title)
- [Abstract 摘要](#abstract)
- [Cover Page 封面](#cover-page)
- [Introduction & Background 引言与背景](#introduction--background)
- [Literature Review 文献综述](#literature-review)
- [Research Questions & Objectives 研究问题与目标](#research-questions--objectives)
- [Methodology & Feasibility 方法与可行性](#methodology--feasibility)
- [Research Plan / Timeline 研究计划](#research-plan--timeline)
- [Expected Outcomes & Significance 预期成果与意义](#expected-outcomes--significance)
- [References 参考文献](#references)

---

## Title

**目的**:一句话说清"做什么 + 怎么做 + 在什么场景",同时让导师一眼看出和他的领域相关。

**要点**:
- 控制在 10-20 词,避免冒号堆叠超过两层。
- 含关键词(便于导师判断方向匹配)。
- 体现方法或场景的创新点,但不夸张。

**模板**:
- `[方法] for [问题] in [场景]: [角度]` → *Federated learning for privacy-preserving EHR diagnosis under non-IID data*
- `[现象/问题]: A [方法] approach toward [目标]` → *Perovskite solar cell degradation: A mechanistic ML approach toward 20-year stability*

**常见错误**:
- 太宽泛(*A Study on Machine Learning*)——导师看不出具体做什么。
- 太窄(*Improving YOLOv8 mAP on COCO by 0.3%*)——像工程报告不像研究。
- 用"Novel"/"Innovative"开头——让标题自我评价,显得心虚。

---

## Abstract

**目的**:200-300 词让招生官 60 秒内知道:做什么、为什么做、怎么做、做出来有什么意义。

**结构(四句式)**:
1. 背景一句:领域现状 + 为什么重要。
2. Gap 一句:现有工作缺什么。
3. 做什么一句:本 RP 提出的 RQ + 方法概述。
4. 意义一句:预期贡献 + 对谁有用。

**模板**:
> `[领域] has achieved [进展], yet [gap 仍存在]. This research investigates [RQ] by [方法概述:数据+技术]. Specifically, it [1-2 个具体目标]. The findings will [理论贡献] and inform [实践/政策影响].`

**常见错误**:
- 写成 introduction 的缩写(堆背景,没说做什么)。
- 用罕见词汇显高级——abstract 是给人快速判断的,清晰第一。
- 超过 300 词——招生官会跳过。

---

## Cover Page

**目的**:基础信息一页呈现。部分学校要求,部分不要。理工科常见包含:
- 姓名、邮箱、电话
- 申请学位(MPhil/PhD)、入学时间
- 目标导师姓名(若已套磁)
- 论文题目(Title)
- 日期

---

## Introduction & Background

**目的**:为 RQ 提供背景,论证"这个问题值得做"。回答 Why(为什么重要)、What(具体什么问题)、So what(做出来谁受益)。

**结构(漏斗式,从宽到窄)**:
1. 宽:领域大背景 + 为什么这个领域当下重要(给数据/事件支撑)。
2. 中:领域内的具体子方向 + 当前进展。
3. 窄:这个子方向里尚未解决的问题(gap 的雏形)。
4. 尖:本 RP 聚焦的 RQ + 为什么现在做(技术成熟/数据可得/政策窗口)。

**要点**:
- 每个"重要"都要有证据:行业报告数据、被引上千的里程碑论文、政策文件。
- 设定边界:"本研究聚焦 X,不涉及 Y"——避免 scope 漂移。
- 简要陈述假说或理论框架(理工科可选,有则写)。

**常见错误**:
- 背景太长,挤掉后面 methodology 篇幅。
- 罗列所有已知知识,像教科书。
- 没有收敛到 RQ,读完不知道作者要干嘛。

---

## Literature Review

**目的**:把研究放进学术脉络,证明 (a) 你读过该读的 (b) 你的工作有 gap 可填 (c) 你的方法有依据。

**组织方式(按主题,不按论文)**:
- 不要"A 做了 X,B 做了 Y,C 做了 Z"流水账。
- 按 theme 分组:每个主题下 cite 多篇,compare + contrast + critique。
- 每个 theme 末尾点一句"这个方向的 gap 是…"。

**5C 写法**:
1. **Cite**:选与 RQ 直接相关的,优先近 3-5 年 + 高被引 original。
2. **Compare**:同主题不同作者的方法/结论相似处。
3. **Contrast**:分歧、争议、辩论在哪。
4. **Critique**:哪些更可信,为什么(assert/demonstrate/argue 用对动词)。
5. **Connect**:回到自己的 RQ——"本工作将利用/扩展/挑战 X"。

**句式模板**:
- 介绍共识:*Prior work has consistently demonstrated that … (Smith 2023; Lee 2024).*
- 指出分歧:*While X is widely accepted, the effect of Y remains contested (Chen 2022; Patel 2023).*
- 点 gap:*However, few studies have examined … under [条件], leaving [问题] unresolved.*
- 衔接自己:*This RP addresses this gap by …*

**常见错误**:
- 引用过老(全 2015 前)——显得没跟前沿。
- 引用无关文献凑数——导师一看就知道没读。
- 只描述不评价——综述要 critique,不是读书笔记。
- 二手资料堆砌(全引综述不引 original)。
- 没有收敛到自己的 RQ。

---

## Research Questions & Objectives

**目的**:把 gap 转成 2-3 个可被回答的问题 + 对应目标。

**要点**:
- RQ 是疑问句,Objective 是动作句(To investigate / To develop / To evaluate)。
- 2-3 个为宜,太多显散;互相要有逻辑递进(如:机制→方法→验证)。
- 每个 RQ 可对应 1 个假设(理工科)。

**模板**:
> RQ1: How does [自变量] affect [因变量] under [条件]?
> RQ2: Can [方法] improve [指标] compared to [baseline]?
> RQ3: What are the trade-offs between [A] and [B] in [场景]?

**对应 Objective**:
> O1: To characterize the effect of X on Y.
> O2: To develop and validate a [方法] that …
> O3: To quantify trade-offs between A and B.

**常见错误**:
- RQ 太宽(*How to improve AI?*)——无法回答。
- RQ 是主题不是问题(*About federated learning*)。
- RQ 之间无逻辑关系,像三个独立项目。

---

## Methodology & Feasibility

**目的**:说服导师"你能做出来"。理工科 RP 的命门,占比最大(~30%)。

**结构**:
1. 总体设计:研究类型(实验/仿真/理论/混合)+ 整体路径。
2. 数据/样本:来源、规模、采集方式、伦理(人/动物需说明)。
3. 方法/技术:每个 RQ 对应 1-2 种方法,具体到工具/算法/设备。
4. 分析:怎么处理数据、用什么指标评估、统计方法。
5. 可行性:为什么你做得出(设备/数据/技能/导师支持)+ 风险与预案。

**写作铁律**:
- 每个方法都要解释"为什么选它而非替代方案"——不只是 to-do list。
- 具体到可复现程度:别人读完能照做。
- 预测风险 + 给 Plan B——导师最怕学生卡死。

详细模板见 `stem-methods.md`。

**常见错误**:
- 只列方法名不展开(就写"用深度学习")。
- 没有替代方案比较。
- 没有风险评估。
- 把方法写得比 RQ 还显眼(方法中心而非问题中心)。

---

## Research Plan / Timeline

**目的**:3-4 年分阶段安排,体现节奏感和可行性。

**格式**:表格或 Mermaid 甘特图。每阶段含:时间、任务、预期产出。

**模板(3 年 PhD)**:
| 阶段 | 时间 | 任务 | 预期产出 |
|---|---|---|---|
| Y1 | M1-9 | 文献深读 + 复现 baseline + 数据准备 | 综述章节、baseline 复现报告 |
| Y1-Y2 | M10-18 | RQ1 实验/仿真 | 第 1 篇会议论文 |
| Y2 | M19-27 | RQ2 方法开发与验证 | 第 2 篇期刊投稿 |
| Y3 | M28-36 | RQ3 整合 + 论文撰写 | 论文初稿、答辩准备 |

**常见错误**:
- 时间排太满没缓冲——科研一定有意外。
- 任务之间无依赖关系——应体现"基于 RQ1 结果做 RQ2"。

---

## Expected Outcomes & Significance

**目的**:预测成果 + 论证意义。回答"So what"。

**要点**:
- 理论贡献(新理解/新方法/新模型)+ 实践贡献(新工具/新政策/新应用)。
- 从"给学校/导师带来什么"角度收尾——体现匹配与价值。
- 用一张图总结研究全流程是加分项。

**常见错误**:
- 主观臆想("will revolutionize the field")——克制。
- 只说理论不说实践(或反之)——理工科最好兼顾。

---

## References

**要点**:
- 另起页,标题居中(References / Bibliography)。
- 用学科标准格式:理工科 IEEE/ACS/Elsevier numeric;交叉学科 APA;历史 Chicago。
- 只列实际引用的(References)或含关键背景(Bibliography,按学校要求)。
- 通常不计入总字数。

**常见错误**:
- 格式不统一(混用 APA 和 IEEE)。
- 引用编造——一定让用户核实,标注 `[需核实]`。
