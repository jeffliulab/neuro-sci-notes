# 神经发育 (Neurodevelopment)

> *从单细胞到 86 billion neuron 网络:neural tube → 增殖 → 迁移 → 分化 → 轴突导向 → 突触形成 → 修剪。关键过程:neurogenesis、apoptosis(过量 neuron 凋亡)、synaptic pruning、myelination(持续至 25 岁)。Critical periods 决定 plasticity 窗口。发育异常 → autism、schizophrenia、ID。*
>
> **难度**:Intermediate
> **前置知识**:[Nervous System Overview](Nervous_System_Overview.md)、细胞生物基础

---

## 1. 主要阶段

```
Neural induction (神经板)
   ↓
Neural tube formation (神经管)
   ↓
Proliferation (神经前体增殖)
   ↓
Migration (放射状迁移)
   ↓
Differentiation (分化为特定 neuron)
   ↓
Axon guidance (生长锥导向)
   ↓
Synaptogenesis (突触形成)
   ↓
Pruning + Myelination (修剪 + 髓鞘)
```

---

## 2. Neural Induction

- Spemann organizer(1924 Nobel)
- BMP 抑制 → 外胚层 → 神经板
- Default model:神经是 ectoderm 默认命运

---

## 3. Neurulation

- Neural plate → neural groove → neural tube
- 闭合失败 → spina bifida / anencephaly
- 叶酸(folate)预防 neural tube defects

---

## 4. Proliferation + Migration

- Ventricular zone radial glia → 神经前体
- **Radial migration**:沿 radial glia 爬升(inside-out:深层先生成)
- **Tangential migration**:interneuron 横向迁
- 异常 → lissencephaly(平滑脑)

---

## 5. Axon Guidance

- 生长锥(growth cone)感受 cue
- **Attractants / Repellents**: Netrin、Slit、Ephrin、Semaphorin
- Gradient sensing → 精确 wiring
- 例:视网膜 → tectum retinotopic map(Sperry chemoaffinity)

---

## 6. Synaptogenesis + 过量

- 早期突触 **过量**生成
- 2-3 岁突触密度峰(超成人)
- "Blooming and pruning"

---

## 7. Apoptosis + Pruning

- ~ 50% neuron 程序性死亡(neurotrophic factor 竞争 — NGF)
- Synaptic pruning:用进废退("use it or lose it")
- 青春期大量 prune(尤 PFC)
- 异常 pruning → schizophrenia(过度)/ autism(不足)假说

---

## 8. Critical Periods

- 特定时窗 plasticity 高
- Hubel & Wiesel:monocular deprivation → 永久 amblyopia(若过窗)
- Language:~ 7 岁后第二语言难达 native
- 机制:GABA 成熟、PV interneuron、perineuronal nets

---

## 9. Myelination

- 出生后持续至 ~ 25 岁
- PFC 最晚(解释青少年决策不成熟)
- 经验依赖(activity-dependent myelination)

---

## 10. PyTorch — Pruning 类比

```python
import torch

def developmental_pruning(weights, activity, prune_ratio=0.5):
    """Use-it-or-lose-it: prune low-activity synapses."""
    importance = weights.abs() * activity  # active + strong survive
    threshold = importance.flatten().quantile(prune_ratio)
    mask = importance >= threshold
    return weights * mask  # pruned network (sparse, like adult brain)
```

→ 类似 deep learning 的 magnitude pruning / lottery ticket。

---

## 11. 发育障碍

- **Autism**: synaptic / connectivity 异常
- **Schizophrenia**: 青春期 over-pruning 假说
- **Fragile X**: FMRP → spine 异常
- **Rett**: MECP2
- **Fetal alcohol**: migration / apoptosis 干扰
- **Microcephaly**: ZIKV、基因 → 增殖不足

---

## 12. Common Pitfalls

### 12.1 Neuron 不再生

成人 hippocampus / olfactory bulb 仍有 neurogenesis(但量小、有争议)。

### 12.2 More synapse = 更聪明

Pruning 是 feature 非 bug;精炼网络更高效。

### 12.3 Critical period 绝对关闭

可被 reopened(药物、训练、环境)。

### 12.4 发育 = 仅基因

经验 + activity 强烈塑造(nature × nurture)。

### 12.5 Myelination 童年完成

PFC myelination 持续到 ~ 25 岁。

---

## 13. Related Concepts

- **同节**:[Nervous System Overview](Nervous_System_Overview.md)、[Neuron Doctrine](Neuron_Doctrine.md)
- **细胞**:[Synapse](../02_Cellular_Molecular/Synapse.md)、[LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)
- **疾病**:[Autism](../08_Neuro_Disorders/Autism.md)、[Schizophrenia](../08_Neuro_Disorders/Schizophrenia.md)

---

## References

1. **Sanes, D. H., Reh, T. A., Harris, W. A.** *Development of the Nervous System*. 3rd ed., 2011.
2. **Hubel, D. H. & Wiesel, T. N.** "The period of susceptibility to the physiological effects of unilateral eye closure in kittens." *J Physiol*, 1970.
3. **Sperry, R. W.** "Chemoaffinity in the orderly growth of nerve fiber patterns." *PNAS*, 1963.
4. **Stiles, J. & Jernigan, T. L.** "The basics of brain development." *Neuropsychol Rev*, 2010.
