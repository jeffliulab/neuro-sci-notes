# 神经潜在基准：NLB 与 FALCON

**神经数据基准（neural benchmarks）** 是 BCI × 深度学习领域的 "**ImageNet**"。**NLB（Neural Latents Benchmark）** 2021 + **FALCON（NeurIPS 2024）** 定义了**标准化评估协议**，让**神经基础模型**（NDT3、POYO、CEBRA）的跨论文对比成为可能。

## 一、为什么需要基准

### BCI 领域的碎片化

2020 年前：
- 每实验室**自己的数据**
- 不同任务、不同指标
- **论文间难比较**
- 进步**难衡量**

### 基准的价值

- **统一评估**：定义任务 + 指标
- **公开排行榜**：推动竞争
- **重现性**：可验证
- **新方法显化**

ImageNet 2012 → CV 深度学习爆发。BCI 同样需要。

## 二、NLB（Neural Latents Benchmark）2021

### 背景

- Pei, Ye et al. (2021, NeurIPS Datasets)
- Chethan Pandarinath、Mackenzie Mathis、Eva Dyer 等推动
- 第一个综合 BCI 基准

### 数据集

- **MC_Maze**：猴子延迟触达任务
- **MC_RTT**：猴子随机触达
- **Area2_Bump**：猴子感觉 + 运动
- **DMFC_RSG**：时间估计
- 共 **4 套、36 小时神经数据**

### 任务

**给定时间窗口神经 spike，预测**：
- 未来 spike（self-supervised）
- 行为（运动学）
- 潜在变量

### 主要指标

- **co-bps（co-smoothing bits per spike）**：未来 spike 预测
- **vel R²**：速度预测
- **FP R²**：前向预测

### 提交方式

- 预测提交到 EvalAI
- 公开排行榜
- 2022-2024 活跃

## 三、NLB 排行榜演变

### 2022 初

- 基线：GLM / LSTM / LFADS
- 最优 **co-bps ~0.3**

### 2023

- **NDT2** 引入
- co-bps 提升到 **~0.4**
- Transformer 显示优势

### 2024

- **NDT3、POYO** 基础模型
- co-bps **~0.5**
- 跨被试预训练作用显著

### 2025+ 预期

- 继续上升
- 饱和点**难预测**

## 四、FALCON（2024 NeurIPS）

### 背景

- **Foundation Animal LLM Cross-ObservatioN**
- 2024 NeurIPS Datasets Track
- 推动者：Dyer、Mathis、Kording 等

### 数据集

更大、更多样：
- **H1**：人类手写（Willett 2021 数据）
- **M1、M2**：猴子运动
- **B1**：鸟类 / 啮齿

共 **~800 小时神经数据**——NLB 的 **20×**。

### 任务

- **"Few-shot calibration"**：预训练 → 新被试/任务少量数据快速校准
- 模拟**临床场景**（BCI 不能每次 10 小时数据）

### 指标

- R² on held-out behavior
- Calibration efficiency
- Cross-subject transfer

### 意义

- **测试神经基础模型**的真正能力
- 跨被试 + 跨任务**不再假设**
- **临床相关**设计

## 五、NLB vs FALCON

| | NLB | FALCON |
| --- | --- | --- |
| 年份 | 2021 | 2024 |
| 数据 | 36 小时 | 800 小时 |
| 物种 | 猴子 | 猴 + 人 + 鸟 |
| 任务 | 固定 | 多 + few-shot |
| 指标 | co-bps | 校准效率 |
| 聚焦 | 解码 | **迁移** |

FALCON 是 **NLB 的基础模型时代演进**。

## 六、其他基准

### BCI Competitions

- **BCI IV、V**：EEG 经典
- **EEG decoding** 标准
- Web 平台：bbci.de/competition

### IBL（International Brain Laboratory）

- 啮齿电生理标准化
- 多中心合作
- **22 名 PI、10 国家**
- 数据 + 行为共享

### DANDI Archive

- 神经数据仓库
- NWB 标准
- 所有 NLB 数据在此

### HCP、NSD

- 人类 fMRI
- **NSD** 是 MindEye 的 ImageNet
- 开源

### Allen Institute

- 小鼠大脑全面数据
- Brain Observatory
- 生态学习

## 七、数据标准

### NWB（Neurodata Without Borders）

- **NWB 2.0** 标准
- HDF5 基础
- 跨实验室兼容

### BIDS

- 脑影像标准
- fMRI、EEG、MEG

### 为什么重要

- 数据再用
- 元数据 + 实验设置
- **减少数据 "垃圾数据 数据"** 问题

## 八、人类 BCI 数据

### 开放有限

- 医疗数据**HIPAA 限制**
- 多数实验室**谨慎共享**

### 开源例子

- **Willett 2021 手写**：部分开源
- **DIDI**：BrainGate 数据共享
- **Physionet**：EEG 开源

### 挑战

- 患者知情同意
- 去标识化（但脑数据可识别——见 [脑数据隐私与认知生物计量](../13_Ethics_Neurorights/脑数据隐私与认知生物计量.md)）
- **伦理审查**

## 九、工具支持

### 评测工具

- **EvalAI**：NLB 主平台
- **HuggingFace Datasets**：FALCON
- **Papers With Code**：整合排名

### 参与方式

- 注册账号
- 下载数据
- 训练模型
- 提交预测
- 排行榜自动更新

### 代码

- 基线**开源**
- 新方法可 fork

## 十、影响

### 对研究

- **方法进化**被标准化记录
- **基础模型**价值证实
- 跨实验室**对话**

### 对工业

- **Precision、Paradromics** 用 NLB 预训练模型
- **Neuralink** 未公开但可能内部用
- **Synchron** 可能合作学术

### 对教育

- 学生可**立即参与**
- 降低 BCI 入门门槛
- **加速人才培养**

## 十一、局限

### 1. 数据规模

- 即便 FALCON 800 小时
- ImageNet 140M 图
- **数据瓶颈** 持续

### 2. 实验设置

- 简单 reaching task 为主
- 自由、自然行为**少**
- 实际 BCI 使用**更复杂**

### 3. 人类数据少

- 主要猴子、啮齿
- 临床迁移**间接**

### 4. 指标限制

- R² / co-bps 不等于**临床有用**
- 需新指标（例如 usability）

## 十二、未来方向

### 1. 更大基准

- **FALCON v2** 预期
- 目标 10,000+ 小时
- 多物种 + 多任务

### 2. 闭环评测

- **在线 BCI 性能**
- 用户主观体验
- 超越**离线** R²

### 3. 临床基准

- **HIPAA 兼容**临床数据
- 真实患者验证
- 联邦学习框架

### 4. 多模态

- 神经 + 行为 + 环境
- **"world model" 基准**
- 走向具身智能

## 十三、逻辑链

1. **基准是 BCI 深度学习的 ImageNet 时刻**。
2. **NLB 2021** 首个综合基准，co-bps 等统一指标。
3. **FALCON 2024** 扩大 20×，聚焦 **few-shot 迁移**。
4. **NWB、BIDS、DANDI** 是数据标准生态。
5. **人类 BCI 数据**受 HIPAA / 伦理限制，开源少。
6. **工业 + 学术** 都使用基准推进。
7. **未来**：更大、更真、闭环、多模态基准。

## 参考文献

- Pei et al. (2021). *Neural Latents Benchmark '21: evaluating latent variable models of neural population activity.* NeurIPS Datasets. https://neurallatents.github.io/
- Karpowicz et al. (2024). *FALCON benchmark.* NeurIPS 2024. https://snel-repo.github.io/falcon/
- Teeters et al. (2015). *Neurodata Without Borders: Creating a Common Data Format for Neurophysiology.* Neuron.
- International Brain Laboratory (2021). *Standardized and reproducible measurement of decision-making in mice.* eLife.
- DANDI Archive. https://dandiarchive.org
