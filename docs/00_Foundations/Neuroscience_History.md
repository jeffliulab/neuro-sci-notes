# 神经科学历史 — 从 Cajal 到 Neuralink

> *从 19 世纪末 Santiago Ramón y Cajal 用 Golgi 染色法首次画出神经元的精美图像,到 21 世纪 Neuralink 把电极阵列植入人脑,神经科学走过了 130 年。本篇梳理关键里程碑:细胞学说、动作电位、突触、神经递质、CT/MRI、脑机接口、Connectomics、optogenetics、Mech Interp 与 AI 的合流。*
>
> **难度**:Introduction
> **前置知识**:无
> **后续阅读**:[神经元](../02_Cellular_Molecular/Neuron.md)、[BCI Foundations](../06_Brain_Computer_Interface/01_Foundations/index.md)

---

## 1. 19 世纪末 — 神经元学说

### 1.1 Camillo Golgi (1873)

意大利 Golgi 发明 **Golgi 染色法**:银盐随机沾染 1-5% 神经元,让单个细胞**完整可见**(包括 dendrite + axon)。

### 1.2 Santiago Ramón y Cajal (1888-1906)

西班牙 Cajal 用 Golgi 法画出**数千张**精美神经元图,证明:
- 神经系统由**分立的细胞**组成 (不是连续网络) — **神经元学说**
- 神经元有**方向性**: dendrite 接收 → axon 输出
- 神经元间通过**接触**通信 (非融合)

Golgi (网络派) vs Cajal (神经元派) 1906 共获 Nobel Prize。Cajal 是对的。

---

## 2. 20 世纪初 — 动作电位

### 2.1 Hodgkin & Huxley (1939-1952)

英国 Hodgkin & Huxley 在乌贼**巨型 axon** (直径 0.5mm,可插电极) 上测电压:
- 动作电位 = Na+ 内流 + K+ 外流 的精密协调
- 提出 **Hodgkin-Huxley 方程** (1952)
- Nobel 1963

### 2.2 Eccles (1950s)

突触电信号机制 (EPSP/IPSP),Nobel 1963。

---

## 3. 20 世纪中 — 神经递质

### 3.1 Otto Loewi (1921)

德国 Loewi 实验:刺激迷走神经使青蛙心跳变慢 → 收集 perfusate 注入另一颗心 → 也变慢。
→ "Vagusstoff" = 乙酰胆碱 (ACh)。首次证明**化学**突触。Nobel 1936。

### 3.2 主要神经递质陆续发现

- ACh (1921)
- 去甲肾上腺素 NE (1946, Euler)
- 多巴胺 DA (1957, Carlsson)
- GABA (1950s)
- 5-HT (1948)
- Glutamate (1959)

---

## 4. 20 世纪 70s-80s — 脑成像革命

### 4.1 CT (Hounsfield 1971)

X-ray 计算重建,首次活体看脑结构。Nobel 1979。

### 4.2 MRI (Lauterbur 1973)

磁共振成像,无辐射高对比度软组织。Nobel 2003。

### 4.3 PET / fMRI (1980-1990)

测大脑功能活动(代谢/血氧)。**fMRI** 成为现代认知神经科学主力工具。

---

## 5. 1990s — 神经回路细胞机制

### 5.1 Hippocampus & Memory

- **O'Keefe 1971** — 发现 place cells (海马 spatial code)
- **Moser 2005** — 发现 grid cells (entorhinal cortex)
- Nobel 2014 (三人共享)

### 5.2 LTP (Bliss & Lømo 1973)

Long-Term Potentiation — 突触可塑性,**学习的细胞基础**。

### 5.3 Patch Clamp (Neher & Sakmann 1976)

测单个 ion channel 电流。Nobel 1991。

---

## 6. 2000s — Connectomics + Optogenetics

### 6.1 Optogenetics (Deisseroth 2005)

在神经元里表达 **光敏 channelrhodopsin**:蓝光开 → 神经元 fire,黄光关。
革命性 — 可**因果**操控特定神经元 (vs 之前的 correlational fMRI)。

### 6.2 Connectomics

- **C. elegans connectome** (1986): 302 neurons 全图
- **Drosophila brain** (2024): FlyEM 全脑 connectome
- **Mouse 1mm³** (2021): MICrONS,~75k neurons + 500M synapses

### 6.3 Single-cell RNA-seq

按 expression 分类神经元 → ~1000+ cell types。

---

## 7. 2010s — 大数据 + AI

### 7.1 BRAIN Initiative (Obama 2013)

美国 $5B 项目:开发大规模神经技术 (Calcium imaging / silicon probes / etc.)。

### 7.2 Human Connectome Project (2010-)

测千名健康人 + 病人 brain connectivity。

### 7.3 DeepMind AlphaFold (2020)

蛋白质结构预测;影响溢出到 neuro (channels structure 等)。

### 7.4 Mech Interp + brain

Anthropic 2024 SAE 启发 neuroscientists 用类似方法理解 brain。

---

## 8. 2020s — BCI 商业化

### 8.1 Neuralink (2016 成立, 2024 首次人体)

Elon Musk 公司,N1 implant — 1024 电极阵列 + 无线传输。
2024 ALS 患者植入,用思维控制鼠标。

### 8.2 Synchron / Paradromics / Precision

竞争对手:更微创 (stentrode) / 更高密度 / 更安全。

### 8.3 临床进展

- 2024-2025: 多个 ALS / 高位脊髓损伤 患者用 BCI 重获沟通
- Speech BCI:UCSF 2023 让 brainstem 中风患者每分钟 80 词

---

## 9. 关键时间线 (visual)

```
1873 ─── Golgi 染色
1888 ─── Cajal 神经元学说
1921 ─── Loewi 化学突触
1939 ─── HH 乌贼 axon 实验
1952 ─── Hodgkin-Huxley 方程
1971 ─── CT + place cells
1973 ─── MRI + LTP
1976 ─── Patch clamp
2005 ─── Optogenetics
2010 ─── HCP, BRAIN Initiative
2016 ─── Neuralink 成立
2024 ─── Neuralink 首次人体植入
2025 ─── Speech BCI 临床突破
```

---

## 10. 学科分支结构

- **Cellular Neuroscience**: 神经元 / 突触 / channels
- **Molecular**: 神经递质 / receptors / gene expression
- **Systems**: 感觉 / 运动 / 学习记忆 / 决策
- **Cognitive**: 注意 / 意识 / 语言
- **Computational**: 模型化 brain function
- **Clinical**: AD / PD / depression / schizophrenia
- **Neurotech**: BCI / DBS / TMS / fMRI

---

## 11. 与 AI 关系

- 1943 McCulloch-Pitts 人工神经元 (受 brain 启发)
- 1958 Rosenblatt Perceptron
- 1980s Hopfield network, Backprop
- 2010s deep learning revolution (CNN ↔ visual cortex hierarchy)
- 2020s LLM ↔ brain reasoning ↔ Mech Interp 三方对话

---

## 12. 当前挑战

- 神经元 spike → cognition 仍 mostly black box
- BCI 仅能 decode 简单动作 (鼠标 / 字符);complex thought 仍难
- 没有 unified theory of brain
- AD / Parkinson 等疾病机制部分未知,治愈缺
- Ethics: 隐私 / consent / 增强

---

## 13. Related Concepts

- **细胞**:[神经元](../02_Cellular_Molecular/Neuron.md)、[突触](../02_Cellular_Molecular/Synapse.md)
- **现代 BCI**:[BCI Foundations](../06_Brain_Computer_Interface/01_Foundations/index.md)
- **AI 关联**:https://jeffliulab.github.io/ai-notes/02_Deep_Learning/ (CNN/Transformer 等)

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., McGraw-Hill, 2021.
2. **Cajal, S. R.** *Histology of the Nervous System*. 1909-1911.
3. **Hodgkin, A. L. & Huxley, A. F.** "A quantitative description of membrane current and its application to conduction and excitation in nerve." *J Physiol*, 1952.
4. **Bear, M. F. et al.** *Neuroscience: Exploring the Brain*. 4th ed., 2015.
5. **Neuralink** — N1 implant + technical specifications. 2024.
6. **FlyEM** — Full Drosophila brain connectome. 2024.
