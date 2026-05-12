# 分层规划：BCI + LLM + 机器人

**BCI + LLM + 机器人**（简称 BLR）是 2024–2026 最活跃的 AI 前沿之一。它把 BCI 提取的**低带宽意图**通过 LLM 扩展为**结构化计划**，再由机器人**执行**——形成一个完整的 Intention-to-Action 管道。

## 一、架构全景

```
┌─────────────────────────────────────────────────────┐
│  大脑                                                 │
│  ├ PPC/PFC: 高层意图（"去厨房拿水"）                    │
│  └ M1: 低层 kinematic                                │
└─────────────────┬───────────────────────────────────┘
                  ↓  neural signals
┌─────────────────────────────────────────────────────┐
│  BCI 解码器                                           │
│  ├ NDT3/CEBRA: 神经 → embedding                     │
│  ├ Speech BCI: 神经 → words                         │
│  └ Intent classifier: → 结构化 intent               │
└─────────────────┬───────────────────────────────────┘
                  ↓  natural language / structured intent
┌─────────────────────────────────────────────────────┐
│  LLM 规划器                                           │
│  ├ 解析意图                                           │
│  ├ 分解为子目标                                        │
│  ├ 生成动作序列                                        │
│  └ 错误恢复 / 对话澄清                                 │
└─────────────────┬───────────────────────────────────┘
                  ↓  action sequence (ROS2 / PDDL)
┌─────────────────────────────────────────────────────┐
│  机器人执行                                           │
│  ├ 运动规划 (MoveIt, RRT*)                           │
│  ├ 视觉感知 (SAM, CLIP)                              │
│  └ 控制 (PID, MPC)                                   │
└─────────────────────────────────────────────────────┘
```

每一层处理不同抽象粒度——这就是**分层规划**的本质。

## 二、为什么需要 LLM 层

### 如果没有 LLM

用户每个动作都要 BCI 精细指定：
- "向前 10 cm" "抓" "抬起" "向左 30 cm" …
- BCI 带宽不够，体验极差

### 加入 LLM

用户说 "给我杯水"，LLM 生成 20+ 步的完整动作序列：
1. 识别 "水"  = 厨房的水瓶
2. 规划移动到厨房
3. 抓取水瓶
4. 回到用户
5. 倒水到用户杯子
6. 送到用户嘴边

BCI 仅需传递**语义级意图**。

### LLM 的核心能力

- **常识推理**："水"放在厨房、咖啡机产出咖啡
- **语言理解**：模糊表达（"我渴")
- **错误恢复**：机器人说"厨房没水"，LLM 建议替代
- **多轮对话**：用户修正时 LLM 适应

## 三、代表系统

### HiCRISP（2023）

**Chen et al.** 提出 **HiCRISP（Hierarchical Closed-loop Robotic Intelligent Self-correction Planner）**：
- LLM 生成任务级计划
- 闭环监控 + 自修正
- 在 BCI + 机器人场景中 demo

### PaLM-E（Google 2023）

**多模态 LLM**：视觉 + 语言 + 动作统一。
- 输入：图像 + 用户指令
- 输出：机器人动作序列
- 结合 BCI 的语言接口可做脑控 PaLM-E

### RT-2（Google 2023）

**Vision-Language-Action（VLA）** 模型：
- 把机器人动作当作语言 token
- 直接从 LLM 输出运动命令
- BCI 可作 "文本提示生成器" 接入

### Voyager（Wang 2023）

**LLM 作为长期规划代理**：
- 技能发现、技能库、自我反思
- 本来用于 Minecraft，但为 BCI 辅助提供模版

## 四、BCI-LLM 接口设计

### 接口 1：自然语言

BCI → 语音/手写 → LLM

**优点**：LLM 原生接受  
**缺点**：带宽低，每分钟 ~60 词

适用：Willett 2023 语音 BCI + GPT-4。

### 接口 2：结构化意图

BCI → JSON/slot filling → LLM

```json
{"action": "fetch", "object": "water", "target": "me"}
```

**优点**：短小、确定性高  
**缺点**：意图词汇受限

### 接口 3：神经 Embedding

BCI → 潜空间向量 → LLM（作为 soft prompt）

**优点**：保留完整神经信息  
**缺点**：需要训练对齐  
**前沿**：**NeuroLM**（2024）尝试直接训练神经-语言对齐

## 五、LLM 在回路的挑战

### 延迟

LLM 推理 500 ms–2 s，对实时交互不够快。
解决：**边缘 LLM**（Llama-3 / Phi）+ 云端 GPT 混合。

### 幻觉

LLM 可能生成不存在的动作、错误物体位置。
解决：
- **Ground（接地）**：LLM 只能调用机器人已有技能
- **视觉验证**：执行前用 CLIP 确认物体存在
- **用户确认**：关键步骤 BCI 确认

### 安全

LLM 可能被用户（或被攻击）诱导做危险动作。
解决：**Constitutional AI** 风格规则约束。

## 六、训练策略

### SFT：监督微调

- 收集 (BCI 意图, LLM 计划, 机器人结果)
- Fine-tune LLM 更懂 BCI 场景

### RLHF：人类反馈强化学习

- 用户评分每个计划好坏
- PPO 优化 LLM 偏好

### In-context prompting

- 给 LLM 当前环境 + 技能库描述
- Zero-shot / few-shot 规划
- 适合快速迭代

## 七、开源工具链

| 工具 | 层级 | 功能 |
| --- | --- | --- |
| **MNE / Kilosort** | BCI 解码 | 预处理 |
| **NDT3 / CEBRA** | BCI 解码 | 潜空间 |
| **LangChain** | LLM | 规划、工具调用 |
| **Voyager / CoT-Robotics** | LLM | 技能学习 |
| **ROS2** | 机器人 | 通信 |
| **MoveIt** | 机器人 | 运动规划 |
| **SAM / CLIP** | 视觉 | 物体识别 |

## 八、监管与部署

BLR 系统的监管复杂：

- **BCI 层**：FDA/NMPA 医疗器械
- **LLM 层**：EU AI Act 高风险 AI
- **机器人层**：ISO 10218（工业）、ISO 13482（服务）

合规路径：**每一层单独认证 + 整体系统认证**。预计 2027–2030 才会有完整 BLR 系统获批商用。

## 九、与类人智能的对应

BLR 管道与 **Human_Like_Intelligence / world_model / JEPA** 思想对应：

| 类人智能 | BLR |
| --- | --- |
| 预测编码（感觉 → 内部状态） | BCI 解码 |
| 世界模型（内部状态 → 行动） | LLM 规划 |
| 运动控制（行动 → 输出） | 机器人执行 |
| 环境反馈 | 视觉/触觉回路 |

BLR **不是 AGI 模拟**，但它是 **"读取真实生物智能 + 注入人工智能"** 的工程模型——两者的互补结构是 BCI × 类人智能合流的根本。

## 十、标志性里程碑

- **2022**：Microsoft + Synchron demo Apple Vision OS BCI 控制
- **2023**：UCSF Metzger avatar：BCI → facial motion + 语言
- **2024 CES**：Synchron + Apple Vision Pro 演示
- **2024-Q4**：Neuralink 患者用 BCI + 语音助手日常对话
- **2026 预期**：完整 BCI + LLM + 机械臂辅助生活 demo

## 十一、逻辑链

1. **BCI 带宽不足**决定了必须有**更高层的"扩展器"**——LLM 是最佳候选。
2. **分层规划**：BCI 提取意图 → LLM 扩展计划 → 机器人执行。
3. **接口设计**有三种（自然语言、结构化、embedding），各有权衡。
4. **延迟、幻觉、安全**是 BLR 的核心工程挑战。
5. **BLR 是 BCI 和类人智能工作的合流点**——2024 后的主流研究方向。

## 参考文献

- Chen et al. (2023). *HiCRISP: An LLM-driven hierarchical closed-loop robotic intelligent self-correction planner.* arXiv:2309.12089.
- Driess et al. (2023). *PaLM-E: an embodied multimodal language model.* arXiv. https://palm-e.github.io/
- Brohan et al. (2023). *RT-2: vision-language-action models transfer web knowledge to robotic control.* CoRL.
- Wang et al. (2023). *Voyager: an open-ended embodied agent with large language models.* arXiv. https://voyager.minedojo.org/
- Metzger et al. (2023). *A high-performance neuroprosthesis for speech decoding and avatar control.* Nature. https://www.nature.com/articles/s41586-023-06443-4
