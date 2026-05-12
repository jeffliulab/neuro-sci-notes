# Synchron Stentrode

**Synchron Stentrode** 是 BCI 的**微创替代路径**——通过血管植入电极，避开开颅手术。2024 年已在 6 位患者中长期工作，是**最安全、最快进入商业**的 BCI 方案。Synchron 与 OpenAI、Apple 的合作预示"**消费级语音/手写 BCI**"可能先于 Neuralink 达到市场。

## 一、Stentrode 装置

### 结构

- **血管支架**（stent）作为电极载体
- 16 个电极固定在网状结构上
- 放入**上矢状窦**（superior sagittal sinus），靠近 M1
- 通过**颈静脉**插入

### 手术

- **血管介入**：颈静脉导管 → 上矢状窦
- 不需开颅
- 1–2 小时，局麻
- 术后**快速恢复**

### 无线数据

- 胸腔植入**BrainOS** 无线模块
- 蓝牙传输
- 无需外露线缆

## 二、为什么血管路径

### 解剖

- 大脑表面覆盖一层静脉窦
- 上矢状窦**正上方 M1 / SMA**
- 血管内信号 ≈ 硬脑膜外 ECoG

### 优势

- **无脑组织损伤**
- 手术安全性接近**支架放置**（已是常规）
- 与血管外科衔接

### 劣势

- 信号**质量低**（距离神经元远）
- 通道数**少**（16）
- 空间分辨率**粗**

## 三、COMMAND 临床试验

### 2021 开始

- FDA IDE 批准（2021）
- 多中心（Mount Sinai、U Pittsburgh、Sydney）
- 目标：**6 患者，1 年随访**

### 入选

- ALS、脊髓损伤
- 手臂功能丧失
- 认知完整

### 首位患者：Philip O'Keefe（澳大利亚 2021）

- ALS 患者
- **2021 发第一条"脑发推"**
- 全球首次 BCI 发社交媒体
- 持续使用 3+ 年

### 美国首位（2022）：Rodney Gorham

- 也是 ALS
- Mount Sinai
- 日常电脑控制

### 2024：6 位患者全部入组

- 英国、美国、澳大利亚
- 综合结果待发表

## 四、能力演示

### 基本控制

- 鼠标光标
- 点击（想象手动作）
- 发短信、邮件
- 浏览网页

### 速度

- ~10–15 点击/分钟
- **低于 Neuralink** 但**日常可用**

### 可靠性

- **长期（3+ 年）稳定**
- 这是 Stentrode 最强优势
- Neuralink 未达到此验证年数

## 五、Synchron × OpenAI 合作

### 2023 公告

- Synchron 集成 **ChatGPT** 到 BCI
- 用户脑控 + LLM 对话增强
- 想说的话 LLM 帮补全

### 实现

- BCI 解码 → 意图
- 意图 → OpenAI API → 生成文本
- 用户选择确认

### 意义

- **LLM 填补低带宽 BCI 的缺口**
- Stentrode 16 通道不够快打字，但**足够选意图**
- 详见 [LLM 后处理融合](../07_Brain_to_Language/LLM后处理融合.md)

## 六、Synchron × Apple 合作

### 2024 消息

- Synchron 与 Apple **Vision Pro** 集成
- BCI 控制 visionOS
- 对瘫痪患者 AR / VR 可达

### 目标

- **消费级 AR + BCI**
- 先医疗后扩展

## 七、Synchron 商业策略

### 聚焦医疗

- 不走 Musk 的炒作路线
- 医生 + 患者**低调推进**
- 2027 预期 FDA PMA

### 合作而非垄断

- 与 Apple、OpenAI 合作
- **"BCI 作为接口"**，而非**"BCI 作为操作系统"**
- 类似 **USB 协议**的定位

### 融资

- 已融 ~$145 M
- Gates Foundation、Khosla、Bezos 投资
- 估值 ~$1B

## 八、技术对比

### vs Neuralink

| | Neuralink | Synchron |
| --- | --- | --- |
| 通道 | 1024 | 16 |
| 侵入 | 皮层内 | 血管内 |
| 手术 | 复杂机器人 | 血管介入 |
| 信号 | Single-unit | LFP-like |
| 速度 | 快 | 慢 |
| 安全 | 研究 | 接近批准标准 |
| 商业 | 2027+ | 2026+ |

### vs Precision

见 [Precision_Paradromics_Blackrock](Precision_Paradromics_Blackrock.md)。

## 九、未来路径

### 1. 更多通道

- 下一代 Stentrode：32 → 64 通道
- 多支架同时植入
- 仍远低于 Neuralink

### 2. 刺激能力

- 目前仅读
- 双向版本（ICMS 从血管内？）研究中

### 3. 消费级

- 结合 LLM → **低通道也能快速意图**
- Apple 等消费伙伴
- 医疗批准后 → 消费健康扩展

### 4. 全球扩展

- 欧洲、亚洲市场
- 价格预期 **$25,000–50,000**（比外科 BCI 便宜）

## 十、适应症扩展

### 近期（2026–2028）

- ALS
- 脊髓损伤
- 闭锁综合征

### 中期（2028–2030）

- 脑卒中（部分运动功能）
- 多发性硬化
- 严重老年衰弱

### 长期（2030+）

- 消费级健康用户
- AR / VR 接口
- 工作辅助

## 十一、局限

### 1. 信号质量

- 血管内距离神经元**数毫米**
- 时间分辨率好，空间差
- 复杂任务（高维运动、语音）不如皮层

### 2. 血管限制

- 电极位置**受血管走向限制**
- 不能精确定位某脑区
- 某些脑区**不可达**

### 3. 血栓风险

- 血管内异物 → 血栓
- 长期抗凝治疗
- 药物副作用

### 4. 规模

- 目前 6 患者——小
- Neuralink 2024 扩展快

## 十二、逻辑链

1. **Stentrode 通过血管植入**，避开开颅——**最安全的 BCI 路径**。
2. **16 通道但长期稳定**——3+ 年无问题验证。
3. **COMMAND 试验 6 患者**跨美英澳，已完成入组。
4. **与 OpenAI 合作** 用 LLM 补偿低带宽 → 实用化。
5. **与 Apple 合作** 指向 AR/VR + BCI 消费方向。
6. **商业化预期 2026+**，比 Neuralink 更早。
7. **局限**：信号质量、血管限制、血栓风险——但安全性压倒性。

## 参考文献

- Mitchell et al. (2023). *Assessment of safety of a fully implanted endovascular brain-computer interface for severe paralysis in 4 patients: the Stentrode with Thought-Controlled Digital Switch (SWITCH) study.* JAMA Neurology.
- Oxley et al. (2021). *Motor neuroprosthesis implanted with neurointerventional surgery improves capacity for activities of daily living tasks in severe paralysis: first in-human experience.* J NeuroInterv Surg.
- Synchron (2023). Press release on ChatGPT integration.
- Opie et al. (2018). *Focal stimulation of the sheep motor cortex with a chronically implanted minimally invasive electrode array mounted on an endovascular stent.* Nat Biomed Eng.
- clinicaltrials.gov: NCT03834857 (SWITCH), NCT05035082 (COMMAND).
