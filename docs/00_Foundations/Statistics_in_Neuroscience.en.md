# Statistics in Neuroscience

> *Neuroscience data is high-dimensional, noisy, small-sample — full of statistical pitfalls. Replication crisis (Button 2013 low power), double dipping (Kriegeskorte 2009), multiple comparisons (fMRI dead salmon), circular analysis are classic traps. Modern: mixed-effects models, permutation test, cluster correction, preregistration. Understanding statistics is prerequisite to reading literature.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: Basic probability + statistics

---

## 1. Neural Data Characteristics

- **High-dimensional**: millions of voxels / thousands of neurons
- **Noisy**: trial-to-trial variability
- **Small samples**: animal / patient n often < 20
- **Nested structure**: trial < neuron < animal < group
- **Temporal correlation**: autocorrelation

---

## 2. Multiple Comparisons Problem

- fMRI: ~ 100,000 voxels, t-test each
- α=0.05 → ~ 5000 false positives
- **Dead salmon study** (Bennett 2009): dead salmon fMRI shows "activation" — satirizes uncontrolled multiple comparisons

### Correction Methods

- **Bonferroni**: overly conservative
- **FDR** (Benjamini-Hochberg): controls false discovery rate
- **Cluster-based permutation**: exploits spatial correlation (mainstream)
- **Random field theory**

---

## 3. Double Dipping

- Using same data to **select** + **test**
- E.g.: pick most responsive voxel → re-test its response (inevitably significant!)
- Kriegeskorte 2009 warning
- Solution: independent train/test split

---

## 4. Statistical Power Crisis

- Button 2013: median power in neuroscience ~ 20%
- Low power → true effects missed + significant results inflated (winner's curse)
- Small sample + large reported effect → not replicable

---

## 5. p-hacking + Garden of Forking Paths

- Multiple analysis paths → report the significant
- Flexible preprocessing / exclusion criteria
- Solution: **preregistration**, registered reports

---

## 6. Recommended Methods

| Problem | Method |
|---|---|
| Nested data | Linear mixed-effects models |
| Multiple comparisons | Cluster permutation / FDR |
| Small sample | Permutation / bootstrap |
| Selection bias | Cross-validation, held-out |
| Effect size | Report effect size + CI (not just p) |
| Replicability | Preregistration |

---

## 7. NumPy — Permutation Test

```python
import numpy as np

def permutation_test(group_a, group_b, n_perm=10000):
    """Non-parametric test of mean difference."""
    observed = group_a.mean() - group_b.mean()
    combined = np.concatenate([group_a, group_b])
    n_a = len(group_a)
    count = 0
    for _ in range(n_perm):
        np.random.shuffle(combined)
        diff = combined[:n_a].mean() - combined[n_a:].mean()
        if abs(diff) >= abs(observed):
            count += 1
    return count / n_perm   # empirical p-value
```

---

## 8. Bayesian Perspective

- Bayes factor instead of p-value
- Quantifies "how much support for null" (p-value can't)
- Hierarchical Bayesian models naturally handle nesting
- But requires prior choice

---

## 9. Decoding / MVPA Pitfalls

- High-dimensional decoder easily overfits
- Must cross-validate
- Above-chance ≠ large effect
- Label leakage (temporal correlation → train/test leak)

---

## 10. Neural Data Replication Movement

- **OHBM**, **registered reports**
- **Neuroimaging data sharing** (OpenNeuro)
- NARPS (2020): 70 teams analyzed same fMRI → conclusions varied enormously
- Highlights analytic flexibility problem

---

## 11. Common Pitfalls

### 11.1 p < 0.05 = True

Under low power, significant often false too; need effect size + replication.

### 11.2 Pick Neuron Then Test

Double dipping → circular reasoning. Must use independent data.

### 11.3 Large Sample Needs No Correction

Large sample still needs multiple comparison correction (voxel count is the issue).

### 11.4 n = Number of Animals

Trials ≠ independent samples; need mixed model (pseudoreplication).

### 11.5 High Decoding = Brain Uses It

Decoder is observer's tool, not evidence of brain mechanism.

---

## 12. Related Concepts

- **Same section**: [Research Methods](Research_Methods.en.md), [Levels of Analysis](Levels_of_Analysis.en.md)
- **Frontiers**: [fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.en.md), [Calcium Imaging](../07_Neurotech_Frontiers/Calcium_Imaging.en.md)
- **AI**: cross-validation, overfitting

---

## References

1. **Button, K. S. et al.** "Power failure: why small sample size undermines the reliability of neuroscience." *Nat Rev Neurosci*, 2013.
2. **Kriegeskorte, N. et al.** "Circular analysis in systems neuroscience." *Nat Neurosci*, 2009.
3. **Bennett, C. M. et al.** "Neural correlates of interspecies perspective taking in the post-mortem Atlantic Salmon." *J Serendipitous Unexpected Results*, 2010.
4. **Botvinik-Nezer, R. et al.** "Variability in the analysis of a single neuroimaging dataset by many teams (NARPS)." *Nature*, 2020.
