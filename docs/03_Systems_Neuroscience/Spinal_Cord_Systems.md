# 脊髓系统 (Spinal Cord Systems)

> *脊髓不只是"电缆":含反射弧、中枢模式发生器(CPG,产生节律运动)、感觉门控、局部回路。上行(感觉)+ 下行(运动)纤维束 + 灰质回路。损伤定位(横断/半切/前角)有特征综合征。是运动控制 + 康复 + 神经假体的关键层级。*
>
> **难度**:Intermediate
> **前置知识**:[Motor_System](Motor_System.md)、[Somatosensory](Somatosensory.md)

---

## 1. 结构

- 31 节段(8C+12T+5L+5S+1Co)
- **灰质**(中央 H 形):背角(感觉)、腹角(运动)、中间外侧柱(自主)
- **白质**(外周束):上行 + 下行
- Rexed laminae(I-X)分层

---

## 2. 主要纤维束

| 束 | 方向 | 功能 |
|---|---|---|
| 后柱(DCML) | 上行 | 精触觉/本体觉 |
| 脊髓丘脑束 | 上行 | 痛/温(交叉早) |
| 皮质脊髓束 | 下行 | 随意精细运动 |
| 红核/网状/前庭脊髓束 | 下行 | 姿势/粗运动 |
| 脊髓小脑束 | 上行 | 本体觉 → 小脑 |

---

## 3. 反射弧

- **牵张反射**(膝跳):肌梭 Ia → α 运动神经元(单突触)
- **屈肌回撤 + 交叉伸肌**:多突触保护反射
- **Golgi 腱器**(Ib):张力 → 抑制(保护)
- 反射可被脊髓 + 下行调节(增益可变)

---

## 4. 中枢模式发生器 (CPG)

- 脊髓内在回路产生节律运动(行走/游泳)**无需节律输入**
- 半中心模型(half-center):互抑振荡
- 离体脊髓 + 药物(如 L-DOPA)→ 仍现"虚拟行走"
- 见 [Neural Circuits](../00_Foundations/Neural_Circuits.md)(CPG motif)

---

## 5. PyTorch — 半中心 CPG

```python
import torch

def half_center_cpg(T=300, dt=0.1, tau=5.0, w_inhib=2.0):
    """Mutual inhibition + adaptation → alternating rhythmic output."""
    a, b = 1.0, 0.0
    fa, fb = 0.0, 0.0   # fatigue/adaptation
    out = []
    for _ in range(T):
        a += dt/tau * (-a + torch.relu(torch.tensor(1.0 - w_inhib*b - fa)))
        b += dt/tau * (-b + torch.relu(torch.tensor(1.0 - w_inhib*a - fb)))
        fa += dt/tau * (-fa + 0.5*a); fb += dt/tau * (-fb + 0.5*b)
        out.append((float(a), float(b)))
    return out   # antiphase rhythm = locomotor pattern
```

---

## 6. 感觉门控

- 背角是疼痛"闸门"(见 [Pain_System](Pain_System.md))
- Presynaptic inhibition、局部中间神经元
- 下行调制(PAG-RVM)
- 非被动中继 → 主动过滤/整合

---

## 7. 损伤综合征(定位)

| 损伤 | 表现 |
|---|---|
| 完全横断 | 损平面下完全瘫 + 感觉丧失 + 自主 |
| **Brown-Séquard**(半切) | 同侧运动+本体觉失,对侧痛温失 |
| 前索综合征 | 痛温 + 运动失,本体觉保 |
| 中央索(脊髓空洞) | 节段性痛温丧失("披肩样") |
| 脊髓休克 | 急性反射消失(后恢复 + 痉挛) |
| ALS | 上下运动神经元(见 [ALS](../08_Neuro_Disorders/ALS.md)) |

---

## 8. 临床 + 神经假体

- **脊髓损伤(SCI)**:康复 + 硬膜外电刺激(EES)→ 重激活 CPG → 部分行走恢复(Courtine)
- **闭环 EES + BCI**:意图解码 → 刺激(脑-脊接口,2023 Lancet)
- **外骨骼**、功能电刺激(FES)
- 神经再生(支架/干细胞)研究中

---

## 9. 与 AI / 机器人

- CPG ↔ 机器人运动控制(节律生成器,四足/蛇形)
- 反射 + 下行 = 分层控制(低层快反射 + 高层规划)
- 脊髓"边缘计算"↔ 分布式控制
- 见 eng-notes 机器人控制

---

## 10. Common Pitfalls

### 10.1 脊髓只是电缆

含反射 + CPG + 感觉门控 + 局部计算。

### 10.2 行走全靠大脑

CPG 在脊髓产生节律;脑启动 + 调节。

### 10.3 反射增益固定

可被脊髓 + 下行可塑调节(状态依赖)。

### 10.4 脊髓休克 = 永久

急性期反射消失,后恢复(常转痉挛)。

### 10.5 SCI 后完全不可恢复

EES + 康复可部分恢复(残余通路 + CPG 重激活)。

---

## 11. Related Concepts

- **同节**:[Motor_System](Motor_System.md)、[Somatosensory](Somatosensory.md)、[Pain_System](Pain_System.md)
- **基础**:[Neural Circuits](../00_Foundations/Neural_Circuits.md)(CPG)
- **疾病**:[ALS](../08_Neuro_Disorders/ALS.md)
- **前沿**:[Closed_Loop_Neuromodulation](../07_Neurotech_Frontiers/Closed_Loop_Neuromodulation.md)(EES)

---

## References

1. **Grillner, S.** "Biological pattern generation: the cellular and computational logic of networks in motion." *Neuron*, 2006.
2. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
3. **Courtine, G. & Sofroniew, M. V.** "Spinal cord repair: advances in biology and technology." *Nat Med*, 2019.
4. **Lorach, H. et al.** "Walking naturally after spinal cord injury using a brain-spine interface." *Nature*, 2023.
