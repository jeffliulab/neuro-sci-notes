# 神经科学研究方法 (Research Methods)

> *神经科学是 multi-level、multi-method 学科。Lesion 研究 (Broca) → 电生理 (intracellular, EEG) → fMRI / 光学 imaging → optogenetics → connectomics。每方法 trade-off (resolution × invasiveness × cost)。理解 method 即理解 scientific claim 的 epistemic 基础。*
>
> **难度**:Beginner
> **前置知识**:无(Foundations 起点)

---

## 1. 方法分类(Resolution-Invasiveness)

| 方法 | 时空 resolution | Invasive | 人/动物 |
|---|---|---|---|
| Lesion | 整脑 | 极 invasive | 二者 |
| Single-unit | μm / ms | 高 | 主动物 |
| Patch clamp | 单细胞 / sub-ms | 高 | 动物 |
| MEA / Neuropixels | μm-mm / ms | 高 | 动物 |
| EEG | cm / ms | 非 invasive | 主人 |
| ECoG | mm / ms | 半 invasive (颅内) | 人 (epilepsy) |
| fMRI BOLD | mm / sec | 非 | 主人 |
| MEG | cm / ms | 非 | 主人 |
| 2-photon Ca²⁺ | μm / 10s ms | 半 invasive (颅窗) | 主动物 |
| fNIRS | cm / sec | 非 | 人 |
| TMS | cm / 10s ms | 非 | 人 |
| Optogenetics | 单细胞 / ms | 高 (基因 + 光纤) | 主动物 |
| Connectomics (EM) | nm / 静态 | postmortem | 主动物 |

---

## 2. Lesion 研究

- **Broca (1861)**: Tan 患者 lesion → 前下额叶 → 语言产生区
- **Wernicke (1874)**: 后颞叶 → 语言理解
- **H.M. (Scoville 1957)**: 双侧 MTL 切除 → episodic memory 缺失
- 现代:stroke、TBI、tumor patient + structural MRI

---

## 3. 电生理

### 3.1 Intracellular

- Patch clamp (Neher & Sakmann, Nobel 1991)
- 单 channel current 可记录
- Action potential 完整波形

### 3.2 Extracellular

- Single-unit recording (Hubel & Wiesel)
- Tetrode、silicon probe
- **Neuropixels 1.0** (2017): 384 channels
- **Neuropixels 2.0** (2021): 5,120 channels

### 3.3 EEG / MEG

- EEG: 表面电极,~ 32-256 channels
- MEG: 量子磁场探测
- 时间 resolution ms,空间差

---

## 4. fMRI / PET

- fMRI BOLD: 血氧 → 间接 neural activity
- T1/T2 weighted、DTI、resting state
- Logothetis 2001: BOLD 关联 LFP > spike
- PET: 注射 radioisotope,适合 receptor binding (DA、5HT)

---

## 5. 光学方法

### 5.1 2-photon imaging

- 红外光 → 减少散射
- Ca²⁺ indicator (GCaMP) 实时 firing
- 单细胞 + 数百细胞同时

### 5.2 Voltage imaging

- 直接膜电位记录(no Ca delay)
- 但 SNR 较低
- 进展中

### 5.3 Miniscope

- 小鼠头戴 1g 显微
- Free-behaving Ca²⁺ imaging

---

## 6. Optogenetics + Chemogenetics

- **Optogenetics**: 基因表达 ChR2 / NpHR → 光控
- **Chemogenetics (DREADD)**: drug 控制
- Causal inference 工具(超 lesion)

---

## 7. Connectomics

- C. elegans (302 neurons) full connectome (White 1986)
- Drosophila full connectome (FlyEM 2024)
- Mouse cortex MICrONS (2025): 1 mm³ EM
- 人类:仅 partial(Allen Brain Atlas、Human Connectome)

---

## 8. PyTorch — 简化 Spike Sorter

```python
import torch

class SimpleSpikeSorter:
    """KMeans on waveform features."""
    def __init__(self, n_clusters=4):
        self.n_clusters = n_clusters
        self.centers = None
    
    def fit(self, waveforms, iter=20):
        """waveforms: (N, T) extracted spikes."""
        idx = torch.randperm(waveforms.shape[0])[:self.n_clusters]
        self.centers = waveforms[idx].clone()
        for _ in range(iter):
            dist = torch.cdist(waveforms, self.centers)
            labels = dist.argmin(dim=1)
            for k in range(self.n_clusters):
                mask = labels == k
                if mask.sum() > 0:
                    self.centers[k] = waveforms[mask].mean(dim=0)
        return labels
```

---

## 9. 行为 paradigm

- **Maze**: Morris water maze、radial arm
- **Operant**: Skinner box
- **Working memory**: delayed response、N-back
- **Decision**: 2AFC、Iowa gambling
- **Social**: 3-chamber test
- **Closed-loop**: real-time stim 调节行为

---

## 10. Common Pitfalls

### 10.1 fMRI 测 spike

BOLD 是间接信号,~ 几秒延迟。

### 10.2 EEG 定位精确

EEG 空间 resolution 厘米级,不能定位单 neuron。

### 10.3 Correlation = Causation

fMRI activation ≠ region 必要;需要 lesion / optogenetic 验证。

### 10.4 Optogenetics 完美

光遗传 expression 不均、heating 等 confound。

### 10.5 Connectome = Function

知道连接 ≠ 知道功能(需要 dynamics + plasticity)。

---

## 11. Related Concepts

- **同节**:[Neuroscience History](Neuroscience_History.md)
- **细胞/分子**:[Action Potential](../02_Cellular_Molecular/Action_Potential.md)
- **前沿**:[fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.md)、[Optogenetics](../07_Neurotech_Frontiers/Optogenetics.md)

---

## References

1. **Carandini, M.** "From circuits to behavior: a bridge too far?" *Nat Neurosci*, 2012.
2. **Sejnowski, T. J. et al.** "Putting big data to good use in neuroscience." *Nat Neurosci*, 2014.
3. **Jun, J. J. et al.** "Fully integrated silicon probes for high-density recording of neural activity." *Nature*, 2017.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
