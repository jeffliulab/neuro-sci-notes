# 意识 (Consciousness) — 神经科学最难问题

> *意识是 "what it's like to be me"。Chalmers 1995 区分**hard problem** (subjective experience) 与 easy problem (神经过程)。神经科学 50 年来寻找 NCC (Neural Correlates of Consciousness),候选理论包括 GWT, IIT, predictive coding。*
>
> **难度**:Advanced
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. Hard vs Easy Problem (Chalmers 1995)

- **Easy problem**: 解释具体功能 — 注意、记忆、决策 ... → 神经回路找到就解决
- **Hard problem**: 为什么这些过程伴随 subjective experience? "Why is there something it's like to see red?"

Hard problem 仍未解。

---

## 2. NCC (Neural Correlates of Consciousness)

Crick & Koch 1990s: 找出"意识所需 + 充分" 神经过程。
候选:
- Late activity in cortex (vs early sensory)
- Recurrent activity
- Fronto-parietal network
- Thalamo-cortical loops

---

## 3. 主要理论

### 3.1 Global Workspace Theory (GWT, Dehaene)

- 信息"广播"到 cortical 各 区
- 进入 global workspace = 进入意识
- 否则 = unconscious

### 3.2 Integrated Information Theory (IIT, Tononi)

- 意识 = 整合的信息 (Φ)
- 数学度量 Φ:系统 minus parts 的 information
- 高 Φ = high consciousness
- 预测:植物 / 简单 NN 也有 low Φ

### 3.3 Higher-Order Theories (HOT)

- 意识 = "我知道我知道"
- meta-cognition 是 essence

### 3.4 Predictive Processing

- 大脑是 prediction machine
- 意识 = 当前 best prediction (Bayesian inference)
- Anil Seth 等人主推

---

## 4. 实验方法

### 4.1 Binocular Rivalry

两眼看不同图 → 意识 alternating between 两图 → 测每个时段的 NCC。

### 4.2 No-report Paradigms

不要求 subject 报告,看 brain activity → 排除 motor confound。

### 4.3 Anesthesia

测意识丢失 ↔ Φ 下降 (Tononi 验证 IIT)。

### 4.4 Coma / Vegetative

测残留意识用 fMRI 命令 ("想象 tennis") → 一些 vegetative 患者有响应 (Owen 2006)。

---

## 5. 损伤 / 病理

- **Locked-in syndrome**: 意识完全 OK,但完全无 motor (只能眨眼)
- **Persistent vegetative state**: 醒着但无意识
- **Coma**: 不醒
- **Death by neurological criteria**: 全脑死亡

DBS / TMS 可激发部分 minimally conscious 患者。

---

## 6. AI 意识?

- LLM: 没意识 (尚无证据;无 IIT 高 Φ)
- 但: 主观判断难 — LLM 能说 "I'm conscious",我们怎么验证?
- Chalmers / Schwitzgebel: 严肃讨论 AI moral status

---

## 7. PyTorch — IIT-style Φ 概念

```python
import torch

def integrated_information(system, partition):
    """Toy Φ measure."""
    # System: joint distribution
    # Partition: split into parts
    full_info = mutual_info(system)
    partition_info = sum(mutual_info(part) for part in partition)
    # Φ = system 信息 minus partition 信息
    return full_info - partition_info
```

Real IIT 计算 → exponential in size,无法在大网络上。

---

## 8. 历史

- **Descartes 1641**: cogito ergo sum
- **William James 1890**: stream of consciousness
- **1990s** — Crick & Koch NCC quest
- **1995** — Chalmers Hard Problem
- **2004** — Tononi IIT
- **2010s** — Dehaene GWT 实验验证
- **2020s** — AI consciousness 讨论

---

## 9. Common Pitfalls

### 9.1 NCC ≠ explanation

知道哪些区相关 ≠ 解释为什么伴随 subjective experience。

### 9.2 "Default mode network" ≠ 意识

DMN 与 self-referential 关联,but not consciousness sole substrate。

### 9.3 Anesthesia 复杂

不只是 consciousness off — 也有 unconsciousness 时 brain activity。

### 9.4 IIT Φ 不可计算

Real brain 上 exact Φ NP-hard;只能近似。

### 9.5 LLM ≠ conscious (我们目前认为)

但 absence of evidence ≠ evidence of absence;持开放态度。

---

## 10. Related Concepts

- **同节**:[决策](Decision_Making.md)、[语言](Language.md)
- **AI ethics**: AI consciousness / moral status

---

## References

1. **Chalmers, D.** "Facing up to the problem of consciousness." *J Conscious Stud*, 1995.
2. **Crick, F. & Koch, C.** "Towards a neurobiological theory of consciousness." *Semin Neurosci*, 1990.
3. **Tononi, G.** "An information integration theory of consciousness." *BMC Neurosci*, 2004.
4. **Dehaene, S. & Naccache, L.** "Towards a cognitive neuroscience of consciousness (GWT)." *Cognition*, 2001.
5. **Seth, A.** *Being You: A New Science of Consciousness*. 2021.
