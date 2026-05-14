# 突触 (Synapse) — 神经元间的化学 / 电连接

> *突触是两个神经元间的功能连接点。**化学突触** (majority) 通过释放神经递质传信号;**电突触** (gap junction) 直接 ion flow。一个 pyramidal neuron 有 10³-10⁴ 个突触输入。突触可塑性 (LTP/LTD) 是学习与记忆的细胞基础。本篇覆盖结构、机制、可塑性。*
>
> **难度**:Intermediate
> **前置知识**:[神经元](Neuron.md)、[动作电位](Action_Potential.md)

---

## 1. 化学突触结构

```
Pre-synaptic terminal ─ synaptic cleft (20-40nm) ─ Post-synaptic membrane
   vesicles + Ca channels                          receptors + ion channels
```

### 1.1 Pre-synaptic 端

- **Vesicles** (40-200nm 直径): 含神经递质 (NT),每个 ~ 1000-10000 分子
- **Active zone**: vesicle 融合释放点
- **Voltage-gated Ca²⁺ channels**: 动作电位到 → 开 → Ca²⁺ inflow

### 1.2 Synaptic cleft (突触间隙)

20-40 nm 间距,含 ECM (extracellular matrix)。

### 1.3 Post-synaptic 端

- **Receptors**:
  - **Ionotropic**: ligand 直接开 ion channel (AMPA, NMDA, GABA-A)
  - **Metabotropic**: GPCR + 二级信使 (慢)
- **PSD (postsynaptic density)**: 富含 scaffold proteins (PSD-95)
- 树突 spine (~1μm head + 0.1μm neck) 是 excitatory 主受体位

---

## 2. 突触传递 step-by-step

```
1. AP 到 axon terminal
2. Voltage Ca channel 开,Ca²⁺ 内流
3. Ca²⁺ 触发 vesicle 与 active zone 融合 (SNARE protein 介导)
4. NT 释放到 cleft (扩散 ~ μs)
5. NT 结合 post-synaptic receptor
6. Ionotropic: ion flow → EPSP / IPSP
   Metabotropic: signaling cascade → slow modulation
7. NT 被 reuptake / degraded / 弥散
```

总延迟 ~ 0.5-2 ms。

---

## 3. EPSP vs IPSP

### 3.1 Excitatory Post-Synaptic Potential (EPSP)

- Glutamate → AMPA/NMDA → Na+ inflow → depolarize
- 单个 EPSP: ~ 0.1-1 mV
- 数十-数百 EPSPs 时空累加 → soma 到阈值 → spike

### 3.2 Inhibitory Post-Synaptic Potential (IPSP)

- GABA → GABA-A → Cl- inflow → hyperpolarize
- 抑制 spike 发生

### 3.3 Summation

- **Temporal**: 同 input 快连续 → 大 EPSP
- **Spatial**: 多 input 同步 → 大 EPSP

---

## 4. 突触可塑性 — 学习的细胞机制

### 4.1 LTP (Long-Term Potentiation)

强 / 高频刺激 → 突触强度**长期增强** (小时-天)
- Bliss & Lømo 1973 海马首次观察
- 涉及 NMDA receptor (Ca dependence + 同时 pre/post 活动)
- "Cells that fire together, wire together" (Hebb 1949)

### 4.2 LTD (Long-Term Depression)

弱 / 低频刺激 → 突触强度长期降低。

### 4.3 STDP (Spike-Timing-Dependent Plasticity)

- Pre spike 略**先于** post → LTP
- Pre spike 略**后于** post → LTD
- 几十 ms 窗口

### 4.4 与 AI Backprop 对比

- Backprop:全局 gradient,需 forward + backward + 完整 label
- STDP:局部 spike timing 规则,生物可实现
- 二者数学上能在某些设定下 equivalent (Lillicrap 2020 review)

---

## 5. 电突触 (Gap Junction)

- Connexin proteins 形成 channel,直接 ion / 小分子通过
- 双向、快(无 delay)
- 主在 interneurons, glia, 心肌
- 占总突触 ~ 1-5%

---

## 6. 突触数量与连接性

- Pyramidal neuron: ~ 10,000 synapses (5,000 input + 5,000 output)
- 全人脑: ~ 10¹⁴ synapses
- 不同 cortex layer 密度差大 (L2/3 高,L1 低)

---

## 7. PyTorch — 突触模型

```python
import torch, torch.nn as nn

class ChemicalSynapse:
    """Simple kinetic model."""
    def __init__(self, tau_rise=1, tau_decay=5, weight=1.0):
        self.r = 0  # receptor open ratio
        self.s = 0  # activated state
        self.tau_rise, self.tau_decay, self.weight = tau_rise, tau_decay, weight
    
    def step(self, pre_spike, dt=0.1):
        # Single-exponential approximation
        if pre_spike:
            self.r = 1
        self.s += (self.r - self.s) / self.tau_rise * dt
        self.r *= (1 - dt / self.tau_decay)
        # PSC (post-synaptic current)
        return self.weight * self.s

# Simulate
syn = ChemicalSynapse()
psc = []
for t in range(1000):
    spike = 1 if t % 100 == 0 else 0
    psc.append(syn.step(spike))
```

---

## 8. 突触缺陷 / 病理

- **Alzheimer**: synapse loss 早于 neuron loss
- **Autism**: synapse pruning 异常
- **Schizophrenia**: NMDA hypofunction 假说
- **Parkinson**: dopaminergic synapse 退化
- **Addiction**: synaptic plasticity hijacked by 药物

---

## 9. 现代研究工具

- **Patch clamp**: 单个突触电生理
- **Imaging**: 钙成像 (Ca²⁺ proxy for activity)
- **Optogenetics**: 光控特定突触
- **CLARITY / iDISCO**: 透明化全脑 imaging
- **Single-synapse RNA**: 突触特异 mRNA

---

## 10. Common Pitfalls

### 10.1 Synapse 是静态错觉

突触不断 turnover,LTP/LTD 全程。

### 10.2 只看 excitatory

Inhibitory 同样重要,balance 关键。

### 10.3 数量 = 功能错觉

某些突触 silent (NMDA-only),不强烈影响 firing。

### 10.4 与 AI weight 不简单对应

突触 weight 是动态、概率化、neuromodulator 依赖。

### 10.5 Glia 参与

近 20 年发现 astrocytes 直接影响 synaptic transmission。

---

## 11. Related Concepts

- **AI 对比**:https://jeffliulab.github.io/ai-notes/02_Deep_Learning/01_Intro/MLP/、https://jeffliulab.github.io/ai-notes/01_AI/03_Frontiers/03_World_Models/WM_Predictive_Coding/

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Bliss, T. V. P. & Lømo, T.** "Long-lasting potentiation of synaptic transmission in the dentate area of the anaesthetized rabbit following stimulation of the perforant path." *J Physiol*, 1973.
3. **Bi, G. & Poo, M.-M.** "Synaptic Modifications in Cultured Hippocampal Neurons (STDP)." *J Neurosci*, 1998.
4. **Bear, M. F. et al.** *Neuroscience: Exploring the Brain*. 4th ed., 2015.
5. **Lillicrap, T. et al.** "Backpropagation and the brain." *Nat Rev Neurosci*, 2020.
