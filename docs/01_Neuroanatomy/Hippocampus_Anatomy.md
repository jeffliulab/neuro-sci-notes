# 海马 (Hippocampus) 解剖

> *海马是 temporal lobe 内的 C 形结构,在 episodic memory + spatial navigation 中扮演核心角色。Patient H.M. (1957 bilateral resection) 让 hippocampus 成为 memory 研究焦点。本篇覆盖 anatomy + circuit + 功能。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](Cortex.md)、[神经元](../02_Cellular_Molecular/Neuron.md)

---

## 1. 解剖结构

```
Entorhinal Cortex (EC) — 入口
    ↓ perforant path
Dentate Gyrus (DG) — granule cells
    ↓ mossy fiber
CA3 — large pyramidal, recurrent
    ↓ Schaffer collateral
CA1 — main output to EC
    ↓
Subiculum → 输出
```

3 大区:DG, CA3, CA1。CA = Cornu Ammonis。

---

## 2. Trisynaptic 回路

经典海马 information flow:

1. **EC layer 2 → DG**: perforant path
2. **DG granule cells → CA3**: mossy fibers
3. **CA3 pyramidal → CA1**: Schaffer collaterals + recurrent CA3 collaterals

每个突触都是 LTP 研究的经典位点。

---

## 3. CA3 Recurrent Network

CA3 pyramidal 之间有 ~ 3% 概率相连 → 强烈 recurrent。
功能:
- **Pattern completion**: 部分线索 → 整体回忆 (Marr 1971 假说)
- **Auto-associative memory**: 类似 Hopfield network

---

## 4. CA1

主要"输出" — pyramidal 投射到 subiculum + EC layer 5。
- 与皮层广泛连接
- Place cells 强 active 在 CA1

---

## 5. Place Cells & Grid Cells

### 5.1 Place Cells (O'Keefe 1971)

CA1 / CA3 pyramidal,在空间特定位置 fire (place field)。
**海马是大脑的 GPS**。Nobel 2014.

### 5.2 Grid Cells (Moser 2005)

EC layer 2 (Medial EC),格栅状 firing — 多个 hexagonal place fields。
为 path integration 提供 metric。

### 5.3 Head-direction, Border, Speed cells

EC + presubiculum 还有方向、边界、速度等专门 cell。

---

## 6. 与 Cortex 关系

海马接收**多模态汇集**输入 (经 EC):
- 视觉、听觉、嗅觉、躯体感觉
- 全脑信息高度整合后进入海马
- 海马输出再传回新皮层 (memory consolidation)

---

## 7. 海马在记忆中的角色

### 7.1 Episodic Memory

- 个人经历回忆 (where, when, what)
- 海马必需 (H.M. case)

### 7.2 Memory Consolidation

- 海马是 **临时仓库**
- 数日-数年间 memory 逐渐转移到 cortex
- Sleep 期间海马 replay 加速 consolidation

### 7.3 Spatial Navigation

- 海马 + EC 在 navigation 中协同
- London taxi driver 海马增大 (Maguire 2000)

---

## 8. 病理

- **H.M.** (1957 bilateral surgery): anterograde amnesia (不能形成新长期记忆)
- **Alzheimer**: 海马是早期受损区
- **Temporal lobe epilepsy**: 海马 sclerosis 常见
- **PTSD**: 海马 volume 减少
- **Depression**: 海马 LTP 降低 + neurogenesis 减少

---

## 9. PyTorch — Hopfield Network (CA3 抽象)

```python
import torch

class HopfieldNetwork:
    """CA3-like auto-associative memory."""
    def __init__(self, N=100):
        self.W = torch.zeros(N, N)
        self.N = N
    
    def store(self, patterns):
        # Hebbian rule
        for p in patterns:
            self.W += torch.outer(p, p) / self.N
        self.W.fill_diagonal_(0)
    
    def recall(self, initial, steps=10):
        s = initial.clone()
        for _ in range(steps):
            s = torch.sign(self.W @ s)
        return s

# Pattern completion
net = HopfieldNetwork(N=64)
patterns = [torch.sign(torch.randn(64)) for _ in range(5)]
net.store(patterns)
partial_cue = patterns[0].clone()
partial_cue[20:30] = 0  # noise
recalled = net.recall(partial_cue)  # should reconstruct
```

---

## 10. 测量

- **fMRI**: 海马激活 in memory tasks
- **In vivo recording**: place cells via tetrode / Neuropixels
- **Imaging**: hippocampal volume (MRI) for AD biomarker
- **Optogenetics**: 激活 / 抑制特定 cells (engram)

---

## 11. Common Pitfalls

### 11.1 海马 ≠ 全部 memory

仅 declarative memory; procedural / motor memory 由小脑 / BG。

### 11.2 H.M. 仍能学新 motor skill

证明 memory 多系统。

### 11.3 Place cell 不只是 navigation

也参与时间序列编码、社交关系等。

### 11.4 Hopfield ≠ real CA3

Hopfield 简化太多;real CA3 有 inhibitory interneurons + plasticity + spike timing。

### 11.5 海马大小 ≠ 智力

人海马 cm³ 级,但 dolphin / 大象更大。重要的是 connectivity 与 cortex。

---

## 12. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Cerebellum](Cerebellum.md)、[Basal Ganglia](Basal_Ganglia.md)
- **细胞**:[LTP/LTD](../02_Cellular_Molecular/LTP_LTD.md)
- **AI 对比**:Hopfield → https://jeffliulab.github.io/ai-notes/01_AI/03_Frontiers/03_World_Models/RSSM_PlaNet/

---

## References

1. **Scoville, W. B. & Milner, B.** "Loss of recent memory after bilateral hippocampal lesions (H.M.)." *J Neurol Neurosurg Psychiatry*, 1957.
2. **O'Keefe, J. & Dostrovsky, J.** "The hippocampus as a spatial map." *Brain Res*, 1971.
3. **Hafting, T. et al.** "Microstructure of a spatial map in the entorhinal cortex (Grid cells)." *Nature*, 2005.
4. **Marr, D.** "Simple memory: a theory for archicortex." *Phil Trans R Soc*, 1971.
5. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
