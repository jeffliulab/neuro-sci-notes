# LTP / LTD — 长时程突触可塑性,学习与记忆的细胞机制

> *1973 年 Bliss & Lømo 在海马齿状回首次记录到 **Long-Term Potentiation (LTP)** — 高频刺激后突触强度持续增强数小时-天。这是 "学习与记忆的细胞机制" 候选机制。50 年研究找到 NMDA receptor + CaMKII + AMPA receptor trafficking 链。*
>
> **难度**:Intermediate-Advanced
> **前置知识**:[突触](Synapse.md)、[神经递质](Neurotransmitters.md)

---

## 1. 实验 protocol

### 1.1 LTP 诱导

- **High-frequency stimulation (HFS, tetanus)**: 100 Hz × 1s
- Recording: field EPSP slope 或 single-cell EPSC
- 强度可增强 200-300%,持续小时-天

### 1.2 LTD 诱导

- **Low-frequency stimulation (LFS)**: 1 Hz × 15 min
- 或 paired-pulse low frequency
- 突触强度降低 50%

---

## 2. 分子机制 (NMDA-dependent LTP)

```
1. Pre AP → Glu release
2. Glu → AMPA receptor → 局部 depolarize
3. Post depolarize 配合 Glu → NMDA receptor 开 (Mg²⁺ block 移除)
4. Ca²⁺ inflow 大量
5. Ca²⁺ → CaMKII 激活
6. CaMKII → AMPA receptor phosphorylate + 插入 PSD
7. ↑ AMPA receptor → 更大 EPSP
8. 长期: gene transcription, protein synthesis
```

### 2.1 NMDA 是 coincidence detector

- 需要 Glu (pre) **和** depolarize (post) 同时
- → "Hebbian": cells that fire together, wire together

### 2.2 LTD: 低 Ca²⁺ → calcineurin → AMPA 内吞

低频低 Ca²⁺ → 不同 enzyme → AMPA receptor 移除。

---

## 3. STDP (Spike-Timing-Dependent Plasticity)

精细 timing 决定 LTP / LTD:

| Pre vs Post 时序 | 结果 |
|---|---|
| Pre 先 post 后 (~ 10 ms) | LTP |
| Post 先 pre 后 (~ 10 ms) | LTD |
| 距离 > 50 ms | 无变化 |

```
       LTP
       /\
      /  \
─────/────\──────────────
              \  /
               \/
               LTD
   -50ms  0  +50ms
```

---

## 4. 早期 vs 晚期 LTP

### 4.1 E-LTP (1-3 hours)

- 不需 protein synthesis
- AMPA receptor 已存在,只 trafficking
- 阻断 protein synthesis 仍能形成

### 4.2 L-LTP (> 3 hours, "synaptic consolidation")

- **需** protein synthesis (用 anisomycin 阻 → LTP 消失)
- CREB 等 transcription factor
- 新 mRNA → 新 protein → 新 synapse

---

## 5. LTP 类型

### 5.1 NMDA-dependent (Schaffer collateral → CA1)

经典 LTP, 描述以上。

### 5.2 Non-NMDA (mossy fiber → CA3)

Pre-synaptic LTP — 增加 release probability。

### 5.3 Cerebellar LTD

小脑学习的核心 — Marr-Albus 理论 (Climbing fiber 教 Parallel fiber)。

---

## 6. 行为关联

### 6.1 海马 LTP ↔ Spatial memory

- LTP 阻断 (NMDA antagonist) → Morris water maze 学习 impair
- 但: LTP ≠ memory itself,只是 neural correlate

### 6.2 Amygdala LTP ↔ Fear conditioning

经典 Pavlov 条件反射 strong 关联 amygdala LTP。

### 6.3 Cortex LTP ↔ Perceptual learning

V1 LTP 关联视觉训练后改善。

---

## 7. PyTorch — STDP 学习规则

```python
import torch

class STDPSynapse:
    def __init__(self, w=0.5, lr=0.01, tau=20):
        self.w = w
        self.lr, self.tau = lr, tau
        self.pre_trace = 0
        self.post_trace = 0
    
    def step(self, pre_spike, post_spike, dt=1):
        # Decay traces
        self.pre_trace *= (1 - dt/self.tau)
        self.post_trace *= (1 - dt/self.tau)
        # Update on spikes
        if pre_spike:
            self.pre_trace += 1
            # Pre after post → LTD
            self.w -= self.lr * self.post_trace
        if post_spike:
            self.post_trace += 1
            # Post after pre → LTP
            self.w += self.lr * self.pre_trace
        # Clip
        self.w = max(0, min(1, self.w))
        return self.w * pre_spike  # 输出贡献
```

---

## 8. 与 Backprop 关系

### 8.1 Backprop

- 全局 gradient: $\frac{\partial L}{\partial w}$
- 需要 backward pass + 完整 label
- 生物不可行 (没有 "send gradient back" 机制)

### 8.2 STDP

- 局部 spike timing 规则
- 不需 global signal
- 但缺乏 credit assignment

### 8.3 现代尝试

- Equilibrium Propagation (Bengio 2017): 数学等价 backprop,生物可行
- Predictive coding: gradients 通过 error neurons
- 综述: Lillicrap "Backpropagation and the brain" (2020)

---

## 9. 病理

- **Alzheimer**: synapse loss + LTP impairment 早于 neuron loss
- **Depression**: hippocampal LTP 降低
- **Addiction**: VTA LTP 被滥用药物 hijack
- **Autism**: NMDA / AMPA 平衡异常

---

## 10. 历史

- **1949** — Hebb 假说 ("cells that fire together")
- **1973** — Bliss & Lømo 海马 LTP 首报
- **1986** — Morris NMDA antagonist 阻 spatial learning
- **1992** — Tsien NMDA-KO mouse — LTP + memory impair
- **1998** — STDP 经典 protocol (Bi & Poo, Markram)
- **2000s** — CaMKII / AMPA trafficking 细节
- **2010s** — 标记记忆 engram (Tonegawa)
- **2020s** — optogenetic LTP induction 验证 causally

---

## 11. Common Pitfalls

### 11.1 LTP ≠ memory

LTP 是细胞现象,memory 是行为现象。Correlation 强,causal 仍未完全证明。

### 11.2 NMDA 不是唯一

mossy fiber + cerebellum LTP 不需 NMDA。

### 11.3 Saturation

LTP 有 ceiling;太多 stimulation 反而 LTD。

### 11.4 In vitro ≠ in vivo

切片 LTP protocol 与活体不完全 transfer。

### 11.5 Network plasticity

LTP 是 synapse level;memory 涉及 network reorganization。

---

## 12. Related Concepts

- **同节**:[突触](Synapse.md)、[神经递质](Neurotransmitters.md)、[Hodgkin-Huxley](Hodgkin_Huxley.md)
- **AI 对比**:https://jeffliulab.github.io/ai-notes/02_Deep_Learning/01_Intro/MLP/

---

## References

1. **Bliss, T. V. P. & Lømo, T.** *J Physiol*, 1973.
2. **Hebb, D. O.** *The Organization of Behavior*. 1949.
3. **Bi, G. & Poo, M.-M.** "Synaptic Modifications in Cultured Hippocampal Neurons (STDP)." *J Neurosci*, 1998.
4. **Tonegawa, S. et al.** "Memory engram cells have come of age." *Neuron*, 2015.
5. **Lillicrap, T. et al.** "Backpropagation and the brain." *Nat Rev Neurosci*, 2020.
