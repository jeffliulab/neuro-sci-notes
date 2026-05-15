# 神经伦理学 (Neuroethics)

> *Neuroethics 研究 neuroscience 的伦理含义:认知增强、读心、神经隐私、意识判定、BCI、神经法学。随 Neuralink、fMRI mind-reading、大脑类器官发展而紧迫。"Neurorights"(认知自由、心理隐私)立法在 Chile、UNESCO 推进。Roskies 2002 分 "ethics of neuroscience" + "neuroscience of ethics"。*
>
> **难度**:Beginner-Intermediate
> **前置知识**:无

---

## 1. 两大分支 (Roskies 2002)

- **Ethics of neuroscience**: 神经科学研究 + 应用的伦理(增强、隐私、实验)
- **Neuroscience of ethics**: 道德判断的神经基础(moral cognition)

---

## 2. 核心议题

| 议题 | 问题 |
|---|---|
| Cognitive enhancement | 聪明药公平吗? |
| Mental privacy | fMRI 读心 → 隐私? |
| Neuro-prediction | 用脑预测犯罪? |
| Consciousness | 植物人有意识? AI 有? |
| Free will | 神经决定论 → 责任? |
| BCI | 身份 + agency |
| Brain organoids | 类器官有意识? |
| Neuromarketing | 操纵消费? |

---

## 3. 认知增强

- Modafinil、Ritalin、tDCS 非治疗性使用
- 类比体育兴奋剂(公平)
- "Cosmetic neurology"
- 公平 vs 自主 vs 强制(职场压力)

---

## 4. 心理隐私 + 读心

- fMRI decoding 重建图像 / 语义(Tang 2023 语义解码)
- Lie detection(fMRI、EEG P300)— 法庭可采性争议
- "Neurorights":mental privacy 立法
- Chile 2021 宪法首列 neurorights

---

## 5. 意识判定

- 植物状态 vs 最小意识状态
- Owen 2006:fMRI 命令跟随检测隐藏意识
- 影响撤除生命支持决定
- AI 意识? Brain organoid 意识?

---

## 6. Free Will + 神经法学

- Libet 1983:RP 早于意识决定 ~ 300 ms(争议)
- 神经决定论 → 法律责任?
- "My brain made me do it" 辩护
- 当代多数 compatibilist 立场

---

## 7. PyTorch — Mental Privacy 风险演示

```python
import torch
import torch.nn as nn

class BrainDecoder(nn.Module):
    """fMRI → stimulus reconstruction (privacy concern demo)."""
    def __init__(self, n_voxels=10000, latent=512):
        super().__init__()
        self.enc = nn.Linear(n_voxels, latent)
        self.dec = nn.Linear(latent, 784)  # reconstruct seen image
    def forward(self, fmri):
        z = torch.relu(self.enc(fmri))
        return torch.sigmoid(self.dec(z))
# Ethical question: who consents to this being run on their brain data?
```

---

## 8. BCI 伦理

- Agency:动作是"我"还是 decoder?
- Identity:植入改变人格?(DBS 案例报告)
- Data ownership:脑数据谁拥有?
- Access equity:谁负担得起?
- 见 [Neuralink](../07_Neurotech_Frontiers/Neuralink.md)

---

## 9. Brain Organoids

- 培养皿 mini-brain(类器官)
- 是否可能有 sentience?
- "Assembloid"、organoid intelligence (OI)
- 监管 vacuum,2023+ 讨论升温

---

## 10. 治理框架

- **UNESCO** 2023 neurotechnology ethics recommendation
- **Chile** 2021 宪法 neurorights
- **EU AI Act**:涉 emotion recognition 限制
- **OECD** 2019 neurotechnology 原则
- Ienca & Andorno 2017:4 项 neurorights(认知自由、心理隐私、心理完整、心理连续)

---

## 11. Common Pitfalls

### 11.1 fMRI 能读任意思想

仅能 decode 受训范围;非全知读心。

### 11.2 Libet 证明无自由意志

实验 + 解读均有争议;不能简单下此结论。

### 11.3 增强 = 纯个人选择

涉社会公平、强制压力,非纯自主。

### 11.4 Organoid = mini human brain

远无完整结构 / 输入;sentience 极不可能(当前)。

### 11.5 神经科学 → 道德结论

Is-ought gap:描述 ≠ 规范。

---

## 12. Related Concepts

- **同节**:[Research Methods](Research_Methods.md)、[Levels of Analysis](Levels_of_Analysis.md)
- **认知**:[Consciousness](../04_Cognitive_Neuroscience/Consciousness.md)、[Social Cognition](../04_Cognitive_Neuroscience/Social_Cognition.md)
- **前沿**:[Neuralink](../07_Neurotech_Frontiers/Neuralink.md)

---

## References

1. **Roskies, A.** "Neuroethics for the new millennium." *Neuron*, 2002.
2. **Ienca, M. & Andorno, R.** "Towards new human rights in the age of neuroscience and neurotechnology." *Life Sci Soc Policy*, 2017.
3. **Owen, A. M. et al.** "Detecting awareness in the vegetative state." *Science*, 2006.
4. **Yuste, R. et al.** "Four ethical priorities for neurotechnologies and AI." *Nature*, 2017.
