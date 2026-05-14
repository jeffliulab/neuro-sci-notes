# 神经元 (Neuron) — 神经系统的基本单元

> *神经元是神经系统的基本结构和功能单元。约 860 亿个神经元 (Azevedo 2009) 组成人脑,通过 100 万亿 synapse 互联。本篇覆盖神经元解剖、主要类型 (锥体 / 中间神经元 / Purkinje)、电生理基本面、与现代深度学习的对比。*
>
> **难度**:Introduction-Intermediate
> **前置知识**:[神经科学历史](../00_Foundations/Neuroscience_History.md)

---

## 1. 神经元基本解剖

<div class="diagram">
<svg viewBox="0 0 720 240" xmlns="http://www.w3.org/2000/svg">
  <text x="360" y="30" text-anchor="middle" font-family="Fraunces, Georgia, serif" font-style="italic" font-weight="600" font-size="18" fill="var(--dia-stroke)">Pyramidal Neuron Structure</text>

  <!-- soma -->
  <circle cx="280" cy="130" r="35" fill="var(--dia-bg-card)" stroke="var(--dia-accent)" stroke-width="2"/>
  <text x="280" y="135" text-anchor="middle" font-family="JetBrains Mono, monospace" font-size="11" fill="var(--dia-accent-deep)">Soma</text>

  <!-- dendrites (left/up) -->
  <path d="M 250 110 Q 200 70 160 50" stroke="var(--dia-stroke)" stroke-width="2" fill="none"/>
  <path d="M 240 130 Q 180 130 150 130" stroke="var(--dia-stroke)" stroke-width="2" fill="none"/>
  <path d="M 250 150 Q 200 180 170 200" stroke="var(--dia-stroke)" stroke-width="2" fill="none"/>
  <text x="120" y="100" text-anchor="middle" font-family="Fraunces, Georgia, serif" font-style="italic" font-size="11" fill="var(--dia-stroke-soft)">Dendrites (inputs)</text>

  <!-- axon (right) -->
  <line x1="315" y1="130" x2="550" y2="130" stroke="var(--dia-stroke)" stroke-width="3"/>
  <text x="420" y="120" text-anchor="middle" font-family="JetBrains Mono, monospace" font-size="11" fill="var(--dia-stroke)">Axon (output)</text>

  <!-- Myelin -->
  <rect x="330" y="125" width="30" height="10" fill="var(--dia-bg-card)" stroke="var(--dia-blue)" stroke-width="1"/>
  <rect x="380" y="125" width="30" height="10" fill="var(--dia-bg-card)" stroke="var(--dia-blue)" stroke-width="1"/>
  <rect x="430" y="125" width="30" height="10" fill="var(--dia-bg-card)" stroke="var(--dia-blue)" stroke-width="1"/>
  <text x="420" y="155" text-anchor="middle" font-family="Fraunces, Georgia, serif" font-style="italic" font-size="10" fill="var(--dia-blue)">Myelin sheaths</text>

  <!-- Axon terminal -->
  <path d="M 550 130 L 580 110 M 550 130 L 600 130 M 550 130 L 580 150" stroke="var(--dia-stroke)" stroke-width="2" fill="none"/>
  <circle cx="595" cy="115" r="5" fill="var(--dia-green)"/>
  <circle cx="610" cy="130" r="5" fill="var(--dia-green)"/>
  <circle cx="595" cy="145" r="5" fill="var(--dia-green)"/>
  <text x="620" y="180" text-anchor="middle" font-family="Fraunces, Georgia, serif" font-style="italic" font-size="11" fill="var(--dia-stroke-soft)">Synapses</text>
</svg>
</div>
<p class="figure-caption">Figure 1 — 典型锥体神经元结构:Dendrite 收集多输入,Soma 整合,Axon 输出 spike,Synapse 传给下一神经元。</p>

### 1.1 Soma (胞体)

- 细胞核 + 细胞质
- 直径 4-100 μm
- 蛋白合成中心
- 整合 dendrite 输入,决定是否发 spike

### 1.2 Dendrites (树突)

- 接收输入 (10³-10⁴ synapses per pyramidal neuron)
- 主动 (含 voltage-gated channel) — 非线性整合
- 分支模式定义 neuron 类型

### 1.3 Axon (轴突)

- 输出端,长可达 1m+ (motor neuron)
- 髓鞘 (myelin) 包裹:由 Schwann cells (PNS) / oligodendrocytes (CNS) 形成
- Myelin 加速信号 (跳跃式传导,saltatory conduction)
- Ranvier nodes 是无髓段,集中 Na+ channels

### 1.4 Axon Terminal (突触前末端)

- 含 vesicles (载满 neurotransmitter)
- Action potential 到 → Ca²⁺ inflow → vesicle 融合释放 NT

---

## 2. 静息膜电位

神经元静息时膜内 -70 mV (相对外部):
- 高浓度细胞内 K⁺ (140 mM)
- 高浓度细胞外 Na⁺ (145 mM) + Cl⁻
- Na⁺/K⁺ pump (ATPase) 维持梯度: 每 cycle 3 Na⁺ out + 2 K⁺ in
- 静息时主要 K⁺ leak channel 开 → 接近 K⁺ 平衡电位 (-90 mV)

Nernst equation:

$$E_K = \frac{RT}{zF} \ln \frac{[K^+]_o}{[K^+]_i}$$

---

## 3. 动作电位 (Action Potential)

经典 1ms event:
1. **去极化** (depolarization): 阈值约 -55 mV,Na⁺ channel 开
2. **峰值**: ~+30 mV (Na⁺ 内流)
3. **复极化**: Na⁺ channel 失活,K⁺ channel 开,K⁺ 外流
4. **超极化** (hyperpolarization): K⁺ channel 慢关
5. **回静息**: Na⁺/K⁺ pump

全有或全无 (all-or-none): 阈值之上一致 spike;之下完全不 fire。

---

## 4. 主要神经元类型

### 4.1 Pyramidal Neurons (锥体)

- Cortex 主要 excitatory neuron
- 三角形 soma + apical dendrite (向上分支)
- ~70% of cortex
- Glutamate 释放

### 4.2 Interneurons (中间)

- 主要 inhibitory (GABA)
- 类型:
  - **PV (Parvalbumin)**: 快 spike, 控制网络 timing
  - **SOM (Somatostatin)**: 调 dendrite
  - **VIP**: disinhibition
- ~20-30% of cortex

### 4.3 Purkinje Neurons

- 小脑特有
- **巨大 dendrite tree** (~200k input)
- 输出 inhibitory (GABA)

### 4.4 Dopamine Neurons (中脑 VTA/SNc)

- ~600k in humans
- 控制 reward / motor
- Parkinson's = SNc 退化

### 4.5 Granule Cells (颗粒细胞)

- 小脑 + 海马齿状回
- 最多类型 (数量最大)

---

## 5. 神经元与 AI 神经网络对比

| 维度 | 生物神经元 | AI neuron |
|---|---|---|
| 输出 | spike (binary, 0-1ms) | continuous (float) |
| 整合 | dendrite 非线性 + soma | 加权求和 |
| 时间 | precise spike timing | 同步 forward pass |
| 学习 | STDP, neuromodulation | Backprop, SGD |
| 数量 | 8×10¹⁰ | GPT-4 ~ 10¹² params |
| 连接 | sparse, local + long | dense matrix |
| 能耗 | 20W brain | 10kW GPU |

→ AI 远比 brain **简化**;brain 至少多 1-2 个数量级复杂。

---

## 6. PyTorch — 简化神经元模型

```python
import torch

class LIFNeuron:
    """Leaky Integrate-and-Fire — 最简 spiking 神经元。"""
    def __init__(self, V_rest=-70, V_th=-55, V_reset=-75, tau=20):
        self.V = V_rest
        self.V_rest, self.V_th, self.V_reset, self.tau = V_rest, V_th, V_reset, tau
    
    def step(self, I_input, dt=0.1):
        # Leaky integration
        dV = (self.V_rest - self.V + I_input) / self.tau * dt
        self.V += dV
        # Spike + reset
        if self.V >= self.V_th:
            self.V = self.V_reset
            return 1  # spike
        return 0

# Simulate
neuron = LIFNeuron()
spikes = []
for t in range(1000):
    I = 12 if 200 < t < 800 else 0  # current pulse
    spikes.append(neuron.step(I))
```


---

## 7. 数量与连接

人脑:
- 86 billion (8.6 × 10¹⁰) neurons (Azevedo 2009)
- 100-500 trillion synapses
- 不同区域密度差大:小脑 80% neurons 但 10% volume
- 总长 axon ~ 165,000 km (绕地球 4 圈)

---

## 8. 与 glia 关系

非神经元细胞 (glia):
- Astrocytes (星形): 营养支持, BBB
- Oligodendrocytes: CNS myelin
- Microglia: 免疫 / 突触 pruning
- Schwann cells: PNS myelin

Glia: neuron ≈ 1:1 (旧观点 10:1 已修正)。

---

## 9. 神经发生 (neurogenesis)

成年大脑仍能产生新神经元:
- Hippocampus dentate gyrus (确认)
- Olfactory bulb (鼠类)
- Cortex (有争议)

→ Stem cell-based therapies 在 Parkinson 等病探索。

---

## 10. Common Pitfalls

### 10.1 "神经元数量决定智能" 误区

人 (8.6 × 10¹⁰) 与 大象 (2.5 × 10¹¹) 大象多。但人 cortex 神经元多 + 高连接。

### 10.2 LIF ≠ real neuron

LIF 是 toy model;real neuron 有 dendritic computation + neuromodulation 等。

### 10.3 Hodgkin-Huxley 仅 axon

HH 在 axon 测;dendrite 动力学不同 (NMDA 等复杂)。

### 10.4 Spike timing 重要

许多 ML 把 firing rate 当 output,忽略 timing。real brain 极依赖 precise timing。

### 10.5 Glia 不只是 "支持"

近 20 年发现 glia 参与 synaptic plasticity / 信号传递。

---

## 11. Related Concepts

- **历史**:[神经科学历史](../00_Foundations/Neuroscience_History.md)
- **AI 对比**:https://jeffliulab.github.io/ai-notes/02_Deep_Learning/01_Intro/MLP/

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Azevedo, F. et al.** "Equal numbers of neuronal and nonneuronal cells make the human brain an isometrically scaled-up primate brain." *J Comp Neurol*, 2009.
3. **Hodgkin, A. L. & Huxley, A. F.** *J Physiol*, 1952.
4. **Cajal, S. R.** *Histology of the Nervous System*. 1909-1911.
5. **Bear, M. F. et al.** *Neuroscience: Exploring the Brain*. 4th ed., 2015.
