# 丘脑 (Thalamus)

> *Thalamus 是 cortex 的"gateway",几乎所有 sensory 信号(嗅觉除外)都经此中转。位于 diencephalon。约 50 个 nuclei,分:relay、association、intralaminar、reticular。LGN(视觉)、MGN(听觉)、VPL(体感)、VPM(脸)、MD(PFC interface)。损伤 → 复杂综合症。Thalamocortical loop 是 consciousness 假设关键。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](Cortex.md)、[Brainstem](Brainstem.md)

---

## 1. 位置

- 中央深部
- 第三脑室两侧
- 上接 cortex,下接 brainstem
- 形似"两个鸡蛋"

---

## 2. 核团分类

### 2.1 Specific relay

- 感觉 + 运动接力
- 1 对 1 项目
- 大部分 cortex 区有对应 thalamic nucleus

### 2.2 Association

- 与 PFC、parietal 高级区交互
- 多对多

### 2.3 Intralaminar

- Arousal、attention
- 与 reticular 系统连接

### 2.4 Reticular

- 包外壳
- GABAergic,inhibitory
- 调节 thalamocortical 振荡

---

## 3. 主要核团 (memorize 关键)

| 核 | 输入 | 输出 |
|---|---|---|
| **LGN** | Retina | V1 |
| **MGN** | Inferior colliculus | A1 |
| **VPL** | DCML, spinothalamic | S1 |
| **VPM** | Trigeminal | S1 (face) |
| **VL / VA** | Cerebellum, BG | M1 |
| **MD** | Limbic | PFC |
| **Pulvinar** | Multi-modal | Parietal, temporal |
| **Anterior** | Mammillary body | Cingulate |

---

## 4. Thalamocortical Loop

```
Cortex (layer 6) ──→ Thalamus
       ↑                ↓
       └────────────────┘
       (layer 4)
```

- Reciprocal 连接
- 振荡:alpha、sleep spindles、delta
- Spindle 在 sleep memory consolidation 关键

---

## 5. Sleep Spindle

- NREM sleep 期 11-15 Hz 短爆发
- Reticular thalamic nucleus 产生
- 与 memory consolidation 关联
- Schizophrenia 中 spindle 减少

---

## 6. PyTorch — Thalamocortical Oscillation

```python
import torch

def thalamocortical_oscillator(T=1000, dt=0.5):
    """Wilson-Cowan-like loop."""
    E = 0.0  # cortex excitatory
    I = 0.0  # thalamic inhibitory
    history = []
    for _ in range(T):
        dE = (-E + 1.0 / (1 + torch.exp(torch.tensor(-(E - I + 0.5)))) ) * dt / 10
        dI = (-I + 1.0 / (1 + torch.exp(torch.tensor(-(E - 0.3)))) ) * dt / 10
        E += dE
        I += dI
        history.append((E.item(), I.item()))
    return history
```

---

## 7. 损伤

- **Thalamic stroke**: ipsilateral hemiparesis、hemisensory loss、cognitive
- **Thalamic pain syndrome (Dejerine-Roussy)**: 中风后慢性中央痛
- **Fatal familial insomnia (FFI)**: PRNP 突变 → 双侧 anterior thalamus 退化 → 完全 insomnia + 死
- **Korsakoff syndrome**: thiamine 缺 → 中央前 thalamus 损 → confabulation + 健忘
- **Coma**: 双侧 thalamus 损 → 意识 loss

---

## 8. Thalamus + Consciousness

- Bilateral lesion → coma 表明 thalamus 必要 conscious
- Crick & Koch:thalamocortical loop 是 NCC
- Llinás 2002:40 Hz thalamocortical 是 consciousness 基底

---

## 9. AI Connection

- **Attention** with relay (like Transformer):有人类比 thalamus
- **Memory consolidation** during sleep 类似 RNN spindle replay
- Modern AI 没明显 thalamus analog,but architecture inspiration 存在

---

## 10. Common Pitfalls

### 10.1 Thalamus = sensory relay

不;还有 PFC interface (MD)、cerebellar relay (VL),attention、consciousness。

### 10.2 Olfaction 经 thalamus

错;嗅觉直入 piriform。

### 10.3 LGN = passive relay

实际:gain control、attention modulation。

### 10.4 Thalamus 100 区是 module

部分 functionally overlapping;不全部 modular。

### 10.5 Coma 单独 thalamus

通常 thalamus + brainstem RAS combined。

---

## 11. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Hippocampus_Anatomy](Hippocampus_Anatomy.md)、[Brainstem](Brainstem.md)、[Basal_Ganglia](Basal_Ganglia.md)、[Cerebellum](Cerebellum.md)
- **系统**:[Visual System](../03_Systems_Neuroscience/Visual_System.md)、[Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md)
- **认知**:[Consciousness](../04_Cognitive_Neuroscience/Consciousness.md)

---

## References

1. **Sherman, S. M. & Guillery, R. W.** *Functional Organization of Thalamocortical Relays*. 1996.
2. **Llinás, R. R. & Steriade, M.** "Bursting of thalamic neurons and states of vigilance." *J Neurophysiol*, 2006.
3. **Crick, F. C. & Koch, C.** "What is the function of the claustrum?" *Phil Trans R Soc B*, 2005.
4. **Halassa, M. M. & Kastner, S.** "Thalamic functions in distributed cognitive control." *Nat Neurosci*, 2017.
