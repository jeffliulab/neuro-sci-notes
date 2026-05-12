# ReFIT 与在线校准

**ReFIT（Recalibrated Feedback Intention-Trained Kalman Filter）** 由 Gilja 等人于 2012 年提出，是 BCI 解码器设计中最重要的工程改进之一——它从根本上改变了"如何训练 BCI 解码器"的方法论。

## 一、传统卡尔曼的训练问题

标准卡尔曼训练需要**配对数据**：神经活动 $\mathbf{y}_t$ 和对应的运动意图 $\mathbf{x}_t$。但瘫痪患者**没有真实运动轨迹**可对齐。

经典做法：**观察任务（OL, open-loop）**——让用户观察光标自动向目标移动，假设用户神经活动编码的就是"向目标"的意图。

**问题**：用户只是**看着**光标，不是**主动意图**控制；神经活动分布与闭环实际使用时显著不同。

## 二、ReFIT 的核心洞察

**用户实际控制时的意图是什么？**

当光标在 $\mathbf{p}_t$、目标在 $\mathbf{g}$、用户有意图向目标去——**意图方向永远是** $\mathbf{g} - \mathbf{p}_t$，无论实际光标走向何方。

这意味着：
- 即使用户解码不准导致光标偏离，**用户的意图仍然是朝向目标**
- 训练时应该用**意图向量**（目标方向），而不是**实际轨迹**

## 三、ReFIT 两阶段训练

```
阶段 1：标准卡尔曼闭环
  用户用初代解码器控制光标
  记录 [神经活动 y_t，实际轨迹 x_t]

阶段 2：意图重标记
  对每个 t，重新标记意图 = 指向当时目标方向
  x_t^refit = normalize(g - p_t)
  
  用 [y_t, x_t^refit] 重训练卡尔曼
  → 新的 H 矩阵、Q/R 矩阵
```

### 假设修正的力量

这个"假设用户做对了"的先验非常强：

- 即使训练期光标走歪，重标记后意图仍是正确方向
- 新解码器学到"神经活动 → 正确意图"的映射
- 闭环时性能显著提升

## 四、Gilja 2012 实验结果

**Gilja et al. (2012, Nature Neurosci)** 在猴子上验证：

- 标准 Velocity-Kalman：~3 bps
- ReFIT-Kalman：**>5 bps**
- 任务完成时间缩短一半

**Jarosiewicz et al. 2015** 把 ReFIT 应用于人类瘫痪患者（BrainGate）：
- 允许 2 年以上稳定使用
- 免重新校准
- 家庭日常使用可行

## 五、CLDA：闭环解码器适应

**CLDA（Closed-Loop Decoder Adaptation）** 是 ReFIT 思想的推广——**解码器在闭环使用中持续学习**。

### SmoothBatch

**Orsborn et al. 2014 Neuron** 提出 **SmoothBatch**：
- 每 100–500 ms 用最新 [y_t, x_t^intent] 更新解码器
- 平滑参数更新（避免震荡）
- 用户和解码器**共同进化**

这是 **"co-adaptation"（共适应）** 的雏形。

### RAX / OFC

**Shanechi et al. 2016** 把 CLDA 建模为**最优反馈控制**问题：
- 用户是一个 OFC（Optimal Feedback Control）代理
- 解码器学习让用户的 OFC 策略达到目标
- 数学上是**双层优化**

## 六、ReFIT 的局限与扩展

### 假设失效的场景

ReFIT 假设"用户一直朝向目标"，但：
- 用户可能走弯路、停顿、悔改
- 复杂任务（绘画、开门）没有明确"目标"
- 自由运动（无目标）无法应用

### 扩展方向

**Dangi et al. 2013** 提出用**最速路径**而非直线作为意图；
**Jarosiewicz et al. 2013** 引入**意图不确定性**加权；
**Zhang et al. 2018** 用**强化学习**替代 ReFIT，让用户意图通过 reward 自动浮现。

## 七、ReFIT 与现代深度学习解码器

ReFIT 的核心思想——**假设用户做对 + 闭环重校准**——在现代深度学习 BCI 中以新形式出现：

### 自监督对齐

**CEBRA / LFADS** 不需要显式的意图标签，通过**行为对比学习**或**重构损失**自动学到潜空间。

### 在线微调

**NDT3 + 在线 fine-tune**：每几分钟用当前 session 数据微调；潜在状态对齐让新 session 无需从头校准。

### 人在回路 RL

**Willett 2023 语音 BCI** 引入**用户重说**纠错——用户听到错词可重说，模型学习修正。

## 八、ReFIT 对 BCI 工程的长期影响

ReFIT 证明了三个重要教训：

1. **训练数据的"意图标签"比"行为标签"更有信息量**
2. **闭环使用本身是训练数据**——静态数据集不够
3. **用户和解码器共进化**——系统设计必须支持这一动态过程

这些教训奠定了所有现代 BCI 校准的工程基石。

## 九、今日最先进的在线校准

### Neuralink PRIME（2024–2025）

Noland Arbaugh 术后电极线回缩 → 仅 ~15% 电极可用。Neuralink 通过：
- **潜空间重映射**：把剩余通道的活动映射到原潜空间
- **在线 ReFIT 变种**：日常使用中持续微调
- **用户适应**：用户学习用更少通道

最终 ITR 仍 > 8 bps，证明**现代 CLDA 能对抗电极失效**。

### Willett 2023 语音 BCI

- 每 session 前 10 分钟校准
- **HMM + RNN** 两层训练
- 用户重说错词作为 fine-tune 数据

## 十、逻辑链

1. **传统卡尔曼的训练数据有问题**——观察任务与闭环意图分布不一致。
2. **ReFIT 的"意图重标记"**利用"用户朝向目标"的先验修正数据。
3. **CLDA/SmoothBatch 推广 ReFIT 到连续在线适应**。
4. **ReFIT 证明闭环本身就是训练信号**——这是 BCI co-adaptation 的起点。
5. **现代深度学习 BCI 延续 ReFIT 精神**：自监督对齐 + 在线微调 + 用户在环学习。

## 参考文献

- Gilja et al. (2012). *A high-performance neural prosthesis enabled by control algorithm design.* Nat Neurosci. https://www.nature.com/articles/nn.3265
- Jarosiewicz et al. (2015). *Virtual typing by people with tetraplegia using a self-calibrating intracortical brain-computer interface.* Sci Transl Med.
- Orsborn et al. (2014). *Closed-loop decoder adaptation shapes neural plasticity for skillful neuroprosthetic control.* Neuron. https://www.cell.com/neuron/fulltext/S0896-6273(14)00871-1
- Shanechi et al. (2016). *Rapid control and feedback rates enhance neuroprosthetic control.* Nat Commun.
- Dangi et al. (2013). *Design and analysis of closed-loop decoder adaptation algorithms for brain-machine interfaces.* Neural Comp.
