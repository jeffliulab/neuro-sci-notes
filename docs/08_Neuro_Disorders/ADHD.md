# 注意缺陷多动障碍 (ADHD)

> *ADHD 是 neurodevelopmental disorder,儿童 ~ 5-7%、成人 ~ 2.5%。核心:inattention + hyperactivity/impulsivity。机制:前额叶-纹状体 dopamine/norepinephrine 失调 + executive function 缺陷。Stimulant(methylphenidate/amphetamine)反直觉地有效。高遗传(~ 74%)。常持续至成人(争议)。*
>
> **难度**:Intermediate
> **前置知识**:[Working Memory](../04_Cognitive_Neuroscience/Working_Memory.md)、[Reward System](../03_Systems_Neuroscience/Reward_System.md)

---

## 1. 临床 (DSM-5)

两症状维度(< 12 岁起病,多场景):
- **Inattention**:粗心、易分心、丢三落四、不能持续
- **Hyperactivity/Impulsivity**:坐不住、话多、插话、难等待

三亚型:predominantly inattentive(旧"ADD")、hyperactive-impulsive、combined。

---

## 2. 流行病学

- 儿童 ~ 5-7%,成人 ~ 2.5%
- 男 : 女 ~ 2-3 : 1(女性易漏诊 — 多 inattentive 型)
- ~ 50-65% 症状持续至成人
- 共病高:学习障碍、ODD、焦虑、抑郁、物质滥用

---

## 3. 神经基础

- **DA / NE 失调**:前额叶-纹状体-小脑环路
- **Executive function 缺陷**:工作记忆、抑制控制、计划
- **延迟厌恶 (delay aversion)**:reward circuit → 偏好即时小奖
- **DMN 调控异常**:task 时 DMN 抑制不足 → 走神
- 影像:PFC、striatum 体积/活动 ↓;成熟延迟(非缺损)假说

---

## 4. 遗传

- Heritability ~ 74%(精神科高)
- 多基因:DAT1、DRD4、DRD5 等(效应小)
- 环境:早产、低出生体重、产前烟酒铅暴露

---

## 5. Stimulant 反常有效

- Methylphenidate(利他林)、Amphetamine(Adderall)
- 阻 DAT/NET → 突触 DA/NE ↑ → **改善** PFC 信号噪比
- "兴奋剂使多动者平静"= 倒 U(最优 DA 水平,Arnsten)
- 非镇静;是优化 PFC catecholamine

---

## 6. PyTorch — 倒 U DA-性能曲线

```python
import torch

def inverted_u_performance(da_level):
    """PFC function vs catecholamine: inverted-U (Arnsten)."""
    optimal = 1.0
    return torch.exp(-((da_level - optimal) ** 2) / 0.3)

# ADHD: low baseline DA -> stimulant moves toward optimal -> better
# Too much (overdose) -> past peak -> worse
```

---

## 7. 治疗

### 7.1 药物

- **Stimulants**(一线):methylphenidate、amphetamine
- **Non-stimulants**:atomoxetine(NET 抑制)、guanfacine/clonidine(α2 激动)
- 兴奋剂滥用 / 转移风险(管制)

### 7.2 非药物

- 行为治疗(尤幼儿首选)
- 父母培训、学校支持
- CBT(成人 executive skill)
- 环境结构化

---

## 8. 执行功能模型

- Barkley:ADHD 核心是 **行为抑制**缺陷 → 影响工作记忆、自我调节、内化言语
- Sonuga-Barke 双通路:executive dysfunction + delay aversion
- 与 [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.md) 密切

---

## 9. 争议

- 过度诊断 / 用药(地区差异大)
- 学龄相对年龄效应(班里最小的易被诊断)
- 成人 ADHD 边界
- "是疾病还是 neurodiversity 谱系"

---

## 10. Common Pitfalls

### 10.1 ADHD = 懒 / 缺管教

是神经发育障碍,非道德 / 教养问题。

### 10.2 兴奋剂使人嗨

ADHD 患者治疗剂量改善专注非欣快;倒 U 优化。

### 10.3 长大自愈

~ 半数持续成人(表现转为内在 inattention)。

### 10.4 只影响男孩

女性多 inattentive 型,被低诊;非真低发。

### 10.5 多动是核心

Inattention + executive dysfunction 常更核心、更持久。

---

## 11. Related Concepts

- **同节**:[Autism](Autism.md)、[Bipolar_Disorder](Bipolar_Disorder.md)、[Depression](Depression.md)
- **认知**:[Working Memory](../04_Cognitive_Neuroscience/Working_Memory.md)、[Attention](../04_Cognitive_Neuroscience/Attention.md)
- **系统**:[Reward System](../03_Systems_Neuroscience/Reward_System.md)、[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)

---

## References

1. **Faraone, S. V. et al.** "Attention-deficit/hyperactivity disorder." *Nat Rev Dis Primers*, 2015.
2. **Arnsten, A. F. T.** "Catecholamine influences on dorsolateral prefrontal cortical networks." *Biol Psychiatry*, 2011.
3. **Barkley, R. A.** "Behavioral inhibition, sustained attention, and executive functions." *Psychol Bull*, 1997.
4. **Sonuga-Barke, E. J. S.** "The dual pathway model of AD/HD." *Neurosci Biobehav Rev*, 2003.
