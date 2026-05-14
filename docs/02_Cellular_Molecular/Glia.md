# 神经胶质 (Glia) — 大脑的"支持细胞"远不止支持

> *Glia 长期被认为只是 neuron 的"胶水",但 1990s 起发现 glia 主动参与 synapse / 免疫 / 代谢 / plasticity。脑内 glia : neuron ≈ 1:1。本篇覆盖 4 大 glia 类型 + 功能 + 与疾病。*
>
> **难度**:Intermediate
> **前置知识**:[神经元](Neuron.md)、[突触](Synapse.md)

---

## 1. 四大 glia 类型

### 1.1 Astrocytes (星形胶质)

最多;星形;包绕 neurons + 血管:
- 维持 K+, Glu 浓度
- 形成 **Blood-Brain Barrier (BBB)**
- 释放 ATP, glutamate ("gliotransmission")
- 参与 LTP

### 1.2 Oligodendrocytes (少突)

CNS 中产生 **myelin** — 一个 oligo 包多 axon。
- MS (multiple sclerosis) 攻击 oligo
- 髓鞘加速信号 10-100×

### 1.3 Microglia (小胶质)

CNS 免疫细胞:
- 巡逻 + 吞噬 debris
- 参与 synaptic pruning (发育)
- Alzheimer / Parkinson 中过度激活 → 炎症

### 1.4 Schwann Cells

PNS 髓鞘 + 神经修复。
1 Schwann 包 1 axon (vs Oligo 1 包多)。

---

## 2. Astrocyte 详细功能

### 2.1 Tripartite Synapse

经典 synapse = pre + post,新发现 **astrocyte 包绕** 形成三方:

```
[Pre]───glutamate───[Post]
   \                /
    \─── astrocyte ─┘ (uptake Glu, release ATP)
```

### 2.2 BBB

Astrocyte end-foot 包绕脑血管 + tight junction → 阻止大分子 / 药物进入脑。
治疗药物设计常需越过 BBB。

### 2.3 K+ siphoning

Astrocyte 吸收 spike 后高 K+,经 gap junction 网络扩散 → 维稳。

### 2.4 Glymphatic system

睡眠时 CSF 通过 astrocyte aquaporin 4 流过脑组织,清除代谢废物 (β-amyloid 等)。

---

## 3. Microglia 详细

### 3.1 监视

正常 ramified 形态 → 长 process 巡逻。
受损 → ameboid 激活态 → 吞噬。

### 3.2 Synaptic pruning

发育期间 microglia 吞掉弱 synapse → 提升神经回路效率。
异常 → 自闭症假说。

### 3.3 Neuroinflammation

过度激活释放 TNF-α, IL-6 等 → 慢性炎症 → 神经退行 (AD, PD, ALS)。

---

## 4. 数量

- Glia : neuron = 1:1 (旧观点 10:1 已被纠正,Azevedo 2009)
- 不同脑区差大:
  - Cerebellum: 4:1 (neuron 多)
  - Cortex: 3:1 (glia 多)
- White matter 多 oligo;gray matter 多 astrocytes + microglia

---

## 5. Glia 与疾病

| 疾病 | Glia 角色 |
|---|---|
| Multiple Sclerosis | Oligo 受免疫攻击 → demyelination |
| ALS | Astrocyte 失功能, microglia 炎症 |
| Alzheimer | Microglia 不能清 amyloid + 炎症 |
| Parkinson | α-synuclein 触发 microglia |
| Glioma | Glia 癌变 (最常见脑瘤) |
| Charcot-Marie-Tooth | Schwann cell 病 |
| Autism | Microglia synaptic pruning 异常 |

---

## 6. 现代研究

### 6.1 Single-cell RNA-seq

发现 ~ 30 astrocyte subtypes,
~ 20 microglia states。

### 6.2 Optogenetic glia

ChR2 在 astrocyte → 光控 gliotransmission → 影响 behavior。

### 6.3 iPSC-derived glia

用 stem cell 分化 astrocyte 测人类病变。

### 6.4 Reactive astrogliosis 检测

GFAP biomarker — 中风 / 创伤后 astrocyte 反应。

---

## 7. PyTorch — Tripartite Synapse 概念模型

```python
import torch

class TripartiteSynapse:
    def __init__(self, w=0.5, glia_capacity=10):
        self.w = w
        self.glu = 0  # synaptic Glu
        self.glia_glu = 0  # astrocyte 吸收
        self.capacity = glia_capacity
    
    def step(self, pre_spike, post_state, dt=1):
        if pre_spike:
            self.glu += 1.0
        # Astrocyte uptake
        uptake = min(self.glu * 0.5, self.capacity)
        self.glu -= uptake * dt
        self.glia_glu += uptake * dt
        # Astrocyte slow ATP release back (gliotransmission)
        atp_release = self.glia_glu * 0.01
        self.glia_glu *= (1 - 0.01 * dt)
        # Effect on post
        epsp = self.w * self.glu + 0.1 * atp_release
        return epsp
```

---

## 8. 实验工具

- **GFAP staining**: astrocyte marker
- **Iba1 staining**: microglia marker
- **MBP staining**: oligo / myelin
- **Calcium imaging**: astrocyte 也有 Ca²⁺ 信号

---

## 9. 历史

- **1858** — Rudolf Virchow 命名 "neuroglia" ("神经胶水")
- **1890s** — Ramón y Cajal 分类 neuron vs glia
- **1990s** — gliotransmission 概念
- **2000s** — synaptic pruning 发现
- **2010s** — glymphatic system (Nedergaard 2012)
- **2020s** — single-cell glia diversity

---

## 10. Common Pitfalls

### 10.1 Glia 不是 passive

50 年来低估 glia,现代研究证明主动参与。

### 10.2 一种 astrocyte 错觉

至少 30 亚型,功能差异大。

### 10.3 Microglia 神经科学的盲点

CNS 大多数研究专注 neuron,microglia 影响被低估。

### 10.4 BBB 复杂

Astrocyte 是 BBB 一部分,但 endothelial cell + pericyte 也是。

### 10.5 In vitro astrocyte ≠ in vivo

培养的 astrocyte morphology + 基因表达 与脑内大不同。

---

## 11. Related Concepts

- **同节**:[神经元](Neuron.md)、[突触](Synapse.md)、[神经递质](Neurotransmitters.md)

---

## References

1. **Verkhratsky, A. & Butt, A.** *Glial Physiology and Pathophysiology*. 2nd ed., 2013.
2. **Allen, N. J. & Lyons, D. A.** "Glia as architects of central nervous system formation and function." *Science*, 2018.
3. **Nedergaard, M.** "Garbage truck of the brain (glymphatic system)." *Science*, 2013.
4. **Azevedo, F. et al.** *J Comp Neurol*, 2009.
5. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
