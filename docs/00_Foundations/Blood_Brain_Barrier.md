# 血脑屏障 (Blood-Brain Barrier)

> *BBB 是 endothelial tight junction + astrocyte endfeet + pericyte 组成的高度选择性屏障,保护 CNS 免受血液毒素 / 病原。代价:~ 98% 小分子药 + 几乎所有大分子药无法入脑 — 是 CNS 药物开发最大障碍。Focused ultrasound、nanoparticle、Trojan horse 是突破策略。*
>
> **难度**:Intermediate
> **前置知识**:[Glia](../02_Cellular_Molecular/Glia.md)、细胞膜运输

---

## 1. 结构(neurovascular unit)

```
血液
  │  endothelial cell (tight junction — 关键)
  │  basement membrane
  │  pericyte
  │  astrocyte endfeet (包裹 > 99% 血管)
脑实质 (neuron)
```

- Tight junction(claudin、occludin)封死细胞间隙
- 无 fenestration(不像外周血管)
- 极高电阻 (~ 2000 Ω·cm²)

---

## 2. 通过方式

| 方式 | 例 |
|---|---|
| 被动扩散 | 小、亲脂(O₂、CO₂、酒精、咖啡因) |
| 载体转运 | GLUT1(glucose)、LAT1(amino acid) |
| 受体介导转胞吞 | 胰岛素、transferrin |
| 主动外排 | P-glycoprotein(把药泵回血) |
| 紧密连接 | 离子、大分子被阻 |

---

## 3. 选择性通过

- **能过**:O₂、CO₂、glucose(转运)、小亲脂分子、酒精、尼古丁、多数麻醉/精神药
- **不能过**:大多抗生素、化疗药、蛋白、抗体、多数 hydrophilic 药

---

## 4. P-glycoprotein 外排

- ABC transporter,主动把外来分子泵回血
- 解释为何许多本可扩散的药仍低脑浓度
- 也是化疗耐药机制

---

## 5. Circumventricular Organs (无 BBB 区)

- Area postrema(呕吐中枢 — 感知血液毒素)
- Median eminence、OVLT、SFO
- 需"采样"血液的区域故意无 BBB

---

## 6. PyTorch — BBB permeability 简化预测

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

## 7. 突破 BBB 策略

| 策略 | 机制 |
|---|---|
| Lipidization | 增亲脂 |
| Prodrug | 入脑后激活 |
| Trojan horse | 偶联 transferrin/insulin receptor ligand |
| Nanoparticle | 包载 + 表面修饰 |
| Focused ultrasound + microbubble | 暂时开 BBB(临床试验中) |
| Intranasal | 绕过(经嗅神经) |
| Intrathecal / ICV | 直接注 CSF |
| Osmotic disruption | mannitol 暂破(侵入) |

---

## 8. 病理 — BBB 破坏

- **Multiple sclerosis**: BBB 破 → 免疫细胞入侵(Gadolinium MRI 检测)
- **Stroke**: 缺血 → BBB 破 → vasogenic edema
- **Brain tumor**: 新生血管 BBB 不全(造影剂强化)
- **Alzheimer**: BBB 功能下降 → Aβ 清除受损
- **Meningitis**: 感染破 BBB
- **Sepsis**: 全身炎症 → BBB 渗漏 → 谵妄

---

## 9. 发育 + 老化

- 新生儿 BBB 较通透(故 kernicterus 风险)
- 老化 BBB 逐渐 leaky
- Pericyte 丢失与 neurodegeneration 关联

---

## 10. Common Pitfalls

### 10.1 BBB 是单层膜

是 neurovascular unit(内皮+周细胞+星形胶质),非单膜。

### 10.2 亲脂必过

P-gp 外排可抵消;TPSA、MW 也关键。

### 10.3 BBB 全脑均一

CVO 区无 BBB;不同区通透性不同。

### 10.4 抗体药能入脑

绝大多数不能;need 工程化(brain shuttle)。

### 10.5 打开 BBB 安全

开放 = 失保护;FUS 时机/剂量需精确。

---

## 11. Related Concepts

- **同节**:[Brain Energy Metabolism](Brain_Energy_Metabolism.md)、[Research Methods](Research_Methods.md)
- **细胞**:[Glia](../02_Cellular_Molecular/Glia.md)
- **疾病**:[Multiple Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.md)、[Stroke](../08_Neuro_Disorders/Stroke.md)
- **前沿**:Focused ultrasound、[DBS](../07_Neurotech_Frontiers/DBS.md)

---

## References

1. **Abbott, N. J. et al.** "Structure and function of the blood-brain barrier." *Neurobiol Dis*, 2010.
2. **Daneman, R. & Prat, A.** "The blood-brain barrier." *Cold Spring Harb Perspect Biol*, 2015.
3. **Pardridge, W. M.** "Drug transport across the blood-brain barrier." *J Cereb Blood Flow Metab*, 2012.
4. **Sweeney, M. D. et al.** "Blood-brain barrier breakdown in Alzheimer disease." *Nat Rev Neurol*, 2018.
