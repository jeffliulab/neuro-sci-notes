# 聚焦超声 (Focused Ultrasound Neuromodulation)

> *Focused ultrasound (FUS) 是唯一能**非侵入 + 深部 + 高空间精度**调节脑的技术。低强度 → 可逆神经调节(机械门控离子通道);高强度 → 热消融(MRgFUS,FDA 批 essential tremor)。+ 微泡 → 暂时开 BBB 递药。是 neuromodulation 最有前景前沿之一。*
>
> **难度**:Advanced
> **前置知识**:[TMS](TMS.md)、[Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md)

---

## 1. 为何独特

| 技术 | 深部 | 空间精度 | 非侵入 |
|---|---|---|---|
| TMS / tDCS | ✗(仅皮层) | 差-中 | ✓ |
| DBS | ✓ | 高 | ✗(植入) |
| **FUS** | ✓ | **高(mm)** | ✓ |

→ FUS 填补"非侵入深部精准"空白。

---

## 2. 强度分级

| 强度 | 效应 | 应用 |
|---|---|---|
| Low (LIFU) | 可逆神经调节 | 研究、抑郁、疼痛 |
| Moderate + 微泡 | 暂时开 BBB | 递药(化疗、基因) |
| High (HIFU) | 热消融(不可逆) | Essential tremor、PD 震颤(MRgFUS) |

---

## 3. 神经调节机制(LIFU)

- 主要 **机械**(非热):声辐射力 + 空化
- 激活**机械门控离子通道**(Piezo1、TRP、K2P)
- 双向(参数依赖):兴奋 or 抑制
- 机制仍部分未明(争议:直接 vs 间接听觉混杂)

---

## 4. MRgFUS — 临床消融

- MRI 引导 + 实时测温
- 数百-千换能器阵列经颅聚焦(相位校正颅骨畸变)
- FDA 批:essential tremor(Vim 丘脑)、PD 震颤
- 无切口、无植入 → 替代部分 DBS / 损毁术

---

## 5. BBB 开放(FUS + 微泡)

- 静注微泡 + FUS → 微泡振荡机械松动 tight junction
- 暂时(数小时)+ 局部 + 可逆开 BBB
- 递送:化疗、抗体、AAV、ASO 入脑
- AD、脑瘤临床试验进行中(见 [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md))

---

## 6. PyTorch — 声场聚焦(简化)

```python
import numpy as np

def fus_focus(N=128, focal_depth=50, wavelength=2.0):
    """Phased array: per-element delay to focus at depth (skull-naive)."""
    elements = np.linspace(-30, 30, N)              # transducer positions
    dist = np.sqrt(elements**2 + focal_depth**2)
    delays = (dist.max() - dist) / wavelength       # phase delays to align
    # Constructive interference at focal point -> mm-scale spot
    return elements, delays
```

---

## 7. 优势 + 局限

| 优 | 局限 |
|---|---|
| 非侵入 + 深部 + mm 精度 | 颅骨衰减/畸变(需相位校正) |
| 可逆(LIFU)或永久(HIFU)可选 | LIFU 机制争议 |
| 可开 BBB 递药 | 设备贵、技术复杂 |
| 无植入感染风险 | 听觉混杂效应(动物) |

---

## 8. 应用前景

- 神经调节:抑郁、OCD、疼痛、成瘾(深部靶点,非侵入)
- 消融:tremor、PD、难治癫痫
- 递药:脑瘤、AD(Aβ 清除 + 抗体递送)
- "Sonogenetics":+ 机械敏感蛋白工程 → 类 optogenetics 但非侵入

---

## 9. 安全

- 热损伤(HIFU 故意;LIFU 须避免)
- 出血(BBB 开放过度 → 微出血)
- 空化失控
- 颅骨产热
- 仍需长期安全数据(尤反复 LIFU)

---

## 10. Common Pitfalls

### 10.1 FUS = 只能消融

低强度可逆神经调节 + BBB 开放是主要前沿。

### 10.2 LIFU 机制已明

机械门控假说主流但仍争议(听觉混杂)。

### 10.3 开 BBB 安全无虑

过度 → 微出血 / 水肿;参数 + 微泡剂量关键。

### 10.4 颅骨无影响

颅骨强衰减 + 畸变;必相位校正(CT-based)。

### 10.5 取代 DBS

互补:FUS 消融不可逆、不可调;DBS 可调可逆。

---

## 11. Related Concepts

- **同节**:[DBS](DBS.md)、[TMS](TMS.md)、[Optogenetics](Optogenetics.md)
- **基础**:[Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md)
- **疾病**:[Parkinson](../08_Neuro_Disorders/Parkinson.md)、[Alzheimer](../08_Neuro_Disorders/Alzheimer.md)

---

## References

1. **Tyler, W. J. et al.** "Ultrasonic modulation of neural circuit activity." *Neuron*, 2008.
2. **Elias, W. J. et al.** "A randomized trial of focused ultrasound thalamotomy for essential tremor." *NEJM*, 2016.
3. **Hynynen, K. et al.** "Noninvasive MR imaging-guided focal opening of the blood-brain barrier." *Radiology*, 2001.
4. **Blackmore, J. et al.** "Ultrasound neuromodulation: a review of results, mechanisms and safety." *Ultrasound Med Biol*, 2019.
