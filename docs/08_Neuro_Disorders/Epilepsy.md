# 癫痫 (Epilepsy)

> *Epilepsy 是 ~ 1% population 慢性 brain 病。神经元 hyper-synchronous 放电 → seizure (癫痫发作)。70% 患者药物可控,30% 难治 (drug-resistant)。DBS, RNS, surgical resection 是新治疗。*
>
> **难度**:Intermediate
> **前置知识**:[离子通道](../02_Cellular_Molecular/Ion_Channels.md)、[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. 临床

ILAE 分类:
- **Focal seizure**: 局部 brain area 启动
  - With awareness (simple partial)
  - Without awareness (complex partial)
- **Generalized seizure**: 全脑同时
  - Tonic-clonic (grand mal)
  - Absence (petit mal)
  - Myoclonic
  - Atonic (drop attacks)

Epilepsy 诊断:**2 次或以上 unprovoked seizure** (or 1 + high risk recurrence)。

---

## 2. 机制

- **Hyper-excitation**: 过多 Glu / NMDA / Nav 异常
- **Hypo-inhibition**: GABA 不足 / GABAR mutation
- 神经元 group synchronous firing → seizure 蔓延
- "Tipping point" 模型

---

## 3. 原因

- **Genetic**: Dravet (Nav1.1), Lennox-Gastaut
- **Structural**: stroke, tumor, malformation
- **Infectious**: encephalitis, neurocysticercosis
- **Metabolic**: hypoglycemia, hyponatremia
- **Autoimmune**: NMDAR encephalitis
- **Unknown** (60% 临床原因不明)

---

## 4. 诊断

- **EEG**: 寻 spike-wave discharges
- **MRI**: 结构异常
- **Long-term video-EEG**: 24-72 hr 监测
- **PET**: ictal (发作时) vs interictal

---

## 5. 治疗

### 5.1 抗癫痫药物 (AED)

- **Sodium channel blockers**: Carbamazepine, Phenytoin, Lamotrigine
- **GABA enhancers**: Valproate, Benzodiazepines
- **AMPA blockers**: Perampanel
- **Multi-mechanism**: Topiramate, Levetiracetam
- 70% 患者可控

### 5.2 手术

- **Resection**: 切除 epileptogenic focus (mesial temporal lobe 是常见)
- **Corpus callosotomy**: 切断 corpus callosum (难治 drop attacks)
- **Hemispherectomy**: 极端 case

### 5.3 神经调控

- **VNS (Vagus Nerve Stimulator)**: 颈部植入,定时刺激
- **RNS (Responsive Neuro-Stimulator)**: NeuroPace, 检测 seizure 即激活
- **DBS**: anterior thalamic nucleus 刺激

### 5.4 Diet

- **Ketogenic diet**: 高脂低碳水,儿童 refractory epilepsy 有效

---

## 6. 状态癫痫 (Status Epilepticus)

- Seizure > 5 分钟 或 反复无 recovery
- 神经损伤 + 死亡风险高
- 急救:Lorazepam IV → Phenytoin → 全麻

---

## 7. PyTorch — Seizure Detection from EEG

```python
import torch
import torch.nn as nn

class SeizureDetector(nn.Module):
    """Detect seizure from 4-channel 1-second EEG."""
    def __init__(self, n_channels=4, n_classes=2):
        super().__init__()
        self.cnn = nn.Sequential(
            nn.Conv1d(n_channels, 32, 5), nn.BatchNorm1d(32), nn.ReLU(),
            nn.MaxPool1d(2),
            nn.Conv1d(32, 64, 5), nn.BatchNorm1d(64), nn.ReLU(),
            nn.MaxPool1d(2),
            nn.AdaptiveAvgPool1d(1),
        )
        self.fc = nn.Linear(64, n_classes)
    
    def forward(self, eeg):
        feat = self.cnn(eeg).squeeze(-1)
        return self.fc(feat)
```

---

## 8. AI 应用

- DL EEG → seizure detection / prediction (前 30 sec 预测)
- Wearable (Empatica Embrace) — 自动 detect 并 alert
- Personalized AED dosing
- Brain network analysis 找 epileptogenic zone

---

## 9. Common Pitfalls

### 9.1 Seizure ≠ Epilepsy

单次 seizure 不一定 epilepsy。

### 9.2 EEG normal 不排除

20% 患者 interictal EEG 正常。需 long-term recording。

### 9.3 AED 副作用

Many AED 影响 cognition / mood / metabolism。

### 9.4 妊娠

Valproate 致畸 → 不用于 child-bearing 年龄。

### 9.5 SUDEP (Sudden Unexpected Death)

~ 1/1000 patient-year — 心肺骤停 in sleep。 

---

## 10. Related Concepts

- **同节**:[Alzheimer](Alzheimer.md)、[Parkinson](Parkinson.md)、[Depression](Depression.md)
- **细胞**:[Ion Channels](../02_Cellular_Molecular/Ion_Channels.md)、[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)
- **BCI**:[BCI Foundations](../06_Brain_Computer_Interface/01_Foundations/index.md)

---

## References

1. **Fisher, R. S. et al.** "Operational classification of seizure types (ILAE)." *Epilepsia*, 2017.
2. **Engel, J.** *Seizures and Epilepsy*. 2nd ed., 2013.
3. **Morrell, M. J.** "Responsive cortical stimulation for the treatment of medically intractable partial epilepsy." *Neurology*, 2011.
