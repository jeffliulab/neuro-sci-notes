# 味觉系统 (Gustatory System)

> *味觉检测可溶分子,五基本味:甜、咸、酸、苦、鲜(umami)。味蕾(taste bud)→ CN VII/IX/X → 孤束核 → 丘脑 VPM → 岛叶味觉皮层。Labeled-line vs 群体编码之争。"Flavor" = 味 + 嗅(retronasal)+ 三叉(辣/温)多感觉。是趋食/避毒的演化核心。*
>
> **难度**:Intermediate
> **前置知识**:[Olfactory_System](Olfactory_System.md)、[Somatosensory](Somatosensory.md)

---

## 1. 五基本味

| 味 | 配体 | 受体 | 适应意义 |
|---|---|---|---|
| 甜 | 糖 | T1R2+T1R3(GPCR) | 能量 |
| 鲜 umami | 谷氨酸 | T1R1+T1R3 | 蛋白/氨基酸 |
| 苦 | 多种毒素样 | T2R 家族(~25 个) | 避毒 |
| 咸 | Na⁺ | ENaC | 电解质 |
| 酸 | H⁺ | OTOP1 | 腐败/未熟 |

(脂肪、钙、水 等"第六味"候选,研究中)

---

## 2. 转导

- **甜/鲜/苦**:GPCR → PLCβ2 → IP3 → Ca²⁺ → TRPM5 → ATP 释放(嘌呤能传给神经)
- **咸**:Na⁺ 经 ENaC 直接去极化
- **酸**:H⁺ 经 OTOP1 质子通道
- 每味蕾含多型受体细胞(I/II/III)

---

## 3. 通路

```
味蕾(舌/腭/咽)
   ↓ CN VII(面)/ IX(舌咽)/ X(迷走)
孤束核(NTS,延髓)
   ↓
丘脑 VPM(腹后内侧)
   ↓
岛叶 / 额盖 味觉皮层(primary)
   ↓
OFC(次级,味-价值)+ 杏仁核
```

(注:味觉**经丘脑**;嗅觉不经 — 见 [Olfactory_System](Olfactory_System.md))

---

## 4. 编码之争

- **Labeled line**:每味专用通道("甜线")— 受体 + NTS 部分支持
- **Across-fiber/群体**:群体模式编码味质
- 现代:外周偏 labeled-line(受体特异),中枢更分布 + 状态依赖

---

## 5. PyTorch — 五味群体编码

```python
import torch

def taste_population_code(stimulus, receptor_affinity):
    """5 receptor types -> population response; OFC reads value."""
    # receptor_affinity: (5, n_tastants); stimulus: (n_tastants,)
    response = torch.sigmoid(receptor_affinity @ stimulus)   # (5,)
    # Hedonic value: sweet/umami positive, bitter negative (innate)
    valence = torch.tensor([1.0, 1.0, -0.3, 0.2, -1.0])
    pleasantness = (response * valence).sum()
    return response, pleasantness
```

---

## 6. Flavor = 多感觉

- "味道"主要是**嗅觉**(retronasal,口后通鼻)+ 味 + 三叉(辣 TRPV1、薄荷 TRPM8、碳酸)
- 捏鼻吃 → 风味大减(实为嗅觉)
- 多感觉整合于 OFC
- 与 [Olfactory_System](Olfactory_System.md) 紧密

---

## 7. 先天 + 可塑

- 甜/鲜 → 先天趋向;苦 → 先天回避(婴儿表情普适)
- 味觉厌恶学习(taste aversion / Garcia effect):单次配对致病 → 长期回避(杏仁核 + 强先天准备)
- 经验 + 文化大幅塑造偏好

---

## 8. 病理 / 调制

- **味觉障碍**:ageusia(无)/ dysgeusia(扭曲)— COVID、化疗、锌缺、神经损
- **状态依赖**:饥饿增甜愉悦,饱足降(alliesthesia)
- **苦味遗传**:PTC/PROP 味盲(TAS2R38 多态)
- 与肥胖 / 糖偏好 / 食欲调控关联

---

## 9. 与 AI / 工程

- 电子舌(电化学传感阵列 + ML)≈ 味觉受体阵列 + 分类
- 群体编码 ↔ 分布式表征
- 风味 = 多模态融合(味+嗅+触)↔ multimodal models
- 趋避价值 ↔ 内在 reward(见 [Reward_System](Reward_System.md))

---

## 10. Common Pitfalls

### 10.1 舌有"味觉地图"

经典分区图是误传;各味全舌可感(密度略异)。

### 10.2 味 = 风味

风味主要靠嗅觉(retronasal);捏鼻验证。

### 10.3 只有 4 味

Umami(鲜)是第五;脂肪等候选研究中。

### 10.4 味觉不经丘脑

味觉**经** VPM 丘脑;嗅觉才不经。

### 10.5 苦 = 不好

苦是避毒先天偏向;非客观"坏"(咖啡/苦瓜文化习得)。

---

## 11. Related Concepts

- **同节**:[Olfactory_System](Olfactory_System.md)、[Somatosensory](Somatosensory.md)、[Reward_System](Reward_System.md)
- **解剖**:[Brainstem](../01_Neuroanatomy/Brainstem.md)(NTS)、[Thalamus](../01_Neuroanatomy/Thalamus.md)
- **认知**:[Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.md)(value)

---

## References

1. **Chandrashekar, J. et al.** "The receptors and cells for mammalian taste." *Nature*, 2006.
2. **Yarmolinsky, D. A. et al.** "Common sense about taste: from mammals to insects." *Cell*, 2009.
3. **Small, D. M.** "Flavor is in the brain." *Physiol Behav*, 2012.
4. **Garcia, J. et al.** "Conditioned aversion to saccharin resulting from exposure to gamma radiation." *Science*, 1955.
