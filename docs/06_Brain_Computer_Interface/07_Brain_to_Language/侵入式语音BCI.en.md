# Invasive Speech BCI

**Invasive speech BCI** is the most active BCI subfield of 2020–2025. The goal is to restore natural conversation for patients who have lost the ability to speak (ALS, brainstem stroke, locked-in syndrome). From Moses 2021 (15 WPM) to Willett 2023 (62 WPM) to Card 2024 (UC Davis), performance has tripled in three years and is closing in on natural conversational speed.

## 1. Neural Basis of Speech

### Speech-related brain regions

- **vSMC (ventral sensorimotor cortex)**: motor encoding of articulators (mouth, tongue, larynx)
- **dPCG (dorsal precentral gyrus)**: near hand area, surprisingly encodes **speech-related muscle activity**
- **Broca's area (Brodmann 44/45)**: language production
- **Superior temporal gyrus (STG)**: speech perception

### Overt vs imagined speech

- **Overt-speech BCI**: decodes neural activity during **actual speech**
- **Imagined-speech BCI**: decodes "thinking of speaking without uttering" (silent speech)
- Current mainstream: **attempted speech** — the patient attempts to speak silently, and the neural signal is still clear

## 2. UCSF Moses 2021 (15 WPM)

**Moses et al. (2021, NEJM)**:

- **Subject**: Pancho (anarthria from brainstem stroke, 18 years)
- **Electrodes**: 128-channel **high-density ECoG** (subdural, vSMC)
- **Decoder**:
  - RNN decodes **word-level**
  - 50-word vocabulary
  - HMM + statistical language model

### Performance

- **15 WPM**
- **>90% accuracy** over a 50-word vocabulary
- **Daily real-time use** — Pancho's first "conversation" with family

### Significance

- **First demonstration that ECoG can drive a real-time speech BCI**
- Feasibility of word-level decoding + LM
- **Speech intent is still decodable after 18 years of anarthria** — the neural activity has not vanished

## 3. Willett 2023 (62 WPM)

**Willett, Kunz et al. (2023, Nature)**:

- **Subject**: Pat Bennett (ALS)
- **Electrodes**: 4 × Utah Array = **256-channel spike** (vSMC + dPCG)
- **Decoder**:
  - Dual RNN (local + global)
  - CTC outputs **phonemes**
  - 3-gram LM + GPT-2 rescoring

### Architecture details

```
Spike rates (256 ch, 20 ms bins)
  ↓
Input RNN (per-area)
  ↓
Main RNN
  ↓
Phoneme logits (41 phonemes + blank)
  ↓
CTC decoding
  ↓
Beam search + 3-gram LM
  ↓
GPT-2 rescoring
  ↓
Text
```

### Performance

- **62 WPM** (natural speech ~150 WPM)
- Vocabulary of 125,000 (full English)
- **WER 9.1%**
- Training data: ~10,000 sentences

### Significance

- Crossed the "usefulness threshold" — exceeds 1/3 of natural conversational speed
- **LM rescoring gives a 50% relative WER reduction** (23% → 9.1%)
- Utah spike > ECoG demonstrates the value of channel density

### Key engineering lessons

1. **dPCG + vSMC is better than vSMC alone** — speech is **distributed**
2. **Spike > LFP > ECoG** — the finer the better
3. **Phoneme > Word** decoding is more flexible (OOV words can be spelled)
4. **LM is indispensable**

## 4. UCSF Metzger 2023 (Avatar, 78 WPM)

**Metzger et al. (2023, Nature)**:

- **Subject**: Ann (brainstem stroke, 18 years)
- **Electrodes**: 253-channel **high-density ECoG** (Paradromics-style)
- **Outputs**:
  - Text + voice synthesis (from recordings of Ann before her wedding)
  - Virtual avatar facial expressions

### Performance

- **78 WPM**
- Vocabulary 1024
- Avatar expression synchronized

### Innovations

- **Three-way parallel decoding**: text, voice, facial muscles
- Voice synthesis uses **the patient's own younger voice**
- The avatar gives the BCI output an **emotional dimension**

## 5. UC Davis Card 2024 (256 WPM milestone)

**Card et al. (2024, NEJM)**:

- **Subject**: Casey Harrell (ALS)
- **Electrodes**: 4 × Utah = 256 channels
- **Highlights**:
  - Larger vocabulary (125K full English)
  - **Peak real-time WPM of 256**
  - Average **62 WPM at WER 3%**

### Significance

- **WER 3% approaches human level**
- Proves the "Willett method" is reproducible and **continues to improve**
- UC Davis becomes the second speech-BCI center alongside UCSF

## 6. Key Technical Components

### Spike sorting (or skipping it)

Modern speech BCIs mostly use **threshold crossings** (TCR) rather than spike sorting — deep networks learn directly from binned spike counts.

### Features

- **Spike rate** (20 ms bins)
- **High-γ power** (80–200 Hz LFP envelope)
- Using both gives the best performance

### Models

- **Willett**: dual RNN + CTC
- **Moses**: GRU-based word classifier
- **Metzger**: Transformer + multi-task

### Vocabulary strategies

- **Closed vocabulary** (Moses): fast but limited
- **Open vocabulary with phonemes** (Willett): slower but unlimited
- **Hybrid**: common words decoded directly + rare words fall back to phonemes

## 7. Role of LMs in Speech BCI

The language model is the **core amplifier** of speech BCI:

### Stage 1: Decoding

Neural → phoneme probabilities (every 20 ms)

### Stage 2: Beam search + n-gram LM

Combine phoneme probabilities × LM likelihood

$$P(\text{word sequence}) = P_{\text{acoustic}}(\text{phonemes}) \cdot P_{\text{LM}}(\text{words})$$

### Stage 3: Neural LM rescoring

Top-K candidates are rescored by GPT-2/GPT-4:

- Willett: GPT-2 rescoring drops WER from 23% to 9.1%
- Future: directly use GPT-4 / Claude

### Theory

BCI signals have low SNR — **the LM prior provides a huge boost**. This mirrors the history of ASR (speech recognition) exactly: the combination of acoustic model + LM was the key to ASR's success.

## 8. Latency & Real-Time Performance

Latency breakdown for speech BCI:

- Neural signal acquisition: 20 ms
- Preprocessing + features: 10 ms
- Neural network inference: 30–50 ms
- LM beam search: 50 ms
- **Total**: ~100–150 ms

Compatible with natural conversation (100–300 ms response time). But GPT-4 rescoring adds 500 ms+ — latency-aware rescoring is needed.

## 9. Multilingual and Cross-Language-Family

Most speech BCIs are **English**. Challenges:

- **Chinese**: tones, characters vs pinyin
- **Korean**: syllable blocks
- **Japanese**: kana + kanji
- **Sign languages**: an entirely different modality

**Ma et al. 2024** (Fudan / Tsinghua) built the first Chinese invasive speech BCI, showing that the method transfers but **vocabulary and LM must be localized**.

## 10. Comparison with Non-Invasive Approaches

| | Non-invasive (MEG/EEG) | Invasive (ECoG/spike) |
| --- | --- | --- |
| Top performance | ~5 WPM (MEG) | 62+ WPM |
| Vocabulary | Small (100–1000) | Large (125K) |
| Surgery | None | Craniotomy |
| Applicable patients | General | ALS, brainstem stroke |

Invasive remains the **only currently practical** solution for restoring speech. Whether non-invasive can catch up is discussed in [Non-invasive Brain-to-Text](非侵入式脑-文本.md).

## 11. Logical Chain

1. **Moses 2021** proved ECoG could deliver a real-time speech BCI (15 WPM).
2. **Willett 2023** reached 62 WPM using Utah spike + LM rescoring — a tipping point.
3. **Metzger 2023 avatar** extended output to **voice + facial expression**.
4. **Card 2024** demonstrated reproducibility and drove WER down to 3%.
5. **LMs are the performance multiplier for speech BCI** — GPT-4/Claude-class models will be used in the future.
6. **Multilingual + cross-family** support is the main extension direction from 2025 onward.

## References

- Moses et al. (2021). *Neuroprosthesis for decoding speech in a paralyzed person with anarthria.* NEJM. https://www.nejm.org/doi/full/10.1056/NEJMoa2027540
- Willett et al. (2023). *A high-performance speech neuroprosthesis.* Nature.
- Metzger et al. (2023). *A high-performance neuroprosthesis for speech decoding and avatar control.* Nature.
- Card et al. (2024). *An accurate and rapidly calibrating speech neuroprosthesis.* NEJM.
- Anumanchipalli et al. (2019). *Speech synthesis from neural decoding of spoken sentences.* Nature.
