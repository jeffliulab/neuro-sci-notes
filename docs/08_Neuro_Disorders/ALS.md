# ALS (肌萎缩侧索硬化症, Amyotrophic Lateral Sclerosis)

> *ALS 是 motor neuron disease 主要类型。Both upper motor neuron (cortex) + lower (anterior horn) 退化 → 进行性肌无力 → 3-5 年呼吸衰竭死亡。Stephen Hawking 患 ALS 50+ 年 (atypical case)。BCI 是当前 ALS 患者沟通的关键技术。*
>
> **难度**:Intermediate
> **前置知识**:[Motor System](../03_Systems_Neuroscience/Motor_System.md)、[神经元](../02_Cellular_Molecular/Neuron.md)

---

## 1. 临床

- 进行性肌无力 + 肌肉萎缩
- 不影响 sensory, cognitive (most cases)
- 通常 50-65 岁发病
- 发病到死亡 ~ 3-5 年 (排除呼吸 support)
- 全球 ~ 30 万患者

---

## 2. 病理

```
Upper motor neuron (Betz cells in M1) 退化
  ↓
Lower motor neuron (anterior horn) 退化
  ↓
肌肉 denervation → 萎缩 + fasciculation
  ↓
最终 全身瘫痪 + 呼吸肌失
```

---

## 3. 类型

- **Sporadic** (90%): 原因未知
- **Familial** (10%): 多 gene mutation
  - SOD1 (~ 20% familial)
  - C9orf72 (~ 40% familial — 也涉 frontotemporal dementia)
  - FUS, TARDBP

---

## 4. 发病机制 (Hypotheses)

- Glutamate excitotoxicity
- Mitochondrial dysfunction
- Oxidative stress
- Protein misfolding + aggregation (TDP-43)
- Neuroinflammation (microglia)
- Axonal transport defects

---

## 5. 诊断

- Clinical exam + EMG (electromyography)
- MRI (排除其他)
- Genetic testing (suspected familial)
- El Escorial criteria

---

## 6. 治疗

### 6.1 药物

- **Riluzole** (1995): NMDA antagonist, 延寿 ~ 3 个月
- **Edaravone** (Radicava): 抗氧化, 延缓 ~ 30% functional decline
- **Tofersen** (2023): antisense oligonucleotide for SOD1 ALS
- **AMX0035**: 2022 FDA approved (后撤回 2024 due to phase 3 failure)

### 6.2 Supportive

- Non-invasive ventilation (BiPAP)
- Feeding tube (PEG) 后期
- Physical / speech therapy
- Wheelchair adaptation

### 6.3 BCI

- 沟通辅助 (eye tracking, EEG-based)
- Neuralink, Synchron, Blackrock 都在 ALS 患者试
- 2024 Synchron + Neuralink 让 ALS 患者用 thought control 平板 / 鼠标

---

## 7. PyTorch — ALS Motor Neuron Loss Simulation

```python
import torch
import numpy as np

class MotorPoolSimulation:
    """Track motor neuron survival over time."""
    def __init__(self, n_motor_neurons=1000):
        self.alive = torch.ones(n_motor_neurons)
        self.muscle_strength = []
    
    def step(self, death_rate_per_year=0.05, dt_years=0.1):
        """Simulate progressive denervation."""
        deaths = torch.rand(len(self.alive)) < death_rate_per_year * dt_years
        self.alive *= (~deaths).float()
        strength = self.alive.mean().item()
        self.muscle_strength.append(strength)
        return strength
```

---

## 8. BCI 在 ALS 中

- **Locked-in stage**: ALS 末期类似 locked-in (全身瘫但意识清)
- **EEG-BCI**: P300 speller, SSVEP (commercial)
- **Implant BCI**:
  - Stentrode (Synchron): 经血管植入, 不需开颅
  - Neuralink N1: 高通道, 试验中
  - BrainGate: 学术 BCI, ALS 患者用 ~ 20 years
- Speech BCI: UCSF 2023 — 80 words / min from brainstem stroke patient

---

## 9. 名人

- **Lou Gehrig** (1939 NYY): 命名 "Lou Gehrig's disease"
- **Stephen Hawking** (1963 onset, 2018 death): 物理学家 + 极典型 long survival
- **Mao Zedong**: 据信也有部分症状 (历史 unclear)
- **Steve Gleason**: NFL → BCI advocate

---

## 10. Common Pitfalls

### 10.1 进展 individual

部分 patient 数年慢病程,部分 1 年快死。

### 10.2 不影响 cognition (大多)

但 5-15% 有 FTD (frontotemporal dementia) overlap。

### 10.3 EMG 诊断不易

需 expert; 早期不显。

### 10.4 没有治愈

只能 delay。家属心理 + financial burden 大。

### 10.5 BCI 不能恢复运动

只是 substitute communication / control external devices。

---

## 11. Related Concepts

- **同节**:[Stroke](Stroke.md)、[Parkinson](Parkinson.md)
- **系统**:[Motor System](../03_Systems_Neuroscience/Motor_System.md)
- **BCI**:[BCI Foundations](../06_Brain_Computer_Interface/01_Foundations/index.md)、[DBS](../07_Neurotech_Frontiers/DBS.md)

---

## References

1. **Brown, R. H. & Al-Chalabi, A.** "Amyotrophic Lateral Sclerosis." *NEJM*, 2017.
2. **DeJesus-Hernandez, M. et al.** "Expanded GGGGCC hexanucleotide repeat in noncoding region of C9ORF72 causes chromosome 9p-linked FTD and ALS." *Neuron*, 2011.
3. **Hochberg, L. R. et al.** "Reach and grasp by people with tetraplegia using a neurally controlled robotic arm (BrainGate)." *Nature*, 2012.
