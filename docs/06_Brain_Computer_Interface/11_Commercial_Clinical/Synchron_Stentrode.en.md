# Synchron Stentrode

**Synchron Stentrode** is the **minimally invasive alternative pathway** for BCI — placing electrodes through blood vessels and avoiding craniotomy. By 2024 it has been operating long-term in 6 patients and is the **safest and fastest-to-market** BCI approach. Synchron's partnerships with OpenAI and Apple foreshadow a future in which **consumer-grade speech/handwriting BCI** may reach the market before Neuralink.

## 1. The Stentrode Device

### Structure

- A **vascular stent** serves as the electrode carrier
- 16 electrodes are anchored on the mesh structure
- Placed in the **superior sagittal sinus**, adjacent to M1
- Inserted via the **jugular vein**

### Surgery

- **Endovascular procedure:** jugular catheter → superior sagittal sinus
- No craniotomy required
- 1–2 hours, local anesthesia
- **Rapid postoperative recovery**

### Wireless Data

- **BrainOS** wireless module implanted in the chest
- Bluetooth transmission
- No exposed cabling

## 2. Why the Endovascular Route

### Anatomy

- The brain surface is covered by a layer of venous sinuses
- The superior sagittal sinus lies **directly above M1 / SMA**
- Intravascular signal ≈ epidural ECoG

### Advantages

- **No brain-tissue injury**
- Surgical safety approaches that of **stent placement** (already routine)
- Interfaces naturally with vascular surgery

### Disadvantages

- **Lower signal quality** (farther from neurons)
- **Few channels** (16)
- **Coarse spatial resolution**

## 3. The COMMAND Clinical Trial

### Launched in 2021

- FDA IDE approval (2021)
- Multi-center (Mount Sinai, U Pittsburgh, Sydney)
- Target: **6 patients, 1-year follow-up**

### Inclusion

- ALS, spinal cord injury
- Loss of arm function
- Cognition intact

### First Patient: Philip O'Keefe (Australia 2021)

- ALS patient
- **Sent the first "brain tweet" in 2021**
- World's first BCI social-media post
- Has continued to use it for 3+ years

### First U.S. Patient (2022): Rodney Gorham

- Also ALS
- Mount Sinai
- Day-to-day computer control

### 2024: All 6 Patients Enrolled

- UK, USA, Australia
- Integrated results pending publication

## 4. Capability Demonstrations

### Basic Control

- Mouse cursor
- Clicks (imagined hand movements)
- Texting, email
- Web browsing

### Speed

- ~10–15 clicks per minute
- **Slower than Neuralink** but **usable day-to-day**

### Reliability

- **Stable long-term (3+ years)**
- This is Stentrode's strongest advantage
- Neuralink has not yet reached this level of longitudinal validation

## 5. Synchron × OpenAI Collaboration

### 2023 Announcement

- Synchron integrated **ChatGPT** into the BCI
- Brain control + LLM dialogue augmentation
- The LLM helps complete what the user wants to say

### Implementation

- BCI decoding → intent
- Intent → OpenAI API → generated text
- User confirms by selection

### Significance

- **LLMs fill the bandwidth gap of low-throughput BCIs**
- Stentrode's 16 channels are too few for fast typing, but **enough to select intents**
- See [LLM post-processing fusion](../07_Brain_to_Language/LLM后处理融合.md)

## 6. Synchron × Apple Collaboration

### 2024 News

- Synchron integrating with Apple **Vision Pro**
- BCI control of visionOS
- AR / VR made accessible to paralyzed patients

### Goal

- **Consumer AR + BCI**
- Medical first, then expansion

## 7. Synchron's Commercial Strategy

### Medical Focus

- Avoids Musk-style hype
- Quiet progress with physicians and patients
- FDA PMA expected 2027

### Cooperate Rather than Monopolize

- Partnering with Apple and OpenAI
- **"BCI as interface"** rather than **"BCI as operating system"**
- Positioning analogous to a **USB protocol**

### Financing

- Raised ~$145 M to date
- Gates Foundation, Khosla, Bezos among investors
- Valuation ~$1B

## 8. Technical Comparison

### vs Neuralink

| | Neuralink | Synchron |
| --- | --- | --- |
| Channels | 1024 | 16 |
| Invasion | Intra-cortical | Intravascular |
| Surgery | Complex robotic | Endovascular |
| Signal | Single-unit | LFP-like |
| Speed | Fast | Slow |
| Safety | Research | Close to approval bar |
| Commercial | 2027+ | 2026+ |

### vs Precision

See [Precision_Paradromics_Blackrock](Precision_Paradromics_Blackrock.md).

## 9. Future Roadmap

### 1. More Channels

- Next-generation Stentrode: 32 → 64 channels
- Multi-stent concurrent implants
- Still far below Neuralink

### 2. Stimulation Capability

- Read-only at present
- Bidirectional versions under study (ICMS from inside the vessel?)

### 3. Consumer Grade

- Combine with LLMs → **low channel count can still deliver fast intent**
- Consumer partners such as Apple
- After medical approval → expansion into consumer health

### 4. Global Expansion

- European and Asian markets
- Expected price **$25,000–50,000** (cheaper than surgical BCIs)

## 10. Indication Expansion

### Near term (2026–2028)

- ALS
- Spinal cord injury
- Locked-in syndrome

### Medium term (2028–2030)

- Stroke (partial motor function)
- Multiple sclerosis
- Severe frailty in the elderly

### Long term (2030+)

- Consumer-grade healthy users
- AR / VR interfaces
- Workplace assistance

## 11. Limitations

### 1. Signal Quality

- Intravascular placement is **several millimeters** from neurons
- Good temporal resolution, poor spatial resolution
- Complex tasks (high-DOF motor, speech) lag behind cortical recordings

### 2. Vascular Constraints

- Electrode location is **constrained by vascular anatomy**
- Cannot precisely target a chosen brain region
- Some regions **are unreachable**

### 3. Thrombosis Risk

- A foreign body inside a vessel → thrombosis
- Long-term anticoagulant therapy
- Drug side effects

### 4. Scale

- Only 6 patients today — small
- Neuralink expanded faster in 2024

## 12. Logic Chain

1. **Stentrode is implanted via blood vessels**, skipping craniotomy — the **safest BCI pathway**.
2. **16 channels but long-term stability** — validated 3+ years without issue.
3. **The COMMAND trial has enrolled all 6 patients** across the U.S., UK, and Australia.
4. **Partnership with OpenAI** uses LLMs to compensate low bandwidth → practical utility.
5. **Partnership with Apple** points to AR/VR + BCI consumer directions.
6. **Commercialization expected 2026+**, ahead of Neuralink.
7. **Limitations:** signal quality, vascular constraints, thrombosis risk — but the safety advantage is overwhelming.

## References

- Mitchell et al. (2023). *Assessment of safety of a fully implanted endovascular brain-computer interface for severe paralysis in 4 patients: the Stentrode with Thought-Controlled Digital Switch (SWITCH) study.* JAMA Neurology.
- Oxley et al. (2021). *Motor neuroprosthesis implanted with neurointerventional surgery improves capacity for activities of daily living tasks in severe paralysis: first in-human experience.* J NeuroInterv Surg.
- Synchron (2023). Press release on ChatGPT integration.
- Opie et al. (2018). *Focal stimulation of the sheep motor cortex with a chronically implanted minimally invasive electrode array mounted on an endovascular stent.* Nat Biomed Eng.
- clinicaltrials.gov: NCT03834857 (SWITCH), NCT05035082 (COMMAND).
