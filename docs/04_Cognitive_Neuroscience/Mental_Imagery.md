# 心理意象 (Mental Imagery)

> *Mental imagery 是无外部刺激下"在脑中看/听"。Kosslyn-Pylyshyn 之争(类比 depictive vs 命题 propositional)是认知科学经典。证据:imagery 激活感觉皮层(V1!)、与知觉共享机制。Aphantasia(无意象)是极端个体差异。连接知觉、记忆、想象、创造。*
>
> **难度**:Intermediate
> **前置知识**:[Visual System](../03_Systems_Neuroscience/Visual_System.md)、[Working Memory](Working_Memory.md)

---

## 1. 核心问题

- 想象一只猫时,脑中是**图像式**(depictive)还是**命题式**(propositional)?
- **Kosslyn**:类比表征(空间、有"图")
- **Pylyshyn**:命题 + tacit knowledge(无真"图")
- 神经证据偏向 Kosslyn(但 propositional 成分存在)

---

## 2. 经典行为证据

- **心理旋转**(Shepard & Metzler 1971):反应时 ∝ 旋转角度(线性!)→ 类比表征
- **意象扫描**:扫描距离 ∝ 时间
- **图像大小效应**:小图细节判断慢
- → 行为模式像操作真实空间表征

---

## 3. 神经证据

- **V1 激活**:生动视觉意象激活早期视皮层(Kosslyn fMRI)
- **拓扑映射**:意象在 V1 保留 retinotopy(大图占更多 V1)
- **知觉-意象共享**:解码器跨知觉/意象泛化
- **反向层级**:意象更依赖 top-down(frontoparietal → 感觉)

---

## 4. PyTorch — 知觉 vs 意象(共享 decoder)

```python
import torch

def perception_vs_imagery(stimulus, top_down, mode='perceive'):
    """Shared sensory representation; imagery is top-down driven."""
    if mode == 'perceive':
        rep = stimulus + 0.3 * top_down            # bottom-up dominant
    else:  # imagine: no stimulus, top-down generates pattern
        rep = top_down                              # generative, weaker, noisier
    return torch.tanh(rep)
# A decoder trained on perception partially generalizes to imagery
```

---

## 5. Aphantasia / Hyperphantasia

- **Aphantasia**:无自主视觉意象(~ 1-4%,Zeman 2015 命名)
- **Hyperphantasia**:意象逼真如知觉
- 极端个体差异(连续谱)
- aphantasia 者仍可空间推理(命题/其他策略)→ 意象非推理必需

---

## 6. 多模态意象

- 视觉(最研究)、听觉(脑内旋律)、运动(motor imagery)、嗅/味
- **Motor imagery**:激活运动系统(无实际动)→ 运动康复 + BCI 用
- 与对应感觉/运动皮层共享

---

## 7. 功能作用

- **记忆**:生动 imagery → 更好编码(method of loci)
- **规划/模拟**:预演未来(prospection,与 episodic 共享网络)
- **想象/创造**:重组表征(见 [Creativity](Creativity.md))
- **情绪**:意象比文字更强情绪(意象暴露疗法)

---

## 8. 临床 + 应用

- **PTSD**:侵入性意象(见 [PTSD](../08_Neuro_Disorders/PTSD.md))
- **意象暴露/重写**:治疗
- **Motor imagery BCI**:想象动作 → 控制(见 [EEG](../07_Neurotech_Frontiers/EEG.md))
- **运动训练**:意象练习提升表现

---

## 9. 与 AI

- 生成模型(diffusion)≈ top-down "想象"图像
- "Mental simulation" ↔ world model rollout(见 ai-notes 世界模型)
- fMRI imagery decoding/重建(Takagi 2023 + diffusion)
- 知觉-意象共享 ↔ encoder-decoder 复用

---

## 10. Common Pitfalls

### 10.1 意象 = 脑中真有图片

depictive vs propositional 长期争论;是功能性类比非字面像素。

### 10.2 V1 激活证明纯图像

V1 卷入但 top-down + 命题成分共存。

### 10.3 人人意象生动

Aphantasia↔hyperphantasia 巨大个体差异。

### 10.4 无意象 = 无法思考视觉问题

Aphantasia 者用替代策略仍能空间推理。

### 10.5 意象 = 弱知觉

机制重叠但方向相反(top-down 主导)+ 生成性。

---

## 11. Related Concepts

- **同节**:[Working Memory](Working_Memory.md)、[Creativity](Creativity.md)、[Consciousness](Consciousness.md)、[Embodied_Cognition](Embodied_Cognition.md)
- **系统**:[Visual System](../03_Systems_Neuroscience/Visual_System.md)
- **疾病**:[PTSD](../08_Neuro_Disorders/PTSD.md)
- **AI**: 生成模型、world model

---

## References

1. **Shepard, R. N. & Metzler, J.** "Mental rotation of three-dimensional objects." *Science*, 1971.
2. **Kosslyn, S. M. et al.** "Neural foundations of imagery." *Nat Rev Neurosci*, 2001.
3. **Pearson, J.** "The human imagination: the cognitive neuroscience of visual mental imagery." *Nat Rev Neurosci*, 2019.
4. **Zeman, A. et al.** "Lives without imagery — congenital aphantasia." *Cortex*, 2015.
