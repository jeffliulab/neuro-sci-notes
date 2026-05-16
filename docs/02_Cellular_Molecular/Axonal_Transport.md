# 轴浆运输 (Axonal Transport)

> *神经元极化:蛋白在胞体合成,轴突可长达 1 m → 需主动运输。马达蛋白:kinesin(顺向,+ 端 → 末端)、dynein(逆向 → 胞体),沿微管轨道。快(囊泡 ~400 mm/d)vs 慢(细胞骨架 ~1 mm/d)。运输障碍 → ALS/AD/HD("transportopathy")。*
>
> **难度**:Intermediate
> **前置知识**:[Neuron](Neuron.md)、[Synaptic_Vesicle_Cycle](Synaptic_Vesicle_Cycle.md)

---

## 1. 为何需要

- 蛋白合成主要在胞体(核糖体);轴突可达 1 m(运动神经元)
- 末端突触需:囊泡蛋白、离子通道、线粒体、mRNA
- 被动扩散太慢(1 m 需数十年)→ **主动马达运输**

---

## 2. 马达 + 轨道

| 马达 | 方向 | 货物 |
|---|---|---|
| **Kinesin** | 顺向(anterograde,+ 端→末端) | 囊泡、线粒体、mRNA |
| **Dynein** | 逆向(retrograde,→胞体) | 信号内体(NGF)、自噬体、降解物 |
| 轨道 | **微管**(极性:+ 端朝轴突末端) | — |
| Myosin | 短程,actin(突触/树突) | 局部递送 |

---

## 3. 快 vs 慢运输

| | 快(fast) | 慢(slow) |
|---|---|---|
| 速度 | ~ 50-400 mm/day | ~ 0.1-3 mm/day |
| 货物 | 膜性细胞器、囊泡 | 细胞骨架(tubulin/NF)、可溶酶 |
| 机制 | 持续马达 | "stop-and-go"(间断快移)|

---

## 4. PyTorch — 双向运输 + 净流

```python
import torch

def axonal_transport(T=1000, p_antero=0.6, step=1.0):
    """Stochastic bidirectional motor stepping; net anterograde flux."""
    pos = 0.0
    traj = []
    for _ in range(T):
        if torch.rand(1) < p_antero:
            pos += step          # kinesin (anterograde)
        else:
            pos -= step          # dynein (retrograde)
        pos = max(pos, 0.0)
        traj.append(pos)
    return traj   # biased random walk → net delivery to terminal
```

---

## 5. 逆向信号

- 末端摄取 NGF/BDNF → "signaling endosome" → dynein 逆运 → 核(存活/转录)
- 损伤信号逆运 → 再生反应
- 病毒(狂犬、HSV、脊灰)劫持逆向运输入 CNS
- 见 [Neurotrophins](Neurotrophins.md)

---

## 6. 线粒体运输

- 线粒体沿轴突双向移动 → 按能量需求定位(突触/Ranvier 节)
- Milton/Miro 衔接 + Ca²⁺ 调停靠(高活动处"锚定")
- 失调 → 能量危机(见 [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md))

---

## 7. 局部翻译

- mRNA + 核糖体运至树突/轴突 → 局部翻译(突触特异)
- 支持突触特异可塑(LTP 晚期需局部蛋白合成)
- "Synaptic tag and capture"机制相关(见 [LTP_LTD](LTP_LTD.md))

---

## 8. 病理("Transportopathy")

- **ALS**:轴突运输缺陷早期(SOD1/TDP-43/dynein 突变,见 [ALS](../08_Neuro_Disorders/ALS.md))
- **AD**:tau 过磷酸化 → 微管不稳 → 运输障碍 → 轴突变性
- **HD**:mutant huntingtin 干扰 dynein/kinesin
- **CMT2**:kinesin/dynein/MFN2 突变(周围神经)
- 远端轴突先变("dying-back")— 长轴突最脆弱

---

## 9. 与 AI / 工程

- 双向马达 + 微管 ↔ 主动物流 / 调度(资源分配到远端)
- 逆向信号 ↔ 长程信用回传(类比;非 backprop)
- 局部翻译 ↔ 边缘计算 / 局部更新
- 能量定位 ↔ 资源约束优化(见 [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md))

---

## 10. Common Pitfalls

### 10.1 蛋白扩散到末端即可

1 m 扩散需数十年;必须主动马达运输。

### 10.2 Kinesin/dynein 单向纯粹

货物常双向"拉锯"(net flux 偏向);可调。

### 10.3 慢运输 = 不重要

细胞骨架/酶慢运输维持轴突结构;关键。

### 10.4 运输与疾病无关

ALS/AD/HD 早期即运输缺陷("dying-back")。

### 10.5 微管是被动轨道

动态(生长/缩)+ tau 等调控 + 极性决定方向。

---

## 11. Related Concepts

- **同节**:[Neuron](Neuron.md)、[Synaptic_Vesicle_Cycle](Synaptic_Vesicle_Cycle.md)、[Neurotrophins](Neurotrophins.md)、[LTP_LTD](LTP_LTD.md)
- **基础**:[Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md)
- **疾病**:[ALS](../08_Neuro_Disorders/ALS.md)、[Alzheimer](../08_Neuro_Disorders/Alzheimer.md)

---

## References

1. **Hirokawa, N. et al.** "Kinesin superfamily motor proteins and intracellular transport." *Nat Rev Mol Cell Biol*, 2009.
2. **Maday, S. et al.** "Axonal transport: cargo-specific mechanisms of motility and regulation." *Neuron*, 2014.
3. **Millecamps, S. & Julien, J.-P.** "Axonal transport deficits and neurodegenerative diseases." *Nat Rev Neurosci*, 2013.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
