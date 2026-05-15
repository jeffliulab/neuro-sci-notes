# 双相情感障碍 (Bipolar Disorder)

> *Bipolar disorder 是情绪在 mania/hypomania 与 depression 间摆动的慢性病,~ 1-2% prevalence,高遗传(~ 70-80%)。Bipolar I(完整躁狂)vs Bipolar II(轻躁狂 + 重抑郁)。Lithium 1949 发现仍是金标准。机制:节律 + 神经可塑 + 离子失调,远未明。自杀风险极高。*
>
> **难度**:Intermediate
> **前置知识**:[Depression](Depression.md)、[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 临床 (DSM-5)

- **Manic episode**(≥ 1 周):情绪高涨/易激惹 + 精力↑ + 睡眠需求↓ + 浮夸 + 冲动(消费/性/投资)
- **Hypomanic**(≥ 4 天,较轻,无显著功能损)
- **Major depressive episode**
- **Bipolar I**:≥ 1 次 mania
- **Bipolar II**:hypomania + 重抑郁(无完整 mania)
- **Cyclothymia**:亚临床波动 ≥ 2 年

---

## 2. 流行病学

- Prevalence ~ 1-2%(BP I + II)
- 起病:青少年晚期-25 岁
- 平均确诊延迟 ~ 8-10 年(常误诊为单相抑郁)
- 自杀率:终生 ~ 15-20×普通人群

---

## 3. 遗传

- Heritability ~ 70-80%(精神科最高之一)
- 多基因 + CACNA1C(钙通道)、ANK3、ODZ4
- 与 schizophrenia 部分共享遗传风险
- 家族聚集明显

---

## 4. 神经基础(假说)

- **离子 / 钙通道**:CACNA1C → 兴奋性失调
- **节律失调**:circadian + sleep 紊乱常触发 episode
- **神经可塑性**:BDNF、mitochondrial 功能
- **单胺 + glutamate**:复杂,非单一
- 影像:amygdala-PFC 调控环路异常

---

## 5. Lithium — 金标准

- Cade 1949 偶然发现
- 减躁狂 + 抗自杀(独特证据)
- 机制不明:GSK-3β 抑制、inositol depletion、神经保护
- 治疗窗窄(0.6-1.2 mmol/L)→ 需血药监测(肾/甲状腺毒)

---

## 6. PyTorch — 双稳态情绪模型

```python
import torch

def bipolar_dynamics(T=2000, dt=0.01, instability=1.3, noise=0.15):
    """Double-well mood model: low instability=stable, high=oscillates."""
    mood = 0.0
    traj = []
    for _ in range(T):
        # dV/dx of double-well; high instability -> switches states
        force = instability * mood - mood**3
        mood += dt * force + noise * torch.randn(1).item() * (dt**0.5)
        traj.append(mood)
    return traj   # bistable: mania (+) <-> depression (-)
```

---

## 7. 治疗

### 7.1 Mood stabilizers

- **Lithium**(首选,抗自杀)
- **Valproate**、**Lamotrigine**(偏抗抑郁极)、**Carbamazepine**

### 7.2 Atypical antipsychotics

- Quetiapine、Olanzapine、Aripiprazole、Lurasidone
- 治急躁狂 + 维持

### 7.3 注意

- **抗抑郁药单用危险**:可诱发 mania / rapid cycling → 须配 mood stabilizer
- **ECT**:难治性 / 急重(高效)
- 心理:心理教育、节律稳定(IPSRT)、睡眠规律

---

## 8. 触发因素

- 睡眠剥夺(强力诱躁)
- 季节(春夏躁、秋冬郁倾向)
- 应激事件
- 抗抑郁药 / 兴奋剂
- 物质滥用

---

## 9. 共病

- 焦虑障碍、物质滥用(高)
- ADHD(尤青少年,鉴别难)
- 代谢综合征(药物 + 病本身)
- 心血管病(寿命缩短)

---

## 10. Common Pitfalls

### 10.1 = 情绪化 / 喜怒无常

是临床综合征(episode 持续数天-周),非日常情绪波动。

### 10.2 抗抑郁药安全

单用可诱 mania / rapid cycling;须 mood stabilizer 保护。

### 10.3 Bipolar II 较轻

抑郁极常更久更致残;自杀风险不低。

### 10.4 Lithium 过时

仍是抗自杀 + 维持金标准;无更优替代。

### 10.5 躁狂 = 开心

常为易激惹 + 痛苦 + 后果严重(非愉悦)。

---

## 11. Related Concepts

- **同节**:[Depression](Depression.md)、[Schizophrenia](Schizophrenia.md)
- **细胞**:[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)、[Ion Channels](../02_Cellular_Molecular/Ion_Channels.md)
- **系统**:[Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md)、[Reward_System](../03_Systems_Neuroscience/Reward_System.md)

---

## References

1. **Grande, I. et al.** "Bipolar disorder." *Lancet*, 2016.
2. **Cade, J. F. J.** "Lithium salts in the treatment of psychotic excitement." *Med J Aust*, 1949.
3. **Geddes, J. R. & Miklowitz, D. J.** "Treatment of bipolar disorder." *Lancet*, 2013.
4. **Craddock, N. & Sklar, P.** "Genetics of bipolar disorder." *Lancet*, 2013.
