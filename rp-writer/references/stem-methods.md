# 理工科方法论模板

Methodology 是理工科 RP 的命门。导师在此判断"这学生能不能做出成果"。本文件给到分学科的模板与可行性论证方法。

## 目录
- [研究类型分类](#研究类型分类)
- [实验型研究模板](#实验型研究模板)
- [仿真/计算型研究模板](#仿真计算型研究模板)
- [数据驱动/ML 研究模板](#数据驱动ml-研究模板)
- [理论型研究模板](#理论型研究模板)
- [变量与假设](#变量与假设)
- [伦理与合规](#伦理与合规)
- [可行性论证四要素](#可行性论证四要素)
- [风险与预案](#风险与预案)

---

## 研究类型分类

理工科 RP 一般属以下之一(或混合):
- **实验型**:材料、化学、生物医学工程、机械(硬件)
- **仿真/计算型**:CFD、FEM、电路仿真、分子动力学
- **数据驱动/ML**:CS、AI、生信、遥感
- **理论型**:物理、数学、理论 CS

先在 methodology 开头声明类型,让导师知道评估标准。

---

## 实验型研究模板

适用:材料、化学、生物医学工程、机械、土木等需做实物实验的方向。

```markdown
### 3.1 Overall Design
This study adopts a [实验类型: controlled / comparative / factorial] experimental design
to investigate [RQ]. The overall workflow proceeds as: 样品制备 → 表征 → 性能测试 → 机理分析。

### 3.2 Materials & Sample Preparation
- 原料:[化学品/材料,纯度,供应商]
- 制备工艺:[步骤、参数(温度/时间/气氛)]
- 样品规模:[n=? 组/批,说明 power analysis 或文献依据]

### 3.3 Characterization / Measurement
- 设备:[型号、厂商、关键参数]
- 测试方法:[XRD/SEM/拉伸/电化学...],每个说明测什么、怎么测
- 重复性:每个条件 ≥ [n] 个平行样

### 3.4 Data Analysis
- 指标:[具体指标,如效率、强度、信噪比]
- 统计:[t-test / ANOVA / 回归],显著性水平 α=0.05
- 软件:[Origin / Python / MATLAB]
```

**为什么选这个方法**:比较 1-2 个替代方法,说明本方法在精度/成本/可行性上的优势。

---

## 仿真/计算型研究模板

适用:CFD、FEM、电路 EDA、分子动力学、DFT 等。

```markdown
### 3.1 Model Setup
- 几何/系统模型:[示意图、边界条件、假设]
- 控制方程:[Navier-Stokes / Maxwell / Schrödinger...]
- 求解器:[ANSYS Fluent / COMSOL / VASP / 自研代码]

### 3.2 Mesh / Discretization
- 网格类型与密度,网格无关性验证(grid independence study)
- 时间步长 / 收敛判据

### 3.3 Validation
- 用 [benchmark 案例 / 实验数据] 验证模型正确性
- 误差 < [X]%

### 3.4 Parametric Study
- 扫描参数:[变量及范围]
- 输出指标:[响应量]
```

**为什么仿真而非实验**:成本/尺度/不可达条件(如极端温度)。

---

## 数据驱动/ML 研究模板

适用:CS、AI、生信、遥感、金融工程等。这一类最容易写得空,务必具体。

```markdown
### 3.1 Datasets
- 来源:[公开数据集名 / 自采,URL 或采集协议]
- 规模:[N 样本,特征维度]
- 划分:train/val/test = X/Y/Z,说明无泄漏(no data leakage)
- 预处理:[归一化、缺失值、增强]

### 3.2 Methods / Models
- 基线(baseline):[至少 2-3 个代表性方法]
- 提出方法:[架构图、关键模块、损失函数]
- 超参搜索:[搜索空间、策略(grid/Bayesian)、评估指标]

### 3.3 Evaluation
- 指标:[accuracy/F1/AUC/RMSE/...],为何选这些
- 统计显著性:[多次随机种子,均值±标准差,k-fold]
- 消融实验(ablation):证明每个模块的贡献

### 3.4 Compute & Reproducibility
- 硬件:[GPU 型号、数量]
- 代码与数据开源计划
```

**常见坑**:只说"用 transformer"不展开;没有 baseline;没有 ablation;指标选不当(不平衡数据用 accuracy)。

---

## 理论型研究模板

适用:数学、理论物理、理论 CS。

```markdown
### 3.1 Problem Formulation
- 形式化定义(符号、定义、假设)
- 要证明的命题/定理陈述

### 3.2 Approach
- 证明思路:[归纳 / 构造 / 反证 / 对偶...]
- 关键引理及其作用

### 3.3 Verification
- 特例验证(special cases)
- 数值实验(若适用)佐证理论
- 与已知结果的一致性
```

---

## 变量与假设

理工科 RP 必须明确变量:
- **自变量**(independent):你操控的
- **因变量**(dependent):你测量的响应
- **控制变量**(controlled):保持不变的
- **中介/调节变量**(mediating/moderating):若有

每个 RQ 对应假设(可证伪):
> H1: 在 [条件] 下,[自变量] 的增加将显著 [提高/降低] [因变量],因为 [机制]。

---

## 伦理与合规

- 涉人研究:需 IRB/伦理委员会批准、知情同意、数据脱敏。
- 涉动物:需 IACUC 批准、3R 原则。
- 人类数据(医疗/EHR):GDPR/HIPAA 合规。
- 双用途/敏感技术(如 AI 安全、合成生物):说明负责任研究措施。
- 理工科纯实验/仿真常可写"This study involves no human/animal subjects; no ethical approval is required."——但别省略这一句,导师会问。

---

## 可行性论证四要素

导师评估 feasibility 看四点,RP 要主动回应:

1. **资源可行**:设备/算力/数据是否可得?(写明用本课题组或合作单位的什么资源)
2. **技术可行**:方法是否成熟或有先例?(引文献证明类似方法已被验证)
3. **人力可行**:3-4 年能否完成?(timeline 给出,留缓冲)
4. **申请人可行**:你有无相关基础?(简要提及相关课程/项目经历,详细在 CV)

---

## 风险与预案

必写。导师最怕学生卡死。模板:

| 风险 | 概率 | 影响 | 预案 |
|---|---|---|---|
| 数据采集失败/获取受限 | 中 | 高 | 备用公开数据集 X;与 Y 实验室合作 |
| 方法精度不达预期 | 中 | 中 | 退而求其次回答 RQ2 的子问题;换 baseline |
| 设备/算力不足 | 低 | 高 | 申请学校 HPC;租用云 GPU |
| 研究方向被抢发 | 低 | 中 | 季度跟踪 arXiv,及时调整角度 |

每条预案要"具体可执行",不要写"will find alternatives"。
