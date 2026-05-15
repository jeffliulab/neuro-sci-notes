# 工作记忆 (Working Memory)

> *Working memory 是 brain 短期 hold + 操控 信息 ($7 \pm 2$ items, Miller 1956)。Baddeley 1974 model:central executive + phonological loop + visuospatial sketchpad + episodic buffer。PFC + parietal 主体。是 reasoning, decision, language 的关键。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)、[海马 + 记忆](../03_Systems_Neuroscience/Hippocampus_Memory.md)

---

## 1. 与 short-term memory 区别

- **Short-term memory**: passive hold (e.g., digit span)
- **Working memory**: active 操作 (manipulate, rearrange)
- WM = STM + control

---

## 2. Baddeley Model (1974)

```
        Central Executive
       /      |        \
Phonological  Visuospatial  Episodic
Loop          Sketchpad     Buffer (added 2000)
(verbal)      (visual)      (multimodal)
```

- **Phonological loop**: 听 / verbal (~ 2 sec)
- **Visuospatial sketchpad**: 视觉 / 空间
- **Episodic buffer**: 与 long-term memory 接口
- **Central executive**: attention + control

---

## 3. 神经基础

- **DLPFC**: 主 WM region
- **PPC** (parietal): visual / spatial WM
- **Broca area**: phonological loop (与语言重叠)
- **Hippocampus**: 长期 storage 接口

---

## 4. Persistent Activity

WM 神经元 sustained firing during delay:
- Goldman-Rakic 1990s monkey 实验
- DLPFC neurons hold item information 数秒
- 类似 "attractor network" 状态

---

## 5. 容量限制

- Miller 1956: $7 \pm 2$ items (但简化, modern: ~ 4)
- Chunking: 1234567 → "1-2-3" "4-5-6" "7" → 3 chunks
- Expert chunking (chess masters) 看似超 capacity

---

## 6. 衰减

- ~ 18 sec without rehearsal (Peterson & Peterson 1959)
- Interference > 衰减 (主因 forgetting)
- Sleep deprivation 严重 affect WM

---

## 7. PyTorch — Sustained Activity Model

```python
import torch

class WorkingMemoryNet(torch.nn.Module):
    """Attractor network for WM."""
    def __init__(self, dim=64, n_attractors=4):
        super().__init__()
        # Recurrent weights from Hebbian learning
        self.W = torch.nn.Parameter(torch.zeros(dim, dim))
        self.dim = dim
    
    def forward(self, x, steps=20):
        h = x
        for _ in range(steps):
            h = torch.tanh(self.W @ h + x * 0.1)  # 持续 input + recurrence
        return h  # converged attractor
    
    def store(self, pattern):
        """Hebbian: W += outer(p, p) / dim."""
        with torch.no_grad():
            self.W += torch.outer(pattern, pattern) / self.dim
```

---

## 8. 训练 WM

- N-back task (1-back, 2-back, 3-back...)
- Dual N-back: 视 + 听 N-back
- Brain training (但 transfer 有争议)
- Stimulant medication (ADHD) 临时 boost

---

## 9. 病理

- **ADHD**: WM 容量 / regulation 失调
- **Schizophrenia**: severe WM impairment
- **Alzheimer**: WM 早期受损
- **Aging**: WM 容量 decline (但 expertise 可补偿)
- **Depression**: rumination 占用 WM resources

---

## 10. WM ↔ AI

- **Transformer context window**: ≈ 短期 WM (但 容量 大得多)
- **RNN hidden state**: 近似 sustained activity
- **Attention mechanism**: 类似 central executive
- LLM "in-context learning" 与人 WM 有 functional overlap

---

## 11. Common Pitfalls

### 11.1 7 ± 2 误读

不是 universal capacity;依 content + chunking。

### 11.2 STM = WM 错误

WM 含 active manipulation,STM 是 passive hold。

### 11.3 Brain training transfer

Improve N-back ≠ improve general intelligence (controversial)。

### 11.4 WM 容量 ≠ IQ

Correlated but distinct constructs。

### 11.5 LLM context ≠ brain WM

LLM 容量 200k+ tokens;brain ~ 4-7。完全不同 substrate。

---

## 12. Related Concepts

- **同节**:[Attention](Attention.md)、[Decision_Making](Decision_Making.md)、[Language](Language.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)
- **系统**:[海马 + 记忆](../03_Systems_Neuroscience/Hippocampus_Memory.md)
- **AI**: Transformer attention — https://jeffliulab.github.io/ai-notes/02_Deep_Learning/05_Transformers/

---

## References

1. **Baddeley, A.** *Working Memory*. Oxford, 1986.
2. **Miller, G. A.** "The magical number seven." *Psychol Rev*, 1956.
3. **Goldman-Rakic, P. S.** "Cellular basis of working memory." *Neuron*, 1995.
4. **D'Esposito, M. & Postle, B. R.** "The cognitive neuroscience of working memory." *Annu Rev Psychol*, 2015.
