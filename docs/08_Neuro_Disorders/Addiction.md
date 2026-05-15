# 成瘾 (Addiction / Substance Use Disorder)

> *成瘾是慢性复发性脑病:mesolimbic dopamine 系统被劫持 → 强迫性使用 + 失控 + 渴求 despite harm。三阶段循环(Koob):binge/intoxication → withdrawal/negative affect → preoccupation/anticipation。涉 VTA-NAcc-PFC。是 RL 失调的典型 — wanting ≠ liking。*
>
> **难度**:Intermediate
> **前置知识**:[Reward System](../03_Systems_Neuroscience/Reward_System.md)、[Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.md)

---

## 1. 定义

- DSM-5 "Substance Use Disorder":失控使用 + 渴求 + 耐受 + 戒断 + 不顾后果
- 慢性 + 复发性
- 行为成瘾(赌博)共享环路 — DSM-5 纳入 gambling disorder

---

## 2. 三阶段循环 (Koob & Volkow)

```
Binge/Intoxication  → NAcc/VTA (DA 奖赏, 强化)
       ↓
Withdrawal/Neg affect → amygdala/CRF (负性情绪, anti-reward)
       ↓
Preoccupation/Anticipation → PFC (渴求, 失控, cue-driven)
       ↓ (回 binge)
```

---

## 3. 神经机制

- **急性**:↑ mesolimbic DA(VTA→NAcc)— 所有成瘾物共性
- **耐受**:DA 受体 / 信号下调 → anhedonia
- **Anti-reward**:CRF/dynorphin → 戒断负性情绪
- **PFC 失控**:前额叶调控↓ → 冲动 + 强迫
- **Cue-induced craving**:glutamatergic、incentive sensitization

---

## 4. Wanting ≠ Liking (Berridge)

- **Wanting**(incentive salience):DA 介导,病理放大
- **Liking**(hedonic):opioid/endocannabinoid,成瘾中**减弱**
- → 成瘾者强烈渴求但不再享受("想要却不喜欢")
- 经典 RL "value" 不足以解释 — incentive sensitization 理论

---

## 5. RL 视角

- 药物 = 非自然大 RPE(直接药理 ↑ DA,不被预测压制)
- → 过度 value 学习,habit(dorsal striatum)接管
- Model-free habit > goal-directed control
- 见 [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.md)

---

## 6. PyTorch — 药物诱发非自然 RPE

```python
import torch

def addiction_rpe(natural_reward=1.0, drug_pharmacological=3.0, predicted=1.0):
    """Drugs add pharmacological DA that prediction can't cancel."""
    natural_rpe = natural_reward - predicted          # learnable -> 0
    # Drug directly raises DA regardless of prediction (Redish 2004)
    drug_rpe = (drug_pharmacological - predicted) + drug_pharmacological * 0.5
    return natural_rpe, drug_rpe   # drug_rpe stays positive -> over-learning
```

---

## 7. 各类成瘾物机制

| 物质 | 主要机制 |
|---|---|
| Opioids | μ-opioid → 抑 GABA → ↑ VTA DA |
| Cocaine | 阻 DAT → ↑ 突触 DA |
| Amphetamine | 逆转 DAT + 促 DA 释放 |
| Nicotine | nAChR → ↑ VTA DA |
| Alcohol | GABA-A↑、NMDA↓、间接 DA |
| Cannabis | CB1 → 去抑制 DA |

→ 共同终点:↑ mesolimbic DA。

---

## 8. 治疗

### 8.1 药物

- **Opioid**:methadone/buprenorphine(替代)、naltrexone(拮抗)、naloxone(急救)
- **Alcohol**:naltrexone、acamprosate、disulfiram
- **Nicotine**:NRT、varenicline、bupropion

### 8.2 心理 / 社会

- CBT、动机访谈、contingency management(最强证据之一)
- 12-step、社区支持
- 复发预防(cue 管理)

### 8.3 实验

- DBS(NAcc)、TMS、psychedelic-assisted(psilocybin for alcohol)

---

## 9. 复发 + 慢性病模型

- 高复发率(类似糖尿病/高血压慢病管理)
- Cue、stress、药物本身触发(reinstatement 模型)
- 长期 PFC + striatal 适应 → 长久脆弱
- "脑病模型"减污名 vs 过度决定论争议

---

## 10. Common Pitfalls

### 10.1 = 意志薄弱 / 道德问题

是慢性脑病(环路劫持),非单纯选择 / 品德。

### 10.2 成瘾者享受用药

Wanting ≫ liking;后期常不享受仍渴求。

### 10.3 戒断 = 治愈

戒断只是起点;复发风险长期存在。

### 10.4 仅 DA / 奖赏

含 anti-reward(CRF)、PFC 失控、习惯系统。

### 10.5 替代治疗 = "换药"

methadone/buprenorphine 稳定环路、降危害,有强证据。

---

## 11. Related Concepts

- **同节**:[Depression](Depression.md)、[Anxiety_Disorders](Anxiety_Disorders.md)、[ADHD](ADHD.md)
- **系统**:[Reward System](../03_Systems_Neuroscience/Reward_System.md)
- **解剖**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、[Amygdala](../01_Neuroanatomy/Amygdala.md)
- **计算**:[Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.md)

---

## References

1. **Koob, G. F. & Volkow, N. D.** "Neurobiology of addiction: a neurocircuitry analysis." *Lancet Psychiatry*, 2016.
2. **Robinson, T. E. & Berridge, K. C.** "The incentive sensitization theory of addiction." *Brain Res Rev*, 1993.
3. **Redish, A. D.** "Addiction as a computational process gone awry." *Science*, 2004.
4. **Volkow, N. D. et al.** "Neurobiologic advances from the brain disease model of addiction." *NEJM*, 2016.
