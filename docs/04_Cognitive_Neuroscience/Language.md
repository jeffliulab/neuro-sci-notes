# 语言 — Broca, Wernicke 与 LLM 时代

> *语言是人类独特能力 (有限例外)。经典区:Broca (产生) + Wernicke (理解)。现代 fMRI / 损伤研究修正,语言依赖广泛 cortical network。LLM 兴起后,语言神经科学与 AI 学科交叉密切。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. 经典模型 (Wernicke-Lichtheim)

- **Broca (44/45, 左额)**: 语言**产生**, 失语称 Broca's aphasia (慢、agrammatical 但理解相对完整)
- **Wernicke (22, 左颞)**: 语言**理解**, 失语称 Wernicke's aphasia (流畅但 nonsense, 不理解)
- **Arcuate fasciculus**: 连接两区,损 → conduction aphasia
- **Angular gyrus**: 读写关键

---

## 2. 现代 multi-stream model

Hickok & Poeppel 2007 双流:

```
Auditory cortex
   ↓
Dorsal stream: STG → Broca (sound → articulation)
Ventral stream: STG → ITG (sound → meaning)
```

---

## 3. 半球分工

- **左半球**: 95% right-handed 人语言 dominant
- **右半球**: prosody (语调), 隐喻, 笑话
- Bilingual: 不同语言略不同区,但 overlap 大

---

## 4. 语言 网络成员

- Broca + Wernicke 已知
- IFG (inferior frontal): syntax
- pSTS (posterior superior temporal sulcus): 意义
- TPJ (temporoparietal junction): 整合
- mPFC: 社交理解
- 海马: 词汇记忆

---

## 5. 失语症种类

| 类型 | 受损区 | 特征 |
|---|---|---|
| Broca's | LIFG (Broca) | 慢, agrammatical |
| Wernicke's | 左 STG | 流畅 nonsense |
| Conduction | Arcuate fasciculus | 重复差 |
| Global | Broca + Wernicke + 间 | 全损 |
| Anomic | Multiple | 找词难 |
| Transcortical | 周围区 | 重复 OK 但 spontaneous 差 |

---

## 6. 与 LLM 对比

| 维度 | Brain | LLM |
|---|---|---|
| 数据 | ~ 1B 词 (人 lifetime) | ~ 10T 词 (GPT-4 train) |
| 学习 | early-life critical period | 一次 train + RLHF |
| 模型 | distributed cortical | 175B-1T params Transformer |
| 推理 | slow + working memory | autoregressive token gen |
| Embodied | yes | no (purely textual) |
| 创造 | yes (新句子) | yes (combinatorial) |

→ LLM 与 brain 神经基础**很不同**,但表面 task 表现接近。

---

## 7. fMRI 语言研究

- 词汇 priming
- Semantic priming (related words 激活 overlapping)
- 句子复杂度 → IFG activation
- 听 vs 读 → 不同 modality 入口,共享 semantic

---

## 8. PyTorch — 简化语言模型概念

```python
import torch.nn as nn

class SimpleBrocaModel(nn.Module):
    """Inspired by speech production circuit."""
    def __init__(self, vocab=10000, dim=256):
        super().__init__()
        self.semantic = nn.Embedding(vocab, dim)  # Wernicke-like
        self.syntactic = nn.LSTM(dim, dim, batch_first=True)  # syntax
        self.articulatory = nn.Linear(dim, vocab)  # Broca-like output
    
    def forward(self, word_ids):
        sem = self.semantic(word_ids)
        syn, _ = self.syntactic(sem)
        return self.articulatory(syn)
```

---

## 9. 现代 BCI Speech

UCSF 2023: Stroke 患者 brainstem 损伤 → 通过 motor cortex 解码 80 words / min。
Stanford 2023: 类似 protocol,正确率 25% (但词汇 1024 大)。

---

## 10. Common Pitfalls

### 10.1 双区模型过简

Network distributed, 不只 Broca + Wernicke。

### 10.2 大模型 ≠ brain

数学 / 实现差异巨大,慎用类比。

### 10.3 Critical period

错过 critical period (e.g. feral children) 语言学习极困难。

### 10.4 Lateralization 不是绝对

左损 → 右半球可代偿 (尤其 early damage)。

### 10.5 fMRI 不能精细到 word

时间分辨率限制 → 不能逐 word 解码。

---

## 11. Related Concepts

- **同节**:[决策](Decision_Making.md)、[意识](Consciousness.md)
- **AI**: LLM https://jeffliulab.github.io/ai-notes/03_Foundation_Models/03_Language_Models/

---

## References

1. **Hickok, G. & Poeppel, D.** "The cortical organization of speech processing." *Nat Rev Neurosci*, 2007.
2. **Geschwind, N.** "The organization of language and the brain." *Science*, 1970.
3. **Broca, P.** "Remarques sur le siège de la faculté du langage articulé." 1861.
4. **Moses, D. A. et al.** "Neuroprosthesis for decoding speech in a paralyzed person." *NEJM*, 2021.
