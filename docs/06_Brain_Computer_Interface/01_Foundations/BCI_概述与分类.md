# BCI 概述与分类

## 一、什么是脑机接口

**脑机接口（Brain-Computer Interface, BCI）** 是一类不依赖外周神经和肌肉、直接在大脑与外部设备之间建立信息通路的系统。它的核心定义包含三个要素：

1. **信号源**：直接记录或刺激中枢神经系统（大脑、脊髓），而不是记录肌电、眼动等外周生理信号。
2. **信息通路**：在大脑与机器之间双向传递信息——读出（decode）或写入（encode）神经活动。
3. **目的性**：服务于通信、控制、感觉恢复、神经调控等具体功能，而非纯粹记录。

一个系统只要同时满足这三条，就属于 BCI 范畴。这个定义排除了纯粹观察性的脑成像（如功能磁共振研究性扫描）和只监测外周的设备（如手表心率 + AI 判断情绪）。

---

## 二、三维分类体系

BCI 系统可以从三个正交维度来分类，这三个维度共同刻画了一个具体 BCI 系统的全部关键属性。

### 维度一：侵入性（Invasiveness）

这是最直接的分类维度，决定了信号的空间分辨率、信噪比和手术风险。

| 类别 | 电极位置 | 典型代表 | 空间分辨率 | 手术风险 |
| --- | --- | --- | --- | --- |
| **全侵入式（Intracortical）** | 皮层内部 | Utah 阵列、Neuropixels、Neuralink N1 | 单神经元级（~0.1 mm） | 高 |
| **皮层表面（ECoG / sEEG）** | 硬膜下 / 大脑皮层表面 | 临床 sEEG、Precision Layer 7 | ~1 mm | 中 |
| **微创（Minimally Invasive）** | 血管内 / 硬膜外 | Synchron Stentrode | ~几 mm | 低 |
| **非侵入式** | 头皮外部 | EEG、MEG、fMRI、fNIRS | cm 级（EEG） | 无 |

信号质量沿着这个序列单调递增，但手术风险也同步递增。**如何在"信号质量"与"手术侵入性"之间取舍**，是 BCI 工程设计的根本问题。

### 维度二：信号方向（Direction）

| 方向 | 定义 | 典型应用 |
| --- | --- | --- |
| **读出（Read-out）** | 从神经活动解码意图或感知 | 光标控制、语音 BCI、手写 BCI |
| **写入（Write-in）** | 向神经系统刺激信号 | 皮层电刺激（ICMS）、视觉假体、DBS |
| **双向（Bidirectional）** | 同时读写 | Flesher 2021 机械臂+触觉反馈、Ganzer 2020 Cell |

传统 BCI 以读出为主；2016 年以后，以双向 BCI 为代表的"读写闭环"成为新前沿（详见 [第 09 章](../09_Sensory_Writing_Bidirectional/index.md)）。

### 维度三：应用与使用场景

| 类别 | 使用对象 | 代表系统 | 监管路径 |
| --- | --- | --- | --- |
| **临床 BCI（Clinical）** | 瘫痪、ALS、失明等残疾患者 | BrainGate、Neuralink、Synchron、Neuracle | FDA IDE / NMPA 三类医疗器械 |
| **神经调控（Neuromodulation）** | 帕金森、癫痫、抑郁等 | Medtronic DBS、NeuroPace RNS | 已上市多款 |
| **研究用 BCI（Research）** | 动物实验、人类实验 | Neuropixels、各大学实验室 | IRB / IACUC |
| **消费级 BCI（Consumer）** | 健康人群 | Muse、Emotiv、OpenBCI | FCC / CE |
| **增强/增强现实 BCI** | 未来愿景 | 尚无真正商用 | 尚无监管框架 |

这个维度最能体现 2024-2026 的变化：**临床 BCI 正在从"单个试验"进入"多家公司、多国家并行商业化"的阶段**。

---

## 三、BCI 信号层级对照

不同采集方式看到的信号尺度差异巨大，这决定了能解码的内容上限。

| 信号 | 时间分辨率 | 空间分辨率 | 采样对象 | 典型通道数 |
| --- | --- | --- | --- | --- |
| **Spike（单神经元动作电位）** | 1 ms | 单细胞 | 10² 神经元 | 100–10k |
| **LFP（局部场电位）** | 1 ms | 100 μm–1 mm | 10³ 神经元群体 | 100–1k |
| **ECoG** | 1 ms | 1 mm | 10⁴ 神经元 | 64–256 |
| **EEG** | 1 ms | 1 cm | 10⁶ 神经元 | 8–256 |
| **MEG** | 1 ms | 5 mm | 10⁶ 神经元 | ~300 |
| **fMRI** | ~1 s（BOLD 延迟） | 1–3 mm | 10⁶ 神经元 | ~10⁵ voxel |
| **fNIRS** | ~1 s | 1–3 cm | 10⁶ 神经元 | ~50 |

**关键规律**：空间分辨率越高，能解码的意图粒度越细。Spike 级别信号才能解码单词级语音、复杂手写、精细手指运动；EEG 级别通常只能解码离散选择（如 P300 拼写器）或大幅运动想象。

---

## 四、主动式 vs 被动式 BCI

沿用 Zander & Kothe 2011 的分类，BCI 按用户意图参与程度还有一个正交划分：

- **主动式（Active BCI）**：用户主动产生意图（想象运动、尝试说话），系统解码并控制外部设备。这是绝大多数临床 BCI 的模式。
- **反应式（Reactive BCI）**：系统呈现外部刺激，用户被动反应；系统解码反应信号。典型代表是 P300 拼写器、SSVEP 拼写器。
- **被动式（Passive BCI）**：系统监测用户的认知或情绪状态，无需用户刻意产生意图。典型应用是疲劳检测、注意力监控。

这个划分对产品设计很关键——主动式 BCI 需要用户训练，被动式 BCI 可以隐形运行。Apple AirPods EEG 专利走的就是被动式路线。

---

## 五、"读脑 / 写脑" 能力今日边界

截至 2026 年初，各项能力的 SOTA 边界：

| 能力 | SOTA | 系统 / 时间 |
| --- | --- | --- |
| 运动想象解码（非侵入） | 6 类分类 ~80% | EEGNet 基线 |
| 二维光标控制（侵入） | 90 bit/min | BrainGate + ReFIT |
| 手写 BCI | 90 字符/分钟 | Willett 2021 Nature |
| 侵入式语音 BCI | 62 WPM, 9.1% WER | Willett 2023 Nature |
| 非侵入语音解码 | 41% 句子识别 | Meta Défossez 2023 |
| fMRI → 图像重建 | 接近照片级 | MindEye2 2024 |
| fMRI → 视频 | 低保真但语义可识别 | MinD-Video 2024 |
| 皮层视觉假体 | 可感知稳定 phosphene | Fernández 2021 |
| 皮层内微刺激（ICMS） | 90% 触觉检测率 | Flesher 2021 |

这些数字在**每 6–12 个月**都在更新。本章后续章节给出各自的详细技术路径。

---

## 六、与"类人智能"研究的共享关切

从 BCI 角度看，"如何把大脑变成可读写的计算对象" 本身就是一个类人智能问题：

- **表征**：神经群体活动坐落在低维流形上，这与"对象中心学习"、"潜空间预测"等类人智能概念是同构的。
- **动力学**：运动皮层作为动力系统（Churchland-Shenoy）与 JEPA 在潜空间的预测是同一问题的两面。
- **学习**：BCI 用户与解码器的共适应（co-adaptation）本质上是一个元学习问题。

这些概念会在 [第 10 章 与具身智能的连接](../10_Embodied_Intelligence_Link/index.md) 集中讨论。

---

## 七、逻辑链

1. **BCI = 中枢神经 × 直接通路 × 双向信息**，这三条同时满足才算 BCI。
2. **三维分类（侵入性 / 方向 / 应用场景）共同刻画一个具体系统**；任何 BCI 产品都应该在这三维坐标系下定位。
3. **信号尺度决定意图粒度**——spike 级才能做精细语音/手写；EEG 级适合离散选择。
4. **2024-2026 的变化发生在"临床 BCI 应用场景"维度**：从单个试验进入多公司多国家商业化。
5. **双向 BCI 正在成为新前沿**，因为读-写闭环是从"控制外物"走向"具身假体"的关键。

## 参考文献

- Wolpaw et al. (2002). *Brain-computer interfaces for communication and control.* Clinical Neurophysiology. — 经典定义性综述
- Zander & Kothe (2011). *Towards passive brain-computer interfaces.* J. Neural Engineering. — 主动/被动分类
- Hochberg et al. (2006). *Neuronal ensemble control of prosthetic devices.* Nature. https://www.nature.com/articles/nature04970
- Willett et al. (2023). *A high-performance speech neuroprosthesis.* Nature. https://www.nature.com/articles/s41586-023-06377-x
- Brain-Computer Interface 综述 (2024). Wiley Brain-X. https://onlinelibrary.wiley.com/doi/full/10.1002/brx2.70024
