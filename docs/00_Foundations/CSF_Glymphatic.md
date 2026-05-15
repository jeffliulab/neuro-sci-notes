# 脑脊液与类淋巴系统 (CSF & Glymphatic System)

> *CSF (cerebrospinal fluid) 在脑室 + 蛛网膜下腔循环,提供 buoyancy、保护、营养、废物清除。Choroid plexus 产生(~ 500 mL/day)。Glymphatic system (Nedergaard 2012):AQP4 介导的 CSF-ISF 交换,睡眠时清除 Aβ/tau — 与 Alzheimer 关联。脑无传统淋巴(直到 2015 发现 meningeal lymphatics)。*
>
> **难度**:Intermediate
> **前置知识**:[Blood Brain Barrier](Blood_Brain_Barrier.md)、[Glia](../02_Cellular_Molecular/Glia.md)

---

## 1. CSF 基本

| 项 | 值 |
|---|---|
| 总量 | ~ 150 mL |
| 日产 | ~ 500 mL(全天换 ~ 3-4 次) |
| 产生 | Choroid plexus(脑室) |
| 吸收 | Arachnoid granulations → 静脉窦 |
| 成分 | 类血浆但低蛋白、低 K⁺ |

---

## 2. 循环路径

```
Choroid plexus (lateral ventricles)
   ↓
Foramen of Monro → 3rd ventricle
   ↓
Cerebral aqueduct → 4th ventricle
   ↓
Foramina (Luschka, Magendie)
   ↓
Subarachnoid space (脑 + 脊髓周围)
   ↓
Arachnoid granulations → venous sinus
```

---

## 3. CSF 功能

- **Buoyancy**: 脑实际重 ~ 1400 g,浮力后 ~ 25 g(防自重压迫)
- **保护**:液体缓冲(冲击)
- **稳态**:离子 + pH 调节
- **清除**:代谢废物运出
- **运输**:激素 / 信号分子

---

## 4. Glymphatic System (Nedergaard 2012)

- "Glial + lymphatic"
- CSF 沿动脉周隙(periarterial)进入
- AQP4(astrocyte endfeet 水通道)介导 CSF ↔ ISF 交换
- 废物(Aβ、tau)沿静脉周隙排出
- **睡眠时活跃**(间质空间扩大 ~ 60%)

---

## 5. 睡眠与清除

- Xie 2013:睡眠时 glymphatic flux ↑,Aβ 清除 ↑
- 解释为何睡眠剥夺 → Aβ 累积 → Alzheimer 风险
- "睡眠是脑的清洗周期"

---

## 6. Meningeal Lymphatics (2015 发现)

- Louveau & Kipnis / Aspelund 2015
- 脑膜确实有 lymphatic vessels(教科书曾说没有!)
- 引流 CSF + 免疫细胞 → 颈深淋巴结
- 改写 CNS"免疫豁免"概念

---

## 7. PyTorch — Glymphatic clearance 简化

```python
import numpy as np

def glymphatic_clearance(abeta0=100, sleep=True, hours=8):
    """Aβ clearance: faster during sleep (Xie 2013)."""
    k = 0.25 if sleep else 0.10   # clearance rate constant /hour
    t = np.arange(hours)
    abeta = abeta0 * np.exp(-k * t)
    return t, abeta   # sleep → much lower residual Aβ
```

---

## 8. 临床

- **Hydrocephalus**: CSF 循环 / 吸收障碍 → 脑室扩大 → 分流术(shunt)
- **Normal pressure hydrocephalus (NPH)**: 步态 + 认知 + 尿失禁三联
- **Idiopathic intracranial hypertension**: CSF 压高
- **CSF leak**: 体位性头痛
- **Lumbar puncture**: CSF 取样(meningitis、MS oligoclonal bands)
- **Alzheimer**: glymphatic 功能下降

---

## 9. CSF 生物标志物

- **Aβ42, tau, p-tau**: Alzheimer 诊断
- **Oligoclonal bands**: MS
- **白细胞 / 蛋白 / 糖**: meningitis
- **14-3-3**: Creutzfeldt-Jakob

---

## 10. Common Pitfalls

### 10.1 脑无淋巴

2015 推翻 — meningeal lymphatics + glymphatic 存在。

### 10.2 Glymphatic 已定论

机制 + AQP4 角色仍有争议(部分重复失败)。

### 10.3 CSF 仅缓冲

还负责清除 + 稳态 + 运输。

### 10.4 睡眠清除 = 唯一功能

睡眠多功能;清除是其一。

### 10.5 Shunt 一劳永逸

Shunt 常堵塞 / 感染 / 过度引流,需修。

---

## 11. Related Concepts

- **同节**:[Blood Brain Barrier](Blood_Brain_Barrier.md)、[Brain Energy Metabolism](Brain_Energy_Metabolism.md)
- **细胞**:[Glia](../02_Cellular_Molecular/Glia.md)(astrocyte AQP4)
- **系统**:[Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md)
- **疾病**:[Alzheimer](../08_Neuro_Disorders/Alzheimer.md)

---

## References

1. **Iliff, J. J. et al.** "A paravascular pathway facilitates CSF flow and clearance of interstitial solutes including amyloid β." *Sci Transl Med*, 2012.
2. **Xie, L. et al.** "Sleep drives metabolite clearance from the adult brain." *Science*, 2013.
3. **Louveau, A. et al.** "Structural and functional features of central nervous system lymphatic vessels." *Nature*, 2015.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
