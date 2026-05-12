# Bidirectional BCI and Channel Separation

A **bidirectional BCI (biBCI)** does **reading + writing** simultaneously — both decoding intent from the brain and stimulating the brain to write signals back. The biggest engineering challenge is **stimulation artifact**: stimulation currents produce enormous artifacts on recording electrodes that mask neural signals. **Channel separation (channel isolation)** is the core technique.

## 1. Why Bidirectional

### Limits of unidirectional BCI

- **Read-only**: we know what the user wants to do, but the user has no feedback perception of the machine's output
- **Write-only**: we can make the user feel things, but we don't know what the user wants

### The power of bidirectional

**Read + write = closed-loop BCI**:
- Motor BCI + ICMS: feel the prosthesis you control
- Visual prosthesis + V1 decoding: wear a camera + see content + system learns user reactions
- Memory prosthesis: read hippocampus + write hippocampus → memory enhancement
- Emotion loop: read prefrontal cortex + deep stimulation → depression treatment

## 2. The Ganzer 2020 Cell Milestone

**Ganzer et al. (2020)** was the first clinical **bidirectional BCI** — restoring **sensation and movement of the patient's own hand** in a spinal-cord-injury patient:

### Subject

- C5/C6 SCI (spinal-cord injury)
- Arm paralysis, partial loss of sensation

### System

```
M1 Utah Array (read)
  ↓
Intent decoding → control forearm stimulation (FES) → own hand moves
  ↓
Pressure sensor on the hand
  ↓
S1 ICMS (write)
  ↓
Sensory feedback
```

### Innovations

- **Simultaneous read and write** — M1 reads, S1 writes
- **Native hand**: not a robotic arm but **the patient's own paralyzed hand**, activated via FES
- Patient reports **"feeling my own hand holding the cup"**

### Results

- Grasp efficiency improved by ~50%
- Tactile-event detection accuracy ~90%
- High subjective "embodiment" ratings

## 3. The Stimulation Artifact Problem

### Scale of the problem

- Typical neural signal: 100 μV
- Typical ICMS current: 20–100 μA
- Resulting **artifact**: on the order of 100 mV — **1000× larger than the signal**

Neighboring electrodes and even other channels on the same chip saturate and cannot record.

### Why it's hard

ICMS must be triggered **simultaneously with the user's action** (contact = sensation) — which is exactly when M1 decoding **most needs real-time signals**. Stimulation time = decoding time produces a **critical conflict**.

## 4. Channel-Separation Techniques

### 1. Blanking

**Disable acquisition during the stimulation instant**:
- Stimulation pulse 200 μs, 1 ms blank before and after
- M1 signal briefly lost for ~2.5 ms
- Decoding algorithm must be **robust to dropout**

Pros: simple; cons: signal loss.

### 2. Template subtraction

- Measure the artifact template in the absence of neural activity
- Subtract the template during real acquisition
- Requires the artifact to be **repeatable**

### 3. Differential amplification + hardware isolation

- Stimulation channels vs. acquisition channels physically separated
- Common-mode rejection cancels artifacts
- Supported by **Blackrock Cerebus + Ripple systems**

### 4. Time-division multiplexing

- Stimulation / acquisition **alternate**
- ~1 kHz switching
- Requires fast switching circuits

### 5. Adaptive filtering

- Learn artifact structure online using ICA / blind source separation
- Retain neural signal after removal

## 5. Closed-Loop System Architecture

Typical structure of modern bidirectional BCI:

```
Brain M1 → neural amplification → decoder → controls assistive device
                                               ↓
                                          sensors (force / position / environment)
                                               ↓
                                          sensory encoder
                                               ↓
                                          ICMS stimulator → S1 brain
```

**Timing budget**: the entire loop < 100 ms (physiological latency), otherwise the "sense of causation" is lost.

## 6. Hochberg Lab biBrainGate

Brown University + Massachusetts General's **BrainGate biBRain** project:

- Multiple arrays (M1 + S1 + PPC)
- Software: **xPC** real-time system
- 2024 first demonstration of a complete bidirectional task
- Goal: **long-term human trials** 2025–2027

## 7. Neuralink's Bidirectional Direction

The N1 architecture natively supports bidirectional operation (**each channel can read or stimulate**):
- 1024 **configurable** channels
- Attempting S1 + M1 dual arrays
- Limited public information

## 8. Engineering Details

### Sampling rate

- Acquisition: 30 kHz / channel
- Stimulation: 10 kHz control
- Synchronization between the two requires a **high-precision hardware clock**

### Low-latency decoding

- Transformer decoders may be **too slow** — RNN / CNN are more real-time
- **NDT3**: ~50 ms latency
- **EEGNet**: ~10 ms
- Choice depends on the task

### Multi-array coordination

- Each array has its own amplifier
- A central processor (FPGA / embedded) does real-time fusion
- Operating system: commonly **ROS2** or a custom real-time kernel

## 9. Closed-Loop Calibration (CLDA × Sensation)

Bidirectional BCI requires **bidirectional calibration**:

- M1 decoder: CLDA ([ReFIT and Online Calibration](../04_Classical_Decoding/ReFIT与在线校准.md))
- S1 encoder: **"which electrode corresponds to which sensation"** calibration
- **Dual learning** — both user and system

This is a complex **co-optimization** problem — but typically stabilizes after a few weeks of training.

## 10. Applications

### Paralysis

**M1 decoding + S1 writing** → robotic arm / FES.

### Bilateral amputation

**M1 decoding + prosthesis + S1 writing** → nearly complete hand function.

### Memory disorders

**Hippocampal read + write** → Alzheimer's and similar diseases. See [Memory Prosthesis](记忆假体.md).

### Psychiatric disease

**Prefrontal read + DBS write** → closed-loop depression treatment (**work by Mayberg and others**).

## 11. Ethics

### Read vs. write rights

The ethics of reading and writing are **asymmetric**:
- Read: privacy concerns
- Write: autonomy concerns (who controls "my brain"?)

### "Out-of-control" moments

When the system **writes automatically** → the user may feel **"taken over"** — emergency-stop mechanisms are mandatory.

### Regulation

The FDA regulates bidirectional BCI more strictly than unidirectional — stimulation risk stacks on decoding dependency.

## 12. Logical Chain

1. **Bidirectional BCI = read + write** closed loop — more natural and more powerful than unidirectional.
2. **Ganzer 2020** was the first human bidirectional system: M1 → own hand → S1.
3. **Stimulation artifact** is the core engineering challenge (1000× the signal).
4. **Channel-separation** techniques: blanking, template subtraction, hardware isolation, time division, adaptive filtering.
5. **Closed-loop latency** < 100 ms is a physiological requirement.
6. **Bidirectional calibration** requires co-learning of decoder + encoder.
7. **Ethics**: the asymmetry of read vs. write and the takeover risk of automatic writing.

## References

- Ganzer et al. (2020). *Restoring the sense of touch using a sensorimotor demultiplexing neural interface.* Cell. https://www.cell.com/cell/fulltext/S0092-8674(20)30347-2
- O'Doherty et al. (2011). *Active tactile exploration using a brain-machine-brain interface.* Nature. — Early bidirectional monkey experiment
- Bouton et al. (2016). *Restoring cortical control of functional movement in a human with quadriplegia.* Nature.
- Flesher et al. (2021). *A brain-computer interface that evokes tactile sensations improves robotic arm control.* Science.
- Rao (2019). *Towards neural co-processors for the brain: combining decoding and encoding in brain-computer interfaces.* Current Opinion in Neurobiology.
