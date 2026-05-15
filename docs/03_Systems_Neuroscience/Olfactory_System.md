# 嗅觉系统 (Olfactory System)

> *嗅觉是唯一不经 thalamus 直入 cortex 的感觉。Buck & Axel 1991 发现 ~ 350 odorant receptors(2004 Nobel)。最 ancient 的感觉,深 link emotion + memory(Proust effect)。Olfactory bulb → piriform → entorhinal → hippocampus。AI olfaction (electronic nose) 模拟。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)、[Hippocampus_Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.md)

---

## 1. 感受器 + 第一级

- **Olfactory epithelium**: 鼻腔顶,~ 5 cm²
- **Olfactory receptor neurons (ORN)**: ~ 6 million,3-4 week 寿命再生
- 每 ORN 表达 1 odorant receptor (1OR-1ORN 规则)
- 350 OR genes(Buck & Axel) → combinatorial coding
- Cilia + mucus 捕获 odorant 分子

---

## 2. 路径

```
Odorant 分子 (空气)
  ↓
Olfactory epithelium (ORN cilia)
  ↓ (cribriform plate)
Olfactory bulb (glomeruli)
  ↓
  ├──→ Piriform cortex (primary)
  ├──→ Amygdala
  ├──→ Entorhinal → Hippocampus
  └──→ OFC (二级)
```

→ 直接到 cortex,不经 thalamus(其他感觉都经 thalamus)。

---

## 3. Olfactory Bulb

- **Glomeruli** ~ 2000 个,每 glomerulus 收同 OR 的 ORN axons
- Convergence: 数千 ORN → 1 glomerulus
- Mitral / tufted cells = output
- Granule cells = inhibitory interneurons → lateral inhibition

---

## 4. Combinatorial Coding

- 1 odorant 激活多 OR(non-specific)
- 1 OR 响应多 odorant
- $350 \times 350 \times ... \times 350$ combinatorial → 万亿种 odor distinguishable
- 类似 NN 分布式表征

---

## 5. Piriform Cortex

- 古皮层(3 层,非 6 层)
- 不像 V1 retinotopy,piriform 无 odotopy(混合 input)
- Like a flat NN doing odor classification
- 与 hippocampus、amygdala 紧密相连 → 嗅觉记忆 + emotion

---

## 6. Proust Effect

- 普鲁斯特玛德兰小蛋糕:气味 → 强烈 episodic memory
- 解释:嗅觉直入 amygdala + hippocampus,无 thalamus gate
- 单一 modality 中:olfactory memory 最 robust 长期

---

## 7. PyTorch — Olfactory Combinatorial Code

```python
import torch
import torch.nn as nn

class OlfactoryEncoder(nn.Module):
    """350 receptors → odor identity classifier."""
    def __init__(self, n_receptors=350, n_odors=1000):
        super().__init__()
        # Each receptor has random affinity to each odor
        self.receptor_affinity = nn.Parameter(torch.randn(n_receptors, n_odors))
    
    def forward(self, odor_concentration):
        """odor_concentration: (B, n_odors) — composition vector."""
        # Receptor responses are combinatorial sum
        response = self.receptor_affinity @ odor_concentration.t()  # (n_receptors, B)
        response = torch.sigmoid(response)  # saturation
        return response  # neural code
```

---

## 8. 人类 vs 动物

- **Human**: ~ 350 functional OR genes
- **Dog**: ~ 800
- **Mouse**: ~ 1000
- 但 absolute olfactory acuity 人类比传说强(McGann 2017)

---

## 9. 病理

- **Anosmia**: 嗅觉丧失
  - COVID-19 重要 symptom
  - Parkinson 早期 marker(损 ORN / bulb)
  - Alzheimer 早期
- **Parosmia**: 嗅觉扭曲(感染、化疗后)
- **Phantosmia**: 幻嗅(temporal seizure)
- **Kallmann syndrome**: 嗅+性 development 同障

---

## 10. AI olfaction

- **Electronic nose**: gas sensor array → ML 分类
- **Aryballe**: 商用 e-nose
- **Osmo (Google AI)**: deep learning + 闻 → 描述
- **Princeton 2024**: GNN predict odor from molecule structure
- Olfaction 是 AI 最难复现 sensor modality

---

## 11. Pheromone

- **Vomeronasal organ (VNO)**: 鼠等用第二嗅
- 人类 VNO 退化,but Major Histocompatibility Complex (MHC) 嗅觉 still influences mate choice
- Pheromone 商品 (Axe) 是 marketing

---

## 12. Common Pitfalls

### 12.1 人类嗅觉差

McGann 2017: 人嗅觉 not 落后于狗 in many aspects。

### 12.2 1 odor = 1 receptor

错;combinatorial coding。

### 12.3 Bulb 无 plasticity

实际:olfactory bulb 是大脑唯一持续 neurogenesis 区域(成人)。

### 12.4 Anosmia 不严重

实际:depression 风险 ↑、food safety ↓、社交 difficulty。

### 12.5 Olfaction = Smell only

也含 flavor (retronasal olfaction during eating)。

---

## 13. Related Concepts

- **同节**:[Visual System](Visual_System.md)、[Auditory_System](Auditory_System.md)、[Somatosensory](Somatosensory.md)
- **解剖**:[Hippocampus_Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.md)、[Amygdala](../01_Neuroanatomy/Amygdala.md)
- **疾病**:Alzheimer 早期 anosmia

---

## References

1. **Buck, L. & Axel, R.** "A novel multigene family may encode odorant receptors." *Cell*, 1991.
2. **Mori, K. & Yoshihara, Y.** "Molecular recognition and olfactory processing." *Prog Neurobiol*, 1995.
3. **McGann, J. P.** "Poor human olfaction is a 19th-century myth." *Science*, 2017.
4. **Lee, B. K. et al.** "A principal odor map unifies diverse tasks in olfactory perception." *Science*, 2023.
