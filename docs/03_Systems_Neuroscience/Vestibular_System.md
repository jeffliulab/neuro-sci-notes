# 前庭系统 (Vestibular System)

> *前庭系统检测头部加速度 + 重力 → 平衡、空间定向、注视稳定(VOR)。内耳:半规管(角加速度)+ 耳石器(线加速度/重力)。与视觉/本体觉多感觉整合。VOR 是最快反射(~ 7 ms)。眩晕、晕动症、太空适应是其失调。机器人 IMU 是其工程对应。*
>
> **难度**:Intermediate
> **前置知识**:[Auditory_System](Auditory_System.md)、[Cerebellum](../01_Neuroanatomy/Cerebellum.md)

---

## 1. 外周器官

| 器官 | 检测 | 数量 |
|---|---|---|
| **半规管**(semicircular) | 角加速度(旋转) | 3 对(互相正交) |
| **椭圆囊**(utricle) | 水平线加速 + 重力 | 1 对 |
| **球囊**(saccule) | 垂直线加速 + 重力 | 1 对 |

毛细胞(hair cell)— 与耳蜗同源,机械门控转导。

---

## 2. 转导机制

- Hair cell stereocilia 偏向 kinocilium → 去极化(兴奋);反向 → 超极化
- 半规管:内淋巴惯性 → cupula 偏 → 角加速
- 耳石器:otolith(碳酸钙晶体)惯性 → 重力 + 线加速
- 静息放电 → 双向编码(增/减)

---

## 3. 中枢通路

```
前庭神经(CN VIII)
   ↓
前庭核(脑干)
   ├→ 眼动核(VOR — 注视稳定)
   ├→ 脊髓(前庭脊髓反射 — 姿势)
   ├→ 小脑(适应 / 校准)
   ├→ 丘脑 → 顶岛前庭皮层(PIVC,空间定向)
   └→ 自主(晕动 → 恶心)
```

---

## 4. VOR(前庭眼反射)

- 头转 → 反向等速眼动 → 视网膜像稳定
- 延迟 ~ 7-10 ms(最快反射之一,开环前馈)
- 增益 ≈ 1(可适应:戴反转棱镜 → 小脑重校准)
- 临床:头脉冲试验测 VOR

---

## 5. PyTorch — VOR 增益适应

```python
import torch

def vor_adaptation(head_velocity, gain, target_gain=1.0, lr=0.01, T=100):
    """Cerebellum-driven gain recalibration to minimize retinal slip."""
    gains = []
    for _ in range(T):
        eye_velocity = -gain * head_velocity
        retinal_slip = head_velocity + eye_velocity      # want ≈ 0
        gain += lr * retinal_slip * head_velocity        # error-driven
        gains.append(gain)
    return gains   # converges toward target_gain
```

---

## 6. 多感觉整合

- 平衡 = 前庭 + 视觉 + 本体觉融合
- 冲突 → 眩晕 / 晕动症(感觉失配理论)
- Bayesian cue combination(见 [Bayesian Brain](../05_Computational_Neuroscience/Bayesian_Brain.md))
- 失重(太空)→ 重新加权 → 太空适应综合征

---

## 7. 病理

- **BPPV**(良性阵发性位置性眩晕):耳石移位入半规管(最常见眩晕,Epley 复位)
- **Ménière 病**:内淋巴积水 → 眩晕 + 耳鸣 + 听力波动
- **前庭神经炎**:急性单侧前庭丧失
- **晕动症**:感觉冲突
- **双侧前庭病**:振动幻视(oscillopsia)、平衡差
- **前庭性偏头痛**

---

## 8. 与工程 / AI

- 半规管 + 耳石 ↔ **IMU**(陀螺 + 加速度计,见 eng-notes IMU)
- VOR ↔ 图像稳定(相机/无人机云台)
- 前庭-视觉融合 ↔ Visual-Inertial Odometry(VIO)
- 感觉失配 ↔ VR 晕动(sensory conflict)

---

## 9. 康复

- 前庭康复训练(VRT):促中枢代偿 + 适应
- Epley 手法(BPPV)
- Gaze stabilization 练习
- 前庭植入(实验,类人工耳蜗)

---

## 10. Common Pitfalls

### 10.1 前庭只管平衡

还有注视稳定(VOR)、空间定向、自主(恶心)。

### 10.2 眩晕 = 头晕

眩晕(vertigo)= 旋转错觉(前庭);头晕(dizziness)更广。

### 10.3 VOR 是慢反馈

最快反射之一(~7 ms,开环前馈)。

### 10.4 一侧失前庭 = 永久失衡

中枢代偿 + 康复可大幅恢复。

### 10.5 与听觉无关

同内耳 + 同源毛细胞 + 共 CN VIII。

---

## 11. Related Concepts

- **同节**:[Auditory_System](Auditory_System.md)、[Somatosensory](Somatosensory.md)、[Motor_System](Motor_System.md)
- **解剖**:[Cerebellum](../01_Neuroanatomy/Cerebellum.md)、[Brainstem](../01_Neuroanatomy/Brainstem.md)
- **计算**:[Bayesian Brain](../05_Computational_Neuroscience/Bayesian_Brain.md)
- **工程**: IMU、VIO — https://jeffliulab.github.io/eng-notes/

---

## References

1. **Goldberg, J. M. et al.** *The Vestibular System: A Sixth Sense*. Oxford, 2012.
2. **Angelaki, D. E. & Cullen, K. E.** "Vestibular system: the many facets of a multimodal sense." *Annu Rev Neurosci*, 2008.
3. **Cullen, K. E.** "The vestibular system: multimodal integration and encoding of self-motion." *Trends Neurosci*, 2012.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
