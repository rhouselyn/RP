# 计算机方向方法论模板

CS 方向 RP 的命门。导师在此判断"这课题做不做得出来、你做不做得起"。本文件给到各子方向的方法论模板与常见坑。

## 目录
- [通用 CS 研究类型](#通用-cs-研究类型)
- [LLM / 大语言模型](#llm--大语言模型)
- [CV / 计算机视觉](#cv--计算机视觉)
- [多模态](#多模态)
- [世界模型 / 具身智能](#世界模型--具身智能)
- [强化学习 RL](#强化学习-rl)
- [MLSys / 系统](#mlsys--系统)
- [CS 通用必备要素](#cs-通用必备要素)
- [CS 风险预案专项](#cs-风险预案专项)
- [CS 伦理与合规](#cs-伦理与合规)

---

## 通用 CS 研究类型

CS RP 一般属以下之一(或混合):
- **算法/方法型**:提出新模型/训练方法/推理算法(最常见)
- **系统型**:新系统/框架/编译器/调度器(MLSys)
- **实证/分析型**:大规模实验分析某现象(如 scaling law、benchmark)
- **理论型**:收敛性/复杂度/泛化界证明

先在 methodology 开头声明类型,让导师知道评估标准。

---

## LLM / 大语言模型

适用:预训练、后训练、对齐、RAG、agent、高效推理、长上下文等。

```markdown
### 3.1 Overall Design
本工作为 [算法/实证] 型研究,聚焦 [RQ]。整体路径:基线复现 → 提出方法 → 消融 → 规模化验证。

### 3.2 Models & Checkpoints
- 基座模型:[Llama-3-8B / Qwen2.5-7B / Mistral-7B 等,注明版本与许可]
- 参数规模范围:[1B/7B/13B,用于 scaling 分析]
- 为何选它:[开源许可、社区基线、与导师组算力匹配]

### 3.3 Data
- 预训练/微调数据:[来源、规模、清洗方式、许可]
- 评测集:[MMLU/MT-Bench/AlpacaEval/自建集,说明版本]
- 划分:train/val/test,明确无泄漏(同一文档不跨集)

### 3.4 Methods
- 基线(≥3):[SFT / DPO / PPO / 特定 SOTA 方法,注明近 1-2 年]
- 提出方法:[架构图/损失函数/训练流程,每个模块给"why"]
- 超参搜索:[空间 + 策略(grid/Bayesian) + 评估指标]

### 3.5 Evaluation
- 自动指标:[准确率/rouge/pass@k/ehuman eval 等,匹配任务]
- 人工评估:[若自动不可靠,给评估协议、评估者数、一致性]
- 统计:[多 seed 均值±std,k-fold,显著性检验]
- 消融:[逐模块移除,证明各组件贡献]

### 3.6 Compute & Reproducibility
- 硬件:[A100/H100 × N,训练时长]
- 代码与 checkpoint 开源计划
```

**常见坑**:只喊"用 LLM"不展开;baseline 用 5 年前 BERT;无消融;算力不写;数据许可不说;用 accuracy 评开放生成。

---

## CV / 计算机视觉

适用:检测、分割、生成、3D、视频、医学影像等。

```markdown
### 3.1 Datasets
- [COCO/ImageNet/ADE20K/LAION 等,注明版本与许可]
- 划分:train/val/test 按官方;自建集说明采集与标注协议

### 3.2 Methods
- 基线(≥3):[YOLOv8/Mask2Former/DINOv2 等近 SOTA]
- 提出方法:[backbone/neck/head 改动,架构图]
- 损失函数与训练策略

### 3.3 Evaluation
- 指标:[mAP/IoU/FID/CLIPScore,匹配任务]
- 统计:多 seed,均值±std
- 消融:逐组件

### 3.4 Compute
- [GPU × N,训练时长,推理延迟]
```

**常见坑**:只跟老 baseline 比;用单一指标;无消融;医学影像不提 IRB。

---

## 多模态

适用:VL/VA/音频-文本/视频-文本/跨模态检索与生成。

```markdown
### 3.1 Modalities & Data
- 模态组合:[image-text/video-text/audio-text]
- 数据:[LAION/CC3M/WebVid 等,许可与规模]
- 对齐方式:[对比学习/交叉注意力/共享编码器]

### 3.2 Methods
- 基线:[CLIP/BLIP-2/LLaVA/Video-LLaVA 等]
- 提出方法:[模态融合架构,为何如此融合]
- 预训练+微调两阶段说明

### 3.3 Evaluation
- 检索:[R@1/5/10]
- 生成:[BLEU/ROUGE/CIDEr + 人工]
- 消融:[单模态 vs 多模态、融合方式对比]

### 3.4 Compute
- [多模态训练通常算力密集,GPU × N,时长]
```

**常见坑**:不说明模态对齐机制;无单模态 baseline 对比;算力低估。

---

## 世界模型 / 具身智能

适用:world model、robot learning、sim-to-real、embodied agent。

```markdown
### 3.1 Simulation & Real
- 仿真器:[Isaac Gym/Habitat/Genesis/MuJoCo,版本]
- 仿真场景:[规模、资产来源]
- 真机(若有):[机器人平台、传感器、安全协议]

### 3.2 Methods
- 基线:[DreamerV3/RT-2/Octo 等近 SOTA]
- 提出方法:[世界模型架构/策略学习/表征学习]
- sim-to-real 策略(若有):[domain randomization/系统识别]

### 3.3 Evaluation
- 仿真指标:[return/success rate/sample efficiency]
- 真机指标:[成功率、泛化到新物体/场景]
- 消融:[世界模型组件、表征维度]

### 3.4 Compute
- [仿真并行数 + 训练 GPU,真机实验时长]
```

**常见坑**:只仿真不提 sim-to-real gap;无 sample efficiency 对比;真机不提安全与伦理。

---

## 强化学习 RL

适用:RLHF、offline RL、multi-agent、MARL、RL 理论。

```markdown
### 3.1 Environments
- [Atari/MuJoCo/Procgen/StarCraft Multi-Agent/自建]
- 版本与奖励设计说明

### 3.2 Methods
- 基线:[PPO/SAC/TD3/DreamerV3 等]
- 提出方法:[算法/网络/奖励设计]
- 超参与稳定性:[多 seed,RL 高方差必须报]

### 3.3 Evaluation
- 指标:[return/success rate/sample efficiency/wall-clock]
- 统计:[≥5 seed,均值±std,有时报 IQM]
- 消融

### 3.4 Compute
- [并行 worker 数、GPU、总步数]
```

**常见坑**:只跑 1 seed;不报 sample efficiency;奖励设计不说明。

---

## MLSys / 系统

适用:训练系统、推理加速、编译器、调度、分布式。

```markdown
### 3.1 System Design
- 目标:[吞吐/延迟/内存/能耗]
- 架构:[系统图,关键模块]

### 3.2 Implementation
- 基于什么:[PyTorch/JAX/Triton/自研]
- 关键优化:[算子融合/量化/并行策略]

### 3.3 Evaluation
- 基线:[vLLM/DeepSpeed/Megatron 等近 SOTA]
- 工作负载:[模型 + 批大小 + 序列长度矩阵]
- 指标:[throughput/latency p50/p99/内存峰值]
- 硬件:[A100/H100/L4,数量]

### 3.4 Reproducibility
- 代码开源 + 配置文件 + 硬件清单
```

**常见坑**:不跟近 SOTA 系统比;硬件不写;只报均值不报尾延迟。

---

## CS 通用必备要素

无论哪个子方向,methodology 必含:

1. **研究类型声明**(算法/系统/实证/理论)
2. **数据/模型/checkpoint**:来源、规模、许可、版本
3. **baseline ≥ 2-3 个**:近 1-2 年 SOTA,说明为何选
4. **提出方法**:每个模块给 why,不只 what
5. **评估指标**:匹配任务(分类/生成/检索/控制/系统性能)
6. **统计严谨**:多 seed 均值±std、显著性检验、k-fold
7. **消融实验**:逐组件,证明各部分贡献
8. **算力与可复现**:GPU 型号数量、训练时长、开源计划
9. **可行性四要素**(资源/技术/人力/申请人)
10. **风险与预案**(含"方向被抢发""算力不足"两条)

---

## CS 风险预案专项

CS 方向除通用风险外,务必含:

| 风险 | 概率 | 影响 | 预案 |
|---|---|---|---|
| 方向被抢发(arXiv 同题) | 中 | 高 | 月度 arXiv 跟踪;强调差异化角度(任务/规模/分析);预留备选 RQ |
| 算力不足/排队 | 中 | 高 | 申请学校 HPC;租用云 GPU;用更小模型验证再 scale |
| 开源 baseline 复现失败 | 中 | 中 | 联系原作者;用官方 checkpoint;退而用次优 baseline 并说明 |
| 评测指标不可靠(如 LLM 自动评) | 中 | 中 | 引入人工评估;多指标交叉;报告一致性 |
| 数据集许可变更/下架 | 低 | 高 | 本地备份;备选数据集;自采小规模验证集 |

---

## CS 伦理与合规

- **人类数据**(用户日志、众包标注、人类评估):需 IRB + 知情同意 + 脱敏。
- **LLM 输出有害性**:对齐/安全研究需说明 red-teaming 协议与缓解措施。
- **偏见与公平**:涉及人口学属性时说明评估与缓解。
- **隐私**:含真实用户数据需 GDPR/HIPAA 合规。
- **双用途**:生成模型(deepfake、代码)需说明负责任发布(水印、分级开源)。
- **纯公开数据+仿真**:可写"This study uses only public datasets and simulation; no human subjects, thus no IRB required."——但别省略,导师会问。
- **算力碳排**:大规模训练可附碳排估算与补偿(部分欧洲院校要求)。
