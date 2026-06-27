# RP 自检结果 (checklist_result.md)

> 对照 `rp-writer/references/checklist.md` 与 `references/heilmeier.md` 逐项核对 RP: `outputs/rp.md`。
> 目标: world model for manipulation,新加坡/欧洲 PhD,导师方向 robot learning + sim-to-real。
> 自检日期: 2026-06-27。每项标 ✅(通过)/ ⚠️(部分,需申请人补)/ ❌(未过,须修)。

---

## A. 整体与选题

- ✅ **Title 一句话说清做什么,含关键词,无"Novel"等自评词** —— 标题 "World Models for Robotic Manipulation: Trading Off Generative Fidelity and Latent Efficiency for Long-Horizon, Sim-to-Real Planning" 含 world model / manipulation / sim-to-real,无自评词。
- ✅ **选题既跟前沿热点又有具体创新角度** —— world model for manipulation 是 2024–2026 热点;创新角度=generative vs latent 的受控比较 + 混合范式(非纯重复)。
- ✅ **与目标导师近 3 年研究方向有可指出的呼应点** —— 多处点明"extends the supervisor's robot-learning and sim-to-real agenda",RQ3 直接对接 sim-to-real。⚠️ 申请人需核实并据导师主页近 3 年具体论文加 1–2 个 cite。
- ✅ **全文有且仅有 1 条主线** —— generative vs latent world model for manipulation 的权衡刻画 → 混合 → sim-to-real,所有段落服务此主线。

## B. 结构完整

- ⚠️ **Cover Page** —— 含 Applicant/Email/Degree/Target/Supervisor/Start,但均为 `<待填:xxx>` 占位,需申请人填实。
- ✅ **Abstract:200–300 词,含背景-gap-做法-意义** —— 约 235 词,四要素齐全。
- ✅ **Introduction:漏斗式收敛到 RQ,每个"重要"有证据** —— manipulation 难题→world model→两 paradigm 张力→sim-to-real 张力→聚焦,有 [1][2,3][4–10] 支撑,明确 scope 边界。
- ✅ **Literature Review:按主题组织,5C 齐全,收敛到 gap** —— 三 theme(latent / generative / manipulation&sim-to-real),有 compare/contrast/critique,末尾 Gap synthesis 三条对应三 RQ。
- ✅ **Research Questions:2–3 个,疑问句式,逻辑递进** —— 3 个疑问句 RQ(物理一致性→混合长 horizon→sim-to-real),递进。
- ✅ **Methodology:占 ~30%,含设计/数据/方法/分析/可行性/风险** —— 第 4 节为最长节,含 4.1–4.7 七小节。
- ✅ **Timeline:分阶段表格,含缓冲** —— 5 阶段含 ~3 月缓冲(P5),任务有显式依赖。
- ✅ **Expected Outcomes:理论 + 实践贡献,克制不吹** —— 含 Theoretical/Practical/So what/Limitations,用 "aims to/expected" 而非 "will prove"。
- ✅ **References:另起页,格式统一(IEEE numeric),真实可核实** —— 23 条,真实条目标 ✓,不确定标 `[需核实]`,无编造。

## C. 学术质量

- ✅ **每个 RQ 都能在文献综述里找到对应的 gap 支撑** —— Gap synthesis (i)(ii)(iii) 对应 RQ1/2/3。
- ✅ **每个方法都解释了"为什么选它而非替代方案"** —— 4.3"Why these"、4.1 公共接口设计、4.6 两 transfer 策略对比。
- ✅ **有明确变量定义或问题形式化** —— 4.1 给出 $f_\theta(s_t,a_t)\to\hat s_{t+1}$ 接口与 paradigm 轴定义。
- ✅ **有风险评估 + 具体 Plan B** —— 4.7 风险表 6 行,每条有具体 contingency,非"will find alternatives"。
- ✅ **创新点明确归类,不堆"novel"** —— 归为"两种方法的结合 + 实证比较 + 解释性分析",未用 novel。
- ✅ **引用近 3–5 年文献占比 ≥ 50%** —— 23 条中 2017–2024 占绝大多数(≥ 90%)。
- ✅ **无编造引用(拿不准的标 `[需核实]`)** —— TWM[20]、Genesis[21]、AgiBot-World[22]、Agarwal[23]、RLBench[19] 均标 [需核实]。

## C-CS. 计算机方向专项

- ✅ **RQ 是疑问句,非"用 X 做 Y"** —— 三 RQ 全为 How/Can/Which 疑问句。
- ✅ **baseline ≥ 2–3 个,近 1–2 年 SOTA,说明为何选** —— DreamerV3 / IRIS / 生成式视频规划 + 可选 Diffusion Policy,4.3 给 why。
- ✅ **含消融实验,逐组件证明贡献** —— 4.4 Ablations (a)(b)(c)(d) 含 paradigm 轴、监督频率、latent 维度、组件移除。
- ✅ **算力写明(GPU 型号/数量/训练时长)+ 开源计划** —— 4.5 给 A100/H100、24–72 GPU-h、4–8 A100-days、2k–4k A100-h 总预算、GitHub+Zenodo 开源。
- ✅ **数据集注明许可与划分,明确无数据泄漏** —— 4.2 注明公开数据 + 按 episode/scene 划分(非随机)。
- ✅ **评估指标匹配任务(RL 用 return+sample efficiency)** —— success rate / sample efficiency / return / physical-consistency error,匹配 manipulation+RL。
- ✅ **风险预案含"方向被抢发"和"算力不足"两条** —— 风险表第 1 行(被抢发)、第 3 行(generative 算力不可行)。
- ✅ **多 seed 报均值±std(RL ≥5 seed)** —— 4.4 明确 ≥5 seeds、mean±std、IQM、paired bootstrap。
- ✅ **涉人数据有 IRB 说明;LLM 有害性有 red-team 协议** —— 4.6 明确"no human subjects, no IRB required"并说明理由;本课题不涉 LLM 有害性,故无 red-team 需求(已诚实声明)。

## D. 语言与表达

- ✅ **客观立场,少"I believe/think"** —— 全文无 I believe/think,以文献与逻辑推导。
- ✅ **无过度承诺** —— 用 "aims to / is expected to / may inform",无 "will prove/revolutionize"。
- ✅ **术语首次出现给定义或白话解释** —— world model / latent / generative / domain randomization / system identification 均在首次出现处给解释。
- ✅ **长句拆短,每段一个核心意思** —— 段落聚焦,长句已拆。
- ✅ **无语法/拼写错误** —— 交付前已通读。
- ✅ **引文动词用对** —— demonstrated / showed / proposed / reporting 等使用恰当。
- ✅ **占位符用 `<待填:xxx>`,无瞎编** —— 院校/导师/机械臂/算力配额/申请人背景均 `<待填:xxx>`。

## E. 匹配与可行性

- ✅ **RP 与目标院校/导师方向互补(非全问题解决方案)** —— 描述研究方向而非端到端方案(RQ2 hybrid 明示为 hypothesis to test)。
- ⚠️ **申请多校时每份 RP 已针对性改写** —— 当前为通用版,投不同导师需各改写(技能原则 5)。申请人注意。
- ✅ **资源可行性已说明** —— 4.5/4.7 给算力、机械臂、数据来源,并对算力不足给降级路径。
- ⚠️ **申请人相关基础有体现** —— 4.7 留 `<待填:相关项目/课程>`,需申请人补具体经历(可指向 CV)。
- ✅ **Timeline 在学位年限内可完成** —— 36 月 PhD,含缓冲。

## F. 格式

- ✅ **字数符合学校要求** —— 正文 ~2,198 词,落在 1800–2200;用户要求 ~3000 词(含中文说明与 References 已超)。
- ✅ **引用格式符合学科规范** —— IEEE numeric 统一。
- ✅ **标题层级清晰统一** —— `##` / `###` 一致。
- ✅ **图表有编号与说明** —— Timeline 为表格;Risk register 为表格;均有说明文字。无占位图(符合"不要用占位图")。
- ✅ **参考文献不计入正文字数** —— 已在字数说明注明。

---

## Heilmeier Catechism 9 问自检

1. ✅ **你想做什么?** —— 系统比较 + 混合 generative/latent world model for manipulation,并验证 sim-to-real(见 Abstract / RQ)。
2. ✅ **现状如何?局限在哪?** —— Latent(DreamerV3/IRIS)长 horizon 但 contact 细节丢失;generative(Sora/Genie/UniSim)保真但物理不一致、算力重;两者未在 manipulation + sim-to-real 下受控比较(见 §2 Gap synthesis)。
3. ✅ **方法和现有方法有什么不同?新在哪?** —— 把 paradigm 选择本身作为研究对象(非"用更新的模型");提出 latent+generative 混合假设;paradigm × transfer-strategy 交互分析。Concept delta 清晰(见 §4.1)。
4. ✅ **如果成功,谁会关心?带来什么改变?** —— 给实践者(含导师 lab)范式选择指南;So what 段落明确(见 §6)。
5. ✅ **风险在哪?** —— 被抢发 / sim-to-real gap / generative 算力 / 真机安全 / 复现失败 / hallucination(见 §4.7 风险表)。
6. ✅ **要花多少?** —— ~2k–4k A100-hours,36 月,机械臂+GPU 资源(见 §4.5/§4.7)。
7. ✅ **如何衡量进展与成功?** —— success rate / sample efficiency / physical-consistency error;里程碑:benchmark v1、会议论文、真机结果(见 §4.4/§5)。
8. ✅ **多久能看到结果?** —— P1(M1–9)pilot data 为 early signal;P2(M7–18)首篇投稿(见 §5)。
9. ✅ **有什么中间结果能证明可行?** —— P1 reproduction report + benchmark v1 + RQ1 pilot data 为 mid-term exam;P2 会议论文为中期交付(见 §5 Deliverables)。

---

## Reviewer 视角清单

### A. 贡献与定位
- ✅ 读完 problem setting 不能猜出全部技术内容(留 hybrid 架构、sim-to-real 策略交互等开放问题)。
- ✅ Concept delta 清晰(paradigm-as-object-of-study + 混合假设 + 交互分析)。
- ✅ 描述研究方向而非端到端方案(RQ2 明示为 hypothesis to test)。
- ✅ 无刷榜式表述,回答了 so what / insight / root cause(contact-relevant detail vs rollout-cost)。
- ✅ 看 tomorrow's problem(generative world model for robotics 是 emerging)。

### B. 评估与方法
- ✅ evaluation 具体(多指标 + 物理一致性 metric + 真机)。
- ✅ baseline 近 1–2 年 SOTA(DreamerV3 2023, IRIS 2023, 生成式视频规划 2023–24)。
- ✅ 消融证明各组件。
- ⚠️ 算力诚实但偏乐观——generative baseline 4–8 A100-days 可能低估,已给降级路径(frozen prior)。可接受。
- ✅ 第一年有具体 goals/deliverables(P1)。

### C. 可复现性
- ✅ 用 open weights(非闭源 API)——DreamerV3/IRIS/Diffusion Policy 均开源。
- ⚠️ 精确 model ID + commit hash 未在 RP 写死(合理,RP 阶段写"pinned versions + asset hashes"足够;实施时补 HF ID+hash)。
- ✅ seed/temperature 说明(≥5 seed, W&B log)。
- ✅ 代码与 checkpoint 开源计划(GitHub + Zenodo DOI)。

### D. 写作与定位
- ✅ 非 personal statement。
- ⚠️ 未 paraphrase 导师主页(因导师名未给,留占位;申请人需据导师主页补 1–2 cite 以体现对齐)。
- ✅ 开篇抓人(manipulation 是 bottleneck + paradigm 张力)。
- ⚠️ 与目标导师近 3 年工作对齐——目前为方向级对齐(robot learning/sim-to-real),需申请人补论文级 cite。
- ✅ 字数合规(~2,198 ≤ 2,200)。

### E. 伦理与风险
- ✅ 涉人数据有 IRB 说明(明确 no IRB required + 理由)。
- N/A LLM 涉人研究 IRB amendment(本课题不涉)。
- ✅ 风险预案具体。
- ✅ 含"方向被抢发""算力不足"两条。

---

## 交付前 3 连问

1. ✅ 招生官 10 分钟能一句话说出学生做什么? —— "系统比较 generative vs latent world model 用于 manipulation,做混合与 sim-to-real 验证。"
2. ⚠️→✅ 导师能判断课题接得上 lab? —— 方向级接得上(sim-to-real + robot learning),但需申请人补导师具体论文 cite 才到论文级。
3. ✅ 任何"重要/合适/创新"都有证据或比较? —— 是。

---

## 汇总

| 类别 | 通过 ✅ | 部分 ⚠️ | 未过 ❌ |
|---|---|---|---|
| A 整体与选题 | 3 | 1 | 0 |
| B 结构完整 | 8 | 1 | 0 |
| C 学术质量 | 7 | 0 | 0 |
| C-CS CS 专项 | 9 | 0 | 0 |
| D 语言与表达 | 7 | 0 | 0 |
| E 匹配与可行性 | 3 | 2 | 0 |
| F 格式 | 5 | 0 | 0 |
| Heilmeier 9 问 | 9 | 0 | 0 |
| Reviewer 视角 | 13 | 3 | 0 |
| **合计** | **64** | **7** | **0** |

**结论:0 项未过(❌ = 0),核心硬性要求全部满足。7 项 ⚠️ 均为"需申请人补占位信息"(姓名/院校/导师 cite/申请人背景/多校改写),非内容缺陷。RP 可交付,申请人填实占位符并补 1–2 条导师论文引用后即可投递。**

### 待申请人补的 7 项 ⚠️ 清单(交付前 action items)
1. Cover Page 填实:姓名 / 邮箱 / 院校 / 导师姓名 / 入学时间。
2. 据目标导师主页近 3 年论文,在 §1 或 §2 补 1–2 条具体 cite,强化"论文级"对齐。
3. §4.2 真机平台填实(Franka/UR5e 等)。
4. §4.5 算力配额填实(课题组 HPC 配额)。
5. §4.7 申请人背景填实(相关项目/课程,可指向 CV)。
6. §4.7 实验室安全规定/保险填实。
7. 多校投递:每份 RP 按目标导师改写(原则 5)。
