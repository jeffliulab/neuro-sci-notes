# 膜电位 (Membrane Potential) — 神经元电信号基础

> *膜电位是神经元 inside vs outside 的电压差,典型静息 -70 mV。源自 ion 浓度梯度 + 选择性 channel + Na+/K+ ATPase pump。是 spike, synaptic activity 的基础。*
>
> **难度**:Intermediate
> **前置知识**:[神经元](Neuron.md)、[离子通道](Ion_Channels.md)

---

## 1. 静息膜电位

典型 cortical neuron:**-70 mV** (intracellular 相对 extracellular)。

成因:
- Na+/K+ ATPase pump 维持 ion gradients (每 cycle 3 Na+ out + 2 K+ in)
- K+ leak channel 开,membrane 接近 K+ 平衡电位 ($E_K \approx -90$ mV)
- 微量 Na+ leak 使 V 略高于 $E_K$

---

## 2. Nernst 方程

单 ion 平衡电位:

$$E_X = \frac{RT}{zF} \ln \frac{[X]_o}{[X]_i}$$

37°C, 单价阳离子:$E_X \approx 61 \log_{10} \frac{[X]_o}{[X]_i}$ (mV)。

典型值:
- $E_{Na} = +60$ mV
- $E_K = -90$ mV
- $E_{Cl} = -75$ mV (人类 neuron)
- $E_{Ca} = +140$ mV (极高 due to 极低 $[Ca]_i$)

---

## 3. Goldman-Hodgkin-Katz (GHK) 方程

多 ion 情况:

$$V_m = \frac{RT}{F} \ln \frac{P_K [K^+]_o + P_{Na} [Na^+]_o + P_{Cl} [Cl^-]_i}{P_K [K^+]_i + P_{Na} [Na^+]_i + P_{Cl} [Cl^-]_o}$$

$P_X$ = 透性。静息时 $P_K \gg P_{Na}$,所以 V 接近 $E_K$ 但略高。

---

## 4. Ion 分布

| Ion | $[X]_i$ (mM) | $[X]_o$ (mM) | $E$ (mV) |
|---|---|---|---|
| Na+ | 12 | 145 | +60 |
| K+ | 140 | 4 | -90 |
| Cl- | 7 | 110 | -75 |
| Ca²⁺ | 10⁻⁴ | 2 | +140 |

→ 这个 disequilibrium 是神经元 work 的根本。

---

## 5. Na+/K+ ATPase Pump

- Membrane 蛋白
- 每 cycle: 3 Na+ out + 2 K+ in, 消耗 1 ATP
- 电生 → 贡献 ~ -10 mV 到静息电位
- 占大脑能量消耗 ~ 50%

---

## 6. PyTorch — Resting Potential Calculation

```python
import numpy as np

def nernst(z, X_in, X_out, T_C=37):
    """Nernst potential in mV."""
    R = 8.314
    F = 96485
    T_K = T_C + 273.15
    return (R * T_K) / (z * F) * np.log(X_out / X_in) * 1000

def ghk(perms, ions_in, ions_out, T_C=37):
    """Goldman-Hodgkin-Katz V_m in mV."""
    R = 8.314; F = 96485
    T_K = T_C + 273.15
    # ions: dict of ion_name → (charge, P, concentration)
    num = sum(perms['K'] * ions_out['K'], perms['Na'] * ions_out['Na'], perms['Cl'] * ions_in['Cl'])
    den = sum(perms['K'] * ions_in['K'], perms['Na'] * ions_in['Na'], perms['Cl'] * ions_out['Cl'])
    return R * T_K / F * np.log(num / den) * 1000

print(f"E_Na = {nernst(1, 12, 145):.1f} mV")  # ~ +60
print(f"E_K = {nernst(1, 140, 4):.1f} mV")    # ~ -90
```

---

## 7. 与 spike 的关系

- 静息 -70
- EPSP / IPSP 局部小波动
- 阈值 -55 → Na+ channel open → AP

---

## 8. 病理

- **Hyperkalemia** (高 K+): 静息 V 上升 (less negative) → 神经过度兴奋 → 心律不齐 / 麻痹
- **Hypokalemia**: 静息 V 下降 → 兴奋性降 → 肌无力
- **离子 channel mutations**: Long QT, periodic paralysis

---

## 9. Common Pitfalls

### 9.1 Nernst ≠ 实际 V

Real $V_m$ 受 multiple ions 影响,需 GHK。

### 9.2 ATP 中断

如脑缺血,ATP 失 → pump 停 → V 衰 → 神经元死。

### 9.3 单位

$E$ 在 mV; concentration 比例只影响 log → 数量级精度。

### 9.4 Ca²⁺ 浓度差极大

10000× — 小 Ca²⁺ inflow 大幅 affect concentration。

### 9.5 温度

体温变 1°C → V 变 ~ 1 mV。

---

## 10. Related Concepts

- **同节**:[神经元](Neuron.md)、[离子通道](Ion_Channels.md)、[动作电位](Action_Potential.md)

---

## References

1. **Hille, B.** *Ion Channels of Excitable Membranes*. 3rd ed., 2001.
2. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
3. **Goldman, D. E.** "Potential, impedance, and rectification in membranes." *J Gen Physiol*, 1943.
