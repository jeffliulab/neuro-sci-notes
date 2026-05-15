# Optogenetics — Advanced & Variants

> *Building on [Optogenetics](Optogenetics.en.md): red-shifted opsins (deeper penetration), step-function opsins (bistable), inhibitory pumps vs channels, Cre-dependent cell specificity, projection-specific (axon terminal illumination), all-optical (light control + light readout), wireless/fiberless (upconversion, sonogenetics). A precision toolbox for causal circuit dissection.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Optogenetics](Optogenetics.en.md), [Calcium Imaging](Calcium_Imaging.en.md)

---

## 1. Opsin Toolbox

| Opsin | Ion | Effect | Wavelength |
|---|---|---|---|
| ChR2 | Na⁺/Ca²⁺ influx | Excite | Blue 470 nm |
| ChrimsonR | Cation | Excite | Red ~ 620 nm (deep) |
| ReaChR / bReaChES | Cation | Excite | Red-shifted |
| NpHR | Cl⁻ pump in | Inhibit | Yellow 590 nm |
| Arch / eArchT | H⁺ pump out | Inhibit | Green |
| GtACR | Cl⁻ channel | Strong inhibit | Blue |
| SFO/SSFO | Bistable ChR | Long-lasting excite | Blue on/green off |

---

## 2. Red-Shifted = Deep Penetration

- Blue light scatters strongly, shallow penetration (< 1 mm)
- Red/near-IR scatters less → deeper
- ChrimsonR, ReaChR → reduce fiber invasiveness
- + upconversion nanoparticles → near-IR drives blue opsin (fiberless)

---

## 3. Step-Function Opsin (Bistable)

- One light pulse **on**, another wavelength **off**
- Neuron maintains de/hyperpolarization long-term (no continuous light)
- Reduces phototoxicity + mimics sustained state modulation

---

## 4. Cell + Projection Specificity

- **Cell-type**: Cre-driven (DIO/FLEx), only specific types express opsin
- **Projection-specific**: illuminate **axon terminals** → control only one projection pathway
- **Retrograde virus** (CAV2, retro-AAV) → label by projection target
- → Precisely dissect "causal role of A→B pathway in behavior X"

---

## 5. All-Optical

- **Light control** (opsin, red) + **light readout** (GCaMP, green) simultaneously
- Needs spectral separation (avoid GCaMP blue excitation mistakenly activating ChR2 → use red opsin)
- All-optical causal circuit manipulation + readout (closed-loop possible)

---

## 6. PyTorch — Projection-Specific Stimulation Model

```python
import torch

def projection_specific_opto(neurons, projection_mask, light_on, gain=5.0):
    """Only neurons projecting to target (mask) + expressing opsin respond."""
    drive = torch.zeros_like(neurons)
    if light_on:
        # Light at axon terminals activates only the masked projection
        drive = projection_mask.float() * gain
    return torch.relu(neurons + drive)   # selective pathway activation
```

---

## 7. Closed-Loop Optogenetics

- Real-time read neural/behavior → trigger light stimulation
- E.g.: detect seizure onset → immediately inhibit (treatment prototype)
- Detect sharp-wave ripple → perturb → test memory causality
- Shares concepts with BCI / closed-loop DBS

---

## 8. Clinical Translation Challenges

- Needs gene delivery (AAV) → safety + control + immunity
- Fiber implant (invasive) → fiberless approaches (upconversion, sonogenetics, magnetothermal)
- Retinitis pigmentosa: ChrimsonR vision restoration **already in human trials** (GenSight, first human optogenetic therapy)
- Long-term expression + phototoxicity

---

## 9. Variant Technologies

- **Chemogenetics (DREADD)**: drugs instead of light, no implant (low temporal precision)
- **Magnetogenetics**: magnetic field (controversial)
- **Sonogenetics**: ultrasound + mechanosensitive channels (non-invasive, see [Focused Ultrasound](Focused_Ultrasound.en.md))
- **Optopharmacology**: light-controlled drugs

---

## 10. Common Pitfalls

### 10.1 ChR2 Fits All Experiments

Blue light shallow + scattered; deep needs red-shifted opsin / fiber.

### 10.2 Inhibitory Opsins All the Same

Pumps (NpHR/Arch) vs channels (GtACR) differ in mechanism + side effects (pumps alter pH/Cl gradients).

### 10.3 Light Stimulation = Physiological

Unnatural synchronous activation; can produce non-physiological patterns.

### 10.4 Cell Specificity Automatic

Depends on promoter/Cre specificity + expression control, needs verification.

### 10.5 = Imminent Clinical Adoption

Retinal trials lead; intracranial clinical still distant (delivery + invasiveness challenges).

---

## 11. Related Concepts

- **Same section**: [Optogenetics](Optogenetics.en.md), [Calcium Imaging](Calcium_Imaging.en.md), [Focused Ultrasound](Focused_Ultrasound.en.md)
- **Cellular**: [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md)
- **Foundation**: [Neural Circuits](../00_Foundations/Neural_Circuits.en.md)

---

## References

1. **Deisseroth, K.** "Optogenetics: 10 years of microbial opsins in neuroscience." *Nat Neurosci*, 2015.
2. **Klapoetke, N. C. et al.** "Independent optical excitation of distinct neural populations (Chrimson/Chronos)." *Nat Methods*, 2014.
3. **Sahel, J.-A. et al.** "Partial recovery of visual function in a blind patient after optogenetic therapy." *Nat Med*, 2021.
4. **Emiliani, V. et al.** "Optogenetics for light control of biological systems." *Nat Rev Methods Primers*, 2022.
