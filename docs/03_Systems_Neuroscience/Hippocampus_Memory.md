# 海马与记忆系统

> *海马是 declarative memory (episodic + semantic) 的核心。患者 H.M. 1957 海马切除后失去新长期记忆形成能力,启动了记忆系统神经科学。本篇覆盖:多 memory 系统、海马 consolidation、replay、与现代 AI 关系。*
>
> **难度**:Intermediate
> **前置知识**:[海马解剖](../01_Neuroanatomy/Hippocampus_Anatomy.md)、[LTP/LTD](../02_Cellular_Molecular/LTP_LTD.md)

---

## 1. 记忆系统分类

```
Memory
├── Declarative (explicit) ─ 海马 + medial temporal lobe
│   ├── Episodic (个人经历)
│   └── Semantic (概念知识)
└── Non-declarative (implicit) ─ 其他脑区
    ├── Procedural (技能) ─ BG + 小脑
    ├── Priming ─ neocortex
    └── Conditioning ─ amygdala / cerebellum
```

H.M. 失去 declarative 但 procedural / motor learning 保留 → 多系统证据。

---

## 2. 海马在 episodic memory 中的角色

### 2.1 编码 (Encoding)

事件 → cortex 多模态 input → EC → DG → CA3 → CA1 → output。
LTP 在多个 synapse 上同时发生 → 形成 episode trace。

### 2.2 储存 (Storage)

CA3 recurrent → auto-associative memory:
- Pattern separation (DG): 相似 episode 区分
- Pattern completion (CA3): 部分线索 → 全 episode

### 2.3 检索 (Retrieval)

线索 → 部分 EC input → CA3 reactivate → CA1 → cortex 重建。

---

## 3. Consolidation (巩固)

### 3.1 时间维度

- **Short-term**: 秒-分钟,海马 active
- **Long-term**: 数月-数年,**逐渐转移到 cortex**
- **Remote memory**: 几年后,海马可能不再必需

### 3.2 Standard Consolidation Theory

Squire 1990s: 海马是临时存储,信息逐步转 cortex。

### 3.3 Multiple Trace Theory

Nadel & Moscovitch:某些 episodic memory 始终需海马 (尤其 vivid 细节)。

### 3.4 睡眠 replay

REM + slow-wave sleep:
- Place cell 序列在睡眠中 replay (Wilson & McNaughton 1994)
- 与白天行为相同序列,加速 consolidation
- 是 "AI replay buffer" 的生物基础

---

## 4. Memory Engrams

Tonegawa 2015+:用 optogenetics 标记 + 激活 specific memory:
- Fear conditioning → 在 DG 标 engram cell
- 后期光激活同 engram → 重现 fear behavior (无 stimulus)
- 证明 memory 有 **specific cellular substrate**

---

## 5. 空间记忆

### 5.1 Place Cells (CA1/CA3)

- 在特定空间位置 fire
- O'Keefe 1971 发现
- 一只小鼠跑 maze,海马 ~ 50% pyramidal 是 place cells

### 5.2 Grid Cells (EC)

- Moser 2005
- 六角形 grid pattern
- 提供 metric for path integration

### 5.3 London Taxi Driver

Maguire 2000: 出租车司机海马 posterior 增大 → 训练塑造海马。

---

## 6. 海马 vs AI 记忆

### 6.1 Episodic Memory in AI

- DNC (Differentiable Neural Computer, Graves 2016): explicit memory bank
- Memory-augmented Transformer
- RAG (Retrieval-Augmented Generation): LLM + 外部 KB

### 6.2 Replay Buffer (RL)

- DQN: experience replay
- 与海马 sleep replay 类比

### 6.3 In-Context Learning

- LLM 短期上下文 ≈ 海马 + working memory
- 长 context (Claude 200k+) 模仿"长期" 

---

## 7. PyTorch — Episodic Memory + Pattern Completion

```python
import torch

class EpisodicMemory:
    """Auto-associative memory like CA3."""
    def __init__(self, dim=128):
        self.dim = dim
        self.memories = []
    
    def store(self, episode):
        # Episode = pattern vector
        self.memories.append(episode.clone())
    
    def recall(self, cue, mask=None):
        """Pattern completion: cue → closest stored memory."""
        if mask is not None:
            distances = [(m - cue)[mask].norm() for m in self.memories]
        else:
            distances = [(m - cue).norm() for m in self.memories]
        best_idx = torch.argmin(torch.tensor(distances))
        return self.memories[best_idx]

mem = EpisodicMemory()
# Store
mem.store(torch.tensor([1., 1, 0, 1, 0, 1]))
mem.store(torch.tensor([0., 1, 1, 0, 1, 1]))
# Recall with partial cue
cue = torch.tensor([1., 1, 0, 0, 0, 0])  # 后面 mask
mask = torch.tensor([True, True, True, False, False, False])
recalled = mem.recall(cue, mask)  # 应回到 first pattern
```

---

## 8. 病理

- **H.M.**: 双侧海马切除 → 完全 anterograde amnesia
- **Alzheimer**: 海马早期受损 → 记忆减退
- **Stress / PTSD**: cortisol 损海马,volume 减少
- **Aging**: 海马功能下降 + neurogenesis 减少
- **Epilepsy (temporal lobe)**: 海马 sclerosis 常见

---

## 9. 治疗 / 增强

- **Memantine** (NMDA 调) — Alzheimer
- **DBS on entorhinal** (实验): 改善 memory in 老人
- **认知训练**: 不改海马 volume,但改善 connectivity
- **物理锻炼**: 增 hippocampal neurogenesis

---

## 10. Common Pitfalls

### 10.1 海马 ≠ 全部记忆

也涉 amygdala (情绪 memory), 小脑 (motor), BG (habit)。

### 10.2 Place cell ≠ pure spatial

也编码 time, social relations。

### 10.3 Replay ≠ exact replay

Sleep replay 是 compressed + shuffled。

### 10.4 H.M. 临床特异

不能 generalize 到所有 amnesia。

### 10.5 fMRI 海马信号

容易混入 cortical contamination;需仔细 ROI。

---

## 11. Related Concepts

- **解剖**:[海马解剖](../01_Neuroanatomy/Hippocampus_Anatomy.md)
- **细胞**:[LTP/LTD](../02_Cellular_Molecular/LTP_LTD.md)
- **AI 对比**:[RAG](https://jeffliulab.github.io/ai-notes/06_AI_Engineering/06_RAG/)、Memory-augmented networks

---

## References

1. **Scoville & Milner.** "Loss of recent memory after bilateral hippocampal lesions." 1957.
2. **Squire, L. R.** "Memory systems of the brain." *Neurobiol Learn Mem*, 2004.
3. **Wilson, M. A. & McNaughton, B. L.** "Reactivation of hippocampal ensemble memories during sleep." *Science*, 1994.
4. **Tonegawa, S. et al.** "Memory engram cells." *Cell*, 2015.
5. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
