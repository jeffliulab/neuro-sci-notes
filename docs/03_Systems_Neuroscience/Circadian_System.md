# 昼夜节律系统 (Circadian System)

> *昼夜节律 ≈ 24 h 内源振荡,主钟在下丘脑视交叉上核(SCN)。分子机制 = 转录-翻译负反馈环(CLOCK/BMAL1 ↔ PER/CRY),2017 Nobel(Hall/Rosbash/Young)。光(ipRGC → SCN)是主授时因子(zeitgeber)。失调 → 时差/轮班病/代谢/情绪。与睡眠紧密但可分离。*
>
> **难度**:Intermediate
> **前置知识**:[Sleep_Wake](Sleep_Wake.md)、[Hypothalamus_Homeostasis](Hypothalamus_Homeostasis.md)

---

## 1. 核心概念

- **Circadian**:内源 ~24 h 节律(恒定条件下持续 → free-running)
- **Zeitgeber**(授时因子):光(主)、进食、温度、社交
- **Entrainment**:外部信号校准内钟到 24 h
- **Phase**:节律相位(相位提前/延迟)

---

## 2. 分子钟(TTFL)

```
CLOCK + BMAL1 (转录激活)
   ↓ 驱动转录
PER + CRY
   ↓ 累积 → 二聚 → 入核
抑制 CLOCK/BMAL1(负反馈)
   ↓ PER/CRY 降解(~24 h 周期)
循环重启
```

→ Transcription-Translation Feedback Loop(2017 Nobel)。几乎每个细胞都有(外周钟)。

---

## 3. SCN 主钟

- 下丘脑 ~ 20,000 神经元(视交叉上方)
- 自主振荡(离体仍 ~24 h)+ 神经元间耦合 → 鲁棒同步
- 输出 → 松果体(褪黑素)、自主、行为、外周钟同步
- 损毁 SCN → 节律消失(行为随机)

---

## 4. 光授时通路

- **ipRGC**(内在光敏视网膜节细胞,含 melanopsin)→ 视网膜下丘脑束 → SCN
- 非成像视觉(盲人若 ipRGC 完好仍可授时)
- 蓝光(~480 nm)最强 → 夜间屏幕抑褪黑素 → 相位延迟

---

## 5. PyTorch — 振荡 + 授时

```python
import numpy as np

def circadian_oscillator(T=120, intrinsic_period=24.5, light_schedule=None):
    """Free-running ~24.5h; light entrains toward 24h."""
    phase = 0.0
    out = []
    for t in range(T):
        omega = 2*np.pi / intrinsic_period
        dphase = omega
        if light_schedule and light_schedule(t):        # light pulse
            # Phase response: advance/delay depending on phase
            dphase += 0.3 * np.sin(phase)               # PRC-like
        phase += dphase
        out.append(np.sin(phase))
    return out   # entrained to 24h with light, drifts without
```

---

## 6. Phase Response Curve (PRC)

- 光对相位的影响**依施加时相**:
  - 主观夜早 → 相位**延迟**
  - 主观夜晚/晨 → 相位**提前**
  - 主观日 → 几无效应
- 解释时差恢复、光疗时机、褪黑素给药策略

---

## 7. 与睡眠(双过程模型)

- **Process C**(circadian):SCN 驱动的觉醒倾向节律
- **Process S**(homeostatic):清醒越久睡压越高(腺苷)
- 睡眠时点 = C × S 交互(Borbély,见 [Sleep_Wake](Sleep_Wake.md))
- 节律 ≠ 睡眠(可分离:强制去同步实验)

---

## 8. 失调 + 健康

- **时差(jet lag)**:外部相位骤变,内钟滞后
- **轮班工作障碍**:慢性失同步 → 代谢/心血管/癌风险↑
- **DSPD/ASPD**:睡眠相位延迟/提前综合征(常基因 PER 变异)
- **季节性情感障碍(SAD)**:光不足 → 光疗有效
- 外周钟失同步(进食时间)→ 代谢病
- "Chronotherapy":按节律给药/治疗

---

## 9. 与 AI / 工程

- 振荡器 + 授时 ↔ 锁相环(PLL)、时钟同步
- 节律建模 ↔ 时间序列周期分解
- Chronobiology + ML:可穿戴推断 chronotype / 健康
- 系统钟同步 ↔ 分布式时钟(类比)

---

## 10. Common Pitfalls

### 10.1 节律 = 睡眠

可分离(Process C vs S);SCN 损节律失但睡眠仍存(碎片化)。

### 10.2 周期正好 24 h

内源常 ~24.2 h(人),靠光每日校准。

### 10.3 只有 SCN 有钟

几乎所有细胞有外周钟;SCN 是协调者。

### 10.4 褪黑素 = 安眠药

是相位信号(timing),非镇静催眠;时机比剂量重要。

### 10.5 光授时靠视杆视锥

主要靠 ipRGC(melanopsin);盲人可保留授时。

---

## 11. Related Concepts

- **同节**:[Sleep_Wake](Sleep_Wake.md)、[Hypothalamus_Homeostasis](Hypothalamus_Homeostasis.md)、[Visual_System](Visual_System.md)(ipRGC)
- **解剖**:[Brainstem](../01_Neuroanatomy/Brainstem.md)
- **疾病**:[Depression](../08_Neuro_Disorders/Depression.md)(SAD)、[Bipolar_Disorder](../08_Neuro_Disorders/Bipolar_Disorder.md)(节律触发)

---

## References

1. **Takahashi, J. S.** "Transcriptional architecture of the mammalian circadian clock." *Nat Rev Genet*, 2017.
2. **Bass, J. & Takahashi, J. S.** "Circadian integration of metabolism and energetics." *Science*, 2010.
3. **Hattar, S. et al.** "Melanopsin-containing retinal ganglion cells." *Science*, 2002.
4. **Borbély, A. A. et al.** "The two-process model of sleep regulation: a reappraisal." *J Sleep Res*, 2016.
