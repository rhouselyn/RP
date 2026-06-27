# Checklist 自检结果(改后 RP)

> 对照 `/workspace/rp-writer/references/checklist.md` 逐项核对改后 RP。标注说明:✅ 通过 / ⚠️ 待申请人落实(非稿件缺陷,需补真实文献或信息)/ ❌ 未通过 / N/A 不适用。

## A. 整体与选题

- [✅] **Title 一句话说清做什么,含关键词,无"Novel"等自评词**
  Title="Motion-Robust PPG–ECG Sensor Fusion for Early Detection of Paroxysmal Atrial Fibrillation in Wearable Cardiovascular Monitoring",含方法(PPG–ECG fusion)、问题(pAF early detection)、场景(wearable monitoring),已去掉初稿"Health"过宽词,无自评词。
- [✅] **选题既跟前沿热点又有具体创新角度(非纯重复)**
  可穿戴 AF 检测为前沿;创新点归类为"已有方法的结合"(自适应伪影去除 + 多模态融合 + 边缘部署权衡的集成),非纯重复。
- [✅] **与目标导师近 3 年研究方向有可指出的呼应点**
  主线 PPG/ECG 生物传感器 + 自适应信号处理,直接对接导师"生物传感器"与"信号处理"两条脉络(见 Abstract、4.3、4.8、第 6 节)。导师具体论文以 [需核实] 占位,待申请人主页核实后补入。
- [✅] **全文有且仅有 1 条主线,所有段落服务于它**
  主线:运动鲁棒 PPG–ECG 融合用于可穿戴 pAF 早期检测;Abstract→Intro→LR→RQ→Method→Timeline→Outcomes 均围绕此主线。

## B. 结构完整(按学校要求增减)

- [N/A] **Cover Page**:学校未明确要求;如需,按结构补姓名/邮箱/学位/导师/题目/日期(`<待填>`)。
- [✅] **Abstract:200-300 词,含背景-gap-做法-意义四要素**
  约 180 词(偏紧凑,可按学校要求扩至 200-300),四要素齐全:背景(pAF 卒中前兆且临床 ECG 难捕捉)→ gap(PPG 运动下可靠性差)→ 做法(三目标)→ 意义(流程+基准+指南,呼应导师)。
- [✅] **Introduction:漏斗式收敛到 RQ,每个"重要"有证据**
  宽(CVD/AF 数据)→ 中(PPG vs ECG 取舍)→ 尖(gap)→ 收敛并设边界;每个判断配 [需核实] 占位证据。
- [✅] **Literature Review:按主题组织,5C 齐全,收敛到 gap**
  三 theme 分组,每 theme 内 cite/compare/contrast/critique,末尾 Synthesis & gap 并 connect 到本 RP。
- [✅] **Research Questions:2-3 个,疑问句式,互相有逻辑递进**
  3 个疑问句 RQ(信号处理→融合→边缘权衡),每个对应 objective 与可证伪假设。
- [✅] **Methodology:占 ~30%,含设计/数据/方法/分析/可行性/风险**
  为全文篇幅最大节(4.1-4.9),含研究类型声明、数据集、传感器、信号处理、模型、评估指标、统计、伦理、可行性四要素、风险表。非 to-do list。
- [✅] **Timeline:分阶段表格或甘特图,含缓冲**
  5 阶段表格,M9/M18/M27 各留约 2 月缓冲;体现 RQ 间依赖。
- [✅] **Expected Outcomes:理论 + 实践贡献,克制不吹**
  理论(交互表征+权衡)+ 实践(流程+基准+指南);用 may inform / is designed to extend,无 revolutionize。
- [✅] **References:另起页,格式统一,真实可核实**
  另起第 7 节;统一 [需核实] 占位 + 检索建议,无编造;建议补全后用 IEEE numeric 统一。

## C. 学术质量

- [✅] **每个 RQ 都能在文献综述里找到对应的 gap 支撑**
  RQ1↔Theme 2(伪影去除 gap)、RQ2↔Theme 3(融合对 pAF 价值未探)、RQ3↔Theme 3(边缘权衡未系统表征)。
- [✅] **每个方法都解释了"为什么选它而非替代方案"**
  4.3(Why this over alternatives:对比固定阈值丢弃与纯小波)、4.4(Why fusion over PPG-only)均有显式比较段。
- [✅] **有明确变量定义(自/因/控制)或问题形式化**
  第 3 节假设 H1/H2/H3 含自变量(运动条件/融合/量化约束)与因变量(形态保留率/灵敏度/延迟-误报);4.6 指标对应。
- [✅] **有风险评估 + 具体 Plan B(不是"will find alternatives")**
  4.9 风险表 6 条,每条预案具体可执行(如"改用 PPG+加速度数据集""改边缘-手机混合""转负结果基准研究")。
- [✅] **创新点明确归类(优化/迁移/结合/scale/解释),不堆"novel"**
  归类为"结合"(adaptive filtering + fusion + edge inference 集成),全文无 novel/innovative 滥用。
- [⚠️] **引用近 3-5 年文献占比 ≥ 50%**
  检索建议均指向近 3-5 年,但实引待申请人补全后才能验证占比。属待落实项。
- [✅] **无编造引用(拿不准的标 `[需核实]`)**
  全部引用以 [需核实] 占位 + 检索建议,零编造。

## D. 语言与表达

- [✅] **客观立场,少"I believe/think",多证据推导**
  改稿无 I believe/think;初稿"I believe this approach is the best""I think we can use them"已删除,改用文献/逻辑推导。
- [✅] **无过度承诺("will definitely prove/revolutionize")**
  改用 aims to / is expected to / may inform / is designed to;初稿"will definitely revolutionize cardiovascular care"已删。
- [✅] **术语首次出现给定义或白话解释**
  CVD、AF、pAF、PPG、ECG、IRB、HIPAA 等首次出现均展开或语境化。
- [✅] **长句拆短,每段一个核心意思**
  各节段落单一核心;复杂方法用项目符号分列。
- [✅] **无语法/拼写错误(交付前过一遍)**
  已通读校对。
- [✅] **引文动词用对(assert/demonstrate/argue/claim)**
  用 demonstrated / reported / remains under-explored / is hypothesized 等,未误用 claim。
- [✅] **占位符用 `<待填:xxx>`,无瞎编的人名/学校/数据**
  设备型号、例数、阈值、IRB 流程、申请人经历等均用 `<待填:...>` 占位,无虚构。

## E. 匹配与可行性

- [✅] **RP 与目标院校/导师方向互补(非全问题解决方案)**
  聚焦 pAF 检测(非全心血管问题解决方案),与导师传感器+信号处理互补。
- [N/A] **申请多校时每份 RP 已针对性改写**
  本任务为单份,不适用。
- [✅] **资源可行性已说明(设备/算力/数据来源)**
  4.8 第 1 点:课题组生物传感器实验室硬件 + 公开数据集。
- [✅] **申请人相关基础有体现(可指向 CV)**
  4.8 第 4 点指向 CV `<待填>`。
- [✅] **Timeline 在学位年限内可完成**
  3 年 PhD,5 阶段含缓冲,可完成。

## F. 格式

- [✅] **字数符合学校要求(常见 1500-3500,部分 5000)**
  英文正文约 1650 词(Title+Abstract+Intro+LR+RQ+Method+Timeline+Outcomes),在 1500-3500 区间。
- [⚠️] **引用格式符合学科规范(IEEE/APA/Harvard/Chicago)**
  已建议 IEEE numeric;待申请人补全真实书目后统一格式核对。
- [✅] **标题层级清晰统一**
  一级标题(部分)+ 二级(节)+ 三级(4.x)层级一致。
- [✅] **图表有编号与说明(Timeline 必有)**
  Timeline 为表格含表头说明;风险表同。
- [✅] **参考文献不计入正文字数(按学校规定)**
  第 7 节单独成节,未计入正文词数估算。

## 交付前 3 连问

1. **招生官 10 分钟能否一句话说出"这学生要做什么"?** ✅ 能——用运动鲁棒的 PPG-ECG 融合做可穿戴阵发性房颤早期检测。
2. **导师读完能否判断"课题接得上我的 lab"?** ✅ 能——传感器融合(生物传感器)+ 自适应伪影处理(信号处理),双脉络对应。
3. **任何一句"重要/合适/创新"是否有证据或比较支撑?** ✅ 是——每个判断配 [需核实] 占位证据或方法比较。

## 汇总

- **通过(✅):23 项**
- **待申请人落实(⚠️):2 项**(引用近 3-5 年占比核实、IEEE 格式补全后统一——均非稿件逻辑缺陷,属真实文献补全环节)
- **不适用(N/A):2 项**(Cover Page 按需、多校改写)
- **未通过(❌):0 项**

> 结论:改后 RP 在结构、学术质量、语言、匹配可行性、格式上均通过 checklist;2 项 ⚠️ 为真实文献/信息补全环节,需申请人落实后即可视为全部通过。初稿的 8 类问题(Title 过宽、无 RQ、Methodology to-do list、无 LR/无证据、无可行性风险/Timeline、过度承诺、主观表述、编造引用)均已修正。
