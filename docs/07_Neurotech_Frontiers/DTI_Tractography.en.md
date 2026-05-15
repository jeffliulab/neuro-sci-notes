# Diffusion Tensor Imaging & Tractography

> *DTI uses the **anisotropic diffusion** of water along axon bundles in white matter to reconstruct large-scale neural pathways. FA (fractional anisotropy), tractography (fiber tracking) are mainstays of macro-connectome (see [Connectomics](../00_Foundations/Connectomics.en.md)) and clinical use (presurgical planning, white matter disease). But only mm-scale tracts, not single axons, with a "crossing fiber" problem.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Connectomics](../00_Foundations/Connectomics.en.md), MRI basics

---

## 1. Principle

- Water Brownian motion: in white matter, **diffuses fast along axon bundles**, slow perpendicular (myelin + membrane constraint)
- Apply multi-direction diffusion gradients → measure diffusion in each direction
- Fit **diffusion tensor** (3×3 symmetric matrix) → principal eigenvector = local fiber direction

---

## 2. Key Metrics

| Metric | Meaning |
|---|---|
| **FA** (fractional anisotropy) | Anisotropy degree (0=isotropic, 1=unidirectional) → white matter integrity |
| **MD** (mean diffusivity) | Average diffusion rate |
| **AD / RD** | Axial / radial diffusivity (myelin vs axon) |
| Principal eigenvector | Fiber direction (used by tractography) |

---

## 3. Tractography

- From seed voxel follow principal direction in "streamlines" → reconstruct fiber tract
- **Deterministic**: single direction following
- **Probabilistic**: sample direction distribution (handles uncertainty)
- Major tracts: corticospinal tract, arcuate fasciculus (language), corpus callosum, cingulum

---

## 4. PyTorch — Tensor Fit + FA

```python
import torch

def fit_tensor_and_fa(signals, bvecs, bval, S0):
    """signals: (n_dir,) diffusion-weighted; bvecs: (n_dir,3)."""
    # log(S/S0) = -b * g^T D g  -> linear least squares for D (6 unique)
    y = -torch.log(signals / S0) / bval
    g = bvecs
    A = torch.stack([g[:,0]**2, g[:,1]**2, g[:,2]**2,
                     2*g[:,0]*g[:,1], 2*g[:,0]*g[:,2], 2*g[:,1]*g[:,2]], dim=1)
    d = torch.linalg.lstsq(A, y).solution
    D = torch.tensor([[d[0],d[3],d[4]],[d[3],d[1],d[5]],[d[4],d[5],d[2]]])
    ev = torch.linalg.eigvalsh(D)
    md = ev.mean()
    fa = torch.sqrt(1.5 * ((ev - md)**2).sum() / (ev**2).sum())
    return D, fa
```

---

## 5. Crossing Fiber Problem

- Single-tensor assumes one direction → fails when voxel has multiple crossing bundles (~ 90% of white matter voxels!)
- Solutions: **HARDI**, **Q-ball**, **CSD** (constrained spherical deconvolution), **DSI**
- High b-value + many directions → resolve multiple fiber directions (fiber ODF)

---

## 6. Applications

- **Presurgical planning**: protect peritumoral tracts (avoid motor/language tracts)
- **White matter disease**: MS, TBI (DAI), leukodystrophy → FA↓
- **Development / aging**: myelination trajectory
- **Psychiatric**: connectopathy (SCZ arcuate fasciculus FA↓ etc.)
- **Macro-connectome**: Human Connectome Project

---

## 7. vs Microscale Connectome

| | DTI tractography | EM connectome |
|---|---|---|
| Scale | mm tracts | nm single synapse |
| In vivo | ✓ | ✗ (postmortem) |
| Whole brain | ✓ | Very local |
| Precision | Low (indirect) | Very high |

→ Complementary: DTI gives in-vivo large-scale map, EM gives microscopic ground truth (see [Connectomics](../00_Foundations/Connectomics.en.md)).

---

## 8. Limitations + Pitfalls

- **Not true neural tracts**: inferred from water diffusion, has false positives/negatives
- Crossing/kissing/fanning fiber ambiguity
- No directionality (A→B vs B→A)
- FA non-specific (myelin / axon / edema all affect)
- "Tractography is not a gold standard" (Maier-Hein 2017 large comparison)

---

## 9. Relation to AI

- Deep learning tractography (learn fiber directions / direct streamlines)
- DTI → connectome → GNN disease classification
- Data-driven fiber ODF estimation

---

## 10. Common Pitfalls

### 10.1 Tractography = Real Axon Tracts

It's statistical inference; high false positives (Maier-Hein 2017).

### 10.2 FA↓ = Axon Damage

Non-specific: myelin, edema, crossing fibers all affect.

### 10.3 Shows Directionality (A→B)

Diffusion has no direction; can't determine directionality.

### 10.4 Single Tensor Sufficient

~ 90% of voxels have crossing fibers → need HARDI/CSD.

### 10.5 = Single-Neuron Connectome

mm-scale tracts, not single axons; complementary to EM, not equivalent.

---

## 11. Related Concepts

- **Same section**: [fMRI BOLD](fMRI_BOLD.en.md), [MEG](MEG.en.md)
- **Foundation**: [Connectomics](../00_Foundations/Connectomics.en.md), [Research Methods](../00_Foundations/Research_Methods.en.md)
- **Disease**: [Multiple Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.en.md), [Traumatic Brain Injury](../08_Neuro_Disorders/Traumatic_Brain_Injury.en.md)

---

## References

1. **Basser, P. J. et al.** "MR diffusion tensor spectroscopy and imaging." *Biophys J*, 1994.
2. **Tournier, J.-D. et al.** "Constrained spherical deconvolution." *NeuroImage*, 2007.
3. **Maier-Hein, K. H. et al.** "The challenge of mapping the human connectome based on diffusion tractography." *Nat Commun*, 2017.
4. **Jones, D. K. et al.** "White matter integrity, fiber count, and other fallacies." *NeuroImage*, 2013.
