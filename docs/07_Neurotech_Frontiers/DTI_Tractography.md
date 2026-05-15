# 弥散张量成像与纤维追踪 (DTI & Tractography)

> *DTI 利用水分子在白质中沿轴突束的**各向异性扩散**重建大尺度神经纤维通路。FA(各向异性分数)、tractography(纤维追踪)是 macro-connectome(见 [Connectomics](../00_Foundations/Connectomics.md))与临床(术前规划、白质病)主力。但仅 mm 级纤维束,非单 axon,且有"交叉纤维"难题。*
>
> **难度**:Advanced
> **前置知识**:[Connectomics](../00_Foundations/Connectomics.md)、MRI 基础

---

## 1. 原理

- 水分子布朗运动:白质内沿轴突束**方向扩散快**,垂直慢(髓鞘 + 膜约束)
- 施加多方向扩散梯度 → 测各方向扩散
- 拟合 **扩散张量**(3×3 对称矩阵)→ 主特征向量 = 局部纤维方向

---

## 2. 关键指标

| 指标 | 意义 |
|---|---|
| **FA**(fractional anisotropy) | 各向异性程度(0=等向,1=单向)→ 白质完整性 |
| **MD**(mean diffusivity) | 平均扩散率 |
| **AD / RD** | 轴向 / 径向扩散(髓鞘 vs 轴突) |
| 主特征向量 | 纤维方向(tractography 用) |

---

## 3. Tractography

- 从 seed 体素沿主方向逐步"流线"追踪 → 重建纤维束
- **确定性**(deterministic):单方向跟随
- **概率性**(probabilistic):采样方向分布(处理不确定)
- 主要束:皮质脊髓束、弓状束(语言)、胼胝体、扣带等

---

## 4. PyTorch — 张量拟合 + FA

```python
import torch

def fit_tensor_and_fa(signals, bvecs, bval, S0):
    """signals: (n_dir,) diffusion-weighted; bvecs: (n_dir,3)."""
    # log(S/S0) = -b * g^T D g  -> linear least squares for D (6 unique)
    y = -torch.log(signals / S0) / bval
    # Design matrix from gradient directions
    g = bvecs
    A = torch.stack([g[:,0]**2, g[:,1]**2, g[:,2]**2,
                     2*g[:,0]*g[:,1], 2*g[:,0]*g[:,2], 2*g[:,1]*g[:,2]], dim=1)
    d = torch.linalg.lstsq(A, y).solution
    D = torch.tensor([[d[0],d[3],d[4]],[d[3],d[1],d[5]],[d[4],d[5],d[2]]])
    ev = torch.linalg.eigvalsh(D)
    md = ev.mean()
    fa = torch.sqrt(1.5 * ((ev - md)**2).sum() / (ev**2).sum())
    return D, fa
```

---

## 5. 交叉纤维问题

- 单张量假设一个方向 → 体素内多束交叉(~ 90% 白质体素!)时失败
- 解法:**HARDI**、**Q-ball**、**CSD**(constrained spherical deconvolution)、**DSI**
- 高 b 值 + 多方向 → 解多纤维方向(fiber ODF)

---

## 6. 应用

- **术前规划**:肿瘤旁纤维束保护(避开运动/语言束)
- **白质病**:MS、TBI(DAI)、leukodystrophy → FA↓
- **发育 / 老化**:髓鞘化轨迹
- **精神病**:connectopathy(SCZ 弓状束 FA↓ 等)
- **Macro-connectome**:Human Connectome Project

---

## 7. 与显微连接组对比

| | DTI tractography | EM connectome |
|---|---|---|
| 尺度 | mm 纤维束 | nm 单 synapse |
| 活体 | ✓ | ✗(postmortem) |
| 全脑 | ✓ | 极局部 |
| 精度 | 低(间接) | 极高 |

→ 互补:DTI 给活体大尺度图,EM 给微观真值(见 [Connectomics](../00_Foundations/Connectomics.md))。

---

## 8. 局限 + 陷阱

- **非真神经束**:推断自水扩散,有 false positive/negative
- 交叉/kissing/fanning 纤维歧义
- 不分方向(A→B vs B→A)
- FA 非特异(髓鞘 / 轴突 / 水肿都影响)
- "Tractography 不是金标准"(Maier-Hein 2017 大比较)

---

## 9. 与 AI

- 深度学习 tractography(学纤维方向 / 直接 streamline)
- DTI → connectome → GNN 疾病分类
- 数据驱动 fiber ODF 估计

---

## 10. Common Pitfalls

### 10.1 Tractography = 真实轴突束

是统计推断;高 false positive(Maier-Hein 2017)。

### 10.2 FA↓ = 轴突损伤

非特异:髓鞘、水肿、交叉纤维都影响。

### 10.3 显示方向性(A→B)

扩散无方向;不能定 directionality。

### 10.4 单张量足够

~ 90% 体素有交叉纤维 → 需 HARDI/CSD。

### 10.5 = 单 neuron connectome

mm 级束,非单 axon;与 EM 互补非等价。

---

## 11. Related Concepts

- **同节**:[fMRI BOLD](fMRI_BOLD.md)、[MEG](MEG.md)
- **基础**:[Connectomics](../00_Foundations/Connectomics.md)、[Research Methods](../00_Foundations/Research_Methods.md)
- **疾病**:[Multiple Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.md)、[Traumatic Brain Injury](../08_Neuro_Disorders/Traumatic_Brain_Injury.md)

---

## References

1. **Basser, P. J. et al.** "MR diffusion tensor spectroscopy and imaging." *Biophys J*, 1994.
2. **Tournier, J.-D. et al.** "Constrained spherical deconvolution." *NeuroImage*, 2007.
3. **Maier-Hein, K. H. et al.** "The challenge of mapping the human connectome based on diffusion tractography." *Nat Commun*, 2017.
4. **Jones, D. K. et al.** "White matter integrity, fiber count, and other fallacies." *NeuroImage*, 2013.
