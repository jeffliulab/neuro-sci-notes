# 深部脑刺激 (DBS, Deep Brain Stimulation)

> *DBS 在 brain 深部植入电极,持续高频刺激改善运动 / 精神症状。1980s 起治 Parkinson;后扩 OCD, depression, epilepsy。10 万+ 患者全球。RNS, focused ultrasound 等为新版。*
>
> **难度**:Intermediate-Advanced
> **前置知识**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、[Brainstem](../01_Neuroanatomy/Brainstem.md)

---

## 1. DBS 系统

3 components:
- **Leads**: 4 contact electrodes 在 target nucleus
- **IPG (Implantable Pulse Generator)**: 胸前 (类心脏起搏器)
- **Extension wires**: 头皮下连接

Stimulation: 高频 (130-185 Hz),pulse width 60-120 μs,voltage 1-5 V。

---

## 2. 主要 target

### 2.1 Parkinson

- **STN (Subthalamic Nucleus)**: 最常 target
- **GPi**: 替代 STN,对 dyskinesia 好
- 显著改善 motor symptoms (75%+ tremor / 50%+ bradykinesia 减)
- 不影响 disease progression

### 2.2 Essential Tremor

- **VIM thalamus**: 对手 tremor 极有效

### 2.3 Dystonia

- **GPi**

### 2.4 OCD

- **ALIC, NAcc**: 难治 OCD,FDA HDE approved

### 2.5 Depression (实验)

- **SCC25 (subcallosal cingulate)**: Mayberg 2005 开创
- 部分难治 depression patient 显著改善
- 试验结果不一致 (BROADEN trial 2017 failed primary endpoint)

### 2.6 Epilepsy

- **Anterior thalamic nucleus (ANT)**: SANTE trial
- FDA approved 2018

---

## 3. 手术流程

1. MRI 规划 target
2. Stereotactic frame (Leksell)
3. Burr hole + electrode insertion
4. **Microelectrode recording** (MER): 验证 target neurons
5. Test stimulation
6. Permanent electrode 锁定
7. IPG 植入 (1 周后)
8. Programming (数周 fine-tune)

---

## 4. 机制 (不完全清)

- **Inhibition of target**: 高频 stim 可能 "disrupts" pathological 信号
- **Excitation of axons**: stimulate efferent fibers
- **Network modulation**: 影响 cortico-BG-thalamic loop
- **Neuromodulator release** changes

---

## 5. Closed-Loop DBS

新一代 (RNS, adaptive DBS):
- 检测病理 signal (beta band oscillation in PD)
- 仅在 needed 时刺
- 电池 寿命 ×3, 副作用 ↓
- Medtronic Percept (2020 FDA)

---

## 6. 安全 / 副作用

- 手术风险:bleeding (~ 1%), infection (~ 3%)
- 刺激副作用:speech, balance, mood
- 长期:device 故障 (lead break, IPG dead)
- 电池 5-15 年换

---

## 7. PyTorch — Closed-loop DBS Algorithm

```python
import torch

class AdaptiveDBSController:
    """Detect beta band oscillation → trigger stim."""
    def __init__(self, beta_threshold=10, stim_freq=130):
        self.beta_threshold = beta_threshold
        self.stim_freq = stim_freq
        self.fft_window = 256
    
    def step(self, lfp_window):
        """LFP from STN micro-electrode."""
        # FFT
        spec = torch.fft.rfft(lfp_window)
        # Beta band power (13-30 Hz)
        beta_power = (spec[13:30].abs() ** 2).mean()
        # Stim if high
        if beta_power > self.beta_threshold:
            return self.stim_freq
        else:
            return 0
```

---

## 8. AI / 数据

- ML predict optimal stim parameters
- Closed-loop algorithm 优化
- 长期 LFP 数据库 (Medtronic Percept)
- Personalized DBS (个性化 target / freq)

---

## 9. 经济 / 普及

- DBS 系统 ~ $50k + 手术
- 全球 10 万+ patients implanted
- 中国, 印度 普及加速
- New device:Boston Scientific Vercise, Abbott Infinity

---

## 10. Common Pitfalls

### 10.1 Not All PD Suitable

DBS 最 effective on **L-DOPA-responsive** motor symptoms;axial symptoms (balance, swallow) 效果差。

### 10.2 Lead Placement Critical

mm 偏差 → 无效或副作用。

### 10.3 Cognitive 副作用

Long-term subtle cognitive decline in some patients。

### 10.4 Programming 复杂

Best parameters individual;需 经验丰富 neurologist。

### 10.5 不可逆 (但可关)

电极永久 but IPG 可关。Side effects reversible by turning off stim。

---

## 11. Related Concepts

- **同节**:[Optogenetics](Optogenetics.md)、[TMS](TMS.md)、[fMRI BOLD](fMRI_BOLD.md)
- **疾病**:[Parkinson](../08_Neuro_Disorders/Parkinson.md)、[Depression](../08_Neuro_Disorders/Depression.md)、[Epilepsy](../08_Neuro_Disorders/Epilepsy.md)

---

## References

1. **Benabid, A. L. et al.** "Long-term suppression of tremor by chronic stimulation of the ventral intermediate thalamic nucleus." *Lancet*, 1991.
2. **Mayberg, H. S. et al.** "Deep brain stimulation for treatment-resistant depression." *Neuron*, 2005.
3. **Krack, P. et al.** "Five-year follow-up of bilateral stimulation of the subthalamic nucleus in advanced Parkinson's disease." *NEJM*, 2003.
4. **Lozano, A. M. et al.** "Deep brain stimulation: current challenges and future directions." *Nat Rev Neurol*, 2019.
