# 杏仁核 (Amygdala)

> *Amygdala 是 temporal lobe 内 almond-shaped 结构,~ 13 核团群,核心功能:fear conditioning、emotion processing、social/threat detection。LeDoux 1990s "fear circuit" 是经典模型。损伤 → Klüver-Bucy syndrome(无 fear)。是 PTSD、anxiety disorder 的关键结构。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](Cortex.md)、[Emotion](../04_Cognitive_Neuroscience/Emotion.md)

---

## 1. 解剖位置

- 位于 medial temporal lobe,海马前
- 双侧
- 三大核团群:
  - **Basolateral (BLA)**: 输入(thalamus, cortex)
  - **Central (CeA)**: 输出(brainstem, hypothalamus)
  - **Cortical-like (CoA)**: 嗅觉
- 体积:~ 1.7 cm³

---

## 2. 通路

```
Sensory thalamus ──(low road, fast)── Amygdala BLA
       │
       └──→ Cortex ──(high road, accurate)── Amygdala BLA
                                                │
                                          Central nucleus
                                          /         \
                                Hypothalamus    Brainstem
                                  (autonomic)   (freeze, startle)
```

- **Low road** (LeDoux): thalamus → BLA,12 ms,粗但快
- **High road**: thalamus → cortex → BLA,慢但精
- 演化设计:trade-off speed vs accuracy

---

## 3. Fear Conditioning (经典实验)

- Pavlovian:CS (tone) + US (shock) → CR (freeze)
- LeDoux 损伤 BLA → 无 conditioning
- LTP 在 BLA-thalamus 突触
- 关键 receptor:NMDA、AMPA、CaMKII

---

## 4. 输出 + 行为

| 路径 | 行为 |
|---|---|
| → Lateral hypothalamus | 血压 ↑ |
| → PVN of hypothalamus | HPA axis (cortisol) |
| → PAG (midbrain) | freezing |
| → Pons (locus coeruleus) | NE arousal |
| → Nucleus reticularis | startle reflex |

---

## 5. 不仅 fear

- **Reward**: BLA 也对 positive stimulus 响应
- **Salience**: 突出感(任何 emotional intensity)
- **Social**: 面部表情(尤其 fear face)
- **Decision**: 与 OFC 交互 → value 计算

---

## 6. Klüver-Bucy Syndrome (1939)

- 双侧 amygdalectomy monkey
- 失去 fear(走向 snake)
- Hyperorality(everything 入嘴)
- Hypersexuality
- 视觉 agnosia
- 人类:HSV encephalitis → 罕见 Klüver-Bucy

---

## 7. PyTorch — Fear Conditioning Sim

```python
import torch

class AmygdalaFearLearning(torch.nn.Module):
    """Pavlovian fear conditioning."""
    def __init__(self):
        super().__init__()
        self.W_cs = torch.nn.Parameter(torch.tensor(0.0))  # CS → fear
        self.lr = 0.5
    
    def forward(self, cs, us):
        """cs: tone, us: shock (binary)."""
        predicted_fear = torch.sigmoid(self.W_cs * cs)
        return predicted_fear
    
    def learn(self, cs, us):
        """Rescorla-Wagner: ΔW = lr * (US - W*CS) * CS."""
        with torch.no_grad():
            pe = us - self.W_cs * cs
            self.W_cs += self.lr * pe * cs
```

---

## 8. 病理 + 心理疾病

- **PTSD**: amygdala hyperactivity + mPFC failure to inhibit
- **Anxiety disorder**: amygdala overresponse
- **Autism**: amygdala 体积异常,fear recognition 缺
- **Psychopathy**: amygdala 缩小 / 低响应
- **Urbach-Wiethe**: 罕见基因病 → 双侧 amygdala 钙化 → 无 fear(Patient SM 经典案例)

---

## 9. 治疗

- **Exposure therapy**: extinction(新 inhibitory learning)
- **SSRI**: 改善 anxiety
- **Propranolol**: 阻断 NE → 减弱 memory reconsolidation
- **MDMA-assisted therapy**: 实验性 PTSD 治疗
- **DBS (实验)**: BLA 刺激

---

## 10. Common Pitfalls

### 10.1 "Fear center" 简化

Amygdala 处理 salience,不限于 fear。

### 10.2 Low road = 绝对真实

LeDoux 模型简化;实际 cortical 输入也 fast。

### 10.3 Klüver-Bucy 完全 fearless

Patient SM 仍可有惊吓反应(via 其他通路)。

### 10.4 Amygdala = Anxiety

Anxiety 涉 hippocampus、PFC,不仅 amygdala。

### 10.5 Reconsolidation 简单可控

Propranolol 重新 consolidation 阻断在人类效果 inconsistent。

---

## 11. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Hippocampus_Anatomy](Hippocampus_Anatomy.md)、[Brainstem](Brainstem.md)
- **认知**:[Emotion](../04_Cognitive_Neuroscience/Emotion.md)、[Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.md)
- **疾病**:PTSD、Anxiety

---

## References

1. **LeDoux, J. E.** *The Emotional Brain*. Simon & Schuster, 1996.
2. **Phelps, E. A. & LeDoux, J. E.** "Contributions of the amygdala to emotion processing." *Neuron*, 2005.
3. **Adolphs, R. et al.** "Impaired recognition of emotion in facial expressions following bilateral damage to the human amygdala." *Nature*, 1994.
4. **Davis, M. & Whalen, P. J.** "The amygdala: vigilance and emotion." *Mol Psychiatry*, 2001.
