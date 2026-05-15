# Blood-Brain Barrier

> *The BBB is a highly selective barrier of endothelial tight junctions + astrocyte endfeet + pericytes, protecting the CNS from blood toxins / pathogens. The cost: ~ 98% of small-molecule drugs + nearly all large-molecule drugs cannot enter the brain — the biggest obstacle in CNS drug development. Focused ultrasound, nanoparticles, Trojan horse are breakthrough strategies.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Glia](../02_Cellular_Molecular/Glia.en.md), membrane transport

---

## 1. Structure (Neurovascular Unit)

```
Blood
  │  endothelial cell (tight junction — key)
  │  basement membrane
  │  pericyte
  │  astrocyte endfeet (wrap > 99% of vessels)
Brain parenchyma (neuron)
```

- Tight junctions (claudin, occludin) seal intercellular gaps
- No fenestration (unlike peripheral vessels)
- Very high electrical resistance (~ 2000 Ω·cm²)

---

## 2. Crossing Modes

| Mode | Example |
|---|---|
| Passive diffusion | Small, lipophilic (O₂, CO₂, alcohol, caffeine) |
| Carrier transport | GLUT1 (glucose), LAT1 (amino acid) |
| Receptor-mediated transcytosis | Insulin, transferrin |
| Active efflux | P-glycoprotein (pumps drug back to blood) |
| Tight junction | Ions, large molecules blocked |

---

## 3. Selective Passage

- **Can cross**: O₂, CO₂, glucose (transport), small lipophilic molecules, alcohol, nicotine, most anesthetics/psychiatric drugs
- **Cannot cross**: most antibiotics, chemo drugs, proteins, antibodies, most hydrophilic drugs

---

## 4. P-glycoprotein Efflux

- ABC transporter, actively pumps foreign molecules back to blood
- Explains why many otherwise-diffusible drugs still have low brain concentration
- Also a chemotherapy resistance mechanism

---

## 5. Circumventricular Organs (No BBB)

- Area postrema (vomiting center — senses blood toxins)
- Median eminence, OVLT, SFO
- Regions needing to "sample" blood deliberately lack BBB

---

## 6. PyTorch — Simplified BBB Permeability Prediction

```python
import torch
import torch.nn as nn

class BBBPermeabilityNet(nn.Module):
    """Predict logBB from molecular descriptors (cheminformatics)."""
    def __init__(self, n_desc=10):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(n_desc, 32), nn.ReLU(),
            nn.Linear(32, 1)  # logBB = log(C_brain / C_blood)
        )
    def forward(self, descriptors):
        # descriptors: MW, logP, TPSA, HBD, HBA, ...
        return self.net(descriptors)
# Rule of thumb: MW<450, logP 1-3, TPSA<90 → likely crosses
```

---

## 7. Strategies to Cross BBB

| Strategy | Mechanism |
|---|---|
| Lipidization | Increase lipophilicity |
| Prodrug | Activate after entry |
| Trojan horse | Conjugate transferrin/insulin receptor ligand |
| Nanoparticle | Encapsulate + surface modification |
| Focused ultrasound + microbubble | Temporarily open BBB (in clinical trials) |
| Intranasal | Bypass (via olfactory nerve) |
| Intrathecal / ICV | Inject CSF directly |
| Osmotic disruption | Mannitol temporary break (invasive) |

---

## 8. Pathology — BBB Disruption

- **Multiple sclerosis**: BBB break → immune cell invasion (Gadolinium MRI detects)
- **Stroke**: ischemia → BBB break → vasogenic edema
- **Brain tumor**: neovascular incomplete BBB (contrast enhancement)
- **Alzheimer's**: BBB function decline → impaired Aβ clearance
- **Meningitis**: infection breaks BBB
- **Sepsis**: systemic inflammation → BBB leak → delirium

---

## 9. Development + Aging

- Neonatal BBB more permeable (hence kernicterus risk)
- Aging BBB gradually leaky
- Pericyte loss linked to neurodegeneration

---

## 10. Common Pitfalls

### 10.1 BBB Is a Single Membrane

It's a neurovascular unit (endothelium+pericyte+astrocyte), not a single membrane.

### 10.2 Lipophilic Always Crosses

P-gp efflux can counteract; TPSA, MW also key.

### 10.3 BBB Uniform Across Brain

CVO regions lack BBB; permeability varies by region.

### 10.4 Antibody Drugs Enter Brain

Most cannot; need engineering (brain shuttle).

### 10.5 Opening BBB Safe

Opening = loss of protection; FUS timing/dose must be precise.

---

## 11. Related Concepts

- **Same section**: [Brain Energy Metabolism](Brain_Energy_Metabolism.en.md), [Research Methods](Research_Methods.en.md)
- **Cellular**: [Glia](../02_Cellular_Molecular/Glia.en.md)
- **Disease**: [Multiple Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.en.md), [Stroke](../08_Neuro_Disorders/Stroke.en.md)
- **Frontiers**: Focused ultrasound, [DBS](../07_Neurotech_Frontiers/DBS.en.md)

---

## References

1. **Abbott, N. J. et al.** "Structure and function of the blood-brain barrier." *Neurobiol Dis*, 2010.
2. **Daneman, R. & Prat, A.** "The blood-brain barrier." *Cold Spring Harb Perspect Biol*, 2015.
3. **Pardridge, W. M.** "Drug transport across the blood-brain barrier." *J Cereb Blood Flow Metab*, 2012.
4. **Sweeney, M. D. et al.** "Blood-brain barrier breakdown in Alzheimer disease." *Nat Rev Neurol*, 2018.
