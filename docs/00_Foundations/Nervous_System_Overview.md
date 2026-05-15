# 神经系统总览 (Nervous System Overview)

> *神经系统分 CNS (brain + spinal cord) 和 PNS (cranial + spinal nerves + 自主神经)。CNS ~ 86 billion neurons。PNS 分 somatic (随意) + autonomic (sympathetic / parasympathetic / enteric)。这是神经科学的"地图",理解任何具体系统前需要的鸟瞰。*
>
> **难度**:Beginner
> **前置知识**:无

---

## 1. 顶层划分

```
Nervous System
├── CNS (Central)
│   ├── Brain
│   └── Spinal cord
└── PNS (Peripheral)
    ├── Somatic (voluntary)
    └── Autonomic (involuntary)
        ├── Sympathetic ("fight or flight")
        ├── Parasympathetic ("rest and digest")
        └── Enteric ("gut brain")
```

---

## 2. CNS — Brain 大区

| 区 | 功能 |
|---|---|
| Cerebrum (cortex) | 高级认知、感觉、运动 |
| Cerebellum | 运动协调、时序、学习 |
| Brainstem | 生命中枢(呼吸、心跳) |
| Diencephalon (thalamus, hypothalamus) | 中继、内稳态 |
| Limbic system | 情绪、记忆 |
| Basal ganglia | 运动选择、习惯 |

---

## 3. CNS — Spinal Cord

- 31 segments(8C + 12T + 5L + 5S + 1Co)
- 灰质(中央 H 形)+ 白质(外周轴突束)
- 反射弧(reflex arc)绕过 brain
- Ascending(感觉)+ descending(运动)tracts

---

## 4. PNS — Somatic

- 控制 skeletal muscle(随意)
- Sensory(afferent)+ motor(efferent)
- 12 对 cranial nerves + 31 对 spinal nerves

---

## 5. PNS — Autonomic

| | Sympathetic | Parasympathetic |
|---|---|---|
| 功能 | Fight/flight | Rest/digest |
| 起源 | Thoracolumbar | Craniosacral |
| NT | NE (post) | ACh |
| 心率 | ↑ | ↓ |
| 瞳孔 | 散大 | 收缩 |
| 消化 | 抑制 | 促进 |

**Enteric**: 肠神经系统,~ 5 亿 neuron,半自主("第二大脑")。

---

## 6. 数量级

| 项 | 数 |
|---|---|
| Brain neurons | ~ 86 billion |
| Glia | ~ 同 neuron 量级 |
| Synapses | ~ 10^14-10^15 |
| Cortex neurons | ~ 16 billion |
| Cerebellum neurons | ~ 69 billion(最多!) |
| Spinal cord neurons | ~ 1 billion |

---

## 7. 方向术语

- **Rostral / Caudal**: 头 / 尾
- **Dorsal / Ventral**: 背 / 腹
- **Medial / Lateral**: 中 / 侧
- **Superior / Inferior**: 上 / 下
- **Afferent / Efferent**: 传入 / 传出
- **Ipsilateral / Contralateral**: 同侧 / 对侧

---

## 8. PyTorch — 系统层级树

```python
nervous_system = {
    "CNS": {
        "brain": ["cortex", "cerebellum", "brainstem", "thalamus", "limbic", "BG"],
        "spinal_cord": ["cervical", "thoracic", "lumbar", "sacral"]
    },
    "PNS": {
        "somatic": ["cranial_nerves", "spinal_nerves"],
        "autonomic": ["sympathetic", "parasympathetic", "enteric"]
    }
}

def count_leaves(d):
    if isinstance(d, list): return len(d)
    return sum(count_leaves(v) for v in d.values())

print(count_leaves(nervous_system))  # 系统组件数
```

---

## 9. 发育起源

- 神经管(neural tube)→ CNS
- 神经嵴(neural crest)→ PNS + 部分其他
- 3 primary vesicles → 5 secondary → 成人 brain
  - Prosencephalon → telencephalon + diencephalon
  - Mesencephalon → midbrain
  - Rhombencephalon → pons/cerebellum + medulla

---

## 10. Common Pitfalls

### 10.1 Cerebrum neuron 最多

错;cerebellum ~ 69B,远超 cortex ~ 16B。

### 10.2 Autonomic = 不可控

部分可训练(biofeedback、呼吸调心率)。

### 10.3 Spinal cord 仅传导

有 reflex + 局部 pattern generator(运动节律)。

### 10.4 Enteric 受 brain 控

很大程度自主,迷走神经仅 modulate。

### 10.5 左脑控左身

错;大多 contralateral(左脑控右身)。

---

## 11. Related Concepts

- **同节**:[Neuroscience History](Neuroscience_History.md)、[Research Methods](Research_Methods.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)、[Brainstem](../01_Neuroanatomy/Brainstem.md)、[Cerebellum](../01_Neuroanatomy/Cerebellum.md)、[Thalamus](../01_Neuroanatomy/Thalamus.md)

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Purves, D. et al.** *Neuroscience*. 6th ed., 2018.
3. **Herculano-Houzel, S.** "The human brain in numbers." *Front Hum Neurosci*, 2009.
4. **Gray, H.** *Gray's Anatomy*. 42nd ed., 2020.
