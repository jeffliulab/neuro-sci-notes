# Parkinson's Disease (PD)

> *Parkinson 是第二常见神经退行性病。Substantia Nigra pars compacta (SNc) 中的 dopamine neurons 退化 → BG 失调 → 运动症状 (resting tremor, bradykinesia, rigidity)。L-DOPA 1960s 革命性,但仍非根治。*
>
> **难度**:Intermediate
> **前置知识**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、[Neurotransmitters (DA)](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 临床特征

经典 4 大症状 (TRAP):
- **Tremor**: 静止时颤抖 (4-6 Hz)
- **Rigidity**: 肌肉僵直 ("齿轮样")
- **Akinesia / bradykinesia**: 运动启动困难 / 慢
- **Postural instability**: 平衡问题

非运动:抑郁, sleep, autonomic, cognitive decline 后期。

---

## 2. Pathology

### 2.1 SNc DA Neuron Loss

- 80%+ SNc DA neurons 退化才出症状
- 黑质 (substantia nigra) 失去黑色 (neuromelanin)

### 2.2 Lewy Bodies

- α-synuclein 蛋白聚集 (inclusion bodies)
- 在 surviving neurons 内
- 也扩散到 cortex (Lewy Body Dementia)

### 2.3 Mitochondrial Dysfunction

- DA neurons 易 oxidative stress
- 与 PINK1 / Parkin 基因 (mitochondrial QC) 关联

---

## 3. 遗传 vs Sporadic

- 90% sporadic (后天)
- 10% genetic (LRRK2, SNCA, PINK1, Parkin, GBA)
- LRRK2 G2019S: AD 常见 mutation (5-7% sporadic 病例)

---

## 4. Pathway dysregulation

```
SNc DA 减少
   ↓
Striatum D1/D2 失衡
   ↓
Direct pathway 弱 (less Go)
Indirect pathway 强 (more No-Go)
   ↓
GPi 过激 → Thalamus 过抑制 → Cortex 运动启动困难
```

---

## 5. 诊断

- Clinical:UK Brain Bank criteria
- DAT-SPECT (DA transporter imaging): striatum DA 减
- MRI:排除其他原因
- 对 L-DOPA 反应:阳性 supports PD

---

## 6. 治疗

### 6.1 药物

- **L-DOPA + Carbidopa**: DA precursor (Levodopa); gold standard
- **DA agonists** (Pramipexole, Ropinirole)
- **MAO-B inhibitors** (Selegiline, Rasagiline): 延 DA 代谢
- **COMT inhibitors**: 延 L-DOPA 半衰期
- **Anticholinergics**: 老药,抑制 ACh (tremor)

### 6.2 手术

- **DBS (Deep Brain Stimulation) on STN / GPi**:
  - 1990s 开发,普及 2000s
  - 电极埋入 STN → 高频刺激 → 改善 motor
  - 不治本但极大改善 quality of life

### 6.3 实验

- **Focused ultrasound thalamotomy** (无创版 DBS)
- **Stem cell therapy** (iPSC → DA neurons → 移植)
- **Anti-α-synuclein antibody**
- **GDNF / neurotrophic factor** (失败试验多次)

---

## 7. 与 AI / Tech

- Wearable sensors (tremor monitoring)
- DL diagnosis from gait video / voice
- DBS algorithm optimization (RL)
- Drug discovery

---

## 8. PyTorch — DBS 概念

```python
import torch

class DBSController:
    """Closed-loop DBS controller."""
    def __init__(self, target_freq=130):
        self.target_freq = target_freq
        self.beta_threshold = 0.5
    
    def control(self, beta_power):
        """Adapt stimulation based on STN beta activity."""
        # 病理 PD 中 STN beta band (13-30 Hz) 高
        if beta_power > self.beta_threshold:
            return self.target_freq  # full stimulation
        else:
            return 0  # off (save battery + reduce side effects)
```

---

## 9. 数字

- 全球 1000 万患者 (2025)
- 平均发病 60 岁
- Early-onset: 10% < 50 岁
- 男 > 女 (1.5×)

---

## 10. Common Pitfalls

### 10.1 80% loss before 症状

早期诊断难。

### 10.2 L-DOPA 不治本

只补 DA,不阻 progression。

### 10.3 L-DOPA dyskinesias

长期用 → 不自主动作 (motor complications)。

### 10.4 非运动症状 严重

抑郁 / 痴呆 / 自主神经失调 — 后期 dominant。

### 10.5 与 Lewy Body Dementia

Spectrum overlap;不完全独立 disease。

---

## 11. Related Concepts

- **同节**:[Alzheimer](Alzheimer.md)、[Depression](Depression.md)
- **解剖**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)
- **NT**:[DA](../02_Cellular_Molecular/Neurotransmitters.md)

---

## References

1. **Parkinson, J.** *An Essay on the Shaking Palsy*. 1817.
2. **Carlsson, A.** "The occurrence, distribution and physiological role of catecholamines in the nervous system." *Pharmacol Rev*, 1959.
3. **Olanow, C. W. et al.** "The scientific and clinical basis for the treatment of Parkinson disease." *Neurology*, 2009.
4. **Lozano, A. M. et al.** "Deep brain stimulation: current challenges and future directions." *Nat Rev Neurol*, 2019.
