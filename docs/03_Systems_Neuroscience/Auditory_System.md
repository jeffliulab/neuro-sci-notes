# 听觉系统 (Auditory System)

> *听觉系统从外耳 → 中耳 → 内耳 (cochlea) → 听神经 → 脑干核 → 丘脑 (MGN) → A1 (primary auditory cortex)。Cochlea 把声波 → tonotopic 神经 firing。本篇覆盖通路 + 编码 + 语音。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. 听觉通路

```
Sound wave (空气压力)
  ↓
External ear → eardrum
  ↓
Middle ear (3 ossicles: malleus, incus, stapes — 阻抗匹配)
  ↓
Cochlea (内耳, 螺旋 fluid-filled)
  ↓
Hair cells (mechanoelectrical transduction)
  ↓
Spiral ganglion / 听神经 (cranial VIII)
  ↓
Cochlear nuclei (brainstem)
  ↓
Superior olive (sound localization)
  ↓
Inferior colliculus (midbrain)
  ↓
MGN (Medial Geniculate Nucleus, thalamus)
  ↓
A1 (primary auditory cortex, 颞叶)
  ↓
A2, Wernicke (language)
```

---

## 2. Cochlea

- 螺旋管,约 2.75 turn
- Basilar membrane 振动
- 不同 frequency 在不同位置振动 (tonotopic):
  - 高频 (20 kHz): cochlea base
  - 低频 (20 Hz): cochlea apex
- ~ 16,000 hair cells per ear

---

## 3. Hair Cells

- **Inner hair cells (IHC)**: 听觉主要 transducer, 3500 per ear
- **Outer hair cells (OHC)**: 主动放大, 12,000 per ear, 受 cochlear amp
- Stereocilia 弯 → mechano-gated channel 开 → K+ inflow → depolarize

OHC 死 (老化, noise damage) → 听力下降。

---

## 4. Tonotopy

频率 → 位置编码,从 cochlea 到 A1:
- A1 也有 tonotopic map (high freq medial → low freq lateral)
- 与 V1 retinotopy 类似

---

## 5. 声源定位

两耳信息差:
- **ITD (Interaural Time Difference)**: < 1 ms (低频)
- **ILD (Interaural Level Difference)**: 高频 (头 shadow)
- Superior olive 处理 ITD/ILD → 输出方向

人能定位 ± 1°。

---

## 6. 语音处理

- **A1**: spectral analysis
- **STG (Superior Temporal Gyrus)**: phoneme
- **Wernicke**: meaning
- **Broca**: speech production

→ Categorical perception (/b/ vs /p/ 等)。

---

## 7. PyTorch — Cochlear Tonotopic Filterbank

```python
import torch
import torch.nn as nn

class CochlearFilterbank(nn.Module):
    """Approx 32 bandpass filters spanning 20 Hz - 20 kHz."""
    def __init__(self, n_filters=32, sample_rate=16000):
        super().__init__()
        self.n_filters = n_filters
        # Mel-scale center frequencies
        mel_freqs = torch.linspace(0, 2595 * torch.log10(torch.tensor(1 + 8000 / 700)), n_filters + 2)
        hz_freqs = 700 * (10**(mel_freqs / 2595) - 1)
        self.freqs = hz_freqs
        self.sample_rate = sample_rate
    
    def forward(self, audio):
        # Compute STFT
        spec = torch.stft(audio, n_fft=512, return_complex=True)
        mag = spec.abs()
        # Map to mel bands (simplified)
        return mag
```

---

## 8. 听力损失

- **传导性** (Conductive): 外耳/中耳 problem (耳屎, 中耳炎)
- **感音神经性** (Sensorineural): hair cell / 听神经 (老化, noise, ototoxic drugs)
- **混合性**

治疗:
- Hearing aids (放大)
- **Cochlear implant**: 电极 bypass hair cells,直接刺听神经
- ABI (auditory brainstem implant): bypass 听神经

---

## 9. 听觉 vs 视觉对比

| 特性 | 听觉 | 视觉 |
|---|---|---|
| Receptor | hair cell | photoreceptor |
| Encoding | tonotopic | retinotopic |
| Cortex | A1 (Heschl gyrus) | V1 (occipital) |
| 时间分辨率 | μs (ITD) | ~ 10 ms |
| 空间分辨率 | ±1° | 1 arcmin (fovea) |
| Memory | 短 (~10 sec echo) | 长 (visual memory) |

---

## 10. AI 应用

- ASR (Automatic Speech Recognition): Whisper, wav2vec
- TTS (Text-to-Speech): VALL-E, TortoiseTTS
- Audio source separation
- Music generation
- Hearing aid AI noise reduction

---

## 11. Common Pitfalls

### 11.1 Tonotopy 不是唯一

也 spectral / temporal coding。

### 11.2 OHC 不能 regenerate (mammals)

一旦 hair cell 死永久。鸟类可再生。

### 11.3 Cochlear implant 不复原

仅 ~ 16-24 electrodes vs ~ 3500 IHC → 听觉粗。

### 11.4 双耳 vs 单耳

单耳听力定位差 (only spectral cue from pinna)。

### 11.5 Noise damage

> 85 dB long-term exposure → permanent hearing loss。

---

## 12. Related Concepts

- **同节**:[Visual System](Visual_System.md)、[Motor System](Motor_System.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)、[Brainstem](../01_Neuroanatomy/Brainstem.md)

---

## References

1. **Pickles, J. O.** *An Introduction to the Physiology of Hearing*. 4th ed., 2012.
2. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
3. **Moore, B. C. J.** *An Introduction to the Psychology of Hearing*. 6th ed., 2012.
