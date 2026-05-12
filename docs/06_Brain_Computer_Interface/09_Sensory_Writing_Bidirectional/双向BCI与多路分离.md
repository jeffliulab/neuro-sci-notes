# 双向 BCI 与多路分离

**双向 BCI（bidirectional BCI, biBCI）** 同时做**读 + 写**——既从大脑解码意图，又刺激大脑写入信号。最大的工程挑战是**刺激 artifact**：刺激电流在采集电极上产生巨大 artifact，掩盖神经信号。**多路分离（channel isolation）** 是核心技术。

## 一、为什么要双向

### 单向 BCI 的局限

- **纯读**：知道用户想做什么，但用户无反馈感知机器输出
- **纯写**：能让用户感觉到东西，但不知道用户想要什么

### 双向的威力

**读 + 写 = 闭环 BCI**：
- 运动 BCI + ICMS：摸到自己控制的假肢
- 视觉假体 + V1 解码：戴摄像头 + 看到内容 + 系统学习用户反应
- 记忆假体：读海马体 + 写海马体 → 记忆增强
- 情绪闭环：读前额叶 + 深部刺激 → 抑郁治疗

## 二、Ganzer 2020 Cell 里程碑

**Ganzer et al. (2020)** 首个临床**双向 BCI**——让脊髓损伤患者**恢复自己手的感觉 + 运动**：

### 被试

- C5/C6 SCI（脊髓损伤）
- 手臂瘫痪、感觉部分丧失

### 系统

```
M1 Utah Array (读)
  ↓
意图解码 → 控制前臂刺激（FES）→ 自己的手动
  ↓
手上压力传感器
  ↓
S1 ICMS (写)
  ↓
感觉反馈
```

### 创新

- **读写同时进行**——M1 读，S1 写
- **原生手**：不是机械臂，而是**用自己瘫痪的手**，通过 FES 激活
- 患者**"感觉到自己的手握住杯子"**

### 结果

- 抓握效率提升 ~50%
- 触觉事件检测准确率 ~90%
- 患者主观"embodiment"评分高

## 三、刺激 artifact 问题

### 问题规模

- 典型神经信号：100 μV
- 典型 ICMS 电流：20–100 μA
- 产生的 **artifact**：100 mV 级——**1000× 大于信号**

相邻电极甚至同芯片其他通道 saturate，无法采集。

### 为什么棘手

ICMS 必须在**用户动作同时**触发（接触即感觉）——此时 M1 解码也**最需要实时信号**。刺激时间 = 解码时间的**关键冲突**。

## 四、多路分离技术

### 1. Blanking

刺激瞬间**禁用采集**：
- 刺激脉冲 200 μs，前后 1 ms blank
- M1 信号暂时丢失 ~2.5 ms
- 解码算法需要**抗丢失**

优点：简单；缺点：信号丢失。

### 2. 模板减法（template subtraction）

- 测量无脑活动时的 artifact 模板
- 真实采集中减去模板
- 要求 artifact **可重复**

### 3. 差分放大 + 硬件隔离

- 刺激通道 vs 采集通道物理分离
- 共模抑制去除 artifact
- **Blackrock Cerebus + Ripple 系统**支持

### 4. 时分复用

- 刺激 / 采集**交替进行**
- ~1 kHz 切换
- 需要快速切换电路

### 5. 自适应滤波

- 使用 ICA / 盲源分离在线学习 artifact 结构
- 去除后保留神经信号

## 五、闭环系统架构

现代双向 BCI 典型结构：

```
大脑 M1 → 神经放大 → 解码器 → 控制 assistive device
                                      ↓
                                    传感器（力/位置/环境）
                                      ↓
                                    感觉编码器
                                      ↓
                                    ICMS 刺激器 → S1 大脑
```

**时间预算**：整个闭环 < 100 ms（生理延迟），否则失去"因果感"。

## 六、Hochberg 组 biBrainGate

Brown University + Massachusetts General 的 **BrainGate biBRain** 项目：

- 多阵列（M1 + S1 + PPC）
- 软件：**xPC** 实时系统
- 2024 首次展示完整 bi-directional 任务
- 目标：**人体长期试验** 2025–2027

## 七、Neuralink 双向方向

N1 架构原生支持双向（**每通道可读或刺激**）：
- 1024 通道**可配置**
- 正尝试 S1 + M1 双阵列
- 公开资料有限

## 八、工程细节

### 采样率

- 采集：30 kHz / 通道
- 刺激：10 kHz 控制
- 两者同步需**高精度硬件时钟**

### 低延迟解码

- Transformer 解码器可能**太慢** —— RNN / CNN 更实时
- **NDT3**：~50 ms 延迟
- **EEGNet**：~10 ms
- 选择取决于任务

### 多阵列协调

- 每阵列独立放大器
- 中央处理器（FPGA / embedded）实时融合
- 操作系统：常见是 **ROS2** 或自定义实时内核

## 九、闭环校准（CLDA × 感觉）

双向 BCI 需要**双向校准**：

- M1 解码器：CLDA（[ReFIT 与在线校准](../04_Classical_Decoding/ReFIT与在线校准.md)）
- S1 编码器：**"哪个电极对应什么触感"** 校准
- 用户学习与系统学习**双重**

这是复杂的**协同优化**问题——但经过几周训练通常稳定。

## 十、应用

### 瘫痪

**M1 解码 + S1 写入** → 机械臂 / FES。

### 双侧截肢

**M1 解码 + 假肢 + S1 写入** → 几乎完整手功能。

### 记忆障碍

**海马体读 + 写** → Alzheimer 等病。见 [记忆假体](记忆假体.md)。

### 精神疾病

**前额叶读 + DBS 写** → 抑郁症闭环治疗（**Mayberg 等的工作**）。

## 十一、伦理

### 读写权

读和写的伦理**非对称**：
- 读：隐私问题
- 写：自主性问题（谁控制"我的大脑"？）

### "不可控"时刻

当系统**自动写入** → 用户可能感到**"被接管"**——紧急停止机制必须。

### 合规

FDA 对双向 BCI 监管比单向更严——刺激风险叠加解码依赖。

## 十二、逻辑链

1. **双向 BCI = 读 + 写** 闭环——比单向更自然、更强大。
2. **Ganzer 2020** 首次人类双向：M1 → 自己的手 → S1。
3. **刺激 artifact** 是核心工程挑战（1000× 信号）。
4. **多路分离**技术：blanking、模板减法、硬件隔离、时分、自适应滤波。
5. **闭环延迟** < 100 ms 是生理要求。
6. **双向校准** 要求解码器 + 编码器协同学习。
7. **伦理**：读 vs 写的非对称性，自动写入的接管风险。

## 参考文献

- Ganzer et al. (2020). *Restoring the sense of touch using a sensorimotor demultiplexing neural interface.* Cell. https://www.cell.com/cell/fulltext/S0092-8674(20)30347-2
- O'Doherty et al. (2011). *Active tactile exploration using a brain-machine-brain interface.* Nature. — 早期双向猴子实验
- Bouton et al. (2016). *Restoring cortical control of functional movement in a human with quadriplegia.* Nature.
- Flesher et al. (2021). *A brain-computer interface that evokes tactile sensations improves robotic arm control.* Science.
- Rao (2019). *Towards neural co-processors for the brain: combining decoding and encoding in brain-computer interfaces.* Current Opinion in Neurobiology.
