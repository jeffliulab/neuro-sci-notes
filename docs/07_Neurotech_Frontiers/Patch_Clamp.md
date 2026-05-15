# 膜片钳 (Patch Clamp)

> *Patch clamp(Neher & Sakmann,1991 Nobel)是单细胞电生理金标准:玻璃微吸管 gigaohm 封接膜片,记录单离子通道或全细胞电流/电压。空间精度无与伦比(单通道 pA)。现代:自动膜片钳、in vivo patch、Patch-seq(电生理 + 转录组)。是 [Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.md)、通道药理学根基。*
>
> **难度**:Advanced
> **前置知识**:[Ion Channels](../02_Cellular_Molecular/Ion_Channels.md)、[Action Potential](../02_Cellular_Molecular/Action_Potential.md)

---

## 1. 原理

- 抛光玻璃微吸管(尖端 ~ 1 μm)轻压细胞膜 + 负压 → **gigaohm seal**(GΩ 封接)
- 高阻封接 → 极低噪声 → 可测 pA 级单通道电流
- 钳制(clamp):固定电压测电流,或固定电流测电压

---

## 2. 四种构型

| 构型 | 接触 | 用途 |
|---|---|---|
| **Cell-attached** | 膜完整 | 单通道(原位) |
| **Whole-cell** | 破膜 | 全细胞电流 / AP / 突触 |
| **Inside-out** | 膜内面朝外 | 胞内调控单通道 |
| **Outside-out** | 膜外面朝外 | 配体门控单通道 |
| **Perforated** | 抗生素打孔 | 全细胞但保胞内成分 |

---

## 3. 电压钳 vs 电流钳

- **Voltage clamp**:固定 V,测 I → 解析离子电流(HH 实验基础!)
- **Current clamp**:注入 I,测 V → 观察 AP、firing pattern
- Dynamic clamp:实时计算注入"虚拟电导"(测试假说)

---

## 4. PyTorch — 单通道电流(随机门控)

```python
import torch

def single_channel_current(T=1000, p_open=0.3, i_single=2.0, tau=10.0):
    """Stochastic 2-state channel: pA-level openings (cell-attached-like)."""
    state = 0
    trace = []
    for t in range(T):
        if state == 0 and torch.rand(1) < p_open / tau:
            state = 1
        elif state == 1 and torch.rand(1) < (1 - p_open) / tau:
            state = 0
        trace.append(state * i_single)   # discrete pA steps
    return torch.tensor(trace)   # ensemble average -> macroscopic I
```

---

## 5. 历史 — HH 与单通道

- Hodgkin-Huxley 用 voltage clamp(乌贼巨轴突)推断离子电导(1952)
- 但未见单通道 — Neher & Sakmann 1976 patch clamp **直接看到**单通道开关
- 证实 HH 的 m³h、n⁴ 是统计平均
- 见 [Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.md)

---

## 6. 现代进展

- **自动膜片钳**:机器人 + 多通道(药筛高通量)
- **In vivo patch**:活体麻醉 / 清醒动物记录单细胞(blind / 2-photon targeted)
- **Patch-seq**:同一 neuron 电生理 + 形态 + 单细胞转录组 → 多模态细胞分型
- **Dynamic clamp**:注入计算电导测试环路假说

---

## 7. 与其它电生理对比

| | Patch clamp | 细胞外记录 | Ca imaging |
|---|---|---|---|
| 信号 | 膜电流/电压(直接) | 场电位 spike | Ca 间接 |
| 精度 | 单通道 pA | 单 neuron spike | 慢 |
| 数量 | 1（少）| 100s（Neuropixels） | 1000s |
| 突触电流 | ✓（唯一） | ✗ | ✗ |
| 通量 | 低 | 中 | 高 |

→ Patch clamp 不可替代:**亚细胞 + 突触电流**精度。

---

## 8. 应用

- 离子通道药理(药物筛选 — 自动膜片钳)
- 突触传递机制(EPSC/IPSC、量子分析)
- 细胞内在兴奋性(firing type 分类)
- 通道病(channelopathy)研究
- Patch-seq 细胞类型图谱(Allen / BICCN)

---

## 9. 局限

- 极低通量(1 细胞 / 长时间,需高技巧)
- 侵入(破膜 dialysis 改变胞内)
- 难长期 / 难自由行为
- 难达深部 / 特定细胞型(需 targeted)

---

## 10. Common Pitfalls

### 10.1 通量高

极低通量(技术 + 时间密集);自动化部分缓解。

### 10.2 Whole-cell 不扰动

破膜 → 胞内 dialysis 改变成分;perforated patch 缓解。

### 10.3 单通道 = 确定性

随机门控;宏观电流是统计平均(HH m/h/n)。

### 10.4 = Neuropixels 类

互补:patch 单细胞亚细胞精度,Neuropixels 大规模群体。

### 10.5 In vivo 不可能

In vivo / awake patch + Patch-seq 已实现(高难)。

---

## 11. Related Concepts

- **同节**:[Calcium Imaging](Calcium_Imaging.md)、[Neuropixels](Neuropixels.md)、[Optogenetics_Advanced](Optogenetics_Advanced.md)
- **细胞**:[Ion Channels](../02_Cellular_Molecular/Ion_Channels.md)、[Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.md)、[Synapse](../02_Cellular_Molecular/Synapse.md)
- **基础**:[Research Methods](../00_Foundations/Research_Methods.md)

---

## References

1. **Neher, E. & Sakmann, B.** "Single-channel currents recorded from membrane of denervated frog muscle fibres." *Nature*, 1976.
2. **Hamill, O. P. et al.** "Improved patch-clamp techniques for high-resolution current recording." *Pflügers Arch*, 1981.
3. **Cadwell, C. R. et al.** "Electrophysiological, transcriptomic and morphologic profiling of single neurons (Patch-seq)." *Nat Biotechnol*, 2016.
4. **Hodgkin, A. L. & Huxley, A. F.** "A quantitative description of membrane current." *J Physiol*, 1952.
