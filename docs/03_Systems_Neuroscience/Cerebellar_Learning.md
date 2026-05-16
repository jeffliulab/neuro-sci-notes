# 小脑学习系统 (Cerebellar Learning System)

> *小脑不只协调运动:执行**监督式误差学习**。Marr-Albus-Ito 理论:平行纤维-Purkinje 突触经爬行纤维"误差信号"LTD 调整。前向模型/内部模型 → 预测 + 校正(VOR 适应、saccade 适应)。近年:小脑亦参认知/情绪。是 supervised learning 的生物范本。*
>
> **难度**:Advanced
> **前置知识**:[Cerebellum](../01_Neuroanatomy/Cerebellum.md)、[Motor_System](Motor_System.md)

---

## 1. 回路(复习 + 学习视角)

```
Mossy fiber → Granule cell(海量,扩展编码)
   ↓ Parallel fiber(~200,000/Purkinje)
Purkinje cell ←── Climbing fiber(下橄榄,1 根/PC,"误差/教学信号")
   ↓(唯一输出,GABA)
深部核 → 丘脑/脑干
```

颗粒细胞层 = 高维稀疏扩展(类 reservoir/SVM kernel,见 [Reservoir Computing](../05_Computational_Neuroscience/Reservoir_Computing.md))。

---

## 2. Marr-Albus-Ito 学习

- **Marr 1969 / Albus 1971**:平行纤维-Purkinje 突触可塑;爬行纤维 = 教学信号
- **Ito**:发现 **PF-PC LTD**(爬行纤维 + 平行纤维共激 → 长时抑制)
- → 小脑 = 监督学习器(误差驱动,perceptron-like)

---

## 3. 监督 vs 三系统对比

| 系统 | 学习类型 | 教学信号 |
|---|---|---|
| **小脑** | 监督 | 爬行纤维(误差) |
| 基底节 | 强化 | 多巴胺(RPE) |
| 皮层/海马 | 无/自监督 | Hebbian/统计 |

→ 大脑三大学习系统分工(Doya 2000)。

---

## 4. 内部模型(Forward/Inverse)

- **Forward model**:由运动指令预测感觉结果(快于真实反馈 → 实时控制)
- **Inverse model**:由目标算所需指令
- 小脑 = 内部模型存储/学习处
- 解释:不能自己挠痒(预测衰减,见 [Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.md))

---

## 5. PyTorch — 小脑感知子(误差驱动 LTD)

```python
import torch

def cerebellar_perceptron(granule_act, weights, climbing_error, lr=0.01):
    """PF-PC LTD: parallel fiber weights decrease when paired with CF error."""
    purkinje_out = (weights * granule_act).sum()         # PC firing
    # Climbing fiber error -> LTD at co-active PF synapses
    weights = weights - lr * climbing_error * granule_act
    return purkinje_out, weights   # supervised, error-corrected
```

---

## 6. 经典适应范式

- **VOR 适应**:戴放大/反转镜 → 增益重校(见 [Vestibular_System](Vestibular_System.md))
- **Saccade 适应**:目标跳变 → 眼动幅度校正
- **Prism adaptation**:棱镜偏移 → 指向重校(去镜后 aftereffect)
- **Eyeblink conditioning**:CS-US 配对(小脑 HVI/中间核,经典)

---

## 7. 超越运动 — 认知小脑

- 后外侧小脑 ↔ PFC(闭环)→ 认知/语言/工作记忆参与
- "Cerebellar cognitive affective syndrome"(Schmahmann)
- 假说:小脑对**认知**也做"协调 + 误差校正 + 内部模型"(统一计算)
- 情绪/社会认知亦涉

---

## 8. 临床

- **小脑损伤**:共济失调、辨距不良、意向性震颤、构音障碍
- **VOR/saccade 适应缺陷**(适应学习损)
- **CCAS**:执行/视空间/语言/情绪钝
- 退行(SCA)、卒中、肿瘤(髓母细胞瘤)
- 与自闭症(小脑发育异常关联)

---

## 9. 与 AI

- Marr-Albus = 早期 perceptron 生物原型
- 颗粒层扩展 ↔ kernel / reservoir / random features
- 监督误差学习 ↔ supervised learning(但局部规则,非 backprop,见 [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.md))
- Forward model ↔ world model / model-based control(见 ai-notes 世界模型)

---

## 10. Common Pitfalls

### 10.1 小脑只管运动

也参认知/语言/情绪(CCAS;统一计算假说)。

### 10.2 学习只靠 PF-PC LTD

也有 LTP、深部核可塑、多位点(更复杂)。

### 10.3 爬行纤维 = 运动指令

是**误差/教学**信号(下橄榄),非指令。

### 10.4 = backprop

是**局部**误差驱动(爬行纤维),非全局梯度回传。

### 10.5 小脑损 = 瘫

是协调/精度/学习障碍(ataxia),非瘫痪。

---

## 11. Related Concepts

- **同节**:[Motor_System](Motor_System.md)、[Vestibular_System](Vestibular_System.md)
- **解剖**:[Cerebellum](../01_Neuroanatomy/Cerebellum.md)
- **计算**:[Reservoir Computing](../05_Computational_Neuroscience/Reservoir_Computing.md)、[Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.md)
- **认知**:[Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.md)
- **AI**: supervised learning、world model

---

## References

1. **Marr, D.** "A theory of cerebellar cortex." *J Physiol*, 1969.
2. **Albus, J. S.** "A theory of cerebellar function." *Math Biosci*, 1971.
3. **Ito, M.** "Cerebellar long-term depression: characterization, signal transduction, and functional roles." *Physiol Rev*, 2001.
4. **Schmahmann, J. D.** "The cerebellum and cognition." *Neurosci Lett*, 2019.
