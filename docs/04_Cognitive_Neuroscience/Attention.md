# 注意 (Attention) — 选择性 + 局限性

> *注意是 brain 选择性 process 信息的机制。人能同时处理 ~ 1 件事 (并行 limited)。Posner 1980 分 3 网络:alerting, orienting, executive。是认知瓶颈。AI Transformer 的 attention 是数学化版本。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. 类型

- **Selective attention**: focus on 1 / ignore others (cocktail party)
- **Divided attention**: multi-task (driving + 听 podcast — limited)
- **Sustained attention**: 长时间 focus (vigilance task)
- **Top-down**: 目标驱动 (找朋友 in crowd)
- **Bottom-up**: stimulus driven (响 sudden noise)

---

## 2. Posner 3 网络

### 2.1 Alerting

- 提高 vigilance (准备 process)
- 涉 locus coeruleus NE
- "ready to receive" 信号

### 2.2 Orienting

- 把 attention 指向特定 spatial / sensory location
- 涉 superior parietal lobule + frontal eye fields
- Overt (eye move) vs covert (no eye move)

### 2.3 Executive

- 解决 conflict (Stroop)
- 涉 ACC (Anterior Cingulate) + dlPFC
- 任务切换

---

## 3. Visual Attention 神经基础

- **V1**: spatial attention gates visual signal
- **FEF (Frontal Eye Fields)**: saccade + covert attention
- **PPC (Posterior Parietal Cortex)**: spatial map
- **Pulvinar (thalamus)**: 协调

---

## 4. Cocktail Party Effect

吵闹场景中 selectively 听 1 个人:
- Spatial cue (位置)
- Spectral (voice pitch)
- Linguistic
- Top-down knowledge (听我的名字 → attention 切换)

---

## 5. Stroop Test

经典 conflict task:
- 显 "RED" 红色 → 易 (congruent)
- 显 "RED" 蓝色 → 慢 (incongruent)
- ACC 处理 conflict

---

## 6. Attention vs AI Transformer

| 维度 | Brain | Transformer |
|---|---|---|
| Bottleneck | yes (limited capacity) | parallel all tokens |
| Top-down | strong | weak (positional encoding) |
| Multi-modal | seamless | requires special encoding |
| Energy | low | high |
| Dynamic | yes | static after train |

Transformer "attention" 是数学化 weighted sum;brain attention 是 selective routing + gain modulation。

---

## 7. PyTorch — Spatial Attention Module

```python
import torch
import torch.nn as nn

class SpatialAttention(nn.Module):
    """Visual spatial attention as gain modulation."""
    def __init__(self, channels):
        super().__init__()
        self.conv = nn.Conv2d(channels, 1, 1)
    
    def forward(self, feature_map, attention_loc=None):
        # feature_map: (B, C, H, W)
        if attention_loc is not None:
            # Bias attention to specific location
            mask = self._gauss_mask(attention_loc, feature_map.shape[-2:])
            attn = torch.sigmoid(self.conv(feature_map)) * mask
        else:
            attn = torch.sigmoid(self.conv(feature_map))
        return feature_map * (1 + attn)
    
    def _gauss_mask(self, loc, shape, sigma=10):
        h, w = shape
        ys, xs = torch.meshgrid(torch.arange(h), torch.arange(w))
        cy, cx = loc
        return torch.exp(-((ys-cy)**2 + (xs-cx)**2) / (2*sigma**2))
```

---

## 8. 病理 / 异常

- **ADHD**: 难 sustain attention;DA / NE 失调
- **Visual neglect**: 顶叶损 → 忽视一侧视野
- **Balint syndrome**: 双侧 PPC 损 → simultanagnosia
- **Hemispatial neglect**: stroke 后忽视对侧

---

## 9. 训 attention

- Meditation: 显示改 ACC + insula
- Video game (action): 提 visual attention span
- ADHD: cognitive training + 药物 (Adderall)

---

## 10. 应用

- UI/UX design (引导 user attention)
- Driver fatigue detection (眼动 + EEG)
- AR/VR (gaze tracking + foveated rendering)
- Marketing (eye tracking)

---

## 11. Common Pitfalls

### 11.1 Multitasking 错觉

实际是快速 switch;每次 switch 损 cost。

### 11.2 Attention ≠ consciousness

可 attend 而不 consciously aware (subliminal priming)。

### 11.3 "ADHD 是 attention deficit"

实际是 attention regulation 问题;部分 ADHD 在 interesting task hyperfocus。

### 11.4 fMRI 假设 attention = activation

实际可能 inhibition / gain change。

### 11.5 Transformer attention 不完全模 brain

---

## 12. Related Concepts

- **同节**:[Decision Making](Decision_Making.md)、[Language](Language.md)、[Consciousness](Consciousness.md)
- **AI**:[Self-Attention](https://jeffliulab.github.io/ai-notes/02_Deep_Learning/05_Transformers/Self_Attention/)

---

## References

1. **Posner, M. I. & Petersen, S. E.** "The attention system of the human brain." *Annu Rev Neurosci*, 1990.
2. **Corbetta, M. & Shulman, G. L.** "Control of goal-directed and stimulus-driven attention." *Nat Rev Neurosci*, 2002.
3. **Carrasco, M.** "Visual attention: the past 25 years." *Vis Res*, 2011.
