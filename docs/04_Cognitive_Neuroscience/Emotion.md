# 情绪 (Emotion) — Amygdala 和情绪网络

> *情绪是 brain 对 stimuli 的快速 evaluation + 反应。Amygdala 是 fear / threat 核心,但 emotion 涉广泛 cortical / subcortical network。基本情绪 vs constructed 情绪是当前 debate。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)、[神经递质](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 经典 vs 现代

### 1.1 经典 "basic emotions" (Ekman 1972)

- 6 universal: happy, sad, fear, anger, disgust, surprise
- 跨文化 facial expression 一致
- Each emotion 有 distinct neural signature

### 1.2 Constructed Emotion (Barrett 2017)

- Emotion 是 brain 主动 construct (基于 prior + context)
- 不是 discrete categories
- 同一 brain signal 不同 context → 不同 emotion experience
- 与 predictive coding 一致

→ Two camps active debate。

---

## 2. 主要 emotion-related 脑区

```
Amygdala — fear / threat detection
Insula — disgust, interoception
Anterior Cingulate (ACC) — distress, empathy
vmPFC — emotion regulation, value
Hypothalamus — autonomic + endocrine response
PAG (brainstem) — defensive behavior
NAcc / VTA — reward, pleasure
```

---

## 3. Amygdala 详细

- ~ 13 nuclei 组合
- Lateral nucleus: 输入 (sensory + cortex)
- Central nucleus: 输出 (autonomic + 行为 via hypothalamus, brainstem)
- BLA (basolateral): fear learning + memory
- 损 → reduced fear (Urbach-Wiethe disease)

---

## 4. Fear Conditioning

经典 Pavlovian:
1. CS (中性, e.g., tone) + US (厌恶, e.g., shock) 配对
2. 学到 CS → 预测 US
3. Amygdala LTP at CS pathway
4. 仅 CS → freeze (CR, conditioned response)

是 PTSD, anxiety 的 model。

---

## 5. Emotion 调节

PFC top-down regulate amygdala:
- **Reappraisal**: 重新 evaluate situation
- **Suppression**: hide behavioral expression
- **Mindfulness**: observe without judgment

Reappraisal > suppression for long-term wellbeing (Gross 2002)。

---

## 6. 自主反应

Emotion → physiological:
- HR 心跳加速 (sympathetic)
- 皮肤电导 ↑ (sweat)
- 瞳孔扩大
- 呼吸 deep / fast
- 胃肠 ↓

→ 是 polygraph (测谎) 基础 (但精度仅 ~ 65%)。

---

## 7. PyTorch — Emotion Classification from Face

```python
import torch
import torch.nn as nn

class EmotionClassifier(nn.Module):
    """6 basic emotions from face image."""
    def __init__(self):
        super().__init__()
        # ResNet-like backbone
        self.backbone = nn.Sequential(
            nn.Conv2d(3, 64, 3, padding=1), nn.ReLU(), nn.MaxPool2d(2),
            nn.Conv2d(64, 128, 3, padding=1), nn.ReLU(), nn.MaxPool2d(2),
            nn.AdaptiveAvgPool2d(1),
        )
        self.fc = nn.Linear(128, 6)  # 6 emotions
    
    def forward(self, face):
        return self.fc(self.backbone(face).flatten(1))
```

---

## 8. 病理

- **PTSD**: amygdala hyperactivity + PFC 抑制不足
- **Anxiety**: 类似 PTSD 但 trigger 不明
- **Depression**: 整体 emotion processing 失调
- **Psychopathy**: amygdala 反应低 (low empathy / fear)
- **Capgras delusion**: 认识但无 emotion → 觉冒充
- **Klüver-Bucy syndrome**: bilateral amygdala 损 → 无惧 + 性 hyperphagia

---

## 9. AI / Affective Computing

- 表情识别 (FER, Facial Expression Recognition)
- 语音 emotion classification (audio)
- 多模态 emotion AI (面 + voice + text + physiology)
- Empathy in chatbot (Replika)
- Applications: mental health screening, marketing, gaming

---

## 10. Common Pitfalls

### 10.1 6 基本 emotion 不绝对

文化差异 (collectivism vs individualism)。

### 10.2 Amygdala 不仅 fear

也 positive emotion (anticipation reward)。

### 10.3 Polygraph 不准

物理 response 可由 anxiety / 不熟悉 trigger。

### 10.4 Emotion ≠ feeling

Emotion: neurobiological;feeling: conscious experience。

### 10.5 Suppress emotion 有害

长期 suppress 增 stress, depression risk。

---

## 11. Related Concepts

- **同节**:[Attention](Attention.md)、[Decision_Making](Decision_Making.md)、[Consciousness](Consciousness.md)
- **细胞**:[神经递质](../02_Cellular_Molecular/Neurotransmitters.md)
- **疾病**:[Depression](../08_Neuro_Disorders/Depression.md)

---

## References

1. **LeDoux, J. E.** *The Emotional Brain*. 1996.
2. **Barrett, L. F.** *How Emotions Are Made*. 2017.
3. **Ekman, P.** "Universal facial expressions." *Science*, 1969.
4. **Gross, J. J.** "Emotion regulation: Affective, cognitive, and social consequences." *Psychophysiology*, 2002.
