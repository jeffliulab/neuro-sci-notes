# 闭环控制与 CLDA

**闭环（closed-loop）** 是 BCI 与传统离线神经解码最本质的区别——用户看到结果、调整神经活动、解码器在交互中持续学习。这种**用户与解码器共进化**的过程叫做 **Co-adaptation**，其技术实现就是 **CLDA（Closed-Loop Decoder Adaptation）**。

## 一、为什么必须闭环

### 开环 BCI 的问题

离线训练 + 静态解码 = **开环 BCI**：
- 用户神经活动因电极漂移、疲劳、学习而变化
- 解码器参数不变 → 性能日渐退化
- 用户无法调整策略，因为他们不知道怎样能被更好地解码

### 闭环的优势

- **用户看到反馈**，能学习"怎么想"让解码器正确
- **解码器持续更新**，适应神经活动变化
- **两者共进化**，形成稳定的 neuroprosthetic skill

**类比**：学习打字时，键盘反馈让你知道敲得是否准确——BCI 需要同样的反馈闭环。

## 二、神经可塑性的关键

Nicolelis 在多年工作中发现：**大脑会为 BCI 分配表征资源**——即便植入位置不"理想"，经过训练后神经活动也会适应 BCI 任务。

这个现象叫 **neuroprosthetic plasticity**：
- 神经元调谐会改变以匹配解码器
- 未被使用的自由度被"剪枝"
- 常用 BCI 任务形成专用神经网络

**工程含义**：**好的 BCI 训练不等于好的离线性能**；必须看闭环下用户能达到多好。

## 三、CLDA 三种策略

### 1. Batch Retraining

定期（每天、每周）用新数据重训练解码器。

- 简单但更新不连续
- 用户在再训练之间**感到性能退化**
- BrainGate 早期用过

### 2. Incremental Update (SmoothBatch)

**Orsborn et al. 2014 Neuron** 提出：

$$\theta_{t+1} = (1-\alpha) \theta_t + \alpha \theta_{\text{new}}$$

- $\theta_t$ 是解码器参数
- $\theta_{\text{new}}$ 是新数据上的解
- $\alpha \in [0.01, 0.1]$ 是平滑步长
- 每 100–500 ms 更新一次

**优点**：平滑、连续、用户感觉稳定。

### 3. 强化学习驱动

**Shanechi lab** 把解码器适应看作 RL 问题：
- 状态：解码器参数
- 行动：参数更新
- 奖励：任务成功、用户满意

用 POMDP 或 PG 优化参数。

## 四、意图假设

CLDA 需要意图标签，但用户没法直接告诉系统"我的意图是什么"。三种常见假设：

### 1. Goal-directed

假设用户在"朝向当前目标"的意图——**ReFIT 的假设**（见 [ReFIT与在线校准](../04_Classical_Decoding/ReFIT与在线校准.md)）。

### 2. 后验验证

用户到达目标后，回溯标注"这段时间意图是 X"。

### 3. 强化信号

用户完成任务时给 + 奖励；失败时 -。让 RL 驱动参数更新。

**SPIKEIRL**（Sani 2023）是典型代表。

## 五、用户学习的三阶段

Orsborn 和后续研究发现用户 BCI 学习分三阶段：

### 阶段 1：探索

- 天数 1–3
- 神经活动高变异性
- 任务成功率低
- 解码器还在适应

### 阶段 2：稳定

- 周数 1–4
- 用户找到稳定策略
- 成功率上升
- 解码器收敛

### 阶段 3：专家化

- 月数 2+
- 神经活动高度一致
- 甚至出现"自动化"——用户不再有意识控制
- 解码器参数固定

**长期 BCI 用户** 如 BrainGate 的 Cathy Hutchinson 使用 5+ 年后已达阶段 3，近乎**肌肉记忆**的神经控制。

## 六、闭环延迟预算

闭环稳定的关键是**端到端延迟**：

```
神经 → 采集 (5ms) → 预处理 (5ms) → 解码 (10ms) → 执行 (10ms) → 反馈 (20ms)
                                                                  ↑
                                                          用户视觉感知 ~30ms
```

**总延迟 < 100ms** 才算"自然"。超过 100ms 用户会感到"滞后"，闭环学习受损。

深度学习解码器的延迟是关键挑战——Transformer 自注意力 O(n²) 复杂度需要优化。

## 七、神经-机器协同适应

**Co-adaptation** 的几种数学视角：

### 博弈视角

用户（U）和解码器（D）玩合作博弈：
- U 选择神经策略 $\pi_U$
- D 选择解码参数 $\theta_D$
- 联合目标：最大化任务性能

Nash 均衡 = 稳定的 co-adaptation 状态。

### 贝叶斯视角

- 用户维护"解码器参数的信念"$P(\theta_D | \text{history})$
- 解码器维护"用户意图的信念"$P(u | \text{neural})$
- 闭环迭代让两个信念收敛

### 控制视角

解码器是 controller，用户是 plant。Co-adaptation 是 **adaptive control** 问题——Dangi 2013 给出形式化框架。

## 八、经典实验：Orsborn 2014

**Orsborn et al. 2014 Neuron** 的核心实验：

1. 给猴子植入 M1 Utah Array
2. 对比三组：
   - (a) 静态解码器（开环训练后固定）
   - (b) SmoothBatch CLDA
   - (c) ReFIT（一次重校准）
3. 测试 1 个月

**结果**：
- (a) 性能持续下降
- (c) 初始高，后期退化
- (b) **持续提升，最终性能最高**

证明了**连续适应 > 一次重校准 > 静态**。

## 九、CLDA 与现代深度学习

### 传统 CLDA

更新线性卡尔曼参数——简单、稳定。

### 深度 CLDA

更新深度网络参数——更复杂：
- 梯度更新容易震荡
- 需要正则化
- 可能"灾难性遗忘"

解决：
- **LoRA** 式低秩更新
- **Elastic Weight Consolidation**
- **Replay buffer**（旧数据 + 新数据混合）

### Foundation Model 上的 CLDA

**NDT3 + 在线微调**：预训练的基础模型 + 每 session 少量 LoRA 更新——结合预训练稳定性 + 在线适应性。

## 十、CLDA 的临床实践

- **Neuralink PRIME**：用户术后 1–2 个月 co-adaptation 达到稳定
- **BrainGate**：用户长期（5+ 年）使用，解码器每月更新一次
- **Pitt ICMS**：同时更新读（M1）和写（S1）编码器

**临床教训**：CLDA 不是 "bonus"，是 BCI 长期可用的**必要条件**。

## 十一、逻辑链

1. **开环 BCI 因神经活动变化必然退化**——闭环是唯一解。
2. **CLDA 让解码器参数持续更新**，配合用户学习。
3. **SmoothBatch / ReFIT / RL 驱动** 是三种主要 CLDA 策略。
4. **用户学习分三阶段**，最终可达"肌肉记忆"级自动化。
5. **深度学习 CLDA 需新工具**（LoRA、EWC、replay）解决遗忘和震荡。
6. **CLDA 是 BCI 从实验室走向临床的必须工程**。

## 参考文献

- Orsborn et al. (2014). *Closed-loop decoder adaptation shapes neural plasticity for skillful neuroprosthetic control.* Neuron. https://www.cell.com/neuron/fulltext/S0896-6273(14)00871-1
- Dangi et al. (2013). *Design and analysis of closed-loop decoder adaptation algorithms for brain-machine interfaces.* Neural Comp.
- Shenoy & Carmena (2014). *Combining decoder design and neural adaptation in brain-machine interfaces.* Neuron.
- Sani et al. (2023). *Reinforcement learning for closed-loop adaptation of brain-computer interfaces.* arXiv.
- Jarosiewicz et al. (2015). *Virtual typing by people with tetraplegia using a self-calibrating intracortical BCI.* Sci Transl Med.
