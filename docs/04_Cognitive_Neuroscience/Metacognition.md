# 元认知 (Metacognition)

> *Metacognition 是"对认知的认知" — 对自己思维、知识、信心的 monitoring。Flavell 1976 提出 term。包括 metamemory(知不知道)、信心评估、错误检测。anterior PFC + parietal 是核心。Animal、infant 也有部分 metacognition。是 consciousness 的一近邻,LLM "calibration" 是 functional parallel。*
>
> **难度**:Advanced
> **前置知识**:[Consciousness](Consciousness.md)、[Working_Memory](Working_Memory.md)

---

## 1. 定义 + 分类

- **Metacognitive knowledge**: 我知道我擅长 X(self-knowledge)
- **Metacognitive monitoring**: 现在我对这答案多 confident?
- **Metacognitive control**: 学习困难 → 复习 / 求帮助

---

## 2. 测量

### 2.1 Confidence judgment

每决策后报 confidence (1-5)
正确高 confidence + 错误低 confidence = high metacognition

### 2.2 Metacognitive sensitivity

Type 2 ROC (Maniscalco & Lau 2012):
- $d'$ measures perception
- meta-$d'$ measures metacognition
- $M_{ratio} = \text{meta-}d' / d'$

### 2.3 Feeling of knowing (FOK)

"This is on the tip of my tongue" — 知道知道但 retrieve 失败

---

## 3. 神经基础

- **Anterior PFC** (BA10): 主 metacognition
- **Dorsolateral PFC**: monitoring + control
- **PCC + precuneus**: self-referential
- **Insula**: 信心 → 感觉
- 损伤 → metacognition 下降但 task 表现不变(分离)

---

## 4. 双过程

- **System 1**: 快、直觉、低 metacog
- **System 2**: 慢、analytic、高 metacog
- Kahneman 2011 *Thinking, Fast and Slow*

---

## 5. Animal Metacognition

- Macaque (Hampton 2001): 拒绝难题 → 暗示知道"不知道"
- Dolphin、rat 类似实验
- 但 alternative explanation:learned avoidance,非真 metacog

---

## 6. Infants + Children

- 3 yo: 极弱 metacog
- 4-5 yo: 通过 false belief task,metacog 出现
- 青春期:metacog 大发展(PFC 成熟)
- ADHD、autism:metacog 缺陷

---

## 7. Dunning-Kruger Effect (1999)

- 无能者 overestimate
- 能者 underestimate(知不知道未知)
- 反映 metacog 与 skill 共变
- 后续 replication 有质疑

---

## 8. PyTorch — Confidence Calibration

```python
import torch
import torch.nn as nn

class CalibratedClassifier(nn.Module):
    """LLM 类似 temperature calibration."""
    def __init__(self, base_model):
        super().__init__()
        self.base = base_model
        self.temperature = nn.Parameter(torch.tensor(1.0))
    
    def forward(self, x):
        logits = self.base(x)
        return logits / self.temperature
    
    def calibrate(self, val_loader):
        """Fit temperature on validation to minimize NLL."""
        # 标准 Platt / temperature scaling
        pass
```

---

## 9. AI / LLM Connection

- **Calibration**: model confidence matches accuracy?
- LLM 常 overconfident(尤 reasoning)
- **Verbalized confidence**: ask LLM "how confident?"(部分 work)
- **Constitutional AI / RLHF**: 改善 calibration
- 但 LLM 是否真 metacog?争议

---

## 10. 改善 metacog

- **Reflection**: 写日记、self-explain
- **Calibration training**: confidence + outcome feedback
- **Education**: 教 metacog strategies
- **Mindfulness**: 增强 monitoring

---

## 11. Common Pitfalls

### 11.1 Metacog = consciousness

不;metacog 是 about cognition 的一具体功能;consciousness 是 broader。

### 11.2 高 confidence = 准

错;Dunning-Kruger。

### 11.3 LLM 报 confidence = 真 metacog

可能是 statistical pattern,非 introspection。

### 11.4 全人 metacog 同

差异大;受 training、PFC integrity 影响。

### 11.5 Animal metacog 明确

仍争议;learned response 替代解释。

---

## 12. Related Concepts

- **同节**:[Consciousness](Consciousness.md)、[Decision_Making](Decision_Making.md)、[Working_Memory](Working_Memory.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)(尤 BA10)
- **AI**: Calibration、constitutional AI

---

## References

1. **Flavell, J. H.** "Metacognitive aspects of problem solving." 1976.
2. **Fleming, S. M. & Dolan, R. J.** "The neural basis of metacognitive ability." *Phil Trans R Soc B*, 2012.
3. **Maniscalco, B. & Lau, H.** "A signal detection theoretic approach for estimating metacognitive sensitivity." *Conscious Cogn*, 2012.
4. **Kahneman, D.** *Thinking, Fast and Slow*. 2011.
