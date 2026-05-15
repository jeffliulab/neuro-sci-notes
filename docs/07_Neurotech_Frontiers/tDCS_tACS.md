# 经颅电刺激 (tDCS / tACS / tRNS)

> *Transcranial electrical stimulation 用弱电流(~ 1-2 mA)经头皮调节皮层兴奋性。tDCS(直流:阳极↑、阴极↓兴奋性)、tACS(交流:夹带振荡)、tRNS(随机噪声)。非侵入、便宜、便携,但效应小 + 个体差异大 + 复制争议。研究 + 增强 + 康复领域热点。*
>
> **难度**:Intermediate
> **前置知识**:[TMS](TMS.md)、[Brain Rhythms](../00_Foundations/Brain_Rhythms.md)

---

## 1. 三种模式

| 模式 | 波形 | 机制 |
|---|---|---|
| **tDCS** | 恒定直流 | 阳极去极化(↑兴奋)/ 阴极超极化(↓) |
| **tACS** | 正弦交流 | 夹带(entrain)内源振荡 |
| **tRNS** | 随机噪声 | 随机共振 / 兴奋性↑ |

---

## 2. tDCS 机制

- 不直接触发 AP(电流太弱)
- **调节静息膜电位** → 改变 firing **概率**(neuromodulatory)
- 阳极(anodal):膜去极化 → 兴奋性↑
- 阴极(cathodal):超极化 → 兴奋性↓
- 长时效应:类 LTP/LTD(NMDA 依赖)

---

## 3. tACS — 振荡夹带

- 以目标频率施加 → entrain 内源 rhythm
- 例:gamma-tACS 改善记忆(假说)
- 频率 / 相位特异
- 与 [Brain Rhythms](../00_Foundations/Brain_Rhythms.md) 直接关联

---

## 4. 与 TMS 对比

| | tES | TMS |
|---|---|---|
| 机制 | 调节(subthreshold) | 触发 AP(suprathreshold) |
| 设备 | 便携、廉价 | 大、贵 |
| 空间精度 | 差(电流弥散) | 较好(聚焦线圈) |
| 效应量 | 小、变异大 | 较强 |
| 用 | 研究 / 增强 | 临床(抑郁 FDA 批) |

---

## 5. PyTorch — tDCS 偏置 firing 概率

```python
import torch

def tdcs_modulation(baseline_input, polarity='anodal', shift=0.15):
    """tDCS shifts membrane potential -> changes spike probability."""
    bias = shift if polarity == 'anodal' else -shift
    # Subthreshold: modulates probability, doesn't force spikes
    p_spike = torch.sigmoid(baseline_input + bias)
    return p_spike   # anodal -> higher firing probability
```

---

## 6. 应用领域

- **研究**:因果探测皮层功能(便携 TMS 替代)
- **抑郁**:tDCS(DLPFC)有些证据(弱于 TMS/ECT)
- **卒中康复**:配合训练
- **疼痛**:M1 tDCS
- **认知增强**:学习、工作记忆(争议大)
- **DIY 社区**:伦理 + 安全担忧

---

## 7. 关键参数

- 电流:1-2 mA(安全上限通常 ≤ 4 mA)
- 电极:大(~ 25-35 cm²)→ 精度差;HD-tDCS 用小电极阵列改善
- 时长:~ 10-30 min
- Montage(电极位置)决定电流路径(建模:current flow simulation)

---

## 8. 复制 + 变异性

- 效应量小,个体差异大(颅骨、解剖、state-dependent)
- 多项 meta 分析结论不一
- "Responder vs non-responder"
- 趋势:个体化建模 + closed-loop + 更严格设计

---

## 9. 安全

- 一般安全(规范参数下)
- 皮肤刺激 / 灼伤(电极接触)
- 短暂头皮刺痛、闪光幻视(tACS retina)
- 长期 / DIY 高电流未知风险
- 禁忌:癫痫史慎、颅内金属

---

## 10. Common Pitfalls

### 10.1 tES 触发动作电位

是 subthreshold 调节,不直接引发 spike(与 TMS 不同)。

### 10.2 阳极必增强表现

兴奋性↑ ≠ 行为必改善(取决任务 + 个体)。

### 10.3 效应强且可靠

效应小 + 变异大;复制争议未解。

### 10.4 空间精确

电流大范围弥散;非聚焦(HD-tDCS 改善有限)。

### 10.5 DIY 安全

参数 / montage 不当有风险;非消费级玩具。

---

## 11. Related Concepts

- **同节**:[TMS](TMS.md)、[DBS](DBS.md)、[EEG](EEG.md)
- **基础**:[Brain Rhythms](../00_Foundations/Brain_Rhythms.md)、[Neuroplasticity](../00_Foundations/Neuroplasticity.md)
- **疾病**:[Depression](../08_Neuro_Disorders/Depression.md)

---

## References

1. **Nitsche, M. A. & Paulus, W.** "Excitability changes induced in the human motor cortex by weak transcranial direct current stimulation." *J Physiol*, 2000.
2. **Herrmann, C. S. et al.** "Transcranial alternating current stimulation: entraining oscillations." *Front Hum Neurosci*, 2013.
3. **Polanía, R., Nitsche, M. A., Ruff, C. C.** "Studying and modifying brain function with non-invasive brain stimulation." *Nat Neurosci*, 2018.
4. **Horvath, J. C. et al.** "Quantitative review of tDCS effects (replication concerns)." *Neuropsychologia*, 2015.
