# 视觉系统 — retina → LGN → V1 → V2-V5

> *视觉系统占大脑 30%+ 处理资源。从视网膜 → 外侧膝状体 (LGN) → V1 → 高层视觉区,层级化处理 edge → shape → object → face。Hubel & Wiesel 1959 V1 simple cell 发现启发了 CNN。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)、[神经元](../02_Cellular_Molecular/Neuron.md)

---

## 1. 视觉通路

```
Retina (photoreceptors + bipolar + ganglion)
   ↓ optic nerve
Optic chiasm (部分交叉)
   ↓
LGN (thalamus 外侧膝状体)
   ↓
V1 (primary visual cortex, 枕叶)
   ↓
V2, V3, V4 (color), V5/MT (motion)
   ↓
两大 stream:
   - Ventral (what): V1→V2→V4→IT — 物体识别
   - Dorsal (where): V1→V2→V5→PPC — 空间 + 运动
```

---

## 2. Retina

3 层:
- **Photoreceptors**:
  - Rods: 暗视, ~100M
  - Cones: 彩色, ~5M (3 类: S/M/L)
- **Bipolar cells**: 中间处理
- **Ganglion cells**: 输出到 LGN
  - M cells: 大、快、亮度
  - P cells: 小、慢、color
  - 共 ~1M (= optic nerve fiber count)

视网膜已做大量计算 (center-surround receptive field, edge detection)。

---

## 3. LGN

外侧膝状体,6 层:
- 1-2: Magnocellular (M cells)
- 3-6: Parvocellular (P cells)
- Koniocellular: 之间 layer

LGN 是 thalamic relay,部分调制但主要 pass-through。

---

## 4. V1 (Primary Visual Cortex, Area 17)

枕叶,~ 140k neurons / mm²。

### 4.1 Receptive Fields

Hubel & Wiesel 1959:
- **Simple cells**: 朝向选择 oriented edges
- **Complex cells**: 朝向 + 位置不变 (类 max pooling)
- **Hypercomplex cells**: 端点检测

### 4.2 Retinotopy

V1 是 retina 的 map — 邻近 retina 区映射邻近 V1 区。

### 4.3 Column 组织

- Orientation columns (~ 200 μm wide)
- Ocular dominance columns
- Hypercolumns (含所有 orientation × 双眼)

---

## 5. 高层视觉 (Ventral Stream)

```
V1 (edges, orientations) →
V2 (illusory contours) →
V4 (color, intermediate shapes) →
IT (Inferotemporal: faces, objects, scenes)
```

- **FFA** (Fusiform Face Area): 人脸专门
- **PPA** (Parahippocampal Place Area): 场景
- **VWFA**: 文字

→ 层级抽象,类似 CNN feature 层。

---

## 6. CNN ↔ V1 类比

| CNN layer | V1 等价 |
|---|---|
| Conv kernel | Simple cell receptive field |
| ReLU | Spike threshold |
| Max pooling | Complex cell (translation invariance) |
| Deeper layers | V2-V4-IT (越来越抽象) |
| Final classifier | IT pattern classifier |

Yamins & DiCarlo (2014):CNN 中间层与 IT 神经元 firing 相关 r > 0.7。

---

## 7. PyTorch — V1 模拟

```python
import torch
import torch.nn as nn

class V1Layer(nn.Module):
    """Gabor-like simple cells."""
    def __init__(self, n_orientations=8):
        super().__init__()
        # Gabor kernels at different orientations
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
        # Convolve with all orientations
        responses = []
        for kernel in self.kernels:
            r = torch.nn.functional.conv2d(image, kernel.unsqueeze(0).unsqueeze(0))
            responses.append(torch.relu(r))
        return torch.cat(responses, dim=1)
```

---

## 8. 病理

- **Cortical blindness**: V1 损伤 → 失明 (Blindsight 仍存)
- **Prosopagnosia**: FFA 损 → 不认脸
- **Achromatopsia**: V4 损 → 色盲
- **Visual neglect**: 顶叶损 → 半边忽视
- **Hemianopsia**: 视野半失

---

## 9. 历史

- **1850s** — Helmholtz 视觉光学
- **1879** — Munk 找 V1 位置
- **1959** — Hubel & Wiesel V1 simple cells (Nobel 1981)
- **1980s** — DiCarlo / Ungerleider what-where streams
- **1990s** — fMRI 高分辨率 retinotopy
- **2010s** — DeepLearning ↔ visual cortex 对比研究
- **2020s** — Connectomics (MICrONS) 全部分 V1 connectome

---

## 10. Common Pitfalls

### 10.1 视觉 ≠ Vision-as-CNN

CNN 仅近似;real cortex 有 feedback, recurrence, attention。

### 10.2 V1 不只 edge

也编码 color, motion, depth。

### 10.3 What-Where 过度简化

近代发现两 stream 大量 cross-talk。

### 10.4 FFA 不专门 face

也响应 expertise objects (鸟类专家的鸟图)。

### 10.5 fMRI 时间分辨率

BOLD signal 几秒延迟,不适合精细 timing 研究。

---

## 11. Related Concepts

- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)
- **AI 对比**:[CNN](https://jeffliulab.github.io/ai-notes/02_Deep_Learning/03_Computer_Vision/CNN/)

---

## References

1. **Hubel, D. H. & Wiesel, T. N.** "Receptive fields of single neurones in the cat's striate cortex." *J Physiol*, 1959.
2. **Yamins, D. L. K. & DiCarlo, J. J.** "Using goal-driven deep learning models to understand sensory cortex." *Nat Neurosci*, 2016.
3. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
4. **Ungerleider, L. G. & Mishkin, M.** "Two cortical visual systems." 1982.
