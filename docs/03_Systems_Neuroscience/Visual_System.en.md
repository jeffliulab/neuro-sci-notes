# Visual System — retina → LGN → V1 → V2-V5

> *Visual processing uses 30%+ of brain resources. From retina → LGN → V1 → higher visual areas, hierarchical processing edge → shape → object → face. Hubel & Wiesel 1959 V1 simple cells inspired CNNs.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Neuron](../02_Cellular_Molecular/Neuron.en.md)

---

## 1. Visual Pathway

```
Retina (photoreceptors + bipolar + ganglion)
   ↓ optic nerve
Optic chiasm (partial crossing)
   ↓
LGN (lateral geniculate, thalamus)
   ↓
V1 (primary visual cortex, occipital)
   ↓
V2, V3, V4 (color), V5/MT (motion)
   ↓
Two streams:
   - Ventral (what): V1→V2→V4→IT — object recognition
   - Dorsal (where): V1→V2→V5→PPC — spatial + motion
```

---

## 2. Retina

3 layers:
- **Photoreceptors**: rods (dim, ~100M), cones (color, ~5M, 3 types S/M/L)
- **Bipolar cells**: intermediate
- **Ganglion cells**: output to LGN (~1M = optic nerve)

Retina already does massive computation (center-surround RF, edge detection).

---

## 3. LGN

6 layers; M (magnocellular), P (parvocellular), K (koniocellular).
Thalamic relay with modulation.

---

## 4. V1 (Primary Visual Cortex)

### 4.1 Receptive Fields (Hubel & Wiesel 1959)

- **Simple cells**: oriented edge detection
- **Complex cells**: orientation + position invariance (like max pooling)
- **Hypercomplex cells**: end-stopping

### 4.2 Retinotopy

V1 is map of retina.

### 4.3 Columnar Organization

- Orientation columns (~200 μm)
- Ocular dominance columns
- Hypercolumns

---

## 5. Higher Visual (Ventral Stream)

```
V1 (edges) → V2 (illusory contours) → V4 (color, shapes) → IT (faces, objects)
```

- **FFA**: face-specific
- **PPA**: scenes
- **VWFA**: text

→ Hierarchical abstraction, similar to CNN feature layers.

---

## 6. CNN ↔ V1 Analogy

| CNN layer | V1 equivalent |
|---|---|
| Conv kernel | Simple cell RF |
| ReLU | Spike threshold |
| Max pooling | Complex cell (translation invariance) |
| Deeper layers | V2-V4-IT (more abstract) |
| Final classifier | IT pattern classifier |

Yamins & DiCarlo (2014): CNN middle layers correlate with IT firing r > 0.7.

---

## 7. PyTorch — V1 Simulation

```python
import torch
import torch.nn as nn

class V1Layer(nn.Module):
    def __init__(self, n_orientations=8):
        super().__init__()
        self.kernels = self._create_gabor_kernels(n_orientations)
    
    def _create_gabor_kernels(self, n):
        kernels = []
        for theta in torch.linspace(0, torch.pi, n):
            kernel = self._gabor(theta)
            kernels.append(kernel)
        return torch.stack(kernels)
    
    def _gabor(self, theta, sigma=3, lambd=8, gamma=0.5, size=11):
        xs, ys = torch.meshgrid(torch.arange(-size, size+1), torch.arange(-size, size+1), indexing='ij')
        x = xs * torch.cos(theta) + ys * torch.sin(theta)
        y = -xs * torch.sin(theta) + ys * torch.cos(theta)
        g = torch.exp(-(x**2 + gamma**2 * y**2) / (2 * sigma**2))
        g = g * torch.cos(2 * torch.pi * x / lambd)
        return g
    
    def forward(self, image):
        responses = []
        for kernel in self.kernels:
            r = torch.nn.functional.conv2d(image, kernel.unsqueeze(0).unsqueeze(0))
            responses.append(torch.relu(r))
        return torch.cat(responses, dim=1)
```

---

## 8. Pathology

- **Cortical blindness**: V1 damage → blindness (blindsight remains)
- **Prosopagnosia**: FFA damage → face recognition deficit
- **Achromatopsia**: V4 damage → color blindness
- **Visual neglect**: parietal damage → half-field neglect

---

## 9. History

- **1850s** — Helmholtz visual optics
- **1959** — Hubel & Wiesel V1 (Nobel 1981)
- **1980s** — what-where streams
- **1990s** — high-resolution fMRI retinotopy
- **2010s** — DeepLearning ↔ visual cortex comparison
- **2020s** — Connectomics (MICrONS) full V1 connectome

---

## 10. Common Pitfalls

### 10.1 Vision ≠ Vision-as-CNN

CNN only approximates; real cortex has feedback, recurrence, attention.

### 10.2 V1 Isn't Just Edges

Also encodes color, motion, depth.

### 10.3 What-Where Oversimplified

Modern findings show extensive cross-talk between streams.

### 10.4 FFA Not Solely Face

Also responds to expertise objects.

### 10.5 fMRI Temporal Resolution

BOLD signal seconds delayed; not for fine timing studies.

---

## 11. Related Concepts

- **Same section**: [Hippocampus + Memory](Hippocampus_Memory.en.md), [Motor System](Motor_System.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md)
- **AI comparison**: [CNN](https://jeffliulab.github.io/ai-notes/02_Deep_Learning/03_Computer_Vision/CNN/)

---

## References

1. **Hubel, D. H. & Wiesel, T. N.** "Receptive fields of single neurones in the cat's striate cortex." *J Physiol*, 1959.
2. **Yamins, D. L. K. & DiCarlo, J. J.** "Using goal-driven deep learning models to understand sensory cortex." *Nat Neurosci*, 2016.
3. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
4. **Ungerleider, L. G. & Mishkin, M.** "Two cortical visual systems." 1982.
