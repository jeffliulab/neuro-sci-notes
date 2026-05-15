# 光遗传学进阶 (Optogenetics — Advanced & Variants)

> *在 [Optogenetics](Optogenetics.md) 基础上:红移 opsin(深穿透)、step-function opsin(双稳态)、抑制性泵 vs 通道、Cre-依赖细胞特异、投射特异(轴突末端照射)、all-optical(光控 + 光读)、无线/无纤维(Cre-on upconversion、sonogenetics)。是因果环路解剖的精密工具箱。*
>
> **难度**:Advanced
> **前置知识**:[Optogenetics](Optogenetics.md)、[Calcium Imaging](Calcium_Imaging.md)

---

## 1. Opsin 工具箱

| Opsin | 离子 | 效应 | 波长 |
|---|---|---|---|
| ChR2 | Na⁺/Ca²⁺ 内流 | 兴奋 | 蓝 470 nm |
| ChrimsonR | 阳离子 | 兴奋 | 红 ~ 620 nm(深) |
| ReaChR / bReaChES | 阳离子 | 兴奋 | 红移 |
| NpHR | Cl⁻ 泵入 | 抑制 | 黄 590 nm |
| Arch / eArchT | H⁺ 泵出 | 抑制 | 绿 |
| GtACR | Cl⁻ 通道 | 强抑制 | 蓝 |
| SFO/SSFO | 双稳态 ChR | 长持兴奋 | 蓝开/绿关 |

---

## 2. 红移 = 深穿透

- 蓝光散射强、穿透浅(< 1 mm)
- 红/近红外散射少 → 更深
- ChrimsonR、ReaChR → 减少光纤侵入
- + upconversion 纳米粒 → 近红外驱动蓝 opsin(无纤维)

---

## 3. Step-Function Opsin (双稳态)

- 一个光脉冲**开**,另一波长**关**
- 神经元长时维持去/超极化(无需持续光)
- 减光毒性 + 模拟持续状态调节

---

## 4. 细胞 + 投射特异

- **细胞型**:Cre 驱动(DIO/FLEx),仅特定类型表达 opsin
- **投射特异**:在**轴突末端**照射 → 只控某一投射通路
- **逆行病毒**(CAV2、retro-AAV)→ 按投射靶标记
- → 精确解剖"A→B 通路在行为 X 中的因果作用"

---

## 5. All-Optical

- **光控**(opsin,红)+ **光读**(GCaMP,绿)同时
- 需光谱分离(避免 GCaMP 蓝激发误激活 ChR2 → 用红 opsin)
- 全光因果环路操控 + 读出(闭环可能)

---

## 6. PyTorch — 投射特异光刺激模型

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

## 7. 闭环光遗传 (Closed-Loop)

- 实时读神经/行为 → 触发光刺激
- 例:检测到 seizure 起始 → 立即抑制(治疗原型)
- 检测 sharp-wave ripple → 扰动 → 测记忆因果
- 与 BCI / closed-loop DBS 思路相通

---

## 8. 临床转化挑战

- 需基因递送(AAV)→ 安全 + 调控 + 免疫
- 光纤植入(侵入)→ 无纤维方案(upconversion、sonogenetics、磁热)
- 视网膜色素变性:ChrimsonR 视觉修复**已进人体试验**(GenSight,首个人类光遗传治疗)
- 长期表达 + 光毒性

---

## 9. 变体技术

- **Chemogenetics (DREADD)**:药物代替光,无需植入(时间精度低)
- **Magnetogenetics**:磁场(争议)
- **Sonogenetics**:超声 + 机械敏感通道(非侵入,见 [Focused Ultrasound](Focused_Ultrasound.md))
- **Optopharmacology**:光控药物

---

## 10. Common Pitfalls

### 10.1 ChR2 适用所有实验

蓝光浅 + 散射;深部需红移 opsin / 纤维。

### 10.2 抑制 opsin 都一样

泵(NpHR/Arch)vs 通道(GtACR)机制 + 副作用不同(泵改变 pH/Cl 梯度)。

### 10.3 光刺激 = 生理性

非自然同步激活;可产生非生理 pattern。

### 10.4 细胞特异自动

依赖 promoter/Cre 特异性 + 表达调控,需验证。

### 10.5 = 即将临床普及

视网膜试验领先;脑内临床仍远(递送 + 侵入挑战)。

---

## 11. Related Concepts

- **同节**:[Optogenetics](Optogenetics.md)、[Calcium Imaging](Calcium_Imaging.md)、[Focused Ultrasound](Focused_Ultrasound.md)
- **细胞**:[Ion Channels](../02_Cellular_Molecular/Ion_Channels.md)
- **基础**:[Neural Circuits](../00_Foundations/Neural_Circuits.md)

---

## References

1. **Deisseroth, K.** "Optogenetics: 10 years of microbial opsins in neuroscience." *Nat Neurosci*, 2015.
2. **Klapoetke, N. C. et al.** "Independent optical excitation of distinct neural populations (Chrimson/Chronos)." *Nat Methods*, 2014.
3. **Sahel, J.-A. et al.** "Partial recovery of visual function in a blind patient after optogenetic therapy." *Nat Med*, 2021.
4. **Emiliani, V. et al.** "Optogenetics for light control of biological systems." *Nat Rev Methods Primers*, 2022.
