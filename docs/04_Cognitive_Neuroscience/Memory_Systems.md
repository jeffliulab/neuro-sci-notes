# 记忆系统 (Memory Systems Taxonomy)

> *记忆非单一:多重分离系统。陈述性(海马依赖:episodic + semantic)vs 非陈述性(procedural、priming、conditioning、习惯)。Squire 分类学 + 患者 H.M.(失 episodic 保 procedural)是奠基证据。理解分类是临床 + AI continual learning 的框架。*
>
> **难度**:Intermediate
> **前置知识**:[Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)、[Working Memory](Working_Memory.md)

---

## 1. Squire 分类学

```
Memory
├── Declarative (explicit, 有意识, 海马依赖)
│   ├── Episodic (事件 + 时空 context)
│   └── Semantic (事实 / 知识)
└── Non-declarative (implicit, 无意识)
    ├── Procedural (技能, 纹状体/小脑)
    ├── Priming (皮层)
    ├── Classical conditioning (杏仁核/小脑)
    └── Non-associative (习惯化/敏化)
```

---

## 2. 时间维度

| 类型 | 时程 | 底物 |
|---|---|---|
| Sensory memory | < 1 sec | 感觉皮层 |
| Working memory | sec | PFC(见 [Working Memory](Working_Memory.md)) |
| Short-term | min | 海马早期 |
| Long-term | 天-终生 | 皮层(consolidation) |

---

## 3. 患者证据(双分离)

- **H.M.**(双侧 MTL 切除):新 episodic 不能,但 procedural(镜画学习)正常 → declarative ≠ procedural
- **遗忘症**:episodic 损,semantic 部分保
- **Parkinson/HD**(纹状体):procedural 损,declarative 相对保
- 双分离 = 系统独立的强证据

---

## 4. Episodic vs Semantic

| | Episodic | Semantic |
|---|---|---|
| 内容 | "我昨天的事" | "巴黎是法国首都" |
| Context | 时空自我 | 去情境化 |
| 主观 | autonoetic(自我穿越时间) | noetic |
| 海马依赖 | 强(尤新) | 巩固后较弱 |
| Tulving 提出 | ✓ | ✓ |

---

## 5. Consolidation

- **Synaptic consolidation**(小时):LTP → 突触稳固
- **Systems consolidation**(月-年):海马 → 新皮层转移
- Standard model vs Multiple Trace Theory(remote episodic 是否仍需海马争议)
- 睡眠 + replay 关键(见 [Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md))

---

## 6. PyTorch — 互补学习系统 (CLS)

```python
import torch

class ComplementaryLearningSystems(torch.nn.Module):
    """McClelland 1995: fast hippocampus + slow neocortex."""
    def __init__(self, dim=64):
        super().__init__()
        self.hpc = torch.nn.Linear(dim, dim)   # fast, sparse, episodic
        self.ctx = torch.nn.Linear(dim, dim)   # slow, distributed, semantic
    def forward(self, x, mode='recall'):
        if mode == 'encode':
            return torch.relu(self.hpc(x))     # rapid one-shot
        return torch.tanh(self.ctx(x))         # consolidated, generalized
```

→ 解释为何需双系统:避免 catastrophic interference(AI continual learning 同问题)。

---

## 7. 与 AI continual learning

- **Catastrophic forgetting**:NN 学新覆盖旧(McCloskey 1989)
- 脑用 CLS(快海马 + 慢皮层 + replay)避免
- AI 借鉴:experience replay(DQN)、EWC、generative replay
- 海马 replay ↔ DQN replay buffer(直接启发)

---

## 8. 失忆类型

- **Anterograde**:不能形成新记忆(H.M.)
- **Retrograde**:丢失旧记忆(常时间梯度,Ribot 定律)
- **Transient global amnesia**:短暂
- **心因性**:无器质损(争议)
- **Confabulation**:虚构填补(Korsakoff)

---

## 9. 与工作记忆区分

- WM:在线维持 + 操作(秒级,PFC)
- 长时记忆:存储(海马 → 皮层)
- 不同系统;WM 损 ≠ LTM 损(双分离)

---

## 10. Common Pitfalls

### 10.1 记忆是单一存储

多重分离系统(declarative vs procedural 双分离)。

### 10.2 记忆像录像回放

是重建性(reconstructive),易受图式 / 暗示扭曲(false memory)。

### 10.3 海马存所有长期记忆

Systems consolidation → 远期记忆转皮层(细节争议)。

### 10.4 Episodic = Semantic

Tulving 区分:autonoetic vs noetic;可分离损伤。

### 10.5 遗忘 = 存储丢失

多为提取失败 / 干扰,非全删除。

---

## 11. Related Concepts

- **同节**:[Working Memory](Working_Memory.md)、[Executive Function](Executive_Function.md)
- **系统**:[Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)、[Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md)
- **细胞**:[LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)
- **AI**: continual learning、experience replay

---

## References

1. **Squire, L. R.** "Memory systems of the brain: A brief history and current perspective." *Neurobiol Learn Mem*, 2004.
2. **Tulving, E.** "Episodic and semantic memory." 1972.
3. **Scoville, W. B. & Milner, B.** "Loss of recent memory after bilateral hippocampal lesions (H.M.)." *J Neurol Neurosurg Psychiatry*, 1957.
4. **McClelland, J. L. et al.** "Why there are complementary learning systems in the hippocampus and neocortex." *Psychol Rev*, 1995.
