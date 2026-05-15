# 体感系统 (Somatosensory System)

> *Somatosensory system 处理皮肤 touch、proprioception、temperature、pain。Skin → DRG → spinal cord → thalamus VPL → S1 cortex 是 hierarchy。Penfield homunculus 表明 S1 有空间映射。Discriminative touch 与 pain 分两 pathway。Robotic tactile sensor 试模拟此 system。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)、[Motor System](Motor_System.md)

---

## 1. 5 大 submodality

- **Discriminative touch** (light touch, vibration, pressure)
- **Proprioception** (joint position, muscle stretch)
- **Temperature** (warm, cold)
- **Pain** (nociception)
- **Itch**

---

## 2. 皮肤 receptor

| Receptor | 类型 | 适应 |
|---|---|---|
| Meissner corpuscle | 触觉 (轻) | Fast adapting (FA) |
| Pacinian corpuscle | 振动 (高频) | FA |
| Merkel cell | 静压 / 形状 | Slow adapting (SA) |
| Ruffini ending | 拉伸 / 滑动 | SA |
| Free nerve endings | 痛 / 温度 | Variable |

---

## 3. Pathway — Discriminative Touch (DCML)

```
Skin → DRG → ipsilateral 上行
              (Dorsal Column)
              ↓
         Medulla (gracile, cuneate)
              ↓ 交叉 (decussation)
              ↓
         Thalamus VPL
              ↓
         S1 cortex (postcentral gyrus)
```

DCML = Dorsal Column - Medial Lemniscus。

---

## 4. Pathway — Pain (Spinothalamic)

```
Skin → DRG → spinal cord
              ↓ 立即交叉 (anterior commissure)
              ↓ ascending lateral column
              ↓
         Thalamus VPL + intralaminar
              ↓
         S1 + ACC + insula
```

→ 与 DCML 不同点:立即交叉、慢、含 emotional component(ACC)。

---

## 5. S1 Cortex

- Brodmann area 3a, 3b, 1, 2 (postcentral gyrus)
- 4 个 stripe each specialize:
  - 3a: proprioception
  - 3b: cutaneous touch
  - 1: texture
  - 2: shape / grip
- 与 motor cortex (M1, area 4) 相邻

---

## 6. Penfield Homunculus

- 1937 Penfield 颅内 mapping
- Body parts proportional to sensory innervation:
  - 嘴唇、手 huge
  - 躯干 small
- Mirror in M1 (motor homunculus)

---

## 7. Receptor Types — Touch + Pain

| 纤维 | 直径 | 速度 | 模态 |
|---|---|---|---|
| **Aα (Ia)** | 大 | 70-120 m/s | 肌梭、本体感 |
| **Aβ** | 中大 | 30-70 m/s | 触觉 |
| **Aδ** | 小 | 5-30 m/s | sharp 痛 / 冷 |
| **C** | 极小,无髓 | 0.5-2 m/s | dull 痛 / 温 / 痒 |

---

## 8. Gate Control Theory (Melzack & Wall 1965)

- Spinal cord 后角 "gate"
- Aβ (touch) → 关 gate → 抑 pain
- C (pain) → 开 gate
- 解释 揉痛处减痛
- TENS、acupuncture 部分依据

---

## 9. PyTorch — Somatosensory Skin Sim

```python
import torch

class TactileSensor(torch.nn.Module):
    """Simulated skin patch with FA + SA receptors."""
    def __init__(self, grid_size=10):
        super().__init__()
        self.grid_size = grid_size
        # FA: time-derivative response
        # SA: sustained response
    
    def forward(self, pressure, dt=0.01):
        # pressure: (T, H, W)
        SA = pressure  # sustained
        FA = torch.diff(pressure, dim=0) / dt  # derivative
        FA = torch.cat([torch.zeros_like(pressure[:1]), FA])
        return SA, FA  # combined response
```

---

## 10. 病理

- **Phantom limb**: 截肢后仍感肢体(cortical re-mapping)
- **Allodynia**: 轻 touch 引剧痛
- **Hyperalgesia**: 痛敏化
- **Tabes dorsalis**: 梅毒晚 → DC degeneration → 共济失调
- **Brown-Séquard**: 半 spinal cord cut → 同侧 DCML 失,对侧 pain 失
- **Capsaicin desensitization**: chili 减痛
- **Diabetic neuropathy**: small fiber 病

---

## 11. Robotic / AI Connection

- **e-skin**: 模拟 多 modality receptor
- **GelSight** / DIGIT: 视觉 触觉(光学 + 弹胶)
- **Tactile RL**: blind grasp 训练
- **Optimus、Figure**: 手指 force sensor

---

## 12. Common Pitfalls

### 12.1 Pain = nociception

不;pain 含 emotional + cognitive layer。

### 12.2 Homunculus 静态

Brain plasticity → 截肢后地图 reorganize。

### 12.3 C fiber 是 pain only

C 含 itch、温度、tickle 等。

### 12.4 Gate control 简化

Melzack 自己后来 expanded 为 neuromatrix theory。

### 12.5 Tactile = touch only

Touch + proprioception + pain + temperature all different。

---

## 13. Related Concepts

- **同节**:[Visual System](Visual_System.md)、[Motor System](Motor_System.md)、[Auditory System](Auditory_System.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)
- **AI**: Tactile sensing

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Penfield, W. & Boldrey, E.** "Somatic motor and sensory representation in the cerebral cortex of man." *Brain*, 1937.
3. **Melzack, R. & Wall, P.** "Pain mechanisms: A new theory." *Science*, 1965.
4. **Johansson, R. S. & Flanagan, J. R.** "Coding and use of tactile signals from the fingertips." *Nat Rev Neurosci*, 2009.
