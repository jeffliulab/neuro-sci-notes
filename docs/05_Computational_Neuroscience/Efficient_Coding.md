# 高效编码假说 (Efficient Coding Hypothesis)

> *Barlow 1961:感觉系统演化为高效表征自然统计 — 去冗余、最大化信息、能量约束。预测 retina/V1 receptive field、sparse coding、natural scene statistics 适应。是 normative theory 典范:从"为何"(信息论)推出"是什么"(神经性质)。直接关联 autoencoder、infomax、稀疏表征。*
>
> **难度**:Advanced
> **前置知识**:[Neural Coding](../00_Foundations/Neural_Coding.md)、信息论基础

---

## 1. 核心假说

- 感觉信号有大量**冗余**(自然图像像素高度相关)
- 神经资源有限(spike 耗能 — 见 [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md))
- → 演化压力:用最少 spike 编码最多信息
- Barlow:"减少冗余"原则

---

## 2. Infomax 原则

最大化输入输出互信息:
$$\max_{f} \; I(X; Y), \quad Y = f(X)$$

- Linsker 1988 infomax
- 约束下(噪声、能量)→ 预测 receptive field
- 等价(高斯下)于去相关 + whitening

---

## 3. 预测的神经性质

| 预测 | 观测 |
|---|---|
| Retina center-surround | ✓ whitening 自然图像 1/f² 谱 |
| V1 oriented Gabor | ✓ sparse coding (Olshausen 1996) |
| Sparse firing | ✓ 低 average rate |
| 适应 (adaptation) | ✓ 重映射到当前统计 |
| Color opponent | ✓ 去相关 LMS cone |

---

## 4. Sparse Coding (Olshausen & Field 1996)

目标:重建自然图像 + 稀疏:
$$\min \; \|I - \sum_i a_i \phi_i\|^2 + \lambda \sum_i |a_i|$$

- 学出的 $\phi_i$ = localized、oriented、bandpass — **像 V1 simple cell**!
- 纯从自然图像统计 + 稀疏约束涌现

---

## 5. Redundancy Reduction vs Redundancy Exploitation

- 早期 Barlow:去冗余(独立成分)
- 后期修正:冗余也有用(噪声鲁棒、纠错)
- 现代:efficient ≠ 完全去冗余;是 task + noise 约束下的最优

---

## 6. PyTorch — Sparse Coding (Olshausen)

```python
import torch

def sparse_coding_step(images, dictionary, lam=0.1, lr=0.01, n_inf=50):
    """Infer sparse codes a, then update dictionary phi."""
    a = torch.zeros(dictionary.shape[1], images.shape[1], requires_grad=True)
    opt = torch.optim.SGD([a], lr=lr)
    for _ in range(n_inf):
        opt.zero_grad()
        recon = dictionary @ a
        loss = ((images - recon)**2).sum() + lam * a.abs().sum()
        loss.backward(); opt.step()
    # Dictionary update (Hebbian-like)
    with torch.no_grad():
        resid = images - dictionary @ a
        dictionary += lr * resid @ a.t()
        dictionary /= dictionary.norm(dim=0, keepdim=True) + 1e-6
    return dictionary, a.detach()
```

---

## 7. 适应 (Adaptation)

- 神经元 dynamically 重标定到当前输入统计
- 对比适应、light adaptation
- = 在线 efficient recoding(最大化有效动态范围)
- 解释 aftereffects(motion aftereffect 等)

---

## 8. 与 AI

| Efficient coding | AI |
|---|---|
| Infomax | InfoMax / InfoNCE、对比学习 |
| Sparse coding | sparse autoencoder、L1、dictionary learning |
| Redundancy reduction | ICA、whitening、BatchNorm |
| Predictive 去冗余 | predictive coding、next-token |
| Natural scene statistics | self-supervised pretraining |

---

## 9. Normative 方法论

- "Normative":从最优性原则推出神经性质(非纯描述)
- 对比 mechanistic(怎么实现)
- Efficient coding 是 Marr computational level 典范(见 [Levels of Analysis](../00_Foundations/Levels_of_Analysis.md))

---

## 10. Common Pitfalls

### 10.1 完全去冗余

冗余对鲁棒 + 纠错有用;非全去。

### 10.2 Infomax 唯一目标

Task-relevant info(非全部 info)更准(IB — information bottleneck)。

### 10.3 解释一切 RF

是强约束 + 假设;非所有区适用(高级区 task-driven 更佳)。

### 10.4 Sparse = 越稀越好

过稀 → 信息丢;有最优 λ。

### 10.5 Normative = 真实机制

解释"为何",不保证 brain 用该 algorithm 实现。

---

## 11. Related Concepts

- **同节**:[Predictive Coding](Predictive_Coding.md)、[Bayesian Brain](Bayesian_Brain.md)、[Free Energy Principle](Free_Energy_Principle.md)
- **基础**:[Neural Coding](../00_Foundations/Neural_Coding.md)、[Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md)
- **系统**:[Visual System](../03_Systems_Neuroscience/Visual_System.md)
- **AI**: 自监督、对比学习

---

## References

1. **Barlow, H. B.** "Possible principles underlying the transformation of sensory messages." 1961.
2. **Olshausen, B. A. & Field, D. J.** "Emergence of simple-cell receptive field properties by learning a sparse code for natural images." *Nature*, 1996.
3. **Linsker, R.** "Self-organization in a perceptual network." *Computer*, 1988.
4. **Simoncelli, E. P. & Olshausen, B. A.** "Natural image statistics and neural representation." *Annu Rev Neurosci*, 2001.
