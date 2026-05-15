# TMS (Transcranial Magnetic Stimulation)

> *TMS (Barker 1985) 用快变磁场在 brain 中诱导 electrical current,**无创**激活 / 抑制 cortex 区。是当前**临床 + 研究**重要工具:depression 治疗、causal brain mapping、cognitive 增强。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)、电磁学基础

---

## 1. 原理

Faraday 电磁感应:变磁场 → 诱导 electric field。

```
电容放电 → 线圈 → 强短磁脉冲 (~1 T, 100 μs)
                    ↓
              脑组织内诱导 E-field
                    ↓
              神经元 depolarize → spike
```

→ 不需 skull / scalp 切开。

---

## 2. 类型

- **Single-pulse TMS**: 单脉冲,causal probing
- **Paired-pulse TMS**: 测 cortical inhibition
- **Repetitive TMS (rTMS)**: 多脉冲序列
  - **High-frequency (5-20 Hz)**: 兴奋 cortex
  - **Low-frequency (1 Hz)**: 抑制 cortex
  - **Theta-burst (TBS)**: short 50Hz bursts at 5Hz → strong + brief

---

## 3. Coil 形状

- **Circular coil**: 浅 + 弥散
- **Figure-8 coil**: 集中 + 精确 (~ 1 cm² target)
- **H-coil**: 深 (5 cm vs 2 cm)

---

## 4. 临床应用

### 4.1 Depression (FDA-approved 2008)

- 高频 rTMS on left dlPFC
- 5 天 / 周 × 4-6 周
- 30-40% response rate (vs drug + therapy resistance)

### 4.2 OCD (FDA 2018)

- Deep TMS on mPFC/ACC

### 4.3 Migraine (FDA 2014)

- Single-pulse TMS at home device

### 4.4 Stroke rehabilitation

- 刺激损伤侧 → 增强 motor recovery

### 4.5 实验

- Schizophrenia, PTSD, addiction (研究中)

---

## 5. 研究 (Causal Mapping)

TMS 是 **causal** tool — 不是 correlation:

```
TMS on Broca → 短暂 disrupt speech → causal
fMRI Broca active in speech → only correlation
```

- TMS on V1 → blindness 单 ms
- TMS on M1 hand area → involuntary finger twitch (MEP, motor evoked potential)
- TMS on dlPFC → working memory disrupt

---

## 6. 安全

- 主要风险: seizure (rare, < 1/1000 in healthy)
- 排除标准: 癫痫史、metal implants、pregnant
- 标准 protocol 安全

---

## 7. 与 tDCS / DBS 对比

| 维度 | TMS | tDCS | DBS |
|---|---|---|---|
| 侵入性 | 无创 | 无创 | 需手术 |
| 精度 | 高 (cm) | 低 | 极高 (mm) |
| 持久 | 分钟 | 短 | 持续 |
| 强度 | 高 | 极低 | 强 |
| 临床 | depression+ | 实验 | Parkinson, OCD |

---

## 8. Mechanism — 不完全清

- 直接 spike (in M1) → MEP
- LTP-like changes (rTMS 累积效应)
- Network modulation
- Neurochemistry (DA, GABA)

---

## 9. PyTorch / Python — TMS pulse simulation

```python
import numpy as np
import matplotlib.pyplot as plt

def tms_pulse(t, peak_T=1.0, decay_us=100):
    """Single biphasic TMS pulse."""
    # Approximate as damped sinusoid
    t_us = t * 1e6  # to microseconds
    return peak_T * np.sin(2 * np.pi * t / (decay_us * 1e-6)) * np.exp(-t_us / decay_us)

t = np.linspace(-50e-6, 200e-6, 1000)
B = tms_pulse(t)
plt.plot(t * 1e6, B)
plt.xlabel('Time (μs)')
plt.ylabel('Magnetic Field (T)')
```

---

## 10. Common Pitfalls

### 10.1 Target uncertainty

线圈定位 ± 5 mm — 不能保证 hit specific cortical region。

### 10.2 Coil heating

长时 protocol → coil 过热。

### 10.3 Subject variability

需 individualized intensity (MEP threshold)。

### 10.4 Acoustic effect

TMS click → 50 dB 类听觉刺激,confound 必须 control。

### 10.5 Placebo

Sham TMS 同样有效果 — 双盲必须。

---

## 11. Related Concepts

- **同节**:[Optogenetics](Optogenetics.md)、[fMRI BOLD](fMRI_BOLD.md)
- **临床**:[Depression](../08_Neuro_Disorders/Depression.md)

---

## References

1. **Barker, A. T. et al.** "Non-invasive magnetic stimulation of human motor cortex." *Lancet*, 1985.
2. **Pascual-Leone, A. et al.** "Transcranial magnetic stimulation in cognitive neuroscience." *Annu Rev Neurosci*, 2000.
3. **Rossi, S. et al.** "Safety, ethical considerations, and application guidelines for the use of transcranial magnetic stimulation in clinical practice and research." *Clin Neurophysiol*, 2009.
4. **George, M. S. et al.** "Daily left prefrontal transcranial magnetic stimulation therapy for major depressive disorder." *Arch Gen Psychiatry*, 2010.
