# 信息论与大脑 (Information Theory in Neuroscience)

> *Shannon 1948 信息论是量化神经编码的语言:entropy、mutual information、channel capacity、Fisher information。用于测 spike train 携带多少 stimulus 信息、感觉系统信道容量、redundancy。Information Bottleneck 连接到深度学习。是 efficient coding、neural coding 的数学基础。*
>
> **难度**:Advanced
> **前置知识**:[Neural Coding](../00_Foundations/Neural_Coding.md)、[Efficient Coding](Efficient_Coding.md)、概率

---

## 1. 基本量

- **Entropy**:$H(X) = -\sum p(x)\log p(x)$ — 不确定性
- **Conditional entropy**:$H(X|Y)$
- **Mutual information**:$I(X;Y) = H(X) - H(X|Y)$
- **Channel capacity**:$C = \max_{p(x)} I(X;Y)$
- 单位:bits(log₂)

---

## 2. 神经编码的 MI

$$I(S;R) = \sum_{s,r} p(s,r) \log \frac{p(r|s)}{p(r)}$$

- $S$:stimulus,$R$:neural response(spike count / pattern)
- 衡量 spike train 携带多少 stimulus 信息
- 上界 = response entropy(编码容量)

---

## 3. 估计方法 + 偏差

- 有限样本 → MI 估计**正偏**(Panzeri-Treves correction)
- 方法:direct、binning、KNN(KSG)、neural network(MINE)
- 高维 → curse of dimensionality
- 需 bias correction + 充足数据

---

## 4. Spike Train 信息

- **Spike count**:rate coding 信息
- **Spike timing**:temporal precision 增 MI(Strong 1998)
- **Information rate**:bits/sec
- 苍蝇 H1 神经元 ~ 数十 bits/sec(经典)

---

## 5. PyTorch — Mutual Information 估计

```python
import torch

def mutual_information_discrete(joint_counts):
    """MI from a joint histogram p(s, r)."""
    p = joint_counts / joint_counts.sum()
    px = p.sum(dim=1, keepdim=True)
    py = p.sum(dim=0, keepdim=True)
    mask = p > 0
    mi = (p[mask] * torch.log2(p[mask] / (px @ py)[mask])).sum()
    return mi  # bits
```

---

## 6. Fisher Information

$$J(\theta) = \mathbb{E}\!\left[\left(\frac{\partial \ln p(r|\theta)}{\partial \theta}\right)^2\right]$$

- 编码精度的局部度量
- Cramér-Rao:$\text{Var}(\hat\theta) \geq 1/J(\theta)$
- Population:tuning curve 斜率决定 Fisher info
- 关联 optimal stimulus coding

---

## 7. Information Bottleneck (Tishby)

$$\min \; I(X;T) - \beta I(T;Y)$$

- 压缩 $X→T$ 同时保留 $Y$ 相关信息
- 解释:感觉系统保 task-relevant、丢冗余
- Tishby 2017:DL 训练过程 = IB 两阶段(争议)
- 连接 efficient coding(见 [Efficient Coding](Efficient_Coding.md))

---

## 8. Redundancy + Synergy

- 多 neuron 联合编码:
  - **Redundant**:重复信息(鲁棒)
  - **Synergistic**:联合 > 各自之和
  - **Independent**
- Partial Information Decomposition(Williams & Beer)
- 群体编码效率分析工具

---

## 9. 与 AI

| 信息论 | AI |
|---|---|
| Mutual information | InfoNCE、MINE、对比学习 |
| Information Bottleneck | 表征学习、压缩 |
| Channel capacity | rate-distortion、量化 |
| Entropy | 交叉熵 loss、最大熵 RL |
| MI estimation | representation probing |

---

## 10. Common Pitfalls

### 10.1 MI 估计无偏

有限样本正偏;必 bias correction。

### 10.2 高 MI = brain 用全部

可能含 brain 不读出的信息(decodable ≠ used)。

### 10.3 Entropy = 信息量(语义)

Shannon 信息 ≠ meaning;只是 statistical surprise。

### 10.4 IB 解释 DL 已定论

Tishby DL-IB 主张有 replication 争议。

### 10.5 更多 bits = 更好编码

需 task-relevant;无关信息无用(IB 视角)。

---

## 11. Related Concepts

- **同节**:[Efficient Coding](Efficient_Coding.md)、[Predictive Coding](Predictive_Coding.md)、[Free Energy Principle](Free_Energy_Principle.md)
- **基础**:[Neural Coding](../00_Foundations/Neural_Coding.md)、[Statistics in Neuroscience](../00_Foundations/Statistics_in_Neuroscience.md)
- **AI**: 对比学习、表征学习

---

## References

1. **Shannon, C. E.** "A mathematical theory of communication." *Bell Syst Tech J*, 1948.
2. **Borst, A. & Theunissen, F. E.** "Information theory and neural coding." *Nat Neurosci*, 1999.
3. **Panzeri, S. et al.** "Correcting for the sampling bias problem in spike train information measures." *J Neurophysiol*, 2007.
4. **Tishby, N. & Zaslavsky, N.** "Deep learning and the information bottleneck principle." *IEEE ITW*, 2015.
