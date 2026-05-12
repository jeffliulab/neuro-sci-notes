# LFADS 与动力学建模

**LFADS（Latent Factor Analysis via Dynamical Systems）** 由 Pandarinath et al. 在 2018 年 Nature Methods 提出，是第一个把**深度学习**与**神经动力学建模**系统结合的 BCI 方法。它将 Churchland-Shenoy 的"群体活动是低维动力系统"理论变成了可计算工具。

## 一、核心思想

假设神经群体活动来自一个**低维非线性动力系统**：

$$\mathbf{z}_t = f_\theta(\mathbf{z}_{t-1}, \mathbf{u}_t) \quad \text{(潜动力学)}$$
$$\mathbf{x}_t \sim \text{Poisson}(\exp(W \mathbf{z}_t)) \quad \text{(观测)}$$

其中：
- $\mathbf{z}_t \in \mathbb{R}^d$ 是潜状态（$d \ll N$, 通常 30–128）
- $\mathbf{u}_t$ 是外部输入
- $\mathbf{x}_t \in \mathbb{R}^N$ 是 spike 计数
- $f_\theta$ 是 GRU 实现的非线性动力学

LFADS 用**变分自编码器（VAE）** 推断这个模型：给定 spike 数据，推出最可能的潜动力学。

## 二、LFADS 架构

```
spike x_{1:T} 
  ↓
Encoder (bidirectional GRU)
  ↓
Initial condition z_0 (后验)
  ↓
Generator (GRU unroll)
  ↓
Latent factors z_{1:T}
  ↓
Linear + exp
  ↓
Poisson rate λ_{1:T}
  ↓
重构 spike（去噪）
```

可选的**推断输入通道**（inferred input）让外部驱动（感觉输入、决策信号）进入动力学。

## 三、训练目标（ELBO）

LFADS 最大化 ELBO：

$$\mathcal{L} = \mathbb{E}_q[\log P(x | z)] - \text{KL}(q(z | x) \| P(z))$$

- 重构项：Poisson 负对数似然
- KL 项：VAE 正则，让后验接近先验

## 四、LFADS 能做什么

### 1. 去噪 / 平滑

把带噪的 spike 计数 → 连续平滑的 rate。**视觉上接近"看到神经元的意图"**。

### 2. 单试次恢复

经典平均需要多试次对齐；LFADS 能**从单试次**推出动力学轨迹。这对 BCI 意义重大——BCI 本质是单试次解码。

### 3. 低维可视化

潜空间 $z_t$ 通常 30–64 维，可用 PCA 再降到 2–3 维可视化——展示**神经状态轨迹**。

### 4. 解码改进

解码器接在 $z_t$ 上（而非原 spike），性能显著优于直接用 spike。

## 五、里程碑结果

**Pandarinath 2018 Nat Methods** 在猴子伸手任务上：
- 单试次 rate 估计 R² = 0.85（经典平滑 0.4）
- 下游解码（位置）R² 比卡尔曼高 20–50%
- 能发现已知的旋转动力学

**Keshtkaran 2022 Nat Methods（AutoLFADS）**：自动化超参调优，降低 LFADS 的实验者门槛。

## 六、LFADS 与神经流形理论

LFADS 是 **Churchland-Shenoy 流形动力学假设的计算实现**：

- **流形**：潜空间 $\mathbb{R}^d$
- **动力学**：$f_\theta$ 是 GRU，学到旋转、吸引子、鞍点等结构
- **跨试次共享**：同一个 GRU 在所有试次上使用，反映"大脑计算复用"

这让 LFADS 不只是一个解码器——它是**用神经网络逆推神经科学假设**的工具。

## 七、LFADS 的变种

### AutoLFADS

自动调节 KL 权重、dropout 等 8+ 超参。

### LFADS + behavioral prior

**Sani 2021** 把行为变量作为 LFADS 潜空间的约束——类似早期 CEBRA 思想。

### TNDM

**Hurwitz 2021** 把 LFADS 扩展到非平稳系统，支持跨 session 迁移。

### iLQR on Latent Dynamics

**Pei 2021** 在 LFADS 潜空间上做**模型预测控制（MPC）**——这是 **"脑机接口的 model-based RL"**。

## 八、LFADS 与 Transformer 的比较

| | LFADS | NDT3 / POYO |
| --- | --- | --- |
| 结构 | VAE + GRU | Transformer |
| 训练 | 单数据集、有监督 | 多数据集、自监督预训练 |
| 潜状态 | 显式连续 | 通过注意力隐式 |
| 跨被试 | 差 | 好（基础模型） |
| 可解释 | 高（动力系统） | 低 |
| 计算 | 快 | 慢但可扩展 |

**LFADS 仍是可解释性和动力学建模的首选**；Transformer 赢在规模和跨被试迁移。

## 九、开源实现

- **[lfads-torch](https://github.com/arsedler9/lfads-torch)**：PyTorch 版
- **[AutoLFADS](https://github.com/snel-repo/autolfads-tf2)**：TensorFlow 2 自动调参
- **NLB（Neural Latents Benchmark）**：统一评估 LFADS 等方法的基准

## 十、对 BCI 工程的遗产

LFADS 留下三个深远影响：

1. **"潜空间 = 解码靶点"**：解码器不必吃原信号，可在学到的潜空间上工作
2. **"动力学建模 = 先验"**：即便神经网络也要尊重"平滑演化"先验
3. **"去噪即解码"**：好的生成模型本身就是好解码器

这些思想贯穿后续 NDT、POYO、CEBRA 等所有深度学习 BCI 工作。

## 十一、逻辑链

1. **经典解码器直接吃 spike**，忽视了群体活动的低维动力学结构。
2. **LFADS 用 VAE + GRU 显式建模潜动力系统**，实现单试次 rate 估计。
3. **Churchland-Shenoy 的流形动力学假设** 在 LFADS 里变成可计算工具。
4. **解码器在潜空间上性能显著优于原 spike 上**——LFADS 的核心工程价值。
5. **LFADS 开启了"动力学建模 + 深度学习"的 BCI 范式**，直接催生 NDT、POYO。

## 参考文献

- Pandarinath et al. (2018). *Inferring single-trial neural population dynamics using sequential auto-encoders.* Nat Methods. https://www.nature.com/articles/s41592-018-0109-9
- Keshtkaran et al. (2022). *A large-scale neural network training framework for generalized estimation of single-trial population dynamics.* Nat Methods.
- Sani et al. (2021). *Modeling behaviorally relevant neural dynamics enabled by preferential subspace identification.* Nat Neurosci. — PSID
- Pei et al. (2021). *Neural latents benchmark '21: evaluating latent variable models of neural population activity.* NeurIPS. https://arxiv.org/abs/2109.04463
- Hurwitz et al. (2021). *Targeted neural dynamical modeling.* NeurIPS.
