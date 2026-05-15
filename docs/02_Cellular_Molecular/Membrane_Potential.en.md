# Membrane Potential — Foundation of Neuronal Electrical Signals

> *Membrane potential is the voltage difference between neuron interior and exterior, typically -70 mV at rest. Arises from ion concentration gradients + selective channels + Na+/K+ ATPase pump. Foundation of spikes and synaptic activity.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neuron](Neuron.en.md), [Ion Channels](Ion_Channels.en.md)

---

## 1. Resting Membrane Potential

Typical cortical neuron: **-70 mV** (intracellular relative to extracellular).

Causes:
- Na+/K+ ATPase pump maintains ion gradients (3 Na+ out + 2 K+ in per cycle)
- K+ leak channels open, membrane near K+ equilibrium ($E_K \approx -90$ mV)
- Small Na+ leak keeps V slightly above $E_K$

---

## 2. Nernst Equation

Single ion equilibrium:

$$E_X = \frac{RT}{zF} \ln \frac{[X]_o}{[X]_i}$$

At 37°C, monovalent cation: $E_X \approx 61 \log_{10} \frac{[X]_o}{[X]_i}$ (mV).

Typical values:
- $E_{Na} = +60$ mV
- $E_K = -90$ mV
- $E_{Cl} = -75$ mV (human neuron)
- $E_{Ca} = +140$ mV (very high due to very low $[Ca]_i$)

---

## 3. Goldman-Hodgkin-Katz (GHK) Equation

Multi-ion case:

$$V_m = \frac{RT}{F} \ln \frac{P_K [K^+]_o + P_{Na} [Na^+]_o + P_{Cl} [Cl^-]_i}{P_K [K^+]_i + P_{Na} [Na^+]_i + P_{Cl} [Cl^-]_o}$$

$P_X$ = permeability. At rest $P_K \gg P_{Na}$, so V close to $E_K$ but slightly higher.

---

## 4. Ion Distribution

| Ion | $[X]_i$ (mM) | $[X]_o$ (mM) | $E$ (mV) |
|---|---|---|---|
| Na+ | 12 | 145 | +60 |
| K+ | 140 | 4 | -90 |
| Cl- | 7 | 110 | -75 |
| Ca²⁺ | 10⁻⁴ | 2 | +140 |

→ This disequilibrium is fundamental to neuron function.

---

## 5. Na+/K+ ATPase Pump

- Membrane protein
- Per cycle: 3 Na+ out + 2 K+ in, consumes 1 ATP
- Electrogenic → contributes ~-10 mV to resting potential
- ~50% of brain energy consumption

---

## 6. PyTorch — Resting Potential Calculation

```python
import numpy as np

def nernst(z, X_in, X_out, T_C=37):
    R = 8.314
    F = 96485
    T_K = T_C + 273.15
    return (R * T_K) / (z * F) * np.log(X_out / X_in) * 1000

print(f"E_Na = {nernst(1, 12, 145):.1f} mV")
print(f"E_K = {nernst(1, 140, 4):.1f} mV")
```

---

## 7. Relation to Spikes

- Rest -70
- EPSP / IPSP local small fluctuations
- Threshold -55 → Na+ channels open → AP

---

## 8. Pathology

- **Hyperkalemia**: rising K+ → V less negative → hyperexcitability → arrhythmia / paralysis
- **Hypokalemia**: V more negative → reduced excitability → muscle weakness
- **Channel mutations**: Long QT, periodic paralysis

---

## 9. Common Pitfalls

### 9.1 Nernst ≠ Actual V

Real $V_m$ influenced by multiple ions; needs GHK.

### 9.2 ATP Interruption

E.g., brain ischemia, ATP fails → pump stops → V decays → neurons die.

### 9.3 Units

$E$ in mV; concentration ratio only affects log → order-of-magnitude precision.

### 9.4 Ca²⁺ Concentration Massive Difference

10000× — small Ca²⁺ inflow drastically affects concentration.

### 9.5 Temperature

Body temp ±1°C → V ±1 mV.

---

## 10. Related Concepts

- **Same section**: [Neuron](Neuron.en.md), [Ion Channels](Ion_Channels.en.md), [Action Potential](Action_Potential.en.md)

---

## References

1. **Hille, B.** *Ion Channels of Excitable Membranes*. 3rd ed., 2001.
2. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
3. **Goldman, D. E.** "Potential, impedance, and rectification in membranes." *J Gen Physiol*, 1943.
