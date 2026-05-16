# 髓鞘形成 (Myelination)

> *髓鞘是少突胶质(CNS)/施万细胞(PNS)缠绕轴突的脂质绝缘层 → 跳跃式传导(saltatory),传速 ↑ 50× + 能耗 ↓。Ranvier 节富集 Nav。活动依赖髓鞘化(adaptive myelination)是学习新机制。脱髓鞘 → MS。是 [Action_Potential](Action_Potential.md) 速度的关键。*
>
> **难度**:Intermediate
> **前置知识**:[Action_Potential](Action_Potential.md)、[Glia](Glia.md)

---

## 1. 谁形成髓鞘

| | CNS | PNS |
|---|---|---|
| 细胞 | 少突胶质细胞 | 施万细胞 |
| 比例 | 1 细胞 → 多轴突(~50) | 1 细胞 → 1 节段 |
| 再生 | 差(OPC 有限) | 较好 |
| 病 | MS、白质病 | GBS、CMT |

---

## 2. 跳跃式传导

- 髓鞘段绝缘 + 低电容 → AP 不在此再生
- **Ranvier 节**(无髓,~1 μm):Nav 高密度 → AP "跳"节再生
- 传速 ∝ 直径(有髓,线性)vs ∝ √直径(无髓)
- ~ 50-120 m/s(有髓)vs 0.5-2 m/s(无髓)

---

## 3. 能量 + 空间优势

- AP 仅在节点再生 → Na⁺/K⁺ 泵耗能大降
- 同传速下髓鞘轴突远细于无髓 → 省空间
- 演化:大脑布线 + 能量约束的解(见 [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md))

---

## 4. PyTorch — 有髓 vs 无髓传速

```python
import torch

def conduction_velocity(diameter, myelinated=True):
    """Myelinated: v ∝ d (linear). Unmyelinated: v ∝ sqrt(d)."""
    if myelinated:
        return 6.0 * diameter            # ~6 m/s per μm (Hursh)
    return 1.0 * torch.sqrt(torch.tensor(float(diameter)))

# Same velocity: myelinated axon far thinner -> wiring economy
```

---

## 5. 发育时程

- 出生后持续至 ~ 25-30 岁
- **后→前**:感觉/运动早,**PFC 最晚** → 解释青少年决策不成熟(见 [Cognitive_Development](../04_Cognitive_Neuroscience/Cognitive_Development.md))
- 关键期 + 经验影响

---

## 6. Adaptive Myelination(活动依赖)

- 神经活动 → OPC 增殖/分化 → 经验依赖髓鞘化
- 学习(钢琴/记忆任务)→ 白质改变(DTI 可测,见 [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.md))
- 调节传导**时序**(同步到达 → 振荡/绑定)
- 髓鞘 = 新可塑性维度(非仅"绝缘")

---

## 7. 节点分子结构

- **Node**:Nav1.6 簇集 + ankyrin-G
- **Paranode**:轴-胶 连接(Caspr/contactin)
- **Juxtaparanode**:Kv1 钾通道
- 结构破坏(自身免疫/基因)→ 传导失败

---

## 8. 病理

- **MS**:CNS 脱髓鞘(自免,见 [Multiple_Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.md))
- **Guillain-Barré**:PNS 急性脱髓鞘(自免)
- **CMT(腓骨肌萎缩)**:遗传性周围神经髓鞘病
- **白质营养不良(leukodystrophy)**:遗传髓鞘代谢病
- **早产**:髓鞘化未成熟 → 脑瘫风险
- 再髓鞘化(remyelination)= MS 治疗新方向

---

## 9. 与 AI / 工程

- 髓鞘 = 绝缘 + 中继 ↔ 信号传输线 + repeater(工程类比)
- 传导延迟可调(adaptive myelin)↔ 可学习延迟 / 时序编码
- 能量/空间最优 ↔ 硬件布线约束
- 见 [Action_Potential](Action_Potential.md)、[Neural Coding](../00_Foundations/Neural_Coding.md)(时序)

---

## 10. Common Pitfalls

### 10.1 髓鞘只是"绝缘胶布"

是活细胞 + 代谢支持 + 活动依赖可塑(adaptive myelination)。

### 10.2 跳跃 = AP 在节间跳过空间

AP 在节点**再生**;节间被动快速电扩布(非真"跳过")。

### 10.3 无髓 = 原始无用

无髓 C 纤维(痛/自主)功能必需;非劣化。

### 10.4 髓鞘化童年完成

PFC 持续至 ~25-30 岁。

### 10.5 髓鞘不影响计算

调时序 → 影响同步/振荡/绑定(计算相关)。

---

## 11. Related Concepts

- **同节**:[Action_Potential](Action_Potential.md)、[Glia](Glia.md)、[Ion_Channels](Ion_Channels.md)
- **基础**:[Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md)、[Neurodevelopment](../00_Foundations/Neurodevelopment.md)
- **疾病**:[Multiple_Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.md)
- **前沿**:[DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.md)

---

## References

1. **Hartline, D. K. & Colman, D. R.** "Rapid conduction and the evolution of giant axons and myelinated fibers." *Curr Biol*, 2007.
2. **Nave, K.-A. & Werner, H. B.** "Myelination of the nervous system: mechanisms and functions." *Annu Rev Cell Dev Biol*, 2014.
3. **Fields, R. D.** "A new mechanism of nervous system plasticity: activity-dependent myelination." *Nat Rev Neurosci*, 2015.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
