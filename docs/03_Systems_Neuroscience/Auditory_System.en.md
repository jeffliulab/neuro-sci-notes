# Auditory System

> *Auditory system: outer ear → middle ear → inner ear (cochlea) → auditory nerve → brainstem nuclei → thalamus (MGN) → A1 (primary auditory cortex). Cochlea converts sound waves → tonotopic neural firing. This article covers pathway + coding + speech.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Auditory Pathway

```
Sound wave (air pressure)
  ↓
External ear → eardrum
  ↓
Middle ear (3 ossicles — impedance matching)
  ↓
Cochlea (inner ear, spiral fluid-filled)
  ↓
Hair cells (mechanoelectrical transduction)
  ↓
Spiral ganglion / auditory nerve (cranial VIII)
  ↓
Cochlear nuclei (brainstem)
  ↓
Superior olive (sound localization)
  ↓
Inferior colliculus (midbrain)
  ↓
MGN (Medial Geniculate Nucleus, thalamus)
  ↓
A1 (primary auditory cortex, temporal lobe)
  ↓
A2, Wernicke (language)
```

---

## 2. Cochlea

- Spiral tube, ~2.75 turns
- Basilar membrane vibrates
- Different frequencies vibrate at different locations (tonotopic):
  - High frequency (20 kHz): cochlea base
  - Low frequency (20 Hz): cochlea apex
- ~16,000 hair cells per ear

---

## 3. Hair Cells

- **Inner hair cells (IHC)**: main auditory transducers, 3500 per ear
- **Outer hair cells (OHC)**: active amplification, 12,000 per ear, cochlear amp
- Stereocilia bend → mechano-gated channels open → K+ inflow → depolarize

OHC death (aging, noise damage) → hearing loss.

---

## 4. Tonotopy

Frequency → location encoding, from cochlea to A1:
- A1 also has tonotopic map (high freq medial → low freq lateral)
- Similar to V1 retinotopy

---

## 5. Sound Localization

Two-ear information differences:
- **ITD (Interaural Time Difference)**: < 1 ms (low frequency)
- **ILD (Interaural Level Difference)**: high frequency (head shadow)
- Superior olive processes ITD/ILD → outputs direction

Human can localize ±1°.

---

## 6. Speech Processing

- **A1**: spectral analysis
- **STG (Superior Temporal Gyrus)**: phonemes
- **Wernicke**: meaning
- **Broca**: speech production

→ Categorical perception (/b/ vs /p/ etc.).

---

## 7. PyTorch — Cochlear Tonotopic Filterbank

```python
import torch
import torch.nn as nn

class CochlearFilterbank(nn.Module):
    def __init__(self, n_filters=32, sample_rate=16000):
        super().__init__()
        self.n_filters = n_filters
        mel_freqs = torch.linspace(0, 2595 * torch.log10(torch.tensor(1 + 8000 / 700)), n_filters + 2)
        hz_freqs = 700 * (10**(mel_freqs / 2595) - 1)
        self.freqs = hz_freqs
        self.sample_rate = sample_rate
    
    def forward(self, audio):
        spec = torch.stft(audio, n_fft=512, return_complex=True)
        mag = spec.abs()
        return mag
```

---

## 8. Hearing Loss

- **Conductive**: outer/middle ear problem (earwax, otitis media)
- **Sensorineural**: hair cell / auditory nerve (aging, noise, ototoxic drugs)
- **Mixed**

Treatment:
- Hearing aids (amplification)
- **Cochlear implant**: electrode bypass hair cells, directly stimulate auditory nerve
- ABI (auditory brainstem implant): bypass auditory nerve

---

## 9. Auditory vs Visual

| Feature | Auditory | Visual |
|---|---|---|
| Receptor | hair cell | photoreceptor |
| Encoding | tonotopic | retinotopic |
| Cortex | A1 (Heschl gyrus) | V1 (occipital) |
| Temporal resolution | μs (ITD) | ~10 ms |
| Spatial resolution | ±1° | 1 arcmin (fovea) |
| Memory | short (~10 sec echo) | long (visual memory) |

---

## 10. AI Applications

- ASR (Automatic Speech Recognition): Whisper, wav2vec
- TTS (Text-to-Speech): VALL-E, TortoiseTTS
- Audio source separation
- Music generation
- Hearing aid AI noise reduction

---

## 11. Common Pitfalls

### 11.1 Tonotopy Not Unique

Also spectral / temporal coding.

### 11.2 OHC Don't Regenerate (mammals)

Once hair cell dies, permanent. Birds can regenerate.

### 11.3 Cochlear Implant Imperfect

Only ~16-24 electrodes vs ~3500 IHC → coarse hearing.

### 11.4 Binaural vs Monaural

Single-ear hearing has poor localization (only spectral cue from pinna).

### 11.5 Noise Damage

> 85 dB long-term exposure → permanent hearing loss.

---

## 12. Related Concepts

- **Same section**: [Visual System](Visual_System.en.md), [Motor System](Motor_System.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Brainstem](../01_Neuroanatomy/Brainstem.en.md)

---

## References

1. **Pickles, J. O.** *An Introduction to the Physiology of Hearing*. 4th ed., 2012.
2. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
3. **Moore, B. C. J.** *An Introduction to the Psychology of Hearing*. 6th ed., 2012.
