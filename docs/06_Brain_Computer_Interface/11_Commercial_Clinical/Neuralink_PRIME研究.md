# Neuralink PRIME 研究

**Neuralink PRIME（Precise Robotically Implanted Brain-Computer Interface）** 是 Neuralink 在 2024–2026 的关键人体试验。从 2024-01 Noland Arbaugh 成为第一位接受 N1 植入的人类，到 2026 年扩展到 12+ 位患者，Neuralink 从 Elon Musk 的愿景走向**临床现实**。

## 一、N1 设备

### 硬件

- **芯片**：1024 通道 ASIC
- **电极**：64 条 threads × 16 电极 = 1024 触点
- **无线**：蓝牙传数据
- **电池**：无线充电
- **体积**：硬币大小（23 mm × 8 mm）
- **植入位置**：颅骨内凹陷处

### R1 手术机器人

- 精密自动手术
- 显微镜级**插入线**
- 避开血管
- 每条线植入 ~30 秒

### 为什么柔性线

- 相比 Utah Array 刚性针
- **减少组织损伤**
- **更好长期稳定性**
- 但**更难插入** → 需要机器人

## 二、PRIME 研究设计

### 申请

- 2023-05 FDA 批准 IDE（Investigational Device Exemption）
- 2023-09 开始招募
- 2024-01 首次植入

### 入选标准

- 四肢瘫痪（脊髓损伤或 ALS）
- 颈部以下丧失自主运动
- 成人（22–75）
- 稳定状态 ≥ 1 年

### 研究目标

- **安全性**：植入 + 长期使用
- **有效性**：BCI 控制电脑
- **可用性**：日常生活辅助

## 三、首位患者：Noland Arbaugh

### 背景

- **Noland Arbaugh**，29 岁
- 2016 跳水事故导致 C4-C5 脊髓损伤
- 肩膀以下瘫痪

### 植入：2024-01-28

- 手术顺利
- 恢复 1 天出院
- 几天内开始测试

### 能力展示

- 用大脑**控制光标**
- 玩国际象棋
- 《文明 6》
- Twitch 直播
- **4 小时连续使用**无疲劳

### 意外：2024-02 线缩回

- 64 条线中 15% 缩回颅骨
- 通道减少 → 解码性能下降
- Neuralink 软件升级补偿

### 解决

- **算法优化**：充分利用剩余通道
- **重新校准**：CLDA 适应新通道分布
- **性能基本恢复**

Noland 成为**BCI 活广告**——媒体曝光度极高。

## 四、2024 后续患者

### 患者 2（2024-08）

- ALS 患者
- 中西部
- 植入成功
- 更好的线保留（手术技术改进）

### 患者 3–5（2024 下半年）

- 继续招募
- 部分神经退行性疾病患者
- 加拿大批准（2024 夏）—— 国际扩张开始

## 五、2025 年扩展

### 患者 6–12（预计）

- 英国、加拿大、美国多中心
- 覆盖不同病因
- 扩大适应症

### 技术迭代

- **N1 v2**（预期 2025-2026）：改进线、更多通道
- **视觉假体 Blindsight**（2024-09 FDA breakthrough device）
- **S1 双向**项目启动

## 六、Neuralink 的技术优势

### 1. 1024 通道

- 最高消费级 BCI 通道数
- vs BrainGate 96
- vs Synchron 16

### 2. 柔性线

- 减少组织反应
- 长期稳定性优势

### 3. R1 自动化

- 手术时间短
- 精度高
- 可扩展

### 4. 全栈

- 硬件 + 软件 + 应用
- 一家公司控制整个 stack

## 七、技术挑战

### 1. 线缩回

- 2024 最大挑战
- 手术技术改进
- 线材料改进（未公开）

### 2. 电极密度

- 1024 通道但**实际使用率**低
- 有效运动解码通道 < 500

### 3. 寿命

- 预期 10+ 年
- 实际尚未验证
- **加速老化测试**进行中

### 4. 延迟

- 蓝牙传输 ~20 ms
- 实时性**够用但不完美**

## 八、商业模式

### 目前：临床试验

- 免费提供给患者
- 收集数据
- FDA 通道建立

### 未来：医疗 + 消费

- **医疗**：2027+ 预期商业化（瘫痪、ALS 治疗）
- **消费**：2030+ 更远期（健康人增强）

### 定价预测

- 医疗版 ~$40,000（类似高级假肢）
- 消费级 ~$5,000（类似高端笔记本）
- 规模化后下降

## 九、竞争格局

详见 [Synchron_Stentrode](Synchron_Stentrode.md)、[Precision_Paradromics_Blackrock](Precision_Paradromics_Blackrock.md)。

| 维度 | Neuralink | Synchron | Precision |
| --- | --- | --- | --- |
| 通道 | 1024 | 16 | 1024 |
| 侵入性 | 高（皮层线） | 极低（血管） | 中（表面） |
| 2026 患者数 | 12+ | 6+ | 10+ |
| FDA | IDE | IDE + Breakthrough | 510k 2025-03 |
| Musk factor | 极大 | 无 | 无 |

Neuralink 最激进；Synchron 最安全；Precision 最商业化。

## 十、争议与批评

### 1. 动物福利

- 2022 SEC 调查（死亡率高）
- 反对者呼吁停止
- Neuralink 回应：医疗研究必需

### 2. 透明度

- 披露不如 BrainGate 学术化
- **Musk 社交媒体 claims > 同行评审**
- 缺少 peer-reviewed 关键数据

### 3. 过度承诺

- Musk 曾承诺：2020 猪、2022 人、"脑机对讲"、"治愈自闭症"
- 实际进度**慢于承诺**
- 但**2024 实际植入**是真实里程碑

### 4. 伦理担忧

- 增强用途（健康人）引发讨论
- [神经权利](../13_Ethics_Neurorights/index.md)

## 十一、对 BCI 行业的影响

### 正面

- **注意力**：BCI 成主流话题
- **资金**：投资流入（Paradromics、Precision、Synchron 均受益）
- **法规**：FDA 加速审批路径

### 负面

- **炒作**：过度承诺引来监管压力
- **伦理恐慌**：普通人对 BCI 恐惧
- **隐私担忧**：Neuralink + Musk 数据使用

## 十二、逻辑链

1. **Neuralink PRIME 2024 开启 BCI 临床新时代**。
2. **N1 硬件**：1024 通道、柔性线、R1 机器人、无线。
3. **Noland Arbaugh 2024-01** 成为第一位 N1 用户，展示实用控制。
4. **2024-08 线缩回**是主要技术挑战，算法补偿。
5. **2025 扩展到 12+ 患者**，国际、多病因。
6. **全栈 + Musk factor** 是 Neuralink 的独特优势。
7. **争议、透明度、伦理** 是长期挑战。

## 参考文献

- Neuralink (2024). *PRIME Study Protocol.* clinicaltrials.gov NCT06429735
- Neuralink (2021). *An integrated brain-machine interface platform with thousands of channels.* J Med Internet Res.
- Noland Arbaugh (2024). 多次 Neuralink 直播 + 社交媒体更新.
- Musk et al. (2019). *An integrated brain-machine interface platform with thousands of channels.* JMIR Preprints.
- FDA (2024). *Breakthrough Device Designation: Neuralink Blindsight.*
