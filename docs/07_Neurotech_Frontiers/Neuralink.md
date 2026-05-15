# Neuralink — 高密度植入式 BCI

> *Neuralink 是 Elon Musk 2016 创办的脑机接口公司,核心技术是 N1 implant — 1,024 个高密度电极 + 6 个 thread bundle + 自动手术机器人 R1。2024 年首例人体植入 (Noland Arbaugh)。目标:解决瘫痪 → 视觉恢复 → 长远 brain-AI symbiosis。是密度上的颠覆,但仍是 invasive 路线,需多年验证。*
>
> **难度**:Intermediate
> **前置知识**:[BCI History](../06_Brain_Computer_Interface/index.md)、[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. 公司背景

- 2016 由 Musk + 联合创办人(Max Hodak 等)成立
- 2020 演示 pig brain implant (Gertrude)
- 2021 演示 monkey playing Pong with thought (Pager)
- 2023 FDA 批准 IDE (Investigational Device Exemption)
- 2024-01 首例人体植入(Noland Arbaugh,脊髓损伤)
- 2024-08 第二例(Alex)
- 2025-05 公开 PRIME 试验前两位受试者表现

---

## 2. N1 Chip 规格

| 参数 | 值 |
|---|---|
| 电极数 | 1,024 |
| Threads | 64 |
| Electrodes / thread | 16 |
| Thread 直径 | 5 μm (头发 1/10) |
| Sample rate | 18 kHz / channel |
| 通信 | BLE → external |
| 功耗 | < 1 W (无线充电) |
| Battery | 全天电池 |

---

## 3. R1 Surgical Robot

- 自动 thread insertion(神经外科医生监督)
- 微显微 + 光学 vessel avoidance
- < 1 hour 手术(开颅 + 植入)
- Reduces human error,关键 commercial viability

---

## 4. 信号处理 Pipeline

```
1,024 electrodes
    ↓ 18 kHz sample
    ↓ ADC + spike detection (on chip)
    ↓ thresholding + sorting
Compressed spike events
    ↓ BLE
External device (Macbook/iPad)
    ↓ decoder (logistic regression / NN)
Cursor / action
```

- On-chip pre-processing 减少 BLE 带宽
- Spike detection threshold 自动校准

---

## 5. PRIME Trial 进展

- **Noland Arbaugh** (Patient 1):
  - 2024-01 implant
  - 数周内 control 屏幕 cursor、玩游戏(Civilization, Mario Kart)
  - 但 thread retraction 减少 active channels → algorithm 补偿
- **Alex** (Patient 2):
  - 2024-08
  - 较 Noland 表现更好
  - 学会 CAD design
- 公开演示:cursor 速度接近健康人

---

## 6. PyTorch — Spike Decoder (简化)

```python
import torch

class SpikeDecoder(torch.nn.Module):
    """Decode 2D cursor from spike rates."""
    def __init__(self, n_channels=1024, hidden=128):
        super().__init__()
        self.encoder = torch.nn.Linear(n_channels, hidden)
        self.lstm = torch.nn.LSTM(hidden, hidden, batch_first=True)
        self.head = torch.nn.Linear(hidden, 2)  # x, y velocity
    
    def forward(self, spike_rates):
        # spike_rates: (B, T, n_channels) — binned rates over time
        h = torch.relu(self.encoder(spike_rates))
        out, _ = self.lstm(h)
        velocity = self.head(out)
        return velocity
```

---

## 7. 竞争对手

| 公司 | 路线 | 现状 |
|---|---|---|
| Neuralink | 1024 thread 植入 | PRIME 试验 |
| Synchron | Stentrode 血管内 | 6 人试验 (BCI from inside blood vessel) |
| Paradromics | Connexus 高密度 | 2025 试验启动 |
| BrainGate | Utah array (96) | 长期临床(经典) |
| Blackrock | Utah / Nanogap | 商业可用 |
| Precision | LMA 微 array | 2025 试验 |
| MindMaze, Cogniwave 等 | 非侵入式 | 不直接竞争 |

---

## 8. 长远目标

- **近**:瘫痪患者 cursor / 输入(text, brain-to-speech)
- **中**:视觉恢复(Blindsight 项目)
- **远**:认知增强、brain-AI 直连(Musk vision)
- **争议**:long-term safety、商业模式、AI 直连风险

---

## 9. 工程挑战

- **Biocompatibility**: thread 周围 gliosis(疤痕组织)
- **Signal degradation**: 12 月后 channel 减少
- **Thread retraction**: Noland 案例 ~ 85% channel lost
- **Wireless power**: 充电不便,植入电池寿命
- **Surgery scaling**: R1 仍 expensive

---

## 10. Common Pitfalls

### 10.1 不是 mind reading

仅解码 motor intent、cursor,不是思想读取。

### 10.2 Telepathy 标签夸张

Musk 的 "telepathy" 名字误导;实际是 motor decoding。

### 10.3 Invasive ≠ Best

血管内 BCI (Synchron) 无开颅,trade-off 不同。

### 10.4 Channels 远不够

人脑 ~ 100B neurons;1024 仅 nano 区域 sampling。

### 10.5 商业模式不明

谁付钱?保险?DARPA?Musk private?

---

## 11. Related Concepts

- **同节**:[DBS](DBS.md)、[Optogenetics](Optogenetics.md)、[fMRI BOLD](fMRI_BOLD.md)
- **BCI 节**:[BCI 综述](../06_Brain_Computer_Interface/index.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)

---

## References

1. **Musk, E. & Neuralink** "An integrated brain-machine interface platform with thousands of channels." *J Med Internet Res*, 2019.
2. **Neuralink** "PRIME Study Progress Update." 2024-05.
3. **Willett, F. R. et al.** "A high-performance speech neuroprosthesis." *Nature*, 2023.
4. **Hochberg, L. R. et al.** "Reach and grasp by people with tetraplegia using a neurally controlled robotic arm." *Nature*, 2012.
