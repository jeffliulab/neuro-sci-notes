# 神经科学统计 (Statistics in Neuroscience)

> *神经科学数据高维、噪声大、样本小 — 统计陷阱多。复制危机(Button 2013 低统计功效)、双重浸入(double dipping, Kriegeskorte 2009)、多重比较(fMRI 死鲑鱼)、循环分析是经典坑。现代:mixed-effects models、permutation test、cluster correction、预注册。理解统计是读懂文献的前提。*
>
> **难度**:Intermediate
> **前置知识**:基础概率统计

---

## 1. 神经数据特点

- **高维**:百万 voxel / 千 neuron
- **噪声大**:trial-to-trial variability
- **样本小**:动物 / 患者 n 常 < 20
- **嵌套结构**:trial < neuron < animal < group
- **时间相关**:autocorrelation

---

## 2. 多重比较问题

- fMRI:~ 100,000 voxel,每个做 t-test
- α=0.05 → ~ 5000 假阳性
- **死鲑鱼研究**(Bennett 2009):死三文鱼 fMRI 现"激活"— 讽刺多重比较失控

### 校正方法

- **Bonferroni**: 过保守
- **FDR** (Benjamini-Hochberg): 控假发现率
- **Cluster-based permutation**: 利用空间相关(主流)
- **Random field theory**

---

## 3. 双重浸入 (Double Dipping)

- 用同一数据**选择** + **检验**
- 例:挑响应最强 voxel → 再测其响应(必然显著!)
- Kriegeskorte 2009 警示
- 解法:独立 train/test split

---

## 4. 统计功效危机

- Button 2013:neuroscience 中位 power ~ 20%
- 低 power → 真效应漏检 + 显著结果膨胀(winner's curse)
- 小样本 + 大效应报告 → 不可复制

---

## 5. p-hacking + Garden of Forking Paths

- 多分析路径 → 选显著的报
- 灵活预处理 / 排除标准
- 解法:**预注册** (preregistration)、registered reports

---

## 6. 推荐方法

| 问题 | 方法 |
|---|---|
| 嵌套数据 | Linear mixed-effects models |
| 多重比较 | Cluster permutation / FDR |
| 小样本 | Permutation / bootstrap |
| 选择偏差 | Cross-validation, held-out |
| 效应量 | 报 effect size + CI(非仅 p) |
| 可复制 | Preregistration |

---

## 7. PyTorch/Numpy — Permutation Test

```python
import numpy as np

def permutation_test(group_a, group_b, n_perm=10000):
    """Non-parametric test of mean difference."""
    observed = group_a.mean() - group_b.mean()
    combined = np.concatenate([group_a, group_b])
    n_a = len(group_a)
    count = 0
    for _ in range(n_perm):
        np.random.shuffle(combined)
        diff = combined[:n_a].mean() - combined[n_a:].mean()
        if abs(diff) >= abs(observed):
            count += 1
    return count / n_perm   # empirical p-value
```

---

## 8. 贝叶斯视角

- Bayes factor 替代 p-value
- 量化"支持 null 多少"(p-value 不能)
- Hierarchical Bayesian models 自然处理嵌套
- 但需 prior 选择

---

## 9. Decoding / MVPA 陷阱

- 高维 decoder 易 overfit
- 必须 cross-validation
- Above-chance ≠ 大效应
- Label leakage(时间相关 → train/test 泄漏)

---

## 10. 神经数据复制运动

- **OHBM**、**registered reports**
- **Neuroimaging data sharing** (OpenNeuro)
- NARPS (2020):70 团队分析同一 fMRI → 结论差异巨大
- 强调 analytic flexibility 问题

---

## 11. Common Pitfalls

### 11.1 p < 0.05 = 真

低 power 下,显著也常假;需 effect size + 复制。

### 11.2 挑 neuron 再测

Double dipping → 循环论证。必独立数据。

### 11.3 大样本不需校正

样本大也需多重比较校正(voxel 数才是问题)。

### 11.4 n=动物数

trial ≠ 独立样本;需 mixed model(pseudoreplication)。

### 11.5 Decoding 高 = brain 这样用

decoder 是观察者工具,非 brain 机制证据。

---

## 12. Related Concepts

- **同节**:[Research Methods](Research_Methods.md)、[Levels of Analysis](Levels_of_Analysis.md)
- **前沿**:[fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.md)、[Calcium Imaging](../07_Neurotech_Frontiers/Calcium_Imaging.md)
- **AI**: cross-validation、overfitting

---

## References

1. **Button, K. S. et al.** "Power failure: why small sample size undermines the reliability of neuroscience." *Nat Rev Neurosci*, 2013.
2. **Kriegeskorte, N. et al.** "Circular analysis in systems neuroscience." *Nat Neurosci*, 2009.
3. **Bennett, C. M. et al.** "Neural correlates of interspecies perspective taking in the post-mortem Atlantic Salmon." *J Serendipitous Unexpected Results*, 2010.
4. **Botvinik-Nezer, R. et al.** "Variability in the analysis of a single neuroimaging dataset by many teams (NARPS)." *Nature*, 2020.
