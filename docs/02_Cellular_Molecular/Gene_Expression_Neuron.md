# 神经元基因表达 (Activity-Dependent Gene Expression)

> *神经活动 → Ca²⁺/cAMP → 转录因子(CREB)→ 即早基因(IEG:c-fos、Arc、Zif268)→ 晚期效应基因 → 长期记忆 + 结构可塑。区分短期(磷酸化,无需转录)vs 长期(需转录/翻译)。表观遗传(甲基化/组蛋白)调长程。c-fos 是"激活标记"成像金标准。*
>
> **难度**:Advanced
> **前置知识**:[Second_Messengers](Second_Messengers.md)、[LTP_LTD](LTP_LTD.md)

---

## 1. 信号 → 基因级联

```
活动 → Ca²⁺ 内流(NMDA/VGCC)+ cAMP
   ↓ CaMKIV / PKA / MAPK 入核
磷酸化 CREB(Ser133)
   ↓ + CBP 共激活
即早基因(IEG)转录:c-fos, Arc, Egr1/Zif268, Npas4
   ↓
晚期反应基因 → 受体/骨架/突触蛋白
   ↓
长期记忆 + 结构可塑(数小时-天)
```

---

## 2. 短期 vs 长期(分子界限)

| | 短期记忆 | 长期记忆 |
|---|---|---|
| 机制 | 已有蛋白磷酸化 | 新转录 + 翻译 |
| 时程 | 分钟-小时 | 小时-终生 |
| 阻断 | — | 转录/翻译抑制剂阻断 |
| Kandel 海兔 | 短时易化 | 长时易化(需 CREB) |

→ "长期记忆需新蛋白合成"(经典 anisomycin 实验)。

---

## 3. 即早基因 (IEG)

| IEG | 角色 |
|---|---|
| **c-fos / c-jun** | 转录因子(AP-1)→ 下游基因 |
| **Arc/Arg3.1** | 效应 IEG;AMPA 受体内化、突触缩放 |
| **Egr1 (Zif268)** | 转录因子;LTP 维持 |
| **Npas4** | 活动诱导;调兴奋/抑制平衡 |

IEG 无需新蛋白即转录(已有 TF 磷酸化触发)。

---

## 4. PyTorch — 活动阈值触发转录

```python
import torch

def activity_dependent_transcription(ca_signal, threshold=0.6, T=100):
    """Sustained Ca2+ -> CREB -> IEG; transient -> no transcription."""
    creb_p = 0.0
    ieg = []
    for t in range(T):
        creb_p += 0.1 * (ca_signal[t] - 0.3 * creb_p)   # integrate + decay
        # IEG transcribed only if CREB phosphorylation crosses threshold
        ieg.append(1.0 if creb_p > threshold else 0.0)
    return ieg   # only strong/sustained activity flips gene program
```

---

## 5. c-fos 成像 — 激活标记

- 神经活动 → c-fos 表达 → 抗体染色/报告基因 → "哪些神经元刚被激活"
- **TRAP/FosTRAP**:c-fos 驱动可诱导 Cre → 标记 + 重激活记忆 engram(见 [Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md))
- Tonegawa engram 操控经典(光遗传 + c-fos 标记)
- 局限:慢(1-2 h)、阈值依赖、非全部活动

---

## 6. 表观遗传调控

- **DNA 甲基化**(DNMT)、**组蛋白乙酰化**(HAT/HDAC)→ 长程基因可及性
- 学习改变表观标记 → 记忆持久(数月-年)
- HDAC 抑制剂增记忆(实验);压力/早期经验 → 持久表观印记
- 跨代表观(争议)

---

## 7. 与记忆巩固

- **细胞巩固**:转录/翻译窗(数小时)→ engram 稳固
- **系统巩固**:海马→皮层(数月,见 [Memory_Systems](../04_Cognitive_Neuroscience/Memory_Systems.md))
- "Synaptic tag and capture":弱突触"标记"捕获胞体合成蛋白 → 解释关联记忆增强(见 [LTP_LTD](LTP_LTD.md))

---

## 8. 临床

- **CREB 通路**:学习记忆药靶(PDE 抑制剂等)
- **Rett**(MECP2,表观调控因子)、**Rubinstein-Taybi**(CBP)→ ID
- **成瘾**:ΔFosB 累积(稳定 IEG 变体)→ 持久行为改变(见 [Addiction](../08_Neuro_Disorders/Addiction.md))
- **抑郁/应激**:表观 + BDNF 转录改变(见 [Depression](../08_Neuro_Disorders/Depression.md))

---

## 9. 与 AI

- 短期(快权重/激活)vs 长期(慢权重/写入)↔ 双时间尺度学习
- 活动门控转录 ↔ 阈值化巩固 / 经验回放后离线整合
- ΔFosB 累积 ↔ 慢积分变量(成瘾持久性)
- 表观 ↔ meta-plasticity / 学习率的学习

---

## 10. Common Pitfalls

### 10.1 记忆 = 突触权重瞬时改变

长期记忆需**基因表达 + 新蛋白**(转录抑制剂可阻断)。

### 10.2 c-fos = 全部活动

阈值 + 慢(1-2 h);非线性、非全标记。

### 10.3 IEG 都是转录因子

Arc 是**效应** IEG(直接调 AMPA),非 TF。

### 10.4 表观遗传不可逆

可逆 + 动态(HDAC/DNMT 双向);但部分持久。

### 10.5 短期/长期同机制

短期=磷酸化;长期=转录/翻译(分子界限明确)。

---

## 11. Related Concepts

- **同节**:[Second_Messengers](Second_Messengers.md)、[LTP_LTD](LTP_LTD.md)、[Neurotrophins](Neurotrophins.md)、[Synapse](Synapse.md)
- **系统**:[Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)
- **认知**:[Memory_Systems](../04_Cognitive_Neuroscience/Memory_Systems.md)
- **疾病**:[Addiction](../08_Neuro_Disorders/Addiction.md)、[Depression](../08_Neuro_Disorders/Depression.md)

---

## References

1. **Kandel, E. R.** "The molecular biology of memory storage: a dialogue between genes and synapses." *Science*, 2001.
2. **West, A. E. & Greenberg, M. E.** "Neuronal activity-regulated gene transcription in synapse development and cognitive function." *Cold Spring Harb Perspect Biol*, 2011.
3. **Tonegawa, S. et al.** "Memory engram cells have come of age." *Neuron*, 2015.
4. **Day, J. J. & Sweatt, J. D.** "Epigenetic mechanisms in cognition." *Neuron*, 2011.
