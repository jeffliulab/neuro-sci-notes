# 闭环神经调控 (Closed-Loop Neuromodulation)

> *传统刺激是开环(固定参数,不管脑状态)。闭环 = 实时读神经/行为 → 算法决策 → 按需刺激。优势:更有效、省电、少副作用、个性化。已批:NeuroPace RNS(响应式癫痫刺激)、adaptive DBS(aDBS,PD beta 触发)。是 neuromodulation + BCI 融合前沿。*
>
> **难度**:Advanced
> **前置知识**:[DBS](DBS.md)、[Brain Rhythms](../00_Foundations/Brain_Rhythms.md)

---

## 1. 开环 vs 闭环

| | 开环 | 闭环 |
|---|---|---|
| 参数 | 固定 / 人工调 | 实时自适应 |
| 触发 | 持续 / 定时 | 按生物标志触发 |
| 电池 | 耗 | 省(按需) |
| 副作用 | 较多(过刺激) | 较少 |
| 个性化 | 低 | 高 |

---

## 2. 闭环回路

```
传感 (LFP / ECoG / spike / 行为)
   ↓
特征提取 (band power, biomarker)
   ↓
决策算法 (阈值 / ML / 控制论)
   ↓
刺激器 (DBS / 皮层 / 光 / 药)
   ↓ (改变脑状态)
回到传感
```

---

## 3. 已批准系统

- **NeuroPace RNS**(2013 FDA):皮层电极检测癫痫样活动 → 即时反刺激中止 seizure(响应式)
- **Adaptive DBS (aDBS)**:Medtronic Percept — 检测 STN **beta 振荡**(PD 运动症状 biomarker)→ 按需调幅
- **Closed-loop SCS**:脊髓刺激按 ECAP 反馈调强

---

## 4. 生物标志 (Biomarker)

| 疾病 | Biomarker |
|---|---|
| PD | STN beta (13-30 Hz) 过强 |
| Epilepsy | 痫样放电 / 高频振荡 |
| Essential tremor | 震颤频率 LFP / IMU |
| Depression | 个体化网络状态(实验) |
| OCD | 边缘环路标志(实验) |

biomarker 质量决定闭环成败。

---

## 5. PyTorch — 响应式刺激控制器

```python
import torch

def closed_loop_controller(lfp_window, beta_threshold=0.6):
    """Detect pathological beta -> trigger stimulation (aDBS-like)."""
    # Band power in beta (simplified via FFT)
    spec = torch.abs(torch.fft.rfft(lfp_window))
    beta_power = spec[13:30].mean()
    total = spec.mean() + 1e-6
    rel_beta = beta_power / total
    stim_amplitude = torch.clamp((rel_beta - beta_threshold) * 5, 0, 3)
    return stim_amplitude   # 0 if below threshold -> battery saved
```

---

## 6. 控制策略层级

- **Threshold-triggered**(RNS):超阈即刺激(简单稳健)
- **Proportional / PID**:按 biomarker 偏差调幅
- **Model-based / 最优控制**:状态空间模型
- **RL / 自适应**:学最优策略(实验,安全约束严)
- 临床偏好可解释 + 稳健 > 黑箱

---

## 7. 优势

- **省电**:按需 → 电池寿命↑(减更换手术)
- **减副作用**:避免过刺激(言语 / 情绪)
- **更有效**:针对病理状态精准干预
- **个性化**:适配个体 biomarker + 波动
- **数据**:长期记录 → 疾病理解 + 优化

---

## 8. 挑战

- **Biomarker**:可靠 + 实时 + 特异(最大瓶颈,尤精神疾病)
- **伪迹**:刺激污染记录(需 artifact rejection)
- **延迟**:感知→决策→刺激须够快(癫痫尤甚)
- **闭环不稳定**:反馈振荡 / 失稳风险
- **算法验证 + 监管**(自适应算法的审批路径)
- 计算 / 功耗(植入内 vs 外部)

---

## 9. 与 BCI 融合

- 闭环刺激 = "写";BCI 解码 = "读" → 双向 BCI
- All-optical 闭环(光读 + 光写,见 [Optogenetics_Advanced](Optogenetics_Advanced.md))
- 记忆假体(检测 → 刺激增强编码,DARPA RAM)
- 情绪 / 精神疾病自适应(实验,伦理敏感)

---

## 10. Common Pitfalls

### 10.1 闭环必优于开环

依赖 biomarker 质量;差 biomarker → 不如稳定开环。

### 10.2 Biomarker 易得

可靠实时特异 biomarker 是最大难题(尤精神病)。

### 10.3 越复杂算法越好

临床重可解释 + 稳健;黑箱难审批 + 风险。

### 10.4 无延迟

感知-决策-刺激延迟关键(癫痫须毫秒级)。

### 10.5 闭环天然稳定

反馈可致振荡 / 失稳;须控制论分析。

---

## 11. Related Concepts

- **同节**:[DBS](DBS.md)、[Optogenetics_Advanced](Optogenetics_Advanced.md)、[Neuralink](Neuralink.md)
- **基础**:[Brain Rhythms](../00_Foundations/Brain_Rhythms.md)
- **计算**:[Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.md)
- **疾病**:[Parkinson](../08_Neuro_Disorders/Parkinson.md)、[Epilepsy](../08_Neuro_Disorders/Epilepsy.md)

---

## References

1. **Sun, F. T. & Morrell, M. J.** "The RNS System: responsive cortical stimulation for the treatment of epilepsy." *Expert Rev Med Devices*, 2014.
2. **Little, S. et al.** "Adaptive deep brain stimulation in advanced Parkinson disease." *Ann Neurol*, 2013.
3. **Sani, O. G. et al.** "Mood variations decoded from multi-site intracranial recordings." *Nat Biotechnol*, 2018.
4. **Bouthour, W. et al.** "Biomarkers for closed-loop deep brain stimulation in Parkinson disease and beyond." *Nat Rev Neurol*, 2019.
