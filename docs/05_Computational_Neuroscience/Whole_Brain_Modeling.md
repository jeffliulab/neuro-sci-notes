# 全脑建模 (Whole-Brain Modeling)

> *全脑建模用 neural mass / mean-field 模型 + connectome 仿真大尺度 brain dynamics。The Virtual Brain (TVB)、Blue Brain、Human Brain Project 是代表。目标:连接 structural connectome 与 fMRI/EEG 功能信号,模拟疾病 + 个性化(virtual brain twin)。粒度权衡是核心争议。*
>
> **难度**:Advanced
> **前置知识**:[Connectomics](../00_Foundations/Connectomics.md)、[Neural Population Dynamics](Neural_Population_Dynamics.md)

---

## 1. 为何全脑

- 认知 = 分布式大尺度网络交互
- 单区 / 单 neuron 模型无法解释 global dynamics
- 连接 connectome(结构)→ functional connectivity(fMRI)
- 疾病 = network-level disruption(connectopathy)

---

## 2. 建模层级

| 层级 | 单元 | 例 |
|---|---|---|
| Detailed | morphological neuron | Blue Brain |
| Spiking network | LIF/Izhikevich | large SNN |
| **Neural mass** | population mean | Wilson-Cowan, Jansen-Rit |
| **Mean-field** | 概率分布 | DMF (dynamic mean field) |
| Whole-brain | region nodes + connectome | TVB |

---

## 3. Neural Mass Model

每脑区一个 population:
$$\tau \dot{r} = -r + S\!\left(\sum_j C_{ij} r_j + I\right)$$

- $C_{ij}$:来自 DTI connectome
- $S$:sigmoid 激活
- 区内简化为 mean activity
- 加 delay(传导)+ noise

---

## 4. The Virtual Brain (TVB)

- 节点 = 脑区(AAL/Desikan parcellation)
- 边 = DTI tractography 强度 + 传导延迟
- 每节点 neural mass model
- 输出 → forward model → 模拟 fMRI BOLD / EEG
- 拟合个体 empirical FC

---

## 5. PyTorch — 简化全脑网络

```python
import torch

def whole_brain_sim(SC, T=2000, dt=0.1, G=0.5, tau=10.0, noise=0.05):
    """SC: (N,N) structural connectome. Wilson-Cowan-like nodes."""
    N = SC.shape[0]
    r = torch.rand(N) * 0.1
    BOLD_proxy = []
    for t in range(T):
        coupling = G * (SC @ r)
        drive = -r + torch.sigmoid(coupling - 1.0)
        r = r + dt / tau * drive + noise * torch.randn(N) * (dt**0.5)
        r = r.clamp(0, 1)
        BOLD_proxy.append(r.clone())
    sim = torch.stack(BOLD_proxy)
    FC = torch.corrcoef(sim.T)            # simulated functional connectivity
    return sim, FC
```

→ 调 G 使 simulated FC 最匹配 empirical FC。

---

## 6. Structure → Function

- 静息态 fMRI FC 部分由 SC 预测
- 但 SC≠FC(功能可绕过直接结构连接)
- Working point:G 调到 criticality → 最佳 FC 拟合
- 解释 resting-state networks (DMN 等)

---

## 7. 临床应用

- **癫痫**:Epileptor 模型 + 个体 connectome → 预测 seizure 传播 → 手术规划(EPINOV trial)
- **Stroke**:lesion → 模拟 network 重组
- **Alzheimer / 精神病**:connectopathy 仿真
- **个性化**:virtual brain twin(数字孪生)

---

## 8. Blue Brain / HBP 争议

- Blue Brain:bottom-up 细节重建(贵、是否必要?)
- Human Brain Project(2013-2023):雄心 vs 交付批评
- 教训:粒度须匹配科学问题(见 [Levels of Analysis](../00_Foundations/Levels_of_Analysis.md))
- Mean-field 在全脑尺度常更实用

---

## 9. 与 AI

- 全脑模型 = 大型动力系统(类 RNN at scale)
- Connectome-constrained RNN(用真 connectome 作 W 先验)
- Digital twin ↔ simulation-based inference
- 但远非 AGI;是机制模型非智能体

---

## 10. Common Pitfalls

### 10.1 越细越好

全脑细节重建参数爆炸;mean-field 常更可约束。

### 10.2 SC = FC

功能连接 ≠ 结构;dynamics 产生间接 FC。

### 10.3 拟合 FC = 理解

高拟合可能 degenerate;需 out-of-sample + 扰动验证。

### 10.4 Neural mass = 真实 population

强简化(忽略 spike、异质性)。

### 10.5 Virtual brain = 意识 / AGI

是机制仿真工具;无 sentience。

---

## 11. Related Concepts

- **同节**:[Neural Population Dynamics](Neural_Population_Dynamics.md)、[Attractor Networks](Attractor_Networks.md)
- **基础**:[Connectomics](../00_Foundations/Connectomics.md)、[Levels of Analysis](../00_Foundations/Levels_of_Analysis.md)
- **前沿**:[fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.md)
- **疾病**:[Epilepsy](../08_Neuro_Disorders/Epilepsy.md)

---

## References

1. **Sanz Leon, P. et al.** "The Virtual Brain: a simulator of primate brain network dynamics." *Front Neuroinform*, 2013.
2. **Deco, G. et al.** "Resting-state functional connectivity emerges from structurally and dynamically shaped slow linear fluctuations." *J Neurosci*, 2013.
3. **Jirsa, V. K. et al.** "The Virtual Epileptic Patient." *NeuroImage*, 2017.
4. **Breakspear, M.** "Dynamic models of large-scale brain activity." *Nat Neurosci*, 2017.
